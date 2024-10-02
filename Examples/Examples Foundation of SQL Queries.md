### **Examples: Foundation of SQL Queries**

```sql
-- ====================================================
-- SETUP: Creating Tables for Practice
-- ====================================================

-- Creating a sample "Employees" table
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

-- Creating a "Departments" table to explore Joins later
CREATE TABLE departments (
    dept_id SERIAL PRIMARY KEY,  -- Unique ID for department
    department_name VARCHAR(50),  -- Name of the department
    location VARCHAR(50)  -- Location of the department
);

-- Inserting sample data into the "Departments" table
INSERT INTO departments (department_name, location) VALUES
('HR', 'New York'),
('IT', 'San Francisco'),
('Finance', 'Chicago'),
('Marketing', 'Los Angeles');

-- ====================================================
-- PRACTICE EXAMPLES
-- ====================================================

-- 1. Retrieving Data with SELECT Statement
-- Retrieve all columns from the "employees" table.
SELECT * FROM employees;

-- Retrieve specific columns: Employee Name and Role.
SELECT name, role FROM employees;

-- Use aliases to rename columns in the result set.
SELECT name AS "Employee Name", salary AS "Annual Salary" FROM employees;

-- 2. Filtering Rows with WHERE Clause
-- Get all employees in the "IT" department.
SELECT name, role FROM employees
WHERE department = 'IT';

-- Retrieve employees hired after 2019.
SELECT name, hire_date FROM employees
WHERE hire_date > '2019-12-31';

-- Use multiple conditions with AND/OR operators.
-- Get all Managers in the "HR" department earning more than 60,000.
SELECT name, salary FROM employees
WHERE role = 'Manager' AND department = 'HR' AND salary > 60000;

-- 3. Sorting Results with ORDER BY
-- Sort employees by salary in ascending order.
SELECT name, salary FROM employees
ORDER BY salary ASC;

-- Sort employees by hire date in descending order.
SELECT name, hire_date FROM employees
ORDER BY hire_date DESC;

-- Sort by multiple columns: first by department, then by salary.
SELECT name, department, salary FROM employees
ORDER BY department ASC, salary DESC;

-- 4. Introduction to Data Types in SQL
-- Retrieve all employees with salary and display the type of salary.
SELECT name, salary, pg_typeof(salary) AS "Salary Data Type" FROM employees;

-- Display different data types: names (VARCHAR) and hire_date (DATE).
SELECT name, hire_date, pg_typeof(hire_date) AS "Hire Date Type" FROM employees;

-- 5. Using Basic Operators in SQL
-- Arithmetic Operators: Calculate bonus (10% of salary).
SELECT name, salary, (salary * 0.10) AS "Bonus" FROM employees;

-- Comparison Operators: Find employees with salaries greater than 60,000.
SELECT name, salary FROM employees
WHERE salary > 60000;

-- Logical Operators: Employees in IT or earning more than 60,000.
SELECT name, department, salary FROM employees
WHERE department = 'IT' OR salary > 60000;

-- 6. Practical Examples with SELECT, WHERE, and ORDER BY
-- Example 1: Find all employees hired between 2018 and 2020, sorted by hire_date.
SELECT name, hire_date FROM employees
WHERE hire_date BETWEEN '2018-01-01' AND '2020-12-31'
ORDER BY hire_date;

-- Example 2: Display the highest and lowest salary in each department.
SELECT department, MAX(salary) AS "Highest Salary", MIN(salary) AS "Lowest Salary"
FROM employees
GROUP BY department;

-- Example 3: Use pattern matching to find employees with names starting with 'D'.
SELECT name FROM employees
WHERE name LIKE 'D%';

-- Example 4: Find employees who are either Managers or have a salary below 60,000.
SELECT name, role, salary FROM employees
WHERE role = 'Manager' OR salary < 60000;

-- Example 5: Combine filtering, sorting, and arithmetic operations.
-- Get employees' names and salaries with a 5% raise, sorted by the new salary.
SELECT name, salary, (salary * 1.05) AS "New Salary"
FROM employees
ORDER BY "New Salary" DESC;

```
