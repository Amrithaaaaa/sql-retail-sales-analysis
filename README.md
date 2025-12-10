# 🛒 Retail Sales Analysis using SQL

## 📌 Project Overview

**Project Name**: Retail Sales Analysis
**Project Level**: Beginner
**Database**: PostgreSQL
**Database Name**: `sql project p1`

This project focuses on analyzing retail sales data using SQL. It demonstrates core SQL concepts such as database creation, data cleaning, exploratory data analysis (EDA), and extracting business insights from transactional data.

---

## 🎯 Project Objectives

* Set up a retail sales database
* Clean and validate raw sales data
* Perform exploratory data analysis (EDA)
* Answer business-focused analytical questions using SQL
* Strengthen proficiency in SQL for data analyst roles

---

## 🗂️ Database Setup

### Database Creation

```sql
CREATE DATABASE retail_sales_db;
```

### Table Creation

```sql
CREATE TABLE retail_sales (
    transactions_id INT PRIMARY KEY,
    sale_date DATE, 
    sale_time TIME,
    customer_id INT, 
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT, 
    cogs FLOAT,
    total_sale FLOAT
);
```

---

## 🧹 Data Exploration & Cleaning

### Record Count

```sql
SELECT COUNT(*)
FROM retail_sales;
```

### Unique Customer Count

```sql
SELECT COUNT(DISTINCT customer_id)
FROM retail_sales;
```

### Unique Product Categories

```sql
SELECT DISTINCT category
FROM retail_sales;
```

### Identify NULL Values

```sql
SELECT *
FROM retail_sales
WHERE sale_date IS NULL
   OR sale_time IS NULL
   OR customer_id IS NULL
   OR gender IS NULL
   OR age IS NULL
   OR category IS NULL
   OR quantity IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL;
```

### Remove Incomplete Records

```sql
DELETE FROM retail_sales
WHERE sale_date IS NULL
   OR sale_time IS NULL
   OR customer_id IS NULL
   OR gender IS NULL
   OR age IS NULL
   OR category IS NULL
   OR quantity IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL;
```

---

## 📊 Business Analysis Queries

### 1️⃣ Sales on a Specific Date

```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

### 2️⃣ Clothing Sales (High Quantity – Nov 2022)

```sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;
```

### 3️⃣ Total Sales by Category

```sql
SELECT category,
       SUM(total_sale) AS total_sales,
       COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```

### 4️⃣ Average Customer Age (Beauty Category)

```sql
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

### 5️⃣ High-Value Transactions

```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
```

### 6️⃣ Transactions by Gender and Category

```sql
SELECT category,
       gender,
       COUNT(*) AS total_transactions
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```

### 7️⃣ Best Performing Month Each Year

```sql
SELECT year, month, avg_sale
FROM (
    SELECT EXTRACT(YEAR FROM sale_date) AS year,
           EXTRACT(MONTH FROM sale_date) AS month,
           AVG(total_sale) AS avg_sale,
           RANK() OVER (
               PARTITION BY EXTRACT(YEAR FROM sale_date)
               ORDER BY AVG(total_sale) DESC
           ) AS rnk
    FROM retail_sales
    GROUP BY 1, 2
) t
WHERE rnk = 1;
```

### 8️⃣ Top 5 Customers by Total Sales

```sql
SELECT customer_id,
       SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

### 9️⃣ Unique Customers per Category

```sql
SELECT category,
       COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
```

### 🔟 Orders by Time Shift

```sql
WITH hourly_sales AS (
    SELECT *,
           CASE
               WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
               WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
               ELSE 'Evening'
           END AS shift
    FROM retail_sales
)
SELECT shift,
       COUNT(*) AS total_orders
FROM hourly_sales
GROUP BY shift;
```

---

## 🔍 Key Insights

* Sales patterns vary significantly across categories and time periods
* A small group of customers contributes a large share of revenue
* High-value transactions highlight premium purchasing behavior
* Sales volume differs across morning, afternoon, and evening shifts

---

## 🛠️ Tools & Skills Used

* PostgreSQL
* SQL 
* Data Cleaning & EDA
* Business Problem Solving

---

## ✅ Conclusion

This project demonstrates practical SQL skills for data analysis, including database design, data cleaning, and extracting insights to support business decisions. It serves as a strong portfolio project for entry-level data analyst / business analyst roles.

---



📌 This project is part of my data analytics / business analytics portfolio and reflects hands-on SQL learning.
