**Advanced Filtering and Grouping**

**Utilizing the IN Operator**

The `IN` operator allows you to specify multiple values in a WHERE clause. It's useful when you want to filter rows based on multiple possible values for a single column.

**Example:**

```sql
-- Retrieve employees from the 'Sales' and 'Marketing' departments
SELECT * FROM employees WHERE department IN ('Sales', 'Marketing');

-- Retrieve products with prices either $100 or $200
SELECT * FROM products WHERE price IN (100, 200);
```

**Understanding the NOT IN Operator**

The `NOT IN` operator is used to exclude rows that match the values specified in the WHERE clause.

**Example:**

```sql
-- Retrieve employees not from the 'Engineering' department
SELECT * FROM employees WHERE department NOT IN ('Engineering');

-- Retrieve products with prices other than $100 or $200
SELECT * FROM products WHERE price NOT IN (100, 200);
```

**Introduction to LIKE Operator for Pattern Matching**

The `LIKE` operator is used to search for a specified pattern in a column. It's commonly used with wildcard characters `%` (matches zero or more characters) and `_` (matches any single character).

**Example:**

```sql
-- Retrieve employees whose last name starts with 'S'
SELECT * FROM employees WHERE last_name LIKE 'S%';

-- Retrieve products with names containing 'phone'
SELECT * FROM products WHERE product_name LIKE '%phone%';
```

**Grouping and Aggregating Data with GROUP BY**

The `GROUP BY` clause is used to group rows that have the same values into summary rows. It's often used with aggregate functions such as `COUNT`, `SUM`, `AVG`, etc.

**Example:**

```sql
-- Count the number of employees in each department
SELECT department, COUNT(*) AS num_employees FROM employees GROUP BY department;

-- Calculate the total sales amount for each category
SELECT category, SUM(price) AS total_sales FROM products GROUP BY category;
```

**Filtering Groups with HAVING Clause**

The `HAVING` clause is used to filter groups based on specified conditions. It's similar to the `WHERE` clause but operates on grouped rows rather than individual rows.

**Example:**

```sql
-- Retrieve departments with more than 3 employees
SELECT department, COUNT(*) AS num_employees FROM employees GROUP BY department HAVING COUNT(*) > 3;

-- Retrieve categories with total sales amount greater than $1000
SELECT category, SUM(price) AS total_sales FROM products GROUP BY category HAVING SUM(price) > 1000;
```

---
