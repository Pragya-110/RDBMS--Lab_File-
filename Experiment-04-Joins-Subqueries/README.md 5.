# EXPERIMENT 04: JOINS, SUBQUERIES, EXISTS, SIMULATED INTERSECT / EXCEPT AND EXPLAIN

---

## STUDENT DETAILS

| **Field** | **Details** |
|---|---|
| **Name** | Pragya Dubey |
| **UID No.** | 25LBCS3110 |
| **Course** | B.Tech CSE (AI/ML-3) |
| **Subject** | RDBMS (25CSH-210) |
| **Submitted To** | Mr. Amit Kumar Sir, Mr. Ilapanda Pavan Kumar Sir |

---

## AIM

Using the Employee schema, write SQL queries with INNER JOIN, LEFT JOIN, self join, 3-way join, correlated subqueries, EXISTS, and simulated INTERSECT and EXCEPT. Compare their execution plans using EXPLAIN.

---

## SOFTWARE / TOOLS USED

- MySQL Server 8.0+
- MySQL Command-Line Client
- Visual Studio Code (VS Code)
- Company database created in Experiment 3

---

## THEORY

### 1. JOINS

| **Join Type** | **Description** |
|---|---|
| INNER JOIN | Returns only the rows that match in both tables. |
| LEFT JOIN | Returns all rows from the left table and matching rows from the right table. |
| RIGHT JOIN | Returns all rows from the right table and matching rows from the left table. |
| Self Join | Joins a table with itself using aliases. |
| Multi-way Join | Joins three or more tables using join conditions. |
| Anti-Join | Returns rows from one table that have no matching rows in another table. |

### 2. SUBQUERIES

- **Non-correlated Subquery:** Runs independently of the outer query.
- **Correlated Subquery:** Refers to a column of the outer query and is logically evaluated for each outer row.
- **EXISTS:** Returns TRUE if the subquery returns at least one row.
- **NOT EXISTS:** Returns TRUE if the subquery returns no rows.

### 3. INTERSECT AND EXCEPT

MySQL supports native INTERSECT and EXCEPT from version 8.0.31. These operations can also be simulated using other SQL techniques.

| **Set Operation** | **Simulation** |
|---|---|
| INTERSECT (A ∩ B) | IN, INNER JOIN or EXISTS |
| EXCEPT (A − B) | NOT IN, LEFT JOIN ... IS NULL or NOT EXISTS |

### 4. EXPLAIN

EXPLAIN displays the execution plan selected by the MySQL optimizer.

Important columns:

- **type:** Shows the access method used by MySQL.
- **key:** Shows the index selected by the optimizer.
- **rows:** Displays the estimated number of rows examined.
- **Extra:** Provides additional information about query execution.

Common access types include ALL, index, range, ref, eq_ref and const.

- **EXPLAIN FORMAT=TREE:** Displays the execution plan as an iterator tree.
- **EXPLAIN ANALYZE:** Executes the query and displays actual execution information, including time and rows.

---

## PROCEDURE

1. Selected the `company_db` database created in Experiment 3.
2. Wrote join queries (J1–J9).
3. Wrote subquery and EXISTS queries (S1–S7).
4. Created INTERSECT and EXCEPT simulations (I1–I3, E1–E3).
5. Verified the simulated operations against native INTERSECT and EXCEPT.
6. Used ANALYZE TABLE to refresh table statistics.
7. Compared execution plans using EXPLAIN, EXPLAIN FORMAT=TREE and EXPLAIN ANALYZE.

---

## HOW TO RUN

First, execute Experiment 3 once to create the database and tables:

```bash
mysql -u root -t -vvv < ../Experiment-03-Employee-Queries/exp03_employee_queries.sql
```

Then execute Experiment 4:

```bash
mysql -u root -t -vvv < exp04_joins_subqueries.sql
```

---

## SOURCE CODE

**SQL File:** `exp04_joins_subqueries.sql`

**Output File:** `output.txt`

---

## OUTPUT

### 1. JOINS

**J2. INNER JOIN with Aggregation – Department Head and Headcount**

```sql
SELECT d.dept_name,
       CONCAT(m.first_name,' ',m.last_name) AS dept_head,
       COUNT(e.emp_id) AS headcount
FROM department d
INNER JOIN employee m ON m.emp_id = d.mgr_emp_id
INNER JOIN employee e ON e.dept_id = d.dept_id
GROUP BY d.dept_name, dept_head;
```

