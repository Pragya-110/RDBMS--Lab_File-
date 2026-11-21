# Experiment 7 - Functional Dependencies and Normalization (UNF → 1NF → 2NF → 3NF → BCNF)

| **Name** | **UID No** | **Course** | **Subject** | **Submitted To** |
|---|---|---|---|---|
| Pragya Dubey | 25LBCS3110 | B.Tech CSE (AI/ML-3) | RDBMS (25CSH-210) | Mr. Amit Kumar Sir, Mr. Ilapanda Pavan Kumar Sir |

---

## Aim
Given a denormalized Sales table, identify its functional dependencies and normalize it step by step from UNF to 1NF, 2NF, 3NF and BCNF. Verify the lossless-join and dependency-preservation properties using SQL queries.

## Software / Tools Used
- MySQL Server 8.0.31+
- MySQL Command-Line Client
- Visual Studio Code (VS Code)

## Theory

**Functional Dependency (FD):** X → Y means that if two rows have the same value of X, they must also have the same value of Y.

### Normal Forms

| **Normal Form** | **Condition** | **Removes** |
|---|---|---|
| UNF | Contains repeating groups and non-atomic values. | Repeating groups |
| 1NF | Every value must be atomic; no repeating groups. | Multi-valued cells |
| 2NF | 1NF + no partial dependency. | Partial dependencies |
| 3NF | 2NF + no transitive dependency. | Non-key dependencies |
| BCNF | Every determinant of a non-trivial FD must be a superkey. | Remaining anomalies |

**Lossless-Join Decomposition:** A decomposition is lossless if joining the decomposed tables reconstructs the original relation without producing spurious tuples.

**Dependency Preservation:** A decomposition is dependency-preserving if every functional dependency can be checked without joining multiple tables.

## Functional Dependencies Identified

| **FD** | **Functional Dependency** |
|---|---|
| F1 | invoice_no → invoice_date, customer_id, salesperson_id |
| F2 | customer_id → customer_name, customer_city |
| F3 | customer_city → customer_state |
| F4 | product_id → product_name, category, unit_price |
| F5 | salesperson_id → salesperson_name |
| F6 | invoice_no, product_id → qty |
| F7 | customer_id, category → account_mgr |
| F8 | account_mgr → category |

**Candidate Key:** (invoice_no, product_id)

## Normalization Steps

### 1. UNF (Unnormalized Form)
- Created the `sales_unf` table.
- Inserted 7 invoices containing 12 invoice lines.
- Product details, quantities and account managers were stored as comma-separated lists.
- These repeating groups violated the atomicity requirement of 1NF.

### 2. 1NF (First Normal Form)
- Converted repeating groups into atomic values.
- Used a helper numbers table and `SUBSTRING_INDEX()` to split comma-separated values.
- Created `sales_1nf` with one row per invoice-product combination.
- Defined the composite primary key `(invoice_no, product_id)`.

### 3. 2NF (Second Normal Form)
- Removed partial dependencies from the composite key.
- Created three tables:
  - `invoice_2nf`
  - `product_2nf`
  - `invoice_line_2nf`
- Separated invoice details, product details and invoice-line information.

### 4. 3NF (Third Normal Form)
- Removed transitive dependencies.
- Created the following tables:
  - `invoice_3nf`
  - `customer_3nf`
  - `city_3nf`
  - `salesperson_3nf`
  - `product_3nf`
  - `invoice_line_3nf`
  - `customer_account_mgr_3nf`
- The `customer_account_mgr_3nf` table satisfies 3NF but not BCNF because `account_mgr → category`, and `account_mgr` is not a superkey.

### 5. BCNF (Boyce-Codd Normal Form)
- Decomposed `customer_account_mgr_3nf` using the dependency `account_mgr → category`.
- Created:
  - `account_mgr_bcnf`
  - `customer_mgr_bcnf`
- Ensured that every determinant in the decomposed relations is a superkey.

## Verification Performed

### Functional Dependency Verification
- Verified all 8 functional dependencies using `GROUP BY` and `HAVING COUNT(DISTINCT ...)`.
- Checked 3 counter-examples that were not functional dependencies.
- Confirmed that the composite candidate key uniquely identifies all 12 rows.

### Lossless-Join Verification
- Reconstructed the original 1NF table by joining the 3NF relations.
- Compared original and rebuilt tables using `EXCEPT` in both directions.
- Both comparisons returned 0 differing rows.
- Verified that the BCNF decomposition also reconstructed the original 3NF relation correctly.

### Lossy Decomposition
- Demonstrated a lossy decomposition using `customer_id` as a common attribute.
- The original table contained 12 rows, while the join produced 22 rows.
- Extra rows were identified as spurious tuples.

### Dependency Preservation
- 3NF preserved all functional dependencies.
- 3NF rejected a duplicate customer-category manager assignment.
- BCNF accepted the same assignment, demonstrating that F7 was not preserved.
- F7 could only be detected by joining the two BCNF tables.
- BCNF directly enforced F8 through the primary key of `account_mgr_bcnf`.

## How to Run

Execute the SQL file using:

```bash
mysql -u root -t -vvv --force < exp07_normalization.sql
```

## Source Code
`exp07_normalization.sql`

## Output
Full execution output is available in:

`output.txt`

## Observations
- All 8 functional dependencies held on the given dataset.
- Three counter-checks were violated, confirming that they were not functional dependencies.
- Normalization reduced repeated data and update anomalies.
- The 3NF decomposition reconstructed all 12 original rows without differences.
- The lossy decomposition produced 22 rows instead of 12 due to spurious tuples.
- 3NF preserved F7, whereas BCNF did not preserve F7.
- BCNF successfully enforced F8.

## Result
The Sales table was successfully normalized from UNF to 1NF, 2NF, 3NF and BCNF. Functional dependencies were verified, and lossless-join and dependency-preservation properties were tested using SQL queries.

## Conclusion
Normalization helps reduce data redundancy and prevents insertion, update and deletion anomalies. 3NF provides a lossless and dependency-preserving decomposition, while BCNF removes remaining redundancy but may not preserve every functional dependency.

---

**Experiment 7 completed successfully.**
