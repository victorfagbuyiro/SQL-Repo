# SQL-Repo
--Create storedata table
Create table storedata (
Row_ID INT,
Order_ID VARCHAR (255),
Order_Date varchar,
Ship_Date varchar,
Ship_Mode VARCHAR (255),
Customer_ID VARCHAR (255),
Product_ID VARCHAR (255),
Sales float,
Discount float
);

--View storedata 
select *
from storedata;

Copy public.storedata
from 'C:\Users\ADMIN PC\Downloads\store_data.csv'
delimiter ',' csv header;



--Create masterproduct table
Create table masterproduct ( 
Product_ID VARCHAR (50),
Category VARCHAR (50),
Sub_Category VARCHAR (50),
Product_Name VARCHAR (215)
);

--View masterproduct Table content
select *
from masterproduct; 

Copy public.masterproduct
from 'C:\Users\ADMIN PC\Downloads\master_product.csv'
delimiter ',' csv header;



Create table mastercustomer (
Customer_ID VARCHAR (255),
Customer_Name VARCHAR (255),
Segment VARCHAR (255),
Country VARCHAR (255),
City VARCHAR (255),
State VARCHAR (255),
Postal_Code VARCHAR (255),
Region VARCHAR (255),
Age INT
);

select *
from mastercustomer;

Copy public.mastercustomer
from 'C:\Users\ADMIN PC\Downloads\master_customer.csv'
delimiter ',' csv header;

--:Which Customer segment makes the most purchases.
select segment,count(segment)
from mastercustomer
group by segment
--Answer:Customer segment

--Q5:How many unique customers, orders and products exist?

select count(distinct customer_id)as num_unique_customers
from mastercustomer
--number of unique customers is 793

select count(distinct order_id)as num_unique_orders
from storedata

--Number of unique orders is 4922

select count(distinct product_id)as num_unique_products
from masterproduct

--number of unique products is 1861

--Q6: Customers who are 30yrs old
select Customer_Name
from mastercustomer
where Age='30'


--Q7: Distribution of Customer age across different region
select region, ROUND(avg(age))
 from mastercustomer
 group by 1
 order by 2 desc;
 
 --Q8:Customer segment makes the most purchase
 Select Segment, count(Segment)
 from mastercustomer
group by 1
order by count DESC;

--The Consumer segment has the most purchases.

--Q9:What are the top selling product categories?
select category,ROUND(sum(sales))
from masterproduct
inner join storedata on (masterproduct.product_id=storedata.product_id)
group by 1;



--Q10:What is the average discount rate for each shipping mode?

select ship_mode, (avg(discount)) as average_discount
from storedata
group by 1
order by 2 desc;

--Q11:how does average sales vary with product sub-categories?

select sub_category,round(avg(sales)) avg_sales
from masterproduct
inner join storedata on (masterproduct.product_id=storedata.product_id)
group by 1
order by 2 desc;

--What is the relationship between discount and sales amount?
select discount, (avg(sales))
from storedata
group by 1
order by 2 desc;
