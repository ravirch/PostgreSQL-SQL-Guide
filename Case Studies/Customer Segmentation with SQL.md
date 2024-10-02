### **Case Study 2: Customer Segmentation with SQL**

#### **Scenario Overview**

You have been provided with customer data and sales information for a company that wants to segment its customers based on various factors such as purchase frequency, total spending, and recency (how recently they made a purchase). Customer segmentation is a crucial aspect of customer relationship management (CRM), and it helps in identifying valuable customer groups for targeted marketing campaigns.

The goal of this case study is to perform customer segmentation using SQL and classify customers into different groups based on predefined criteria.

- **Business Objectives:**
  1. Calculate key metrics for each customer, such as total spending, number of purchases, and recency of last purchase.
  2. Segment customers into groups such as "High Value," "Medium Value," and "Low Value" based on these metrics.
  3. Identify target segments for future marketing and retention campaigns.

- **Data Model Overview:**
  We will be working with the following tables:
  1. **Customers:** Contains basic customer information.
  2. **Orders:** Stores details of each customer order.
  3. **Products:** Contains product details for each order.
  4. **Order_Items:** Stores information about the items in each order.

Let’s define these tables and populate them with sample data for our analysis.

---

### **Step 1: Create Tables for Customer Segmentation**

```sql
-- Creating the "Customers" table
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    join_date DATE DEFAULT CURRENT_DATE
);

-- Creating the "Products" table
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    category VARCHAR(50),
    price NUMERIC(10, 2)
);

-- Creating the "Orders" table
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customers(customer_id),
    order_date DATE NOT NULL,
    total_amount NUMERIC(10, 2) NOT NULL
);

-- Creating the "Order_Items" table
CREATE TABLE order_items (
    order_item_id SERIAL PRIMARY KEY,
    order_id INTEGER REFERENCES orders(order_id),
    product_id INTEGER REFERENCES products(product_id),
    quantity INTEGER NOT NULL,
    line_total NUMERIC(10, 2) GENERATED ALWAYS AS (quantity * (SELECT price FROM products WHERE products.product_id = order_items.product_id)) STORED
);
```

---

### **Step 2: Insert Sample Data**

```sql
-- Inserting data into the "Customers" table
INSERT INTO customers (customer_name, email, join_date) VALUES
('Alice Johnson', 'alice@example.com', '2021-01-15'),
('Bob Smith', 'bob@example.com', '2021-03-22'),
('Charlie Lee', 'charlie@example.com', '2021-07-19'),
('David Brown', 'david@example.com', '2021-11-05'),
('Eva White', 'eva@example.com', '2022-02-10');

-- Inserting data into the "Products" table
INSERT INTO products (product_name, category, price) VALUES
('Laptop', 'Electronics', 800.00),
('Smartphone', 'Electronics', 600.00),
('Tablet', 'Electronics', 300.00),
('Chair', 'Furniture', 150.00),
('Desk', 'Furniture', 250.00);

-- Inserting data into the "Orders" table
INSERT INTO orders (customer_id, order_date, total_amount) VALUES
(1, '2022-01-05', 1600.00),  -- Alice
(2, '2022-01-10', 600.00),   -- Bob
(3, '2022-01-15', 300.00),   -- Charlie
(1, '2022-02-01', 1000.00),  -- Alice
(4, '2022-02-12', 450.00),   -- David
(5, '2022-03-05', 250.00),   -- Eva
(2, '2022-03-15', 1200.00),  -- Bob
(3, '2022-03-22', 600.00);   -- Charlie

-- Inserting data into the "Order_Items" table
INSERT INTO order_items (order_id, product_id, quantity) VALUES
(1, 1, 2),   -- 2 Laptops for Alice
(2, 2, 1),   -- 1 Smartphone for Bob
(3, 3, 1),   -- 1 Tablet for Charlie
(4, 2, 1),   -- 1 Smartphone for Alice
(5, 4, 3),   -- 3 Chairs for David
(6, 5, 1),   -- 1 Desk for Eva
(7, 1, 2),   -- 2 Laptops for Bob
(8, 3, 2);   -- 2 Tablets for Charlie
```

---

### **Step 3: Answer Key Business Questions**

1. **What is the Total Spending of Each Customer?**

```sql
SELECT c.customer_name, SUM(o.total_amount) AS total_spending
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_name
ORDER BY total_spending DESC;
```

**Result:**

| customer_name | total_spending |
|---------------|----------------|
| Alice Johnson | 2600.00        |
| Bob Smith     | 1800.00        |
| Charlie Lee   | 900.00         |
| David Brown   | 450.00         |
| Eva White     | 250.00         |

- **Insight:** **Alice Johnson** is the top spender.

---

2. **How Many Orders Has Each Customer Placed?**

```sql
SELECT c.customer_name, COUNT(o.order_id) AS total_orders
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_name
ORDER BY total_orders DESC;
```

**Result:**

| customer_name | total_orders |
|---------------|--------------|
| Alice Johnson | 2            |
| Bob Smith     | 2            |
| Charlie Lee   | 2            |
| David Brown   | 1            |
| Eva White     | 1            |

- **Insight:** **Alice**, **Bob**, and **Charlie** have placed multiple orders, indicating a higher level of engagement.

---

3. **When Was the Last Order Placed by Each Customer?**

```sql
SELECT c.customer_name, MAX(o.order_date) AS last_order_date
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_name
ORDER BY last_order_date DESC;
```

**Result:**

| customer_name | last_order_date |
|---------------|-----------------|
| Charlie Lee   | 2022-03-22      |
| Bob Smith     | 2022-03-15      |
| Eva White     | 2022-03-05      |
| David Brown   | 2022-02-12      |
| Alice Johnson | 2022-02-01      |

- **Insight:** **Charlie Lee** made the most recent purchase, showing recent activity.

---

4. **Segment Customers into Groups Based on Spending:**

Let’s classify customers into three groups based on their total spending:
- **High Value:** Spending > 1500
- **Medium Value:** 1000 < Spending ≤ 1500
- **Low Value:** Spending ≤ 1000

```sql
SELECT customer_name, total_spending,
       CASE
           WHEN total_spending > 1500 THEN 'High Value'
           WHEN total_spending > 1000 AND total_spending <= 1500 THEN 'Medium Value'
           ELSE 'Low Value'
       END AS customer_segment
FROM (
    SELECT c.customer_name, SUM(o.total_amount) AS total_spending
    FROM customers c
    INNER JOIN orders o ON c.customer_id = o.customer_id
    GROUP BY c.customer_name
) AS spending_summary
ORDER BY total_spending DESC;
```

**Result:**

| customer_name | total_spending | customer_segment |
|---------------|----------------|------------------|
| Alice Johnson | 2600.00        | High Value       |
| Bob Smith     | 1800.00        | High Value       |
| Charlie Lee   | 900.00         | Low Value        |
| David Brown   | 450.00         | Low Value        |
| Eva White     | 250.00         | Low Value        |

- **Insight:** **Alice** and **Bob** are high-value customers, ideal for special loyalty programs.

---
