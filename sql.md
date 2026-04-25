# sql queries and analysis
## ques1. Retrieve the total number of orders placed.
```sql
use pizza;
select * from order_details;
SELECT 
    COUNT(order_id) AS totalorders
FROM
    order_details;
```
## ques2.Calculate the total revenue generated from pizza sales.
``` sql
use pizza;
select 
ROUND (SUM(order_details.quantity * pizzas.price),2) as total_sales
from order_details join pizzas
on  order_details.pizza_id = pizzas.pizza_id;
```
## ques3.Identify the highest-priced pizza.
```sql
SELECT 
    pizza_types.name, pizzas.price AS pizzaprice
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
ORDER BY pizzaprice DESC
LIMIT 1;
```
## ques4.List the top 5 most ordered pizza types along with their quantities.
```sql
select pizza_types.name,sum(order_details.quantity) as Quantity
from pizza_types join pizzas 
on pizza_types.pizza_type_id = pizzas.pizza_type_id
join order_details 
on order_details.pizza_id = pizzas.pizza_id
group by pizza_types.name order by Quantity desc limit 5;
```
## ques5.Join the necessary tables to find the total quantity of each pizza category ordered
```sql
SELECT 
    pizza_types.category,
    SUM(order_details.quantity) AS quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
ORDER BY quantity DESC;
```
## ques6.Determine the distribution of orders by hour of the day.
```sql
SELECT hour(time) as hour ,count(order_id) as order_count
from orders 
group by hour(time) ;
```
## ques7.Join relevant tables to find the category-wise distribution of pizzas.
```sql
SELECT category , count(name) from pizza_types
 group by category ;
```
## ques8.Group the orders by date and calculate the average number of pizzas ordered per day.
```sql
SELECT 
    ROUND(AVG(quantity), 0)
FROM
    (SELECT 
        orders.date, SUM(order_details.quantity) AS quantity
    FROM
        orders
    JOIN order_details ON orders.order_id = order_details.order_id
    GROUP BY orders.date) AS order_quantity;
```
## ques9.Determine the top 3 most ordered pizza types based on revenue.
```sql
SELECT 
    pizza_types.name,
    SUM(pizzas.price * order_details.quantity) AS revenue
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY revenue DESC
LIMIT 3;
```
