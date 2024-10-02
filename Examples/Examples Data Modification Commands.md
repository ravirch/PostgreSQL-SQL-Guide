### **Examples: Data Modification Commands**

```sql
-- ====================================================
-- SETUP: Creating Tables for Data Modification Commands
-- ====================================================

-- Creating the "Employees" table
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    role VARCHAR(50),
    department_id INTEGER,
    salary NUMERIC(10, 2)
);

-- Creating the "Departments" table
CREATE TABLE departments (
    department_id SERIAL PRIMARY KEY,
    department_name VARCHAR(50)
);

-- Inserting sample data into "Departments"
INSERT INTO departments (department_name) VALUES
('HR'),       -- department_id = 1
('IT'),       -- department_id = 2
('Finance'),  -- department_id = 3
('Marketing');-- department_id = 4

-- ====================================================
-- INSERTING DATA INTO TABLES
-- ====================================================

-- 1. Insert a new employee into the "Employees" table.
INSERT INTO employees (name, role, department_id, salary)
VALUES ('Alice', 'Manager', 1, 75000);

-- 2. Insert a new employee with missing department (will be NULL).
INSERT INTO employees (name, role, salary)
VALUES ('Bob', 'Developer', 60000);

-- 3. Insert multiple employees in a single query.
INSERT INTO employees (name, role, department_id, salary)
VALUES ('Charlie', 'Analyst', 3, 50000),
       ('David', 'Sales Executive', 4, 55000),
       ('Eve', 'Developer', 2, 72000);

-- 4. Insert data using a subquery.
-- Copy all "IT" employees into a new table "it_employees".
CREATE TABLE it_employees AS
SELECT name, role, salary
FROM employees
WHERE department_id = 2;

-- View the data in "it_employees" table.
SELECT * FROM it_employees;

-- ====================================================
-- UPDATING EXISTING RECORDS
-- ====================================================

-- 5. Update the salary of all employees in the "Finance" department.
UPDATE employees
SET salary = salary * 1.10  -- Increase salary by 10%
WHERE department_id = 3;

-- 6. Update the role of "Bob" to "Senior Developer".
UPDATE employees
SET role = 'Senior Developer'
WHERE name = 'Bob';

-- 7. Set the department to "IT" for all Developers.
UPDATE employees
SET department_id = 2
WHERE role = 'Developer';

-- 8. Use a subquery to set the salary to the average salary of the department.
UPDATE employees
SET salary = (SELECT AVG(salary) FROM employees e2 WHERE e2.department_id = employees.department_id)
WHERE department_id IS NOT NULL;

-- 9. Conditional update: Set the department to `NULL` for employees earning less than 60,000.
UPDATE employees
SET department_id = NULL
WHERE salary < 60000;

-- View updated "employees" table.
SELECT * FROM employees;

-- ====================================================
-- DELETING RECORDS FROM TABLES
-- ====================================================

-- 10. Delete the employee "David" from the "Employees" table.
DELETE FROM employees
WHERE name = 'David';

-- 11. Delete all employees in the "Marketing" department.
DELETE FROM employees
WHERE department_id = (SELECT department_id FROM departments WHERE department_name = 'Marketing');

-- 12. Delete employees with a salary below 55,000.
DELETE FROM employees
WHERE salary < 55000;

-- 13. Delete all employees from the "IT" department.
DELETE FROM employees
WHERE department_id = 2;

-- View remaining employees.
SELECT * FROM employees;

-- ====================================================
-- TRUNCATING TABLES
-- ====================================================

-- 14. Truncate the "it_employees" table to remove all rows quickly.
TRUNCATE TABLE it_employees;

-- View the truncated table (should show no rows).
SELECT * FROM it_employees;

```
