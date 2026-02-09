# SQL_RETAIL_SALES
**Project Overview**
### Project Title: Retail Sales Analysis
## Database:

**This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.**

### Objectives
Set up a retail sales database: Create and populate a retail sales database with the provided sales data.
Data Cleaning: Identify and remove any records with missing or null values.
Exploratory Data Analysis (EDA): Perform basic exploratory data analysis to understand the dataset.
Business Analysis: Use SQL to answer specific business questions and derive insights from the sales data.

### Project Structure
**1. Database Setup**
Database Creation: The project starts by creating a database named project.
Table Creation: A table named retail_sales is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.
CREATE DATABASE project;

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
**2. Data Exploration & Cleaning**
Record Count: Determine the total number of records in the dataset.
Customer Count: Find out how many unique customers are in the dataset.
Category Count: Identify all unique product categories in the dataset.
Null Value Check: Check for any null values in the dataset and delete records with missing data.
-- DATA CLEANING

select count(*)from retail_sales;

select * from retail_sales;

select * from retail_sales
where transaction_id is null;

select * from retail_sales
where sale_date is null;

select * from retail_sales
where sale_time is null
or
customer_id is null
or gender is null;

**-- DATA EXPLORATION**

-- HOW MANY SALES WE HAVE?

select count(*) as total_sales from retail_sales;

-- how many unique customer we have?

select count(distinct(customer_id)) as total_customer from retail_sales;

-- how many category we have?

select distinct(category) from retail_sales;

**-- data analysis and busniess key problem with answers.**

-- Q.1] WRITE SQL QUERY TO RETRIVE ALL COLUMNS FOR SALES MADE ON '2022-11-05'.

select * from retail_sales
where sale_date='2022-11-05';

-- Q2].WRITE A SQL QUERY TO RETRIVE ALL TRANSACTION WHERE THE CATEGORY IS "CLOTHING" AND THE QUANTITY SOLD IS MORE THAN 4 IN THE MONTH OF NOV-2022.

SELECT *
FROM retail_sales
WHERE category = 'clothing'
  AND DATE_FORMAT(sale_date, '%Y-%m') = '2022-11'
  AND quantiy >= 4;
  
-- Q3]. WRITE A SQL QUERY TO CALCULATE THE TOTAL SALES FOR EACH CATEGORY
	
    select category,
    sum(total_sale) as net_sales,
    COUNT(*) AS TOTAL_ODERS
    from retail_sales
    group by category;
    
-- Q4]. WRITE A SQL QUERY TO FIND THE AVARGE AGE OF CUSTOMERS WHO PURCHESED ITEM FROM 'BEAUTY' CATEGORY.
    
    SELECT
		round(avg(age),2) as avg_age
    from retail_sales
    where category="beauty";
    
-- Q5]. write a sql query to find all transaction where the total_sale is greater than 1000.
    
    select * from retail_sales
    where total_sale > 1000;
    
-- Q6]. write a sql query to find the total number of transcation made by each gender in each category.
    
    select 
		category,
        gender,
        count(transaction_id) as total_transaction
	from retail_sales
    group by category,gender
    order by category;
    
-- Q7]. write a sql query to calculate the avg sales for each month. find out best selling moth in the year.
    
    select * from
    (
    select
		year(sale_date) as year,
		month(sale_date) as month,
		avg(total_sale) as total_sale,
        rank() over(partition by year(sale_date)order by avg(total_sale)desc) as rnk
		from retail_sales
		group by year,month
	)as t1
    where rnk=1;
		
-- Q8]. write a query to find the top 5 customers based on highest total sale.
    
    select customer_id,
    sum(total_sale) as total_sales
    from retail_sales
    group by customer_id
    order by total_sales desc limit 5;
    
-- 9]. write sql squry to find the number of unique customers who purcheseditems from each category.
    
    select 
    category,
    count(distinct(customer_id)) as unique_customer
    from retail_sales
    group  by category;
    
-- 10]. write a sql query to create each shift and number of orders morning <=12,afternoon between 12 & 17 ,evining >17)
    
SELECT
    CASE
        WHEN HOUR(sale_time) < 12 THEN 'Morning'
        WHEN HOUR(sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END AS shift,
    COUNT(transaction_id) AS total_orders
FROM retail_sales
GROUP BY shift
ORDER BY total_orders;
    
### Findings
* Customer Demographics: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
* High-Value Transactions: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
* Sales Trends: Monthly analysis shows variations in sales, helping identify peak seasons.
* Customer Insights: The analysis identifies the top-spending customers and the most popular product categories.


### Reports
* Sales Summary: A detailed report summarizing total sales, customer demographics, and category performance.
* Trend Analysis: Insights into sales trends across different months and shifts.
* Customer Insights: Reports on top customers and unique customer counts per category.


### Conclusion
This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

