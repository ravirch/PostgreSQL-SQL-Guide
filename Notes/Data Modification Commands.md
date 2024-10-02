### Data Modification Commands

Data modification commands are used to manipulate the data within tables. The primary commands include **INSERT**, **UPDATE**, and **DELETE**. These commands enable you to add new data, modify existing records, and remove unwanted data, respectively. Let’s explore each command with its syntax, use cases, and practical examples.

---

#### **1. Inserting Data into Tables**

The `INSERT` statement is used to add new rows of data into a table. You can insert data into all columns of a table or specific columns.

- **Syntax:**
  ```sql
  INSERT INTO table_name (column1, column2, ...)
  VALUES (value1, value2, ...);
  ```

- **Key Points:**
  - You can omit the column names if you’re inserting values for all columns, but the order must match the table structure.
  - Use `DEFAULT` to insert default values specified in the table definition.

- **Examples:**
  1. Insert data into all columns:
     ```sql
     INSERT INTO employees (name, department_id, salary)
     VALUES ('George', 2, 65000);
     ```
     This inserts a new employee, `George`, into the `employees` table.

  2. Insert data into specific columns:
     ```sql
     INSERT INTO employees (name, salary)
     VALUES ('Hannah', 70000);
     ```
     This only populates `name` and `salary`, leaving `department_id` as `NULL`.

  3. Insert multiple rows in a single query:
     ```sql
     INSERT INTO employees (name, department_id, salary)
     VALUES ('Ivy', 1, 50000),
            ('Jack', 3, 55000),
            ('Kate', 2, 60000);
     ```

  4. Insert data using a subquery:
     Copy all employees from the "HR" department to a new table called `hr_employees`.
     ```sql
     INSERT INTO hr_employees (name, salary)
     SELECT name, salary
     FROM employees
     WHERE department_id = 1;
     ```

---

#### **2. Updating Existing Records**

The `UPDATE` statement is used to modify existing records in a table. You can update one or more columns for a specific set of rows.

- **Syntax:**
  ```sql
  UPDATE table_name
  SET column1 = value1, column2 = value2, ...
  WHERE condition;
  ```

- **Key Points:**
  - Always use a `WHERE` clause to specify which rows to update. Without it, **all** rows in the table will be updated.
  - Multiple columns can be updated in a single query.

- **Examples:**
  1. Update a single column:
     Increase the salary of employees in the "IT" department by 10%.
     ```sql
     UPDATE employees
     SET salary = salary * 1.10
     WHERE department_id = 2;
     ```
     This query updates all employees in the IT department.

  2. Update multiple columns:
     Change `David`’s department to "Finance" and update his salary.
     ```sql
     UPDATE employees
     SET department_id = 3, salary = 60000
     WHERE name = 'David';
     ```

  3. Use a subquery to update:
     Set the salary of employees to the average salary of their department.
     ```sql
     UPDATE employees
     SET salary = (SELECT AVG(salary) FROM employees e2 WHERE e2.department_id = employees.department_id);
     ```

  4. Conditional update:
     Set the department to `NULL` for employees earning less than 55,000.
     ```sql
     UPDATE employees
     SET department_id = NULL
     WHERE salary < 55000;
     ```

---

#### **3. Deleting Records from Tables**

The `DELETE` statement is used to remove one or more rows from a table. You can delete based on a condition or remove all rows from a table.

- **Syntax:**
  ```sql
  DELETE FROM table_name
  WHERE condition;
  ```

- **Key Points:**
  - Always use a `WHERE` clause to avoid deleting all rows unintentionally.
  - For large tables, it’s recommended to test with a `SELECT` query first to ensure the correct rows are targeted.

- **Examples:**
  1. Delete a single row:
     Remove the employee named `George`.
     ```sql
     DELETE FROM employees
     WHERE name = 'George';
     ```

  2. Delete multiple rows:
     Remove all employees in the "Marketing" department.
     ```sql
     DELETE FROM employees
     WHERE department_id = 4;
     ```

  3. Delete rows based on a condition:
     Remove employees with salaries below 60,000.
     ```sql
     DELETE FROM employees
     WHERE salary < 60000;
     ```

  4. Delete all rows from a table:
     ```sql
     DELETE FROM employees;
     ```
     This removes all data from the `employees` table.

- **Use `TRUNCATE` for Faster Deletes:**
  If you need to delete **all** rows from a table and reset it, use `TRUNCATE` instead of `DELETE`.
  ```sql
  TRUNCATE TABLE employees;
  ```

---
