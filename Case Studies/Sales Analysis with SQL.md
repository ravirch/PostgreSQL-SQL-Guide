### **Examples: Advanced Filtering and Grouping**

```sql
-- ====================================================
-- SETUP: Creating Tables for Advanced Filtering and Grouping
-- ====================================================

-- Creating a sample "Employees" table for practice
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,  -- Auto-incrementing ID
    name VARCHAR(50) NOT NULL,  -- Employee Name
    role VARCHAR(50) NOT NULL,  -- Job Role
    hire_date DATE,  -- Date of Hiring
    salary NUMERIC(10, 2),  -- Salary of Employee
    department VARCHAR(50)  -- Department Name
);

-- Inserting sample data into the "Employees" table
INSERT INTO employees (name, role, hire_date, salary, department) VALUES
('Alice Johnson', 'Manager', '2018-05-15', 75000, 'HR'),
('Bob Smith', 'Developer', '2019-03-20', 55000, 'IT'),
('Charlie Lee', 'Analyst', '2020-01-10', 60000, 'Finance'),
('David Brown', 'Developer', '2021-06-17', 50000, 'IT'),
('Eva White', 'Manager', '2017-11-23', 80000, 'Finance'),
('Frank Green', 'Analyst', '2019-07-05', 58000, 'HR');

-- ====================================================
-- PRACTICE EXAMPLES
-- ====================================================

-- 1. Utilizing the IN Operator
-- Example 1.1: Retrieve employees who work in the "IT" or "Finance" departments.
SELECT name, department FROM employees
WHERE department IN ('IT', 'Finance');

-- Example 1.2: Retrieve employees whose roles are either "Manager" or "Analyst".
SELECT name, role FROM employees
WHERE role IN ('Manager', 'Analyst');

-- 2. Understanding the NOT IN Operator
-- Example 2.1: Retrieve employees who do NOT work in the "HR" or "Finance" departments.
SELECT name, department FROM employees
WHERE department NOT IN ('HR', 'Finance');

-- Example 2.2: Retrieve employees whose roles are NOT "Developer".
SELECT name, role FROM employees
WHERE role NOT IN ('Developer');

-- 3. Introduction to LIKE Operator for Pattern Matching
-- Example 3.1: Retrieve employees whose names start with 'A'.
SELECT name FROM employees
WHERE name LIKE 'A%';

-- Example 3.2: Retrieve employees whose names end with 'n'.
SELECT name FROM employees
WHERE name LIKE '%n';

-- Example 3.3: Retrieve employees whose names contain 'e' anywhere.
SELECT name FROM employees
WHERE name LIKE '%e%';

-- Example 3.4: Retrieve employees whose names have exactly five characters.
SELECT name FROM employees
WHERE name LIKE '_____';

-- 4. Handling NULLs in SQL Queries
-- Example 4.1: Adding a NULL value to explore handling NULLs.
UPDATE employees
SET salary = NULL
WHERE name = 'Frank Green';  -- Set Frank's salary as NULL for demonstration

-- Example 4.2: Retrieve employees who have a NULL salary.
SELECT name, salary FROM employees
WHERE salary IS NULL;

-- Example 4.3: Retrieve employees who have a defined (non-NULL) salary.
SELECT name, salary FROM employees
WHERE salary IS NOT NULL;

-- Example 4.4: Retrieve all employees, replacing NULL salaries with '0'.
SELECT name, COALESCE(salary, 0) AS salary FROM employees;

-- 5. Grouping and Aggregating Data with GROUP BY
-- Example 5.1: Find the number of employees in each department.
SELECT department, COUNT(*) AS "Number of Employees"
FROM employees
GROUP BY department;

-- Example 5.2: Calculate the total salary for each department.
SELECT department, SUM(salary) AS "Total Salary"
FROM employees
GROUP BY department;

-- Example 5.3: Find the average salary for each role.
SELECT role, AVG(salary) AS "Average Salary"
FROM employees
GROUP BY role;

-- Example 5.4: Find the minimum and maximum salary for each department.
SELECT department, MIN(salary) AS "Minimum Salary", MAX(salary) AS "Maximum Salary"
FROM employees
GROUP BY department;

-- 6. Filtering Groups with HAVING Clause
-- Example 6.1: Find departments with more than 1 employee.
SELECT department, COUNT(*) AS "Number of Employees"
FROM employees
GROUP BY department
HAVING COUNT(*) > 1;

-- Example 6.2: Find roles with an average salary greater than 55,000.
SELECT role, AVG(salary) AS "Average Salary"
FROM employees
GROUP BY role
HAVING AVG(salary) > 55000;

-- Example 6.3: Find departments where the total salary is less than 150,000.
SELECT department, SUM(salary) AS "Total Salary"
FROM employees
GROUP BY department
HAVING SUM(salary) < 150000;

-- Example 6.4: Retrieve roles with a maximum salary below 60,000.
SELECT role, MAX(salary) AS "Maximum Salary"
FROM employees
GROUP BY role
HAVING MAX(salary) < 60000;

```
