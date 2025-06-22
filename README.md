# Pizza-sales-Analysis-Using-SQL

## Project Overview

**Project Title** : Pizza Sales Analysis
**Database** : 'pizzahut'
This project analysis a pizza store sales data using sql. The main goal is that derive valuable insighgts about sales performance, popular items, revenue ets. it showcases the sql join, aggregate function, some business related queriws.

## Dataset Description
The dataset is split across multiple table , often in .csv file.<br>
- **order_details:** Contains items ordered in each order.
- **orders:** Contains order id, date and time.
- **pizza_types:** Contains pizza_type_id, name, category, ingredients.
- **pizzas:** Contain pizza_id,pizza_type_id, size, price.

## Objectives
- Explore and clean dataset.
- Understand Customer behavior.
- Identify top selling pizzas and categories.
- Calculate total renenue, ang order etc.

## Tools Used
- SQL(MYSQL)
- SQL Workbench

## Sample Analysis Performed

- **Retrieve the total number of orders placed.**
```sql
select count(order_id) total_order from orders;
``` 
