# EXPERIMENT 03: EMPLOYEE-DEPARTMENT-PROJECT SCHEMA AND BASIC SQL QUERIES

---

## STUDENT DETAILS

| **Field** | **Details** |
|:---|:---|
| **Name** | Pragya Dubey |
| **UID No.** | 25LBCS3110 |
| **Course** | B.Tech CSE (AI/ML-3) |
| **Subject** | RDBMS (25CSH-210) |
| **Submitted To** | Mr. Amit Kumar Sir, Mr. Ilapanda Pavan Kumar Sir |

---

## AIM

To create an Employee-Department-Project schema and insert at least 30 employees across 5 departments and 8 projects. To write SQL queries demonstrating Selection, Projection, Aggregate Functions, GROUP BY, HAVING, CASE expressions and ORDER BY.

---

## SOFTWARE / TOOLS USED

- MySQL Server 8.0+
- MySQL Command-Line Client
- Visual Studio Code (VS Code)

---

## THEORY

| **Concept** | **Relational Algebra** | **SQL** |
|:---|:---|:---|
| Selection | σ condition (R) | WHERE |
| Projection | π columns (R) | SELECT |
| Aggregate Functions | 𝒢 | COUNT, SUM, AVG, MIN, MAX |
| GROUP BY | Grouping 𝒢 agg (R) | GROUP BY |
| HAVING | σ after 𝒢 | HAVING |
| CASE | — | Conditional IF-THEN-ELSE expression |
| ORDER BY | τ | ASC / DESC |

### Logical Order of SQL Query Evaluation

```text
FROM → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT
```

---

## DATABASE SCHEMA

The following four tables are created:

### 1. DEPARTMENT

```text
department (
    dept_id PRIMARY KEY,
    dept_name UNIQUE,
    location,
    mgr_emp_id FOREIGN KEY → employee
)
```

### 2. EMPLOYEE

```text
employee (
    emp_id PRIMARY KEY,
    first_name,
    last_name,
    gender,
    dob,
    hire_date,
    job_title,
    salary,
    city,
    dept_id FOREIGN KEY → department,
    manager_id FOREIGN KEY → employee
)
```

### 3. PROJECT

```text
project (
    project_id PRIMARY KEY,
    project_name UNIQUE,
    dept_id FOREIGN KEY → department,
    budget,
    start_date,
    end_date,
    status
)
```

### 4. WORKS_ON

```text
works_on (
    emp_id PRIMARY KEY / FOREIGN KEY → employee,
    project_id PRIMARY KEY / FOREIGN KEY → project,
    role,
    hours_per_week
)
```

---

## DATABASE DETAILS

| **Description** | **Count / Details** |
|:---|:---|
| Departments | 5 |
| Employees | 32 |
| Projects | 8 |
| Project Assignments | 39 |
| Reporting Hierarchy | 4 Levels |

### Departments

1. Engineering – Bengaluru
2. Sales – Mumbai
3. Marketing – Delhi
4. Finance – Pune
5. HR – Hyderabad

**Note:** Project 108, "AI Customer Chatbot", has Planned status and currently has no employees assigned.

The database `company_db` is reused in Experiments 4, 5 and 6. Re-running this script resets the data.

---

## PROCEDURE

1. Created the `company_db` database with four tables.
2. Defined Primary Keys, Foreign Keys and CHECK constraints.
3. Inserted department records.
4. Inserted employee records by adding manager rows before their reports.
5. Set department heads and added the `fk_dept_mgr` foreign key.
6. Inserted project records.
7. Inserted employee-project assignments into `works_on`.
8. Wrote 18 SQL queries covering the required operations.
9. Executed the queries and recorded their outputs.

---

## HOW TO RUN

Execute the following command in the MySQL terminal:

```bash
mysql -u root -t -vvv < exp03_employee_queries.sql
```

---

## SOURCE CODE

**File Name:** `exp03_employee_queries.sql`

The SQL file contains:

- Database and table creation
- Sample data insertion
- 18 SQL queries
- Selection and Projection
- Aggregate Functions
- GROUP BY and HAVING
- CASE expressions
- ORDER BY

---

## OUTPUT

### 1. ROW COUNTS AFTER LOADING DATA

The following query verifies the total records in all four tables:

