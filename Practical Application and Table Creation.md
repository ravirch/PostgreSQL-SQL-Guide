**Practical Application and Table Creation**

## Creating Database, Tables, and Specifying Constraints

In SQL, databases are containers for tables and other objects. You can create a new database and then create tables within it to store your data. Let's start by creating a database and then a table within that database.

### Example 1: Creating a Database
```sql
CREATE DATABASE my_database;
```
This command creates a new database named `my_database`.

### Example 2: Switching to the Database
```sql
USE my_database;
```
This command switches the active database context to `my_database`.

### Example 3: Creating a Simple Table
```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    age INT
);
```
In this example, we create a table named `students` with columns for `student_id`, `first_name`, `last_name`, and `age`. The `student_id` column is designated as the primary key, ensuring each student has a unique identifier.

## Constraints

### Column-Level Constraints

Column-level constraints are applied directly to a column when it's defined. They specify rules or conditions that data in the column must adhere to.

#### Example 4: Adding a NOT NULL Constraint
```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    department VARCHAR(50)
);
```
In this example, the `first_name` and `last_name` columns are defined with a `NOT NULL` constraint, ensuring that these fields cannot be empty.

#### Example 5: Reference Constraint
A reference constraint, also known as a foreign key constraint, establishes a relationship between two tables.

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100),
    email VARCHAR(100)
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    order_date DATE,
    customer_id INT,
    total_amount DECIMAL(10, 2),
    CONSTRAINT fk_customer FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```

### Table-Level Constraints

Table-level constraints apply to the entire table rather than individual columns. They can enforce rules involving multiple columns.

#### Example 6: Adding a Check Constraint
```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    price DECIMAL(10, 2),
    CONSTRAINT check_price CHECK (price > 0)
);
```
Here, we ensure that the price of a product is always greater than 0.

#### Example 7: Table Constraint on Multiple Columns
```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    phone_number VARCHAR(20),
    CONSTRAINT uc_email_phone UNIQUE (email, phone_number)
);
```
This constraint ensures that the combination of `email` and `phone_number` is unique for each employee.

## Data Manipulation

### Inserting Data

#### Example 8: Inserting a Single Record
```sql
INSERT INTO students (student_id, first_name, last_name, age)
VALUES (1, 'John', 'Doe', 20);
```
This query inserts a new student with the specified details into the `students` table.

#### Example 9: Inserting Multiple Records
```sql
INSERT INTO students (student_id, first_name, last_name, age)
VALUES (2, 'Jane', 'Smith', 22),
       (3, 'Emily', 'Johnson', 21);
```
Here, we insert multiple students into the `students` table in a single query.

### Managing Data: Updating and Deleting

#### Updating Data
The `UPDATE` statement modifies existing data in a table.

##### Example 10: Updating Records
```sql
UPDATE students
SET age = 23
WHERE first_name = 'John' AND last_name = 'Doe';
```
This query updates the age of the student named 'John Doe' to 23.

#### Deleting Data
The `DELETE FROM` statement removes one or more records from a table.

##### Example 11: Deleting a Single Record
```sql
DELETE FROM students
WHERE student_id = 2;
```
This query deletes the student with the specified student_id from the `students` table.

##### Example 12: Deleting Records Based on a Condition
```sql
DELETE FROM students
WHERE age >= 25;
```
Here, we delete all students from the `students` table who are 25 years old or older.

### Deleting Tables or Databases

#### Example 13: Deleting a Table
```sql
DROP TABLE students;
```
This command deletes the `students` table from the database.

#### Example 14: Deleting a Database
```sql
DROP DATABASE my_database;
```
This command deletes the `my_database` database and all its tables and data.

## Altering Tables

### Adding a Column

#### Example 4: Adding a Column
```sql
ALTER TABLE students
ADD COLUMN gender CHAR(1);
```
This command adds a new column `gender` to the `students` table.

### Modifying a Column

#### Example 5: Modifying a Column
```sql
ALTER TABLE students
MODIFY COLUMN age INT UNSIGNED;
```
This command modifies the data type of the `age` column to `UNSIGNED INT`.

### Dropping a Column

#### Example 6: Dropping a Column
```sql
ALTER TABLE students
DROP COLUMN gender;
```
This command drops the `gender` column from the `students` table.

---
