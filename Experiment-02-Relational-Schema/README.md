# EXPERIMENT 02: ER DIAGRAM TO RELATIONAL SCHEMA (MYSQL)

---

## STUDENT DETAILS

| **Field** | **Details** |
|:---|:---|
| **Name** | Pragya Dubey |
| **UID No.** | 25LBCS3110 |
| **Course** | B.Tech CSE (AI/ML-3) |
| **Subject** | RDBMS (25CSH-210) |
| **Submitted To** | Mr. Amit Kumar Sir |
| **Submitted To** | Mr. Ilapanda Pavan Kumar Sir |

---

## AIM

To convert the ER Diagram created in Experiment 1 into a Relational Schema and implement it using MySQL. To apply Primary Key, Foreign Key, NOT NULL, UNIQUE and referential integrity constraints such as ON DELETE CASCADE and ON DELETE SET NULL.

---

## THEORY

### 1. ER Diagram to Relational Schema

ER-to-Relational Mapping is the process of converting an Entity Relationship Diagram into relational tables.

**Mapping Rules:**

- **Strong Entity:** Converted into a separate table.
- **Weak Entity:** Converted into a table containing the primary key of its owner entity.
- **One-to-One Relationship:** A foreign key is added to one of the participating tables.
- **One-to-Many Relationship:** Primary key of the one-side entity is added as a foreign key to the many-side entity.
- **Many-to-Many Relationship:** A separate table is created.
- **Multivalued Attribute:** A separate table is created for the multivalued attribute.

### 2. Integrity Constraints

Integrity constraints maintain the accuracy and consistency of data.

| **Constraint** | **Description** |
|:---|:---|
| PRIMARY KEY | Uniquely identifies each record. |
| FOREIGN KEY | Maintains relationships between tables. |
| NOT NULL | Prevents NULL values. |
| UNIQUE | Prevents duplicate values. |
| Referential Integrity | Ensures valid references between tables. |

---

## RELATIONAL SCHEMA

The following 15 tables are created for the E-commerce Database:

1. Customer
2. Customer_Phone
3. Address
4. Seller
5. Seller_Contact
6. Category
7. Product
8. Electronics
9. Clothing
10. Book
11. Book_Author
12. Orders
13. Order_Item
14. Payment
15. Delivery

---

## PROCEDURE

1. Open MySQL Command Line or Terminal.
2. Create the E-commerce database.
3. Convert the ER Diagram into relational tables.
4. Define Primary Keys and Foreign Keys.
5. Apply NOT NULL and UNIQUE constraints.
6. Apply ON DELETE CASCADE and ON DELETE SET NULL.
7. Insert sample records into the tables.
8. Execute SQL queries to verify the database.
9. Test referential integrity constraints.

---

## EXECUTION COMMAND

```bash
mysql -u root -t -vvv --force < exp02_ecommerce_schema.sql
```

---

## SOURCE CODE

**File Name:** `exp02_ecommerce_schema.sql`

[Click Here to View SQL Source Code](https://github.com/dipanaditya0022-byte/RDBMS_LAB/blob/main/Experiment-02-Relational-Schema/exp02_ecommerce_schema.sql)

---

## OUTPUT

### 1. Table Creation

All 15 relational tables are created using MySQL CREATE TABLE statements.

### 2. Sample Data Insertion

Sample records are inserted into the respective database tables.

### 3. Constraint Testing

The following constraints are tested:

- Primary Key Constraint
- Foreign Key Constraint
- NOT NULL Constraint
- UNIQUE Constraint
- Referential Integrity Constraint

### 4. Referential Actions

| **Action** | **Function** |
|:---|:---|
| ON DELETE CASCADE | Deletes related records automatically. |
| ON DELETE SET NULL | Sets the foreign key value to NULL. |
| RESTRICT | Prevents deletion when related records exist. |

### 5. Constraint Violation Testing

Invalid INSERT and DELETE operations are performed to verify the working of database constraints.

---

## OBSERVATIONS

1. ER Diagram entities were successfully converted into relational tables.
2. Primary Key and Foreign Key constraints were implemented.
3. Integrity constraints maintained database consistency.
4. Referential actions were tested.
5. MySQL handled invalid operations according to the defined constraints.

---

## RESULT

The ER Diagram was successfully converted into a Relational Schema and implemented in MySQL. All required tables and integrity constraints were created and tested.

---

## CONCLUSION

This experiment helped in understanding the process of converting an ER Diagram into a Relational Schema. It also provided practical knowledge of MySQL table creation, keys, integrity constraints and referential actions.
