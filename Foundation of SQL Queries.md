**Foundation of SQL Queries**

**Introduction to SQL and its Applications**

SQL (Structured Query Language) is a powerful tool used for managing and manipulating relational databases. It allows users to interact with databases by performing various operations such as retrieving, updating, inserting, and deleting data. SQL is widely used across industries for tasks ranging from simple data retrieval to complex data analysis and reporting.

**Retrieving Data with SELECT Statement**

The `SELECT` statement is one of the fundamental commands in SQL used to retrieve data from a database table. It allows you to specify the columns you want to retrieve and the table from which to retrieve them.

**Example:**

```sql
-- Retrieve all columns from a table
SELECT * FROM employees;

-- Retrieve specific columns from a table
SELECT employee_id, first_name, last_name FROM employees;
```

**Filtering Rows with WHERE Clause**

The `WHERE` clause is used to filter rows based on specified conditions. It allows you to extract only the rows that meet certain criteria.

**Example:**

```sql
-- Retrieve employees who are from the 'Sales' department
SELECT * FROM employees WHERE department = 'Sales';

-- Retrieve employees who joined the company after January 1, 2020
SELECT * FROM employees WHERE hire_date > '2020-01-01';
```

**Sorting Results with ORDER BY**

The `ORDER BY` clause is used to sort the result set in ascending or descending order based on one or more columns.

**Example:**

```sql
-- Retrieve employees sorted by their last name in ascending order
SELECT * FROM employees ORDER BY last_name;

-- Retrieve employees sorted by their salary in descending order
SELECT * FROM employees ORDER BY salary DESC;
```


---