**Output:**

| dept_name | dept_head | headcount |
|---|---|---:|
| Engineering | Ananya Iyer | 11 |
| Sales | Suresh Patel | 7 |
| Marketing | Ritu Malhotra | 6 |
| Finance | Gopal Krishnan | 4 |
| Human Resources | Farah Khan | 4 |

**J3. LEFT JOIN – Projects with Number of Members**

```sql
SELECT p.project_id, p.project_name, p.status,
       COUNT(w.emp_id) AS members
FROM project p
LEFT JOIN works_on w ON w.project_id = p.project_id
GROUP BY p.project_id, p.project_name, p.status
ORDER BY members;
```

**Output:**

| project_id | project_name | status | members |
|---:|---|---|---:|
| 108 | AI Customer Chatbot | Planned | 0 |
| 103 | Cloud Migration | Completed | 3 |
| 107 | Campus Hiring Drive 2025 | Completed | 4 |
| 106 | Payroll Automation | Active | 5 |
| 102 | Mobile App Revamp | Active | 6 |
| 101 | UPI Payments Gateway | Active | 7 |
| 104 | Diwali Festive Campaign | Active | 7 |
| 105 | Tier-2 Dealer Expansion | Active | 7 |

**J4. LEFT JOIN with IS NULL – Employees Not Assigned to Any Project**

```sql
SELECT e.emp_id,
       CONCAT(e.first_name,' ',e.last_name) AS employee,
       e.job_title
FROM employee e
LEFT JOIN works_on w ON w.emp_id = e.emp_id
WHERE w.emp_id IS NULL;
```

**Output:**

| emp_id | employee | job_title |
|---:|---|---|
| 1 | Rajesh Sharma | Chief Executive Officer |
| 17 | Divya Bhat | Key Account Manager |
| 25 | Gopal Krishnan | Chief Financial Officer |

**J5. SELF JOIN – Employee with Manager**

```sql
SELECT e.emp_id,
       CONCAT(e.first_name,' ',e.last_name) AS employee,
       e.job_title,
       CONCAT(m.first_name,' ',m.last_name) AS manager,
       m.job_title AS manager_title
FROM employee e
LEFT JOIN employee m ON e.manager_id = m.emp_id
ORDER BY e.emp_id
LIMIT 15;
```

**J6. SELF JOIN – Number of Direct Reports**

```sql
SELECT m.emp_id,
       CONCAT(m.first_name,' ',m.last_name) AS manager,
       COUNT(e.emp_id) AS direct_reports
FROM employee m
JOIN employee e ON e.manager_id = m.emp_id
GROUP BY m.emp_id, manager
ORDER BY direct_reports DESC;
```

**J8. THREE-WAY JOIN – Employee, Works_On and Project**

```sql
SELECT CONCAT(e.first_name,' ',e.last_name) AS employee,
       p.project_name,
       w.role,
       w.hours_per_week
FROM employee e
JOIN works_on w ON w.emp_id = e.emp_id
JOIN project p ON p.project_id = w.project_id
WHERE p.project_id IN (101, 104)
ORDER BY p.project_name, w.hours_per_week DESC;
```

**J9. FOUR-WAY JOIN – Employees Working on Another Department's Project**

```sql
SELECT CONCAT(e.first_name,' ',e.last_name) AS employee,
       ed.dept_name AS home_dept,
       p.project_name,
       pd.dept_name AS project_dept
FROM employee e
JOIN department ed ON ed.dept_id = e.dept_id
JOIN works_on w ON w.emp_id = e.emp_id
JOIN project p ON p.project_id = w.project_id
JOIN department pd ON pd.dept_id = p.dept_id
WHERE e.dept_id <> p.dept_id;
```

---

### 2. CORRELATED SUBQUERIES AND EXISTS

**S2. Employees Earning More Than Their Department's Average Salary**

```sql
SELECT e.emp_id, e.first_name, e.dept_id, e.salary
FROM employee e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employee e2
    WHERE e2.dept_id = e.dept_id
)
ORDER BY e.dept_id, e.salary DESC;
```

**Output:**

