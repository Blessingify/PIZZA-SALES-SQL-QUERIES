# PIZZA-SALES-SQL-QUERIES
Pizza King company need a deep analysis for each of their pizza sales, to gain more insights into thir bussiness performance 

### Tools
- SQL: Data Analysis
- Power BI: Creating report and Visiualization
## KPI 
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
<img width="119" height="64" alt="Screenshot 2026-10-24 192443" src="https://github.com/user-attachments/assets/38d53711-443e-4d28-b1ae-014bb97b2239" />


# Problem Statement
- Daily Trend for Total Order
  ``` sql
  select  datename(Dw, order_date) as order_day, count(distinct order_id) as total_orders from pizza_sales
   group by datename (dw, order_date)

  note : To get the total values by the days of the week , The date Function is used to extract each day
  ```
  <img width="147" height="137" alt="Screenshot 2026-10-24 192222" src="https://github.com/user-attachments/assets/fe6733a6-d5e1-472a-b474-868eee4d8d08" />


- Monthly trend for total orders
  ``` sql
  select  datename(Month, order_date) as month_name, count(distinct order_id) as total_orders from pizza_sales
  group by datename (Month, order_date)
  order by total_orders desc
   note: Using the Order by query, monthly trend shows july to have the highest number of order followed by may and the january

  ```
  <img width="139" height="193" alt="Screenshot 2026-10-24 192109" src="https://github.com/user-attachments/assets/a3e1522e-bd69-45b6-bbf6-325fca676fce" />

- Percentage of sales by pizza category
``` sql
select pizza_category,sum(total_price) as Total_sales, sum(total_price) * 100 /
(select sum(total_price) from pizza_sales where month(order_date) = 1) as Percentage_sale 
from pizza_sales
where month(order_date) = 1
group by pizza_category
```
<img width="269" height="88" alt="Screenshot 2026-10-24 222436" src="https://github.com/user-attachments/assets/00dce889-328b-4c87-991b-8d3370c59579" />


- Percentage of sales by pizza size
  ``` sql
  select pizza_size,cast(sum(total_price) as decimal (10,2)) as Total_sales, cast(sum(total_price) * 100 /
  (select sum(total_price) from pizza_sales where datepart(quarter,order_date)= 1)as decimal (10,2)) as Percentage_sale
  from pizza_sales
  where datepart(quarter,order_date)= 1
  group by pizza_size
  order by Percentage_sale desc
  ```
   <img width="209" height="112" alt="Screenshot 2026-10-24 230412" src="https://github.com/user-attachments/assets/842fc814-a125-4e11-b3a3-390140f2320a" />

- Total pizza sold by pizza category
``` sql
select pizza_category,  count(distinct order_id) total_orders
from pizza_sales
group by pizza_category
```

<img width="161" height="97" alt="Screenshot 2026-10-25 222241" src="https://github.com/user-attachments/assets/6e7648eb-0acd-4adf-bdcd-e0cc3a8dee68" />

- Top 5 best seller by Revenue, total quantity and total order
### by revenue
  ```sql
   select Top 5 pizza_name, sum(total_price) Total_revenue
from pizza_sales group by pizza_name, pizza_category
order by Total_revenue desc
```
<img width="212" height="118" alt="Screenshot 2026-10-30 134509" src="https://github.com/user-attachments/assets/f929d352-9ddf-473a-9ead-6e432bb8ce20" />

### by total quantity
```sql
select Top 5 pizza_name,  sum(quantity) pizza_quantity
from pizza_sales
group by pizza_name
order by pizza_quantity desc
```
<img width="226" height="107" alt="Screenshot 2026-10-30 134627" src="https://github.com/user-attachments/assets/b5d90cfb-112a-4217-8a10-7498874b69b6" />


### by total order
``` sql
select Top 5 pizza_name,  count(distinct order_id) total_order
from pizza_sales
group by pizza_name
order by total_order desc
```
<img width="215" height="110" alt="Screenshot 2026-10-30 134759" src="https://github.com/user-attachments/assets/c99e8044-c7a0-46f5-95db-9b972994f968" />




###  NOTE
If you want to apply the Month, Quarter, Week fliters to the above queries you can use 'Where' clause. Follow some of below examples;
```sql
select  datename(month, order_date) as order_day, count(distinct order_id) as total_orders
from pizza_sales
where Month (order_id)=1
    group by datename (month, order_date)

* Here Month(order_date) = 1 indicates that the output is for the month of January.
Month(order_date)= 4 indicates output for Month of Apirl

  select  datename(Dw, order_date) as order_day, count(distinct order_id) as total_orders
from pizza_sales
where datename(Quarter, order_date) = 1
   group by datename (dw, order_date)

* Here Datename(Quarter, Order_date) = 1 indicates that the output is for the Quarter 1
Month(order_date) = 3 indicates output for quarter 3
```



# Dashboaard
###  Page 1 
Give a general visual overview analysis, showing the Totals,  percentage and trends overtime

<img width="696" height="385" alt="Screenshot 2026-10-30 135525" src="https://github.com/user-attachments/assets/4fb27af3-f833-4662-959f-e17ea94f25cd" />

- The above reports show July and January has the highest sold pizza, which also shows saturday and sunday to have the highest number of total orders


### Page 2
Using Bar charts, this shows us the comparison between the best performing  and the least popular pizza options


<img width="712" height="391" alt="Screenshot 2026-10-30 141906" src="https://github.com/user-attachments/assets/8a078965-b7ef-4f3f-9deb-32fb7422fbf9" />

- The above view show The Thia chicken pizza to be the highest contributor in revenue, although when it comes to the highest Total Order and Quantity, The Calssic Deluxe Chicken takes the lead


## Recommendation

-   Having 'The Brie Carre Pizza' to be the least contributor when it comes revenue, order and quantity, there should be a price review which can be tagged as a discount sale for order to increase
-   Sales reduction can be place at certain hours on Sunday and Monday to increase the total daily orders. This price discounts can be strategically placed around noon, which is around when people are likely to have their lunch breaks or out and about 
- When it come the the pizza size, a pairing collab can be done for the XX-Large pizza, by initiating that it could come with a free topping or a drink. This will size the percentage order and also the average pizza sold.














  
