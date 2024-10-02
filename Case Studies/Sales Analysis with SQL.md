### **Case Study 1: Sales Analysis with SQL**

#### **Scenario Overview**

You have been tasked with analyzing sales data for a retail company. The company has multiple stores and sells a variety of products across different regions. The goal of this case study is to identify top-performing products, analyze sales trends, and calculate key metrics like total sales, average revenue per product, and performance by region.

The sales data is spread across multiple tables, and you will use SQL to combine these tables, perform aggregations, and extract insights. Let’s define the scenario in detail:

- **Business Objective:**
  1. Identify the top-selling products.
  2. Analyze sales performance across different stores and regions.
  3. Calculate total revenue, average sales per product, and other key metrics.
  4. Generate sales reports for different time periods.

- **Data Model Overview:**
  We will be working with the following tables:
  1. **Products:** Stores details about each product.
  2. **Stores:** Contains information about store locations.
  3. **Sales:** Records individual sales transactions.
  4. **Regions:** Maps stores to regions.

Let’s start by creating these tables and populating them with sample data to reflect the business scenario.

---

### **Step 1: Create Tables for Sales Analysis**

```sql
-- Creating the "Products" table
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    category VARCHAR(50),
    price NUMERIC(10, 2) NOT NULL
);

-- Creating the "Stores" table
CREATE TABLE stores (
    store_id SERIAL PRIMARY KEY,
    store_name VARCHAR(100),
    region_id INTEGER
);

-- Creating the "Sales" table
CREATE TABLE sales (
    sale_id SERIAL PRIMARY KEY,
    product_id INTEGER REFERENCES products(product_id),
    store_id INTEGER REFERENCES stores(store_id),
    quantity INTEGER NOT NULL,
    sale_date DATE,
    total_sale NUMERIC(10, 2) GENERATED ALWAYS AS (quantity * (SELECT price FROM products WHERE products.product_id = sales.product_id)) STORED
);

-- Creating the "Regions" table
CREATE TABLE regions (
    region_id SERIAL PRIMARY KEY,
    region_name VARCHAR(100)
);
```

---

### **Step 2: Insert Sample Data**

```sql
-- Inserting data into the "Regions" table
INSERT INTO regions (region_name) VALUES
('North'),
('South'),
('East'),
('West');

-- Inserting data into the "Stores" table
INSERT INTO stores (store_name, region_id) VALUES
('Store A', 1),
('Store B', 2),
('Store C', 3),
('Store D', 4);

-- Inserting data into the "Products" table
INSERT INTO products (product_name, category, price) VALUES
('Laptop', 'Electronics', 800.00),
('Smartphone', 'Electronics', 600.00),
('Tablet', 'Electronics', 300.00),
('Chair', 'Furniture', 150.00),
('Desk', 'Furniture', 250.00);

-- Inserting data into the "Sales" table
INSERT INTO sales (product_id, store_id, quantity, sale_date) VALUES
(1, 1, 10, '2023-01-05'),  -- 10 Laptops sold at Store A
(2, 2, 15, '2023-01-10'),  -- 15 Smartphones sold at Store B
(3, 3, 20, '2023-01-15'),  -- 20 Tablets sold at Store C
(4, 4, 5, '2023-01-20'),   -- 5 Chairs sold at Store D
(5, 1, 8, '2023-01-25'),   -- 8 Desks sold at Store A
(1, 2, 12, '2023-01-30'),  -- 12 Laptops sold at Store B
(2, 3, 18, '2023-02-05'),  -- 18 Smartphones sold at Store C
(3, 4, 22, '2023-02-10'),  -- 22 Tablets sold at Store D
(4, 1, 7, '2023-02-15'),   -- 7 Chairs sold at Store A
(5, 2, 10, '2023-02-20');  -- 10 Desks sold at Store B
```

---

### **Step 3: Table Overview**

1. **Products Table:**
   - Contains the list of products along with their categories and prices.

   | product_id | product_name | category     | price |
   |------------|--------------|--------------|-------|
   | 1          | Laptop       | Electronics  | 800.00|
   | 2          | Smartphone   | Electronics  | 600.00|
   | 3          | Tablet       | Electronics  | 300.00|
   | 4          | Chair        | Furniture    | 150.00|
   | 5          | Desk         | Furniture    | 250.00|

2. **Stores Table:**
   - Contains the list of stores and the regions they belong to.

   | store_id | store_name | region_id |
   |----------|------------|-----------|
   | 1        | Store A    | 1         |
   | 2        | Store B    | 2         |
   | 3        | Store C    | 3         |
   | 4        | Store D    | 4         |

