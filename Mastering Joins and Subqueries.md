**Mastering Joins and Subqueries**

**Understanding JOINs in SQL**

Joins are used to combine rows from two or more tables based on a related column between them. There are several types of joins, including INNER JOIN, LEFT JOIN, RIGHT JOIN, and FULL JOIN.

**Example:**

```sql
-- Retrieve employee information along with their department
SELECT e.*, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id;

-- Retrieve product information along with their category
SELECT p.*, c.category_name
FROM products p
INNER JOIN categories c ON p.category_id = c.category_id;
```

**Performing Inner Joins, Outer Joins, and Cross Joins**

- Inner Join: Returns only the rows that have matching values in both tables.
- Left Outer Join (or Left Join): Returns all rows from the left table and matching rows from the right table.
- Right Outer Join (or Right Join): Returns all rows from the right table and matching rows from the left table.
- Cross Join: Returns the Cartesian product of the two tables, i.e., all possible combinations of rows.

**Example:**

```sql
-- Retrieve all employees and their corresponding department (Left Join)
SELECT e.*, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.department_id;

-- Retrieve all employees and their corresponding department (Right Join)
SELECT e.*, d.department_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.department_id;

-- Retrieve all possible combinations of employees and departments (Cross Join)
SELECT e.*, d.department_name
FROM employees e
CROSS JOIN departments d;
```

**Working with Subqueries**

A subquery is a query nested within another query. It can be used in various parts of a SQL statement, such as the SELECT, FROM, WHERE, and HAVING clauses.

**Example:**

```sql
-- Retrieve employees whose salary is higher than the average salary
SELECT * FROM employees WHERE salary > (SELECT AVG(salary) FROM employees);

-- Retrieve products with prices higher than the average price in their category
SELECT * FROM products WHERE price > (SELECT AVG(price) FROM products WHERE category_id = p.category_id);
```

---
