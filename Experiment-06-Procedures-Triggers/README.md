# Experiment 6 - Stored Procedure with Error Handling and Triggers for Validation and Audit

| **Name** | **UID No** | **Course** | **Subject** | **Submitted To** |
|---|---|---|---|---|
| Pragya Dubey | 25LBCS3110 | B.Tech CSE (AI/ML-3) | RDBMS (25CSH-210) | Mr. Amit Kumar Sir, Mr. Ilapanda Pavan Kumar Sir |

---

## Aim
To create a stored procedure `transfer_employee(emp_id, new_dept_id)` with proper validation and error handling, and implement triggers for salary validation and audit logging.

## Objectives
- To create a stored procedure for transferring employees between departments.
- To validate employee and department details before transfer.
- To implement error handling using `SIGNAL`.
- To create triggers for salary validation.
- To maintain an audit log for employee insert, update, and delete operations.

## Concepts Used
- Stored Procedures
- Transactions
- Error Handling using `SIGNAL`
- BEFORE INSERT Trigger
- BEFORE UPDATE Trigger
- AFTER INSERT Trigger
- AFTER UPDATE Trigger
- AFTER DELETE Trigger
- Audit Logging

## Procedure Implementation
The stored procedure `transfer_employee(emp_id, new_dept_id)` performs the following operations:

1. Checks whether the input parameters are NULL.
2. Verifies that the employee exists.
3. Verifies that the target department exists.
4. Checks whether the employee is already in the target department.
5. Prevents the transfer of a department head.
6. Prevents the transfer of employees who have direct reports.
7. Removes old department project assignments.
8. Updates the employee's department and manager.
9. Records the transfer details in the audit log.
10. Uses a
