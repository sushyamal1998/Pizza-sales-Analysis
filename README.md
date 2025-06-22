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
- ** Calculate the total revenue generated from pizza sales.**
```sql
select round(sum(p.price * od.quantity),2) revenue 
from order_details od join pizzas p on od.pizza_id = p.pizza_id;
```
- **Identify the highest-priced pizza.**
```sql
select  pt.name, price highest_price 
from pizzas p join pizza_types pt on pt.pizza_type_id = p.pizza_type_id
order by price desc limit 1;
```
- **Find Hourly Order Volume.**
```sql
select hour(order_time) order_hour, count(*) order_count
from orders
group by order_hour
order by order_hour;
```
- **Identify the most common pizza size ordered.**
```sql
select sum(od.quantity), p.size 
from order_details od join pizzas p on p.pizza_id = od.pizza_id
group by p.size
order by sum(od.quantity) desc limit 1;
```
- **List the top 5 most ordered pizza types along with their quantities.**
```sql
select pt.name, sum(od.quantity) total_sold
from order_details od
join pizzas p on od.pizza_id = p.pizza_id
join pizza_types pt on p.pizza_type_id = pt.pizza_type_id
group by pt.name
order by total_sold desc limit 5;
```
- **Join the necessary tables to find the total quantity of each pizza category ordered.**
```sql
select sum(od.quantity), pt.category
from order_details od join pizzas p on p.pizza_id = od.pizza_id
join pizza_types pt on pt.pizza_type_id = p.pizza_type_id
group by pt.category;
```
- **Determine the distribution of orders by hour of the day.**
```sql
select hour(order_time) , count(order_id)
from orders
group by hour(order_time);
```
- **Join relevant tables to find the category-wise distribution of pizzas.**
```sql
select count(distinct pizza_type_id), category
from pizza_types 
group by category;
```
- **Group the orders by date and calculate the average number of pizzas ordered per day.**
```sql
select avg(quan) from
(select o.order_date,sum(od.quantity) quan
from orders o join order_details od on o.order_id = od.order_id
group by o.order_date) order_quantity;
```
- **Determine the top 3 most ordered pizza types based on revenue.**
```sql
select pt.name, sum(p.price* od.quantity) reveneu
from order_details od join pizzas p on p.pizza_id = od.pizza_id
join pizza_types pt on pt.pizza_type_id = p.pizza_type_id
group by pt.name
order by reveneu desc limit 3;
```
- **Calculate the percentage contribution of each pizza type to total revenue.**
```sql
select pt.category, (sum(p.price* od.quantity) /
   (select sum(p.price* od.quantity) 
   from order_details od join pizzas p on p.pizza_id = od.pizza_id
   join pizza_types pt on pt.pizza_type_id = p.pizza_type_id))*100 reveneu_percentage
from order_details od join pizzas p on p.pizza_id = od.pizza_id
join pizza_types pt on pt.pizza_type_id = p.pizza_type_id
group by pt.category;
```
- **Analyze the cumulative revenue generated over time.**
```sql
with sales as 
   (select o.order_date, sum(od.quantity* p.price) as revenue
   from order_details od join pizzas p on p.pizza_id = od.pizza_id
   join orders o on o.order_id = od.order_id
   group by o.order_date)
select order_date,
sum(revenue) over(order by order_date) cum_revenue
from sales;
```
- **Determine the top 3 most ordered pizza types based on revenue for each pizza category.**
```sql
with category_revenue as(
   select pt.name, pt.category, sum(p.price* od.quantity) reveneu
   from order_details od join pizzas p on p.pizza_id = od.pizza_id
   join pizza_types pt on pt.pizza_type_id = p.pizza_type_id
   group by pt.category,pt.name),
category_rank as 
(select category, name,
   rank() over(partition by category order by reveneu desc) rnk
   from category_revenue)
select category, name , rnk
from category_rank
where rnk <= 3;
```

## Conclusion
This project demonstrates how SQL can be used for business analytics, allowing companies to make data-driven decisions about product strategy, pricing, and customer experience using transactional sales data.

