# Bike-Stores-Data-Analysis-SQL

## Project Overview
The Bike Stores project analyzes a dataset containing detailed information about orders from a bike store. This analysis aims to gain insights into sales performance, customer demographics, and product trends.By leveraging SQL queries, the project uncover patterns, identify key metrics, and provide actionable recommendations to optimize sales strategies and enhance business operations within the bike store.

## Data sources
The primary dataset used for this analysis is the "Bikestore" dataset from [Bike Stores Raw Data,SQL](https://youtu.be/1pHYKdyRvrw?si=qj8dLivydpEItiuf)

## Tools used
- Excel - Data Cleaning 
- SQL- Writing queries
- 
## Data Cleaning/Preparation
In the initial data-cleaning phase, I performed the following tasks;
- Data loading and inspection
- Handling inconsistent values
- Data formatting

## Expository Data Analysis(EDA)- SQL
EDA involved exploring the sales data to answer key questions, such as;
- Total revenue generated by each city or state, and which location contributes the most to sales?
- How does the revenue trend vary over time? Can any seasonal patterns or sales spikes be indenified?
-  Underperforming sales reps based on their total sales figures
- Distribution of revenue across different product categories, and which categories contribute the most to overall sales?
- Are there any customer segments that consistently contribute to higher sales revenue?
- Which brands are the most popular among customers?
- 
## Others Analysis include
- Identifying trends in customer retention or repeat purchases over time
- Identifying trends in the frequency of bike purchases among repetitive customers over time


```SQL
CREATE DATABASE BikeStores

SELECT * FROM Bike_Stores

/*Total revenue generated by each city or state, and which location contributes the most to sales?*/
SELECT city, state, SUM(revenue) AS total_revenue
FROM Bike_Stores
GROUP BY city, state
ORDER BY total_revenue DESC;

ALTER TABLE Bike_Stores ADD order_month INT;

UPDATE Bike_Stores
SET order_month = MONTH(order_date);

ALTER TABLE Bike_Stores
ALTER COLUMN order_month VARCHAR(50); 

UPDATE Bike_Stores
SET order_month =
    CASE 
        WHEN order_month = 1 THEN 'January'
        WHEN order_month = 2 THEN 'February'
        WHEN order_month = 3 THEN 'March'
        WHEN order_month = 4 THEN 'April'
        WHEN order_month = 5 THEN 'May'
        WHEN order_month = 6 THEN 'June'
        WHEN order_month = 7 THEN 'July'
        WHEN order_month = 8 THEN 'August'
        WHEN order_month = 9 THEN 'September'
        WHEN order_month = 10 THEN 'October'
        WHEN order_month = 11 THEN 'November'
        WHEN order_month = 12 THEN 'December'
    END;

/*How does the revenue trend vary over time? Can any seasonal patterns or sales spikes be indenified?*/
SELECT order_month , SUM(revenue) AS monthly_revenue
FROM Bike_Stores
GROUP BY order_month 
ORDER BY order_month;


/*Identify any underperforming sales reps based on their total sales figures*/
SELECT sales_rep, SUM(revenue) AS total_sales
FROM Bike_Stores
GROUP BY sales_rep
ORDER BY total_sales ASC;


/*What is the distribution of revenue across different product categories, 
and which categories contribute the most to overall sales?*/
SELECT category_name, SUM(revenue) AS total_revenue
FROM Bike_Stores
GROUP BY category_name
ORDER BY total_revenue DESC;


/*Are there any customer segments that consistently contribute to higher sales revenue*/
SELECT customers, SUM(revenue) AS total_revenue
FROM Bike_Stores
GROUP BY customers
ORDER BY total_revenue DESC;


/*Which brands are the most popular among customers?*/
SELECT brand_name, SUM(revenue) AS total_revenue
FROM Bike_Stores
GROUP BY brand_name
ORDER BY total_revenue DESC;


/*Identify any trends in customer retention or repeat purchases over time?*/
SELECT customers, COUNT(DISTINCT order_date) AS repeat_purchase_count
FROM Bike_Stores
GROUP BY customers
HAVING COUNT(DISTINCT order_date) > 1
ORDER BY repeat_purchase_count DESC;


SELECT CASE WHEN COUNT(DISTINCT order_date) = 1 THEN 'New Customer' ELSE 'Repeat Customer' END AS customer_type,
       AVG(revenue) AS avg_revenue_per_order
FROM Bike_Stores


/*Identify any trends in the frequency of bike purchases among repetitive customers over time*/
SELECT customers,order_month,
COUNT(*) AS bike_purchase_count
FROM Bike_Stores
WHERE customers IN (SELECT customers FROM Bike_Stores GROUP BY customers HAVING COUNT(DISTINCT product_name) > 1)
AND product_name = 'Electra Townie Original 7D EQ - 2016'
GROUP BY customers, order_month
ORDER BY order_month, bike_purchase_count DESC;
```
## Review and Conclusion
SQL queries has been used in this project to extract actionable insights from the dataset. By delving into various aspects of sales performance, customer behavior, and product trends; valuable findings have been uncovered. These insights provide a solid foundation for optimizing strategies and driving business growth. Going forward, continued monitoring and leveraging data-driven approaches will be crucial for maintaining competitiveness and enhancing overall performance.












