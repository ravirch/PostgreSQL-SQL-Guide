### Mastering Joins and Subqueries

#### **1. Understanding JOINs in SQL**

A **JOIN** in SQL is used to combine rows from two or more tables based on a related column between them. There are different types of joins, each serving a different purpose:

- **Inner Join**: Returns only rows with matching values in both tables.
- **Left (Outer) Join**: Returns all rows from the left table, and the matched rows from the right table.
- **Right (Outer) Join**: Returns all rows from the right table, and the matched rows from the left table.
- **Full (Outer) Join**: Returns all rows from both tables, with `NULL` in places where the join condition is not met.
- **Cross Join**: Returns the Cartesian product of two tables, i.e., every row of the first table paired with every row of the second.

Let’s set up the tables and explore these join types with practical examples.

---

#### **2. Performing Inner Joins, Outer Joins, and Cross Joins**

- **Table Setup for Join Examples:**

```sql
-- Creating the "Employees" table
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    department_id INTEGER
);

-- Creating the "Departments" table
CREATE TABLE departments (
    department_id SERIAL PRIMARY KEY,
    department_name VARCHAR(50)
);

-- Inserting sample data into "Employees"
INSERT INTO employees (name, department_id) VALUES
('Alice', 1),
('Bob', 2),
('Charlie', 3),
('David', NULL);  -- David has no assigned department

-- Inserting sample data into "Departments"
INSERT INTO departments (department_name) VALUES
('HR'),  -- department_id = 1
('IT'),  -- department_id = 2
('Finance');  -- department_id = 3
```

- **Table 1: Employees**

| id | name    | department_id |
|----|---------|---------------|
| 1  | Alice   | 1             |
| 2  | Bob     | 2             |
| 3  | Charlie | 3             |
| 4  | David   | NULL          |

- **Table 2: Departments**

| department_id | department_name |
|---------------|-----------------|
| 1             | HR              |
| 2             | IT              |
| 3             | Finance         |
| 4             | Marketing       |

---

- **Full Outer Join Example:**
  
  A **Full Outer Join** returns all rows from both tables. If a row from either table doesn’t have a match, the result will have `NULL` values for the unmatched columns.

  ```sql
  SELECT employees.name, departments.department_name
  FROM employees
  FULL OUTER JOIN departments ON employees.department_id = departments.department_id;
  ```

  **Result:**

  | name    | department_name |
  |---------|-----------------|
  | Alice   | HR              |
  | Bob     | IT              |
  | Charlie | Finance         |
  | David   | NULL            |  -- David has no matching department
  | NULL    | Marketing       |  -- Marketing has no matching employee

---

- **Inner Join Example:**
  Combine `employees` and `departments` where `department_id` matches.

  ```sql
  SELECT employees.name, departments.department_name
  FROM employees
  INNER JOIN departments ON employees.department_id = departments.department_id;
  ```

  **Result:**

  | name    | department_name |
  |---------|-----------------|
  | Alice   | HR              |
  | Bob     | IT              |
  | Charlie | Finance         |

- **Left Join Example:**
  Retrieve all employees and their departments, including employees without a matching department.

  ```sql
  SELECT employees.name, departments.department_name
  FROM employees
  LEFT JOIN departments ON employees.department_id = departments.department_id;
  ```

  **Result:**

  | name    | department_name |
  |---------|-----------------|
  | Alice   | HR              |
  | Bob     | IT              |
  | Charlie | Finance         |
  | David   | NULL            |

- **Right Join Example:**
  Retrieve all departments and their employees, including departments without any assigned employees.

  ```sql
  SELECT employees.name, departments.department_name
  FROM employees
  RIGHT JOIN departments ON employees.department_id = departments.department_id;
  ```

  **Result:**

  | name    | department_name |
  |---------|-----------------|
  | Alice   | HR              |
  | Bob     | IT              |
  | Charlie | Finance         |
  | NULL    | Marketing       |  -- No employees in the Marketing department

- **Cross Join Example:**
  Retrieve the Cartesian product of employees and departments.

  ```sql
  SELECT employees.name, departments.department_name
  FROM employees
  CROSS JOIN departments;
  ```

  **Result:** (Every employee paired with every department)

  | name    | department_name |
  |---------|-----------------|
  | Alice   | HR              |
  | Alice   | IT              |
  | Alice   | Finance         |
  | Alice   | Marketing       |
  | Bob     | HR              |
  | Bob     | IT              |
  | Bob     | Finance         |
  | Bob     | Marketing       |
  | ...     | ...             |

---

#### **3. Self Joins for Advanced Data Analysis**

A **Self Join** is used when a table needs to be joined with itself to compare rows. This is useful in scenarios like finding employees reporting to the same manager.

- **Example:**
  Find employees who work in the same department.

  ```sql
  SELECT e1.name AS "Employee 1", e2.name AS "Employee 2", e1.department_id
  FROM employees e1
  INNER JOIN employees e2 ON e1.department_id = e2.department_id
  WHERE e1.id <> e2.id;
  ```

  **Result:**

  | Employee 1 | Employee 2 | department_id |
  |------------|------------|---------------|
  | Alice      | Bob        | 1             |

---

#### **4. Using UNION and UNION ALL for Merging Results**

The `UNION` and `UNION ALL` operators combine the results of two or more `SELECT` statements.

- **`UNION`**: Removes duplicate rows.
- **`UNION ALL`**: Keeps all rows, including duplicates.

- **Example:**
  Retrieve all unique department names and employee names.

  ```sql
  SELECT name AS "Name" FROM employees
  UNION
  SELECT department_name FROM departments;
  ```

  **Result:**

  | Name      |
  |-----------|
  | Alice     |
  | Bob       |
  | Charlie   |
  | David     |
  | HR        |
  | IT        |
  | Finance   |

- **Example:**
  Combine the list of employees and departments, including duplicates.

  ```sql
  SELECT name AS "Name" FROM employees
  UNION ALL
  SELECT department_name FROM departments;
  ```

---

#### **5. Working with Subqueries**

A **Subquery** is a query nested within another query and is used to perform more complex filtering and data manipulation.

- **Subquery in `WHERE` Clause:**
  Retrieve employees who earn more than the average salary.

  ```sql
  SELECT name, salary
  FROM employees
  WHERE salary > (SELECT AVG(salary) FROM employees);
  ```

- **Subquery in `FROM` Clause:**
  Find the average salary of each department using a subquery.

  ```sql
  SELECT department_name, avg_salary
  FROM (SELECT department_id, AVG(salary) AS avg_salary
        FROM employees
        GROUP BY department_id) AS department_averages
  INNER JOIN departments ON department_averages.department_id = departments.department_id;
  ```

- **Subquery in `SELECT` Clause:**
  Display each employee’s salary and how it compares to the department average.

  ```sql
  SELECT name, salary,
         (SELECT AVG(salary) FROM employees e2 WHERE e2.department_id = employees.department_id) AS department_avg
  FROM employees;
  ```

  **Result:**

  | name    | salary | department_avg |
  |---------|--------|----------------|
  | Alice   | 75000  | 75000          |
  | Bob     | 55000  | 55000          |
  | Charlie | 60000  | 60000          |

---
