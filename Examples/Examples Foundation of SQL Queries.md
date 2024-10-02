### Foundation of SQL Queries

#### **1. Introduction to SQL and its Applications**

**Structured Query Language (SQL)** is the standard language used to interact with relational databases. It allows you to define, manipulate, query, and control access to data stored in a database. Whether you want to retrieve specific information, update records, or enforce data integrity, SQL provides a comprehensive set of commands to handle these tasks.

- **Applications of SQL:**
  - **Data Retrieval:** Use SQL to extract data from one or more tables using the `SELECT` statement.
  - **Data Insertion and Updates:** Add new data to the database using the `INSERT` command or modify existing data using `UPDATE`.
  - **Data Deletion:** Remove data with the `DELETE` command.
  - **Data Definition:** Create or modify tables and their structure using `CREATE` and `ALTER` commands.
  - **Data Access Control:** Manage user permissions and security using commands like `GRANT` and `REVOKE`.

- **Common SQL Use Cases:**
  - **Data Analysis:** Extracting, aggregating, and filtering data for reporting and analysis.
  - **Backend Operations:** Managing user data and metadata for applications.
  - **Database Administration:** Structuring databases, optimizing performance, and ensuring data integrity.

**Example:**
If you want to view a list of all employees in an organization, you would use a `SELECT` statement to retrieve data from the `Employees` table.

---

#### **2. Retrieving Data with the SELECT Statement**

The `SELECT` statement is the primary command used to retrieve data from a database. It allows you to specify which columns to view, apply conditions, and even manipulate how data is displayed.

**Syntax:**
```sql
SELECT column1, column2, ...
FROM table_name;
```

- **Key Components:**
  - **`SELECT`**: Specifies the columns to retrieve.
  - **`FROM`**: Specifies the table to pull the data from.

**Examples:**
1. **Select all columns from a table:**
   ```sql
   SELECT * FROM employees;
   ```
   This query retrieves all columns from the `employees` table.

2. **Select specific columns:**
   ```sql
   SELECT name, role FROM employees;
   ```
   This query displays only the `name` and `role` columns of the employees.

3. **Using Aliases for Better Readability:**
   ```sql
   SELECT name AS "Employee Name", role AS "Job Role" FROM employees;
   ```
   Aliases (`AS`) can be used to rename columns in the result set for better clarity.

---

#### **3. Filtering Rows with WHERE Clause**

The `WHERE` clause is used to filter records based on specific conditions. It only returns rows that meet the criteria defined in the condition.

**Syntax:**
```sql
SELECT column1, column2
FROM table_name
WHERE condition;
```

- **Key Components:**
  - **`WHERE`**: Specifies the condition that must be met for a row to be included.

**Examples:**
1. **Retrieve employees in a specific role:**
   ```sql
   SELECT name, role FROM employees
   WHERE role = 'Manager';
   ```
   This query returns only those employees whose role is `Manager`.

2. **Using multiple conditions with AND/OR:**
   ```sql
   SELECT name, salary FROM employees
   WHERE salary > 50000 AND role = 'Developer';
   ```
   This query filters employees who are Developers and earn more than 50,000.

3. **Using Pattern Matching with LIKE:**
   ```sql
   SELECT name FROM employees
   WHERE name LIKE 'A%';
   ```
   This query returns employee names starting with the letter 'A'.

---

#### **4. Sorting Results with ORDER BY**

The `ORDER BY` clause is used to sort the result set based on one or more columns. By default, it sorts in ascending order, but you can specify descending order as well.

**Syntax:**
```sql
SELECT column1, column2
FROM table_name
ORDER BY column1 [ASC|DESC];
```

- **Key Components:**
  - **`ORDER BY`**: Specifies the column to sort by.
  - **`ASC`**: Sorts in ascending order (default).
  - **`DESC`**: Sorts in descending order.

**Examples:**
1. **Sort employees by salary in ascending order:**
   ```sql
   SELECT name, salary FROM employees
   ORDER BY salary ASC;
   ```
   This query sorts employees by salary, showing the lowest salary first.

2. **Sort employees by name in descending order:**
   ```sql
   SELECT name, role FROM employees
   ORDER BY name DESC;
   ```
   This query sorts employees by their names in reverse alphabetical order.

3. **Sort by multiple columns:**
   ```sql
   SELECT name, role, salary FROM employees
   ORDER BY role ASC, salary DESC;
   ```
   This query sorts first by role in ascending order, and then by salary within each role in descending order.

---

#### **5. Introduction to Data Types in SQL**

Data types define the kind of data that can be stored in a column. Choosing the right data type is crucial for data integrity, performance, and precision.

- **Common Data Types:**
  1. **Integer Types:**
     - `INTEGER`: Standard integer.
     - `SMALLINT`, `BIGINT`: Variants for smaller or larger ranges.
  
  2. **String Types:**
     - `VARCHAR(n)`: Variable-length string.
     - `CHAR(n)`: Fixed-length string.
     - `TEXT`: Unlimited-length string.

  3. **Date and Time Types:**
     - `DATE`: Stores date (Year-Month-Day).
     - `TIME`: Stores time (Hours:Minutes:Seconds).
     - `TIMESTAMP`: Stores date and time.

  4. **Boolean Type:**
     - `BOOLEAN`: Stores `TRUE`, `FALSE`, or `NULL`.

- **Example of Using Data Types:**
  ```sql
  CREATE TABLE employees (
      id SERIAL PRIMARY KEY,
      name VARCHAR(50),
      hire_date DATE,
      salary NUMERIC(10, 2)
  );
  ```
  This table uses `VARCHAR` for names, `DATE` for hire dates, and `NUMERIC` for salary values.

---

#### **6. Basic Operators in SQL**

Operators are used in SQL to perform operations on data, including arithmetic, comparison, and logical operations.

1. **Arithmetic Operators:**
   - `+`: Addition
   - `-`: Subtraction
   - `*`: Multiplication
   - `/`: Division

2. **Comparison Operators:**
   - `=`: Equal to
   - `<>` or `!=`: Not equal to
   - `>`: Greater than
   - `<`: Less than

3. **Logical Operators:**
   - `AND`: Returns true if both conditions are true.
   - `OR`: Returns true if either condition is true.
   - `NOT`: Reverses the result of a condition.

**Example:**
```sql
SELECT name, salary FROM employees
WHERE salary > 40000 AND role = 'Analyst';
```
This query retrieves all analysts earning more than 40,000, combining conditions with `AND`.

---