```sql
SELECT (SELECT COUNT(*) FROM department) AS departments,
       (SELECT COUNT(*) FROM employee) AS employees,
       (SELECT COUNT(*) FROM project) AS projects,
       (SELECT COUNT(*) FROM works_on) AS assignments;
```

**Output:**

| Departments | Employees | Projects | Assignments |
|---:|---:|---:|---:|
| 5 | 32 | 8 | 39 |

---

### 2. SELECTION (Q1–Q3)

Selection is used to retrieve specific rows based on given conditions.

- **Q1:** Employees earning more than ₹1,00,000 per month.
- **Q2:** Female employees in Engineering hired after 2015.
- **Q3:** Employees from Mumbai and Pune earning between ₹50,000 and ₹1,00,000 with job titles containing "Executive".

**Result:** The queries successfully retrieved records using WHERE, AND, IN, BETWEEN and LIKE.

---

### 3. PROJECTION (Q4–Q6)

Projection is used to retrieve selected columns from a table.

- **Q4:** Display employee names and job titles.
- **Q5:** Display unique cities using DISTINCT.
- **Q6:** Calculate annual CTC and years of service using computed columns.

**Result:** Required columns and distinct values were successfully retrieved.

---

### 4. AGGREGATE FUNCTIONS AND GROUP BY (Q7–Q9)

Aggregate functions perform calculations on a set of records.

| **Function** | **Purpose** |
|:---|:---|
| COUNT() | Counts records |
| SUM() | Calculates total |
| AVG() | Calculates average |
| MIN() | Finds minimum value |
| MAX() | Finds maximum value |

- **Q7:** Calculate total employees, monthly payroll, average salary, minimum salary and maximum salary.
- **Q8:** Calculate employee count and salary statistics department-wise.
- **Q9:** Calculate employee count by department and gender.

**Q7 Output:**

| Total Employees | Monthly Payroll | Average Salary | Minimum Salary | Maximum Salary |
|---:|---:|---:|---:|---:|
| 32 | 3471000.00 | 108468.75 | 45000.00 | 350000.00 |

---

### 5. HAVING (Q10–Q11)

HAVING is used to filter groups after applying GROUP BY.

- **Q10:** Display departments whose average salary is above ₹1,00,000.
- **Q11:** Display projects having more than 4 members, counting only employees working at least 10 hours per week.

**Result:** HAVING successfully filtered grouped records based on aggregate conditions.

---

### 6. CASE EXPRESSIONS (Q12–Q14)

CASE is used to apply conditional logic inside SQL queries.

- **Q12:** Classify employees into Leadership, Senior, Mid-level and Junior salary bands.
- **Q13:** Calculate salary band distribution department-wise.
- **Q14:** Calculate proposed salaries using different hike percentages for departments.

**Result:** CASE expressions successfully performed conditional classification and calculations.

---

### 7. ORDER BY (Q15–Q18)

ORDER BY is used to sort records in ascending or descending order.

- **Q15:** Sort employees by department and salary.
- **Q16:** Display the top 5 highest-paid employees.
- **Q17:** Sort projects according to custom project status order.
- **Q18:** Calculate total project budget by status using GROUP BY WITH ROLLUP.

**Result:** Records were successfully sorted and summarized.

---

## OBSERVATIONS

1. WHERE filters individual rows before grouping.
2. HAVING filters groups after aggregation.
3. COUNT(e.emp_id) counts non-NULL values, whereas COUNT(*) counts rows.
4. CASE can classify rows, create conditional calculations and define custom sorting.
5. ORDER BY supports multiple columns with different sorting directions.
6. LIMIT is used to retrieve top-N records.
7. WITH ROLLUP adds a grand-total row.

---

## RESULT

The database schema was successfully created with 5 departments, 32 employees and 8 projects. All 18 SQL queries were executed to demonstrate Selection, Projection, Aggregate Functions, GROUP BY, HAVING, CASE and ORDER BY.

---

## CONCLUSION

In this experiment, I implemented an Employee-Department-Project database using MySQL. I learned how to use Selection, Projection, Aggregate Functions, GROUP BY, HAVING, CASE expressions and ORDER BY.

I also understood the difference between WHERE and HAVING, conditional operations using CASE and sorting records using ORDER BY.
