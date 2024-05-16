**Practical Application and Table Creation**

**Creating Tables and Specifying Constraints**

In SQL, tables are created using the `CREATE TABLE` statement. You can also specify constraints to enforce rules on the data stored in the table, such as primary keys, foreign keys, and check constraints.

**Example:**

```sql
-- Create a new table for storing customer information
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100) UNIQUE,
    phone VARCHAR(20)
);

-- Create a table for orders with foreign key constraint
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    total_amount DECIMAL(10, 2),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```

**Column-Level and Table-Level Constraints in SQL**

- Column-level constraints are specified within the column definition.
- Table-level constraints are specified after all column definitions.

**Example:**

```sql
-- Create a table with table-level and column-level constraints
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10, 2) CHECK (price >= 0),
    CONSTRAINT unique_product_name UNIQUE (product_name)
);
```
