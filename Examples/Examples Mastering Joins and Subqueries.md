### **Examples: Mastering Joins and Subqueries**

```sql
-- ====================================================
-- SETUP: Creating Tables for Joins and Subqueries
-- ====================================================

-- Creating the "Employees" table
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    department_id INTEGER,
    salary NUMERIC(10, 2)
);

-- Creating the "Departments" table
CREATE TABLE departments (
    department_id SERIAL PRIMARY KEY,
    department_name VARCHAR(50),
    location VARCHAR(50)
);

-- Inserting sample data into "Employees"
INSERT INTO employees (name, department_id, salary) VALUES
('Alice', 1, 75000),
('Bob', 2, 60000),
('Charlie', 3, 50000),
('David', NULL, 55000),  -- No assigned department
('Eve', 2, 72000),
('Frank', 4, 68000);  -- Employee in a department that doesn't exist in the initial department table

-- Inserting sample data into "Departments"
INSERT INTO departments (department_name, location) VALUES
('HR', 'New York'),      -- department_id = 1
('IT', 'San Francisco'), -- department_id = 2
('Finance', 'Chicago'),  -- department_id = 3
('Marketing', 'Los Angeles'); -- department_id = 4 (No employees initially)

-- ====================================================
-- PRACTICE EXAMPLES FOR JOINS
-- ====================================================

-- 1. Inner Join: Retrieve employees with their department names.
SELECT employees.name, departments.department_name, employees.salary
FROM employees
INNER JOIN departments ON employees.department_id = departments.department_id;

-- 2. Left Join: Retrieve all employees with their department names (including employees without a department).
SELECT employees.name, departments.department_name, employees.salary
FROM employees
LEFT JOIN departments ON employees.department_id = departments.department_id;

-- 3. Right Join: Retrieve all departments with their employees (including departments without employees).
SELECT employees.name, departments.department_name, departments.location
FROM employees
RIGHT JOIN departments ON employees.department_id = departments.department_id;

-- 4. Full Outer Join: Retrieve all employees and departments, including unmatched rows.
SELECT employees.name, departments.department_name, employees.salary, departments.location
FROM employees
FULL OUTER JOIN departments ON employees.department_id = departments.department_id;

-- 5. Cross Join: Retrieve the Cartesian product of employees and departments.
SELECT employees.name, departments.department_name
FROM employees
CROSS JOIN departments;

-- 6. Self Join: Find pairs of employees in the same department.
SELECT e1.name AS "Employee 1", e2.name AS "Employee 2", e1.department_id
FROM employees e1
INNER JOIN employees e2 ON e1.department_id = e2.department_id
WHERE e1.id <> e2.id;

-- ====================================================
-- PRACTICE EXAMPLES FOR UNION AND UNION ALL
-- ====================================================

-- 7. Using UNION: Get a combined list of all unique employee names and department names.
SELECT name AS "Name" FROM employees
UNION
SELECT department_name FROM departments;

-- 8. Using UNION ALL: Get a combined list of all employee names and department names (including duplicates).
SELECT name AS "Name" FROM employees
UNION ALL
SELECT department_name FROM departments;

-- ====================================================
-- PRACTICE EXAMPLES FOR SUBQUERIES
-- ====================================================

-- 9. Subquery in WHERE Clause: Retrieve employees whose salary is above the average salary.
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- 10. Subquery in FROM Clause: Retrieve the average salary for each department.
SELECT department_name, avg_salary
FROM (SELECT department_id, AVG(salary) AS avg_salary
      FROM employees
      GROUP BY department_id) AS department_averages
INNER JOIN departments ON department_averages.department_id = departments.department_id;

-- 11. Subquery in SELECT Clause: Display each employee’s salary and how it compares to the department average.
SELECT name, salary,
       (SELECT AVG(salary) FROM employees e2 WHERE e2.department_id = employees.department_id) AS department_avg
FROM employees;

-- 12. Correlated Subquery: Retrieve employees whose salary is greater than the average salary in their department.
SELECT name, salary
FROM employees e1
WHERE salary > (SELECT AVG(salary) FROM employees e2 WHERE e2.department_id = e1.department_id);

-- 13. EXISTS Subquery: Check which departments have employees.
SELECT department_name
FROM departments
WHERE EXISTS (SELECT 1 FROM employees WHERE employees.department_id = departments.department_id);

-- 14. NOT EXISTS Subquery: Retrieve departments with no assigned employees.
SELECT department_name
FROM departments
WHERE NOT EXISTS (SELECT 1 FROM employees WHERE employees.department_id = departments.department_id);
```