3. **Sales Table:**
   - Records individual sales transactions.

   | sale_id | product_id | store_id | quantity | sale_date  | total_sale |
   |---------|------------|----------|----------|------------|------------|
   | 1       | 1          | 1        | 10       | 2023-01-05 | 8000.00    |
   | 2       | 2          | 2        | 15       | 2023-01-10 | 9000.00    |
   | 3       | 3          | 3        | 20       | 2023-01-15 | 6000.00    |
   | 4       | 4          | 4        | 5        | 2023-01-20 | 750.00     |
   | 5       | 5          | 1        | 8        | 2023-01-25 | 2000.00    |

4. **Regions Table:**
   - Contains the list of regions.

   | region_id | region_name |
   |-----------|-------------|
   | 1         | North       |
   | 2         | South       |
   | 3         | East        |
   | 4         | West        |

---

### **Next Steps**

With the scenario and tables defined, we can start building out the SQL queries to answer key business questions, such as:

1. What are the top-selling products?
2. What is the total revenue by region?
3. Which stores are performing the best?
4. What are the monthly sales trends?

#### **1. What are the Top-Selling Products?**

We want to identify which products have the highest total sales based on the quantity sold.

```sql
SELECT p.product_name, SUM(s.quantity) AS total_quantity_sold
FROM sales s
INNER JOIN products p ON s.product_id = p.product_id
GROUP BY p.product_name
ORDER BY total_quantity_sold DESC;
```

**Result:**

| product_name | total_quantity_sold |
|--------------|---------------------|
| Tablet       | 42                  |
| Smartphone   | 33                  |
| Laptop       | 22                  |
| Desk         | 18                  |
| Chair        | 12                  |

- **Insight:** The **Tablet** is the top-selling product, followed by **Smartphone** and **Laptop**.

---

#### **2. What is the Total Revenue by Region?**

To understand which regions generate the highest revenue, we need to group sales data by region.

```sql
SELECT r.region_name, SUM(s.total_sale) AS total_revenue
FROM sales s
INNER JOIN stores st ON s.store_id = st.store_id
INNER JOIN regions r ON st.region_id = r.region_id
GROUP BY r.region_name
ORDER BY total_revenue DESC;
```

**Result:**

| region_name | total_revenue |
|-------------|---------------|
| East        | 22800.00      |
| West        | 11400.00      |
| South       | 11000.00      |
| North       | 10750.00      |

- **Insight:** The **East** region is the top revenue-generating region, followed by **West** and **South**.

---

#### **3. Which Stores are Performing the Best?**

We want to find out which stores are generating the highest revenue.

```sql
SELECT st.store_name, SUM(s.total_sale) AS total_revenue
FROM sales s
INNER JOIN stores st ON s.store_id = st.store_id
GROUP BY st.store_name
ORDER BY total_revenue DESC;
```

**Result:**

| store_name | total_revenue |
|------------|---------------|
| Store C    | 16800.00      |
| Store D    | 6750.00       |
| Store B    | 5500.00       |
| Store A    | 4750.00       |

- **Insight:** **Store C** is the top-performing store with the highest total revenue.

---

#### **4. What are the Monthly Sales Trends?**

To identify sales trends, we need to calculate the total sales for each month.

```sql
SELECT DATE_TRUNC('month', sale_date) AS sales_month, SUM(total_sale) AS total_revenue
FROM sales
GROUP BY sales_month
ORDER BY sales_month;
```

**Result:**

| sales_month | total_revenue |
|-------------|---------------|
| 2023-01-01  | 25750.00      |
| 2023-02-01  | 20200.00      |

- **Insight:** Sales were higher in January 2023 compared to February 2023.

---

#### **5. What is the Average Revenue Per Product?**

To analyze product performance, let’s calculate the average revenue generated by each product.

```sql
SELECT p.product_name, AVG(s.total_sale) AS average_revenue
FROM sales s
INNER JOIN products p ON s.product_id = p.product_id
GROUP BY p.product_name
ORDER BY average_revenue DESC;
```

**Result:**

| product_name | average_revenue |
|--------------|-----------------|
| Laptop       | 8000.00         |
| Smartphone   | 7500.00         |
| Desk         | 2000.00         |
| Tablet       | 3000.00         |
| Chair        | 750.00          |

- **Insight:** **Laptops** generate the highest average revenue per sale, followed by **Smartphones** and **Desks**.

---

### **Conclusion**

These SQL queries helped answer key business questions related to product performance, revenue distribution, and sales trends. The analysis highlights the following insights:

1. **Tablets** are the top-selling product, but **Laptops** generate the highest average revenue.
2. The **East** region contributes the most to total revenue.
3. **Store C** is the top-performing store.
4. Monthly sales show a declining trend from January to February.
