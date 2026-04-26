# SQL QUERIES AND ANALYSIS
## QUES1: Retrieve the total number of orders placed.
```sql
use pizza;
select * from order_details;
SELECT 
    COUNT(order_id) AS totalorders
FROM
    order_details;
```
## QUES2: Calculate the total revenue generated from pizza sales.
```sql
 use pizza;
select 
ROUND (SUM(order_details.quantity * pizzas.price),2) as total_sales
from order_details join pizzas
on  order_details.pizza_id = pizzas.pizza_id;
```
## QUES3: Identify the highest-priced pizza.
``` sql
SELECT 
    pizza_types.name, pizzas.price AS pizzaprice
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
ORDER BY pizzaprice DESC
```
##  QUES4: Identify the most common pizza size ordered.
```sql
SELECT pizzas.size,count(order_details.order_details_id) as ordermade
from pizzas join order_details
on pizzas.pizza_id = order_details.pizza_id
group by pizzas.size order by ordermade desc;
```
##  QUES5: List the top 5 most ordered pizza types along with their quantities.
```sql
select pizza_types.name,sum(order_details.quantity) as Quantity
from pizza_types join pizzas 
on pizza_types.pizza_type_id = pizzas.pizza_type_id
join order_details 
on order_details.pizza_id = pizzas.pizza_id
group by pizza_types.name order by Quantity desc limit 5;
```
##  QUES6: Join the necessary tables to find the total quantity of each pizza category ordered.
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
## QUES7: Determine the distribution of orders by hour of the day.
```SQL
SELECT hour(time) as hour ,count(order_id) as order_count
from orders 
group by hour(time) ;
```
## QUES8: Group the orders by date and calculate the average number of pizzas ordered per day.
```SQL
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
### QUES9: Determine the top 3 most ordered pizza types based on revenue.
```SQL
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
## QUES10: Calculate the percentage contribution of each pizza type to total revenue.
```SQL
 SELECT pizza_types.category, 
round(sum(pizzas.price * order_details.quantity) / (SELECT round(sum(pizzas.price * order_details.quantity),2) as total_sales
from order_details
join pizzas on pizzas.pizza_id =order_details.pizza_id )*100,2) as revenue 
from pizza_types join pizzas 
on pizza_types.pizza_type_id = pizzas.pizza_type_id 
join order_details on order_details.pizza_id = pizzas.pizza_id
group by pizza_types.category order by revenue desc;
```
## QUES11: Analyze the cumulative revenue generated over time.
```SQL
select date ,sum(revenue)over (order by date)as cum_revnue 
from 
(select orders.date, sum(order_details.quantity * pizzas.price) as revenue
from order_details join pizzas
on order_details.pizza_id = pizzas.pizza_id
join orders on orders.order_id = order_details.order_id
group by orders.date)as sales;
```
## QUES12:Determine the top 3 most ordered pizza types based on revenue for each pizza category.
```SQL
select name , revenue from 
(select category , name  ,revenue ,rank() over (partition by category order by revenue desc)as rn 
from 
(select pizza_types.category ,pizza_types.name,
sum(order_details.quantity * pizzas.price) as revenue
from pizza_types join pizzas
on pizza_types.pizza_type_id = pizzas.pizza_type_id
join order_details on order_details.pizza_id =pizzas.pizza_id
group by pizza_types.category ,pizza_types.name) as a) as b 
where rn<=3;
```



