
# Bike rental shop - SQL Case study 



Introduction:
Emily is the shop owner, and she would like to gather data to help her grow the business. She has hired you as an SQL specialist to get the answers to her business questions such as How many bikes does the shop own by category? What was the rental revenue for each month? etc. The answers are hidden in the database. You just need to figure out how to get them out using SQL.



## Understanding the database:


The shop's database consists of 5 tables:
 
       -custome
       -bike
       -rental
       -membership_type
       -membership       
## Problem statement with solutions

  1.Emily would like to know how many bikes the shop owns by category.  
Display the category name and the number of bikes the shop owns in each category (call this column number_of_bikes ). Show only the categories where the number of bikes is greater than 2 .


    select category, count(1) as number_of_bikes 
    from bike
    group by category
    having count(1) > 2;


2.Emily needs a list of customer names with the total number of memberships purchased by each.

For each customer, display the customer's name and the count of memberships purchased (call this column membership_count ). Sort the results by membership_count , starting with the customer who has purchased the highest number of memberships.

    select c.name, count(m.id) as membership_count 
    from membership m
    right join customer c on c.id=m.customer_id
    group by c.name
    order by count(1) desc;


3.Emily is working on a special offer for the winter months. Can you help her prepare a list of new rental prices?
For each bike, display its ID, category, old price per hour (call this column old_price_per_hour ), discounted price per hour (call it new_price_per_hour ), old 
price per day (call it old_price_per_day ), and discounted price per day (call it 
new_price_per_day ).


    select id, category,
    price_per_hour as old_price_per_hour,
    case when category = 'electric' then round(price_per_hour - (price_per_hour*0.1) ,2)
	when category = 'mountain bike' then round(price_per_hour - (price_per_hour*0.2) ,2)
    else round(price_per_hour - (price_per_hour*0.5) ,2)
    end as new_price_per_hour, price_per_day as old_price_per_day,
    case when category = 'electric' then round(price_per_day - (price_per_day*0.2) ,2)
	when category = 'mountain bike' then round(price_per_day - (price_per_day*0.5) ,2)
    else round(price_per_day - (price_per_day*0.5) ,2)
    end as new_price_per_day
    from bike;


4.Emily is looking for counts of the rented bikes and of the available bikes in each category.
Display the number of available bikes (call this column 
available_bikes_count ) and the number of rented bikes (call this column rented_bikes_count ) by bike category.


    select category,
    count(case when status ='available' then 1 end) as available_bikes_count, count(case when status ='rented' then 1 end) as rented_bikes_count
    from bike
    group by category;


5.Emily has asked you to get the total revenue from memberships for each combination of year, month, and membership type.
Display the year, the month, the name of the membership type (call this column membership_type_name ), and the total revenue (call this column total_revenue ) for every combination of year, month, and membership type. Sort the results by year, month, and name of membership type.


    select extract(year from start_date) as year,
    extract(month from start_date) as month, mt.name as membership_type_name, sum(total_paid) as total_revenue
    from membership m
    join membership_type mt on m.membership_type_id = mt.id
    group by year, month, mt.name
    order by year, month, mt.name