| emp_id | first_name | dept_id | salary |
|---:|---|---:|---:|
| 1 | Rajesh | 10 | 350000.00 |
| 2 | Ananya | 10 | 220000.00 |
| 3 | Vikram | 10 | 160000.00 |
| 9 | Aditya | 10 | 155000.00 |
| 12 | Suresh | 20 | 200000.00 |
| 13 | Neha | 20 | 130000.00 |
| 17 | Divya | 20 | 90000.00 |
| 19 | Ritu | 30 | 190000.00 |
| 20 | Sanjay | 30 | 110000.00 |
| 25 | Gopal | 40 | 210000.00 |
| 29 | Farah | 50 | 170000.00 |
| 30 | Varun | 50 | 95000.00 |

**S3. Lowest-Paid Employee of Each Department**

```sql
SELECT e.dept_id, e.first_name, e.last_name, e.salary
FROM employee e
WHERE e.salary = (
    SELECT MIN(salary)
    FROM employee x
    WHERE x.dept_id = e.dept_id
);
```

**S5. Departments Having at Least One Completed Project**

```sql
SELECT d.dept_id, d.dept_name
FROM department d
WHERE EXISTS (
    SELECT 1
    FROM project p
    WHERE p.dept_id = d.dept_id
      AND p.status = 'Completed'
);
```

**Output:**

| dept_id | dept_name |
|---:|---|
| 10 | Engineering |
| 50 | Human Resources |

**S6. Employees Not Working on Any Project**

```sql
SELECT e.emp_id, e.first_name, e.last_name
FROM employee e
WHERE NOT EXISTS (
    SELECT 1
    FROM works_on w
    WHERE w.emp_id = e.emp_id
);
```

**Output:**

| emp_id | first_name | last_name |
|---:|---|---|
| 1 | Rajesh | Sharma |
| 17 | Divya | Bhat |
| 25 | Gopal | Krishnan |

**S7. Managers Having at Least One Report Earning More Than 1,00,000**

```sql
SELECT m.emp_id, m.first_name, m.job_title
FROM employee m
WHERE EXISTS (
    SELECT 1
    FROM employee e
    WHERE e.manager_id = m.emp_id
      AND e.salary > 100000
);
```

---

### 3. SIMULATED INTERSECT AND EXCEPT

**I1. INTERSECT Using IN – Employees Working on Both Projects 101 and 102**

```sql
SELECT emp_id
FROM works_on
WHERE project_id = 101
  AND emp_id IN (
      SELECT emp_id
      FROM works_on
      WHERE project_id = 102
  );
```

**Output:**

| emp_id |
|---:|
| 6 |
| 10 |

**I2. INTERSECT Using INNER JOIN**

```sql
SELECT a.emp_id, e.first_name
FROM works_on a
JOIN works_on b
  ON a.emp_id = b.emp_id
 AND b.project_id = 102
JOIN employee e ON e.emp_id = a.emp_id
WHERE a.project_id = 101;
```

**I3. INTERSECT Using EXISTS – Cities with Engineering and Sales Staff**

```sql
SELECT DISTINCT e.city
FROM employee e
WHERE e.dept_id = 10
  AND EXISTS (
      SELECT 1
      FROM employee s
      WHERE s.dept_id = 20
        AND s.city = e.city
  );
```

**Output:**

| city |
|---|
| Bengaluru |

**E1. EXCEPT Using NOT IN – Employees on Project 101 but Not on Project 102**

```sql
SELECT emp_id
FROM works_on
WHERE project_id = 101
  AND emp_id NOT IN (
      SELECT emp_id
      FROM works_on
      WHERE project_id = 102
  );
```

**Output:**

| emp_id |
|---:|
| 2 |
| 3 |
| 4 |
| 5 |
| 11 |

**E2. EXCEPT Using LEFT JOIN and IS NULL**

```sql
SELECT a.emp_id, e.first_name
FROM works_on a
LEFT JOIN works_on b
  ON a.emp_id = b.emp_id
 AND b.project_id = 102
JOIN employee e ON e.emp_id = a.emp_id
WHERE a.project_id = 101
  AND b.emp_id IS NULL;
```

**E3. EXCEPT Using NOT EXISTS – Engineering Cities Without Sales Staff**

```sql
SELECT DISTINCT e.city
FROM employee e
WHERE e.dept_id = 10
  AND NOT EXISTS (
      SELECT 1
      FROM employee s
      WHERE s.dept_id = 20
        AND s.city = e.city
  );
```

**Output:**

| city |
|---|
| Chennai |
| Pune |
| Kochi |
| Hyderabad |

---

### 4. VERIFICATION WITH NATIVE SET OPERATORS

**INTERSECT**

