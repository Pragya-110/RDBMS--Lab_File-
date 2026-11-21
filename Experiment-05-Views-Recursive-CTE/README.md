# Experiment 5 - Views, View Updatability and Recursive CTE

| **Name** | **UID No** | **Course** | **Subject** | **Submitted To** |
|---|---|---|---|---|
| Pragya Dubey | 25LBCS3110 | B.Tech CSE (AI/ML-3) | RDBMS (25CSH-210) | Mr. Amit Kumar Sir, Mr. Ilapanda Pavan Kumar Sir |

## Aim
To create SQL views for department salary summaries and employee hierarchies, test view updatability, and implement recursive CTEs to display employee reporting chains.

## Software / Tools Used
- MySQL Server 8.0+
- MySQL Command-Line Client
- Visual Studio Code
- Existing `company_db` database from Experiment 3

## Theory

### 1. Views
A view is a virtual table created using a SELECT statement. It is used to simplify complex queries, provide data security, and hide unnecessary columns.

**Advantages of Views:**
- Simplifies complex SQL queries.
- Provides data security.
- Hides sensitive information.
- Supports logical data independence.

### 2. View Updatability
An updatable view allows INSERT, UPDATE, and DELETE operations on the underlying base table.

A view is generally not updatable if it contains:
- Aggregate functions.
- GROUP BY or HAVING clauses.
- DISTINCT or UNION.
- Subqueries in the SELECT list.
- Outer joins.

**WITH CHECK OPTION:** It prevents INSERT or UPDATE operations that would make a row disappear from the view.

### 3. Recursive CTE
A Recursive CTE is a Common Table Expression that refers to itself to process hierarchical data.

It contains two parts:
- **Anchor Member:** Provides the starting rows.
- **Recursive Member:** Repeatedly retrieves related rows.

It is useful for employee hierarchies, reporting chains, and organizational structures.

## Procedure

1. Used the existing `company_db` database.
2. Created six views:
   - `v_dept_salary_summary`
   - `v_employee_hierarchy`
   - `v_engineering_staff`
   - `v_high_earners`
   - `v_emp_basic`
   - `v_emp_dept`
3. Checked view updatability using `information_schema.VIEWS`.
4. Performed 13 INSERT, UPDATE, and DELETE tests through different views.
5. Used ROLLBACK to restore the original database records.
6. Implemented six recursive CTE queries:
   - R1: Top-down organisation tree.
   - R2: Bottom-up reporting chain.
   - R3: Reporting chain of every Engineering employee.
   - R4: Total team size and payroll.
   - R5: Hierarchy depth.
   - R6: Recursive CTE saved inside a view.

## How to Run

First, execute Experiment 3 to create and populate the database:

```bash
mysql -u root -t -vvv < ../Experiment-03-Employee-Queries/exp03_employee_queries.sql
```

Then execute Experiment 5:

```bash
mysql -u root -t -vvv --force < exp05_views_cte.sql
```

## Source Code

**File:** `exp05_views_cte.sql`

The SQL file contains:
- Creation of department salary summary view.
- Creation of employee hierarchy view.
- View updatability tests.
- INSERT, UPDATE, DELETE operations.
- Recursive CTE queries for employee reporting chains.

### Example SQL Query

```sql
CREATE VIEW v_dept_salary_summary AS
SELECT d.dept_id, d.dept_name, d.location,
       COUNT(e.emp_id) AS headcount,
       SUM(e.salary) AS monthly_payroll,
       ROUND(AVG(e.salary), 2) AS avg_salary
FROM department d
LEFT JOIN employee e ON e.dept_id = d.dept_id
GROUP BY d.dept_id, d.dept_name, d.location;
```

### Example Recursive CTE

```sql
WITH RECURSIVE org_chart AS (
    SELECT emp_id, manager_id,
           CONCAT(first_name, ' ', last_name) AS name,
           0 AS level
    FROM employee
    WHERE manager_id IS NULL

    UNION ALL

    SELECT e.emp_id, e.manager_id,
           CONCAT(e.first_name, ' ', e.last_name),
           oc.level + 1
    FROM employee e
    JOIN org_chart oc ON e.manager_id = oc.emp_id
)
SELECT * FROM org_chart;
```

## Output

**File:** `output.txt`

The output file contains:
- Department-wise salary summary.
- Employee hierarchy.
- View updatability status.
- Results of INSERT, UPDATE, and DELETE tests.
- Recursive reporting chains.
- Team size, payroll, and hierarchy depth.

## Observations

| **Test** | **Operation** | **Result** |
|---|---|---|
| T1 | UPDATE through simple view | Successful |
| T2 | INSERT through simple view | Successful |
| T2b | INSERT with missing NOT NULL columns | Error 1423 |
| T3–T4 | WITH CHECK OPTION tests | Error 1369 |
| T5 | UPDATE without CHECK OPTION | Successful |
| T6–T7 | UPDATE / DELETE on aggregate view | Error 1288 |
| T8 | UPDATE on LEFT JOIN view | Error 1288 |
| T9 | UPDATE on INNER JOIN view | Successful |
| T10 | UPDATE on two base tables | Error 1393 |
| T11 | DELETE on join view | Error 1395 |
| T12 | INSERT with missing NOT NULL columns | Error 1423 |

**Additional Observations:**
- The hierarchy contains 4 levels.
- The CEO has 31 other employees in the reporting hierarchy.
- ROLLBACK restored the original database records.

## Result

Six views were created successfully. Different view updatability rules were tested, and recursive CTEs were implemented to display employee reporting chains and organizational hierarchy.

## Conclusion

Views simplify complex SQL queries and provide data security. Simple views can be updatable, whereas aggregate and certain join views have restrictions. WITH CHECK OPTION maintains view conditions. Recursive CTEs help retrieve hierarchical data and reporting chains efficiently.
