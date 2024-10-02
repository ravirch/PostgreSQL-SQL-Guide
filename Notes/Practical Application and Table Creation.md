### Practical Application and Table Creation

In this topic, we’ll explore how to create tables and specify various constraints in PostgreSQL to enforce data integrity and relationships between tables. Constraints are rules applied to columns or tables to restrict the kind of data that can be inserted. This ensures that the data stored in a database is accurate, reliable, and consistent.

---

#### **1. Creating Tables and Specifying Constraints**

Creating tables is one of the fundamental operations in SQL. A table consists of columns, each defined with a specific data type, and optional constraints that define rules for the data.

- **Syntax:**
  ```sql
  CREATE TABLE table_name (
      column1 data_type constraint1,
      column2 data_type constraint2,
      ...
  );
  ```

- **Example:**
  Create a basic `employees` table with some constraints.
  ```sql
  CREATE TABLE employees (
      id SERIAL PRIMARY KEY,  -- Primary key constraint
      name VARCHAR(50) NOT NULL,  -- Name cannot be NULL
      department VARCHAR(50),
      salary NUMERIC(10, 2),
      hire_date DATE DEFAULT CURRENT_DATE  -- Default constraint
  );
  ```

- **Explanation:**
  - `SERIAL PRIMARY KEY`: `id` will be the unique identifier for each employee, auto-incrementing for each new record.
  - `NOT NULL`: Ensures that the `name` column cannot have a `NULL` value.
  - `DEFAULT CURRENT_DATE`: Sets the default value of `hire_date` to the current date if not specified.

---

#### **2. Column-Level Constraints**

Column-level constraints are defined at the column level and apply only to a single column. They enforce rules such as uniqueness, non-null values, and primary keys.

- **Common Column-Level Constraints:**
  1. **PRIMARY KEY:**
     - Ensures that each row in the table has a unique identifier.
     - Automatically implies `NOT NULL`.
     - Only one primary key is allowed per table.
     - **Syntax:**
       ```sql
       id SERIAL PRIMARY KEY
       ```

  2. **UNIQUE:**
     - Ensures that all values in a column are distinct.
     - Unlike `PRIMARY KEY`, multiple `UNIQUE` constraints can exist in a table.
     - **Syntax:**
       ```sql
       email VARCHAR(100) UNIQUE
       ```

  3. **NOT NULL:**
     - Ensures that a column cannot have `NULL` values.
     - **Syntax:**
       ```sql
       name VARCHAR(50) NOT NULL
       ```

  4. **DEFAULT:**
     - Assigns a default value to a column if no value is provided during insert.
     - **Syntax:**
       ```sql
       hire_date DATE DEFAULT CURRENT_DATE
       ```

- **Example:**
  Create a `students` table with different column-level constraints.
  ```sql
  CREATE TABLE students (
      student_id SERIAL PRIMARY KEY,  -- Primary key constraint
      name VARCHAR(100) NOT NULL,     -- Cannot be NULL
      email VARCHAR(100) UNIQUE,      -- Must be unique
      enrollment_date DATE DEFAULT CURRENT_DATE  -- Default value
  );
  ```

---

#### **3. Table-Level Constraints**

Table-level constraints are defined separately from the column definitions and can apply to multiple columns. They are mainly used for relationships between tables and enforcing complex conditions.

- **Common Table-Level Constraints:**
  1. **FOREIGN KEY:**
     - Establishes a relationship between two tables by referencing the primary key of another table.
     - Ensures referential integrity, i.e., values in the foreign key column must exist in the referenced primary key column.
     - **Syntax:**
       ```sql
       department_id INTEGER REFERENCES departments(department_id)
       ```

  2. **CHECK:**
     - Ensures that all values in a column meet a specific condition.
     - Useful for enforcing business rules.
     - **Syntax:**
       ```sql
       salary NUMERIC(10, 2) CHECK (salary > 0)
       ```

  3. **COMPOSITE PRIMARY KEY:**
     - A primary key constraint that consists of more than one column.
     - **Syntax:**
       ```sql
       PRIMARY KEY (column1, column2)
       ```

- **Example:**
  Create an `orders` table with different table-level constraints.
  ```sql
  CREATE TABLE orders (
      order_id SERIAL PRIMARY KEY,  -- Primary key constraint
      customer_id INTEGER REFERENCES customers(customer_id),  -- Foreign key constraint
      product_id INTEGER REFERENCES products(product_id),
      quantity INTEGER CHECK (quantity > 0),  -- Quantity must be positive
      order_date DATE DEFAULT CURRENT_DATE
  );
  ```

- **Explanation:**
  - `customer_id INTEGER REFERENCES customers(customer_id)`: `customer_id` must exist in the `customers` table.
  - `CHECK (quantity > 0)`: Ensures that the `quantity` is always greater than zero.

---
