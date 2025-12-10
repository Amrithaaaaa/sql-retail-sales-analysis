# sql-retail-sales-analysis

📌 Project Overview

Project Name: Retail Sales Analysis
Project Level: Beginner
Database Used: PostgreSQL
Database Name: sql_project_p1

This project focuses on analyzing retail sales data using SQL. It demonstrates core SQL concepts required for an entry-level Data analyst/ Business analyst role, including database creation, data cleaning, exploratory data analysis (EDA), and business-driven querying.

The objective of this project is to convert raw sales data into meaningful business insights using structured SQL queries.

🎯 Project Objectives

Create and manage a retail sales database using SQL

Perform data cleaning by identifying and removing incomplete records

Conduct exploratory data analysis to understand sales patterns

Answer real-world business questions using SQL queries


🗂️ Database & Table Structure
Database Creation

CREATE DATABASE p1_retail_db;

Table Creation

CREATE TABLE retail_sales
(
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

🧹 Data Cleaning & Exploration
1. Basic Exploration

Total records in the dataset

Total unique customers

Unique product categories

SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

2. Handling Missing Values

Identified records containing NULL values

Removed incomplete records to ensure data quality

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

📊 Business Analysis & SQL Queries

Below are key business questions answered during this analysis:

1. Sales on a Specific Date

SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';

2. Clothing Sales with High Quantity in Nov 2022

SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;

3. Total Sales by Category

SELECT 
    category,
    SUM(total_sale) AS total_sales,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;

4. Average Age of Beauty Category Customers

SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';

5. High-Value Transactions

SELECT *
FROM retail_sales
WHERE total_sale > 1000;

6. Transactions by Gender and Category

SELECT 
    category,
    gender,
    COUNT(*) AS total_transactions
FROM retail_sales
GROUP BY category, gender
ORDER BY category;

7. Best Performing Month Each Year (Average Sales)

SELECT year, month, avg_sale
FROM (
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER (
            PARTITION BY EXTRACT(YEAR FROM sale_date) 
            ORDER BY AVG(total_sale) DESC
        ) AS rank
    FROM retail_sales
    GROUP BY 1, 2
) t
WHERE rank = 1;

8. Top 5 Customers by Total Sales

SELECT 
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;

9. Unique Customers per Category

SELECT 
    category,
    COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;

10. Sales by Time Shift

WITH hourly_sales AS (
    SELECT *,
           CASE
               WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
               WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
               ELSE 'Evening'
           END AS shift
    FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM hourly_sales
GROUP BY shift;

  

Key Insights

Customers belong to diverse age groups with varying purchasing patterns

Certain transactions indicate high-value or premium purchases

Monthly trend analysis helps identify peak sales periods

A small group of customers contributes significantly to total revenue

Sales distribution varies across different time shifts


✅ Conclusion

This project demonstrates my ability to:

Design and manage relational databases

Clean and prepare data for analysis

Apply SQL to answer real-world business questions

Extract actionable insights from raw transactional data

It serves as a strong foundation project for aspiring data analysts and showcases practical SQL skills relevant to entry-level roles.