```sql
SELECT emp_id
FROM works_on
WHERE project_id = 101

INTERSECT

SELECT emp_id
FROM works_on
WHERE project_id = 102;
```

**EXCEPT**

```sql
SELECT emp_id
FROM works_on
WHERE project_id = 101

EXCEPT

SELECT emp_id
FROM works_on
WHERE project_id = 102;
```

Both native operations were checked against their simulated versions. The returned rows were the same.

---

### 5. EXECUTION PLANS USING EXPLAIN

**X1. Correlated Subquery – Traditional EXPLAIN**

```sql
EXPLAIN FORMAT=TRADITIONAL
SELECT e.emp_id, e.first_name, e.salary
FROM employee e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employee e2
    WHERE e2.dept_id = e.dept_id
);
```

**X2. Same Query Rewritten Using JOIN**

```sql
EXPLAIN FORMAT=TRADITIONAL
SELECT e.emp_id, e.first_name, e.salary
FROM employee e
JOIN (
    SELECT dept_id, AVG(salary) AS avg_sal
    FROM employee
    GROUP BY dept_id
) d ON d.dept_id = e.dept_id
WHERE e.salary > d.avg_sal;
```

**X3. Tree Format Execution Plan**

```sql
EXPLAIN FORMAT=TREE
SELECT e.emp_id, e.first_name, e.salary
FROM employee e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employee e2
    WHERE e2.dept_id = e.dept_id
);
```

**X4. Comparing NOT EXISTS, NOT IN and LEFT JOIN**

```sql
EXPLAIN FORMAT=TRADITIONAL
SELECT e.emp_id
FROM employee e
WHERE NOT EXISTS (
    SELECT 1
    FROM works_on w
    WHERE w.emp_id = e.emp_id
);
```

```sql
EXPLAIN FORMAT=TRADITIONAL
SELECT e.emp_id
FROM employee e
WHERE e.emp_id NOT IN (
    SELECT w.emp_id
    FROM works_on w
);
```

```sql
EXPLAIN FORMAT=TRADITIONAL
SELECT e.emp_id
FROM employee e
LEFT JOIN works_on w ON w.emp_id = e.emp_id
WHERE w.emp_id IS NULL;
```

**X5. THREE-WAY JOIN – EXPLAIN ANALYZE**

```sql
EXPLAIN ANALYZE
SELECT e.first_name, p.project_name
FROM employee e
JOIN works_on w ON w.emp_id = e.emp_id
JOIN project p ON p.project_id = w.project_id
WHERE p.status = 'Active';
```

**X6. Effect of Index on a Filter Column**

```sql
EXPLAIN FORMAT=TRADITIONAL
SELECT emp_id, first_name
FROM employee
WHERE city = 'Mumbai';
```

Create an index:

```sql
CREATE INDEX idx_emp_city ON employee(city);
```

Check the execution plan again:

```sql
EXPLAIN FORMAT=TRADITIONAL
SELECT emp_id, first_name
FROM employee
WHERE city = 'Mumbai';
```

Remove the index:

```sql
DROP INDEX idx_emp_city ON employee;
```

---

## OBSERVATIONS

| **Comparison** | **Observation** |
|---|---|
| Correlated Subquery vs JOIN | The correlated subquery uses a dependent subquery, while the JOIN version calculates department averages using a derived table. |
| NOT EXISTS vs NOT IN vs LEFT JOIN | MySQL can optimize these forms as anti-joins. |
| Three-Way JOIN | The optimizer uses join conditions and indexes to retrieve matching records. |
| Index Before and After | The city filter changes from a full table scan (ALL) to an index lookup (ref) after creating an index. |
| INTERSECT and EXCEPT | Simulated operations returned the same rows as the native set operators. |
| LEFT JOIN | Project 108 remains in the result with zero members, unlike an INNER JOIN. |

---

## RESULT

All the join types, correlated subqueries, EXISTS / NOT EXISTS, and simulated INTERSECT and EXCEPT queries were written and checked successfully. The simulated set operations were verified against MySQL's native INTERSECT and EXCEPT. Execution plans were compared using EXPLAIN, EXPLAIN FORMAT=TREE and EXPLAIN ANALYZE.

---

## CONCLUSION

Different SQL queries can produce the same result using joins, subqueries and EXISTS. EXPLAIN helps understand how MySQL executes these queries. Indexing and suitable query structures can improve query performance and reduce unnecessary data scanning.
