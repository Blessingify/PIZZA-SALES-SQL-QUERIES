# PIZZA-SALES-SQL-QUERIES

# KPI 
- Total Revenue
  ```sql
  select round(sum(total_price),1)  as Total_revenue from pizza_sales
  ```
- Average order value
``` sql
select sum(Total_price)/count (distinct order_id) AVG_Order_Value from pizza_sales
```
- Total pizza sold
  ```sql
  select sum(quantity) Total_pizza_sold from pizza_sales
  ```
- Total Order placed
    ```sql
    select count(distinct order_id) Total_pizza_order from pizza_sales
  ```
- Average pizza per order
  ``` sql
  select cast(cast(sum(quantity) as decimal (10,2)) / cast (count(distinct order_id) as decimal (10,2)) as decimal (10,2)) Avg_pizza_perorder from pizza_sales
```
note: the cast query is used to convert whole numbers in decimal to get the full value 
``` 
  
