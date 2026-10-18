E-Commerce Database Management and Analysis Project

1. Introduction & Objective

Project Purpose

The goal of this project is to design, implement, and query a relational database (ecommercedb) tailored for an online retail platform. 
Modern e-commerce businesses rely heavily on structured data storage to manage product inventories, track customer activity, and analyze sales performance.

Business Questions Addressed

Which products fall into specific pricing tiers or categories (e.g., Fashion, Electronics)?

What are the total values, minimums, and maximums associated with our product catalog and order transactions?

How can we analyze customer ordering behavior using different types of database joins (Inner, Left, Right, and Cross Joins)?

2. Database Design & Schema

The database consists of four interconnected tables designed to maintain data integrity and support complex relational queries:

customers Table: Stores customer profiles including a unique ID (customer_id), name, email, and recent activity (login_date). 
(Note: The schema was updated to drop the city column and include login_date for engagement tracking).

products Table: Maintains inventory details such as product name, category, and unit price.

orders Table: Records transaction timestamps linked to specific customers via a foreign key (customer_id).

order_items Table: Acts as a junction/transaction-detail table capturing line-item specifics (quantity, item price) linking orders and products.


Entity Relationships (ERD Summary)

Customers to Orders: One-to-Many  — One customer can place multiple orders.
Orders to Order Items: One-to-Many  — One order can contain multiple items.
Products to Order Items: One-to-Many  — One product can appear in multiple order line items.

3. SQL Queries & Logic
Below is the core SQL logic implemented in the project, categorized by functionality:

A. Data Retrieval & Filtering

-- Retrieve products priced at or above 200
select product_id,product_name,price from products
where price >= 200;

-- Filter products exclusively within the 'fashion' category
select product_id,product_name,price from products
where category = 'fashion';

B. Aggregations & Grouping

-- Calculate overall metrics for orders and product pricing
select count(*) as total_orders
from orders;

select sum(price) as total_price
from products;

select 
min(price) as lowest_price,
max(price) as highest_price
from products;

-- Analyze order items by order ID using filtering and aggregation
select order_id, count(order_item_id) as total_items
from order_items
where item_price >= 200
group by order_id;

-- Comprehensive order summary including total revenue, min, and max item prices per order
select order_id, sum(quantity * item_price) as total_order,
min(item_price) as lowest_order,
max(item_price) as highest_order
from order_items
where quantity > 0 
group by order_id;

C. Relational Joins
-- Inner Join: Matches customers who have successfully placed orders
select 
customers.customer_name,
orders.customer_id
from customers inner join orders on
customers.customer_id = orders.customer_id;

-- Left Join: Displays all customers, including those who haven't placed any orders yet
select customers.customer_id,customers.customer_name,
orders.order_id,orders.order_date
from customers left join orders on
customers.customer_id = orders.customer_id;

-- Right Join: Displays all orders and their associated customer details
select customers.customer_id,customers.customer_name,
orders.order_id,orders.order_date
from customers right join orders on
customers.customer_id = orders.customer_id;

-- Cross/Conditional Join: Evaluates product data against matching order item records
select products.product_id,products.product_name,
order_items.order_item_id,order_items.item_price
from products cross join order_items on
products.product_id = order_items.product_id;

4. Results & Visual Evidence

1. Customers Table (customers)
customer_id  customer_name       email_id	                                        login_date
101	          kumar                  kumar.mc@gmail.com	26-10-2025
102	          senthil	senthil@gmail.com	 	27-10-2025
103	          ramesh	rammm@gmail.com		30-10-2025
104	          lakshmi	lakshmiammal@gmail.com	2/11/2025
105	          priya 	priyaluv@gmail.com		15-11-2025

2. Products Catalog (products)

product_id	product_name	category	      price
1	pendrive		electronics	     400
2	chair		furniture	     200
3	phant		fashion	     500
4	mobileholder	accessories     50
5	perfume		fashion	     100

3. Orders Log (orders)

order_id	order_date 	customer_id
1001	10-02-2026	101
1002	20-02-2026	101
1003	09-03-2026	103
1004	10-03-2026	102
1005	02-04-2026	102


Query Execution & Analysis Results
1. Filtering Products by Price ( >200)
Query Goal: Extract premium products with prices equal to or greater than Rs.200.

product_id	  product_name	price
1	  pendrive         	400
2	  chair	                    200
3	  phant	                    500

Filtering Products by Category (fashion)

Query Goal: Isolate all products categorized under fashion
product_id	 product_name	price
3	phant		500
5	perfume		100

Order & Price Range Financial Summary
Query Goal: Aggregate line items per order to evaluate 
total order amount, minimum line item price, and maximum line item price.

order_id	total_order	lowest_order	highest_order
1001	400		200		200
1002	1750	50		400
1003	400		400		400
1005	200		200		200

Relational Join Output Comparison
1. Inner Join Output
Logic: Displays only customers who have placed orders.

customer_name	customer_id
kumar		101
kumar		101
senthil		102
senthil		102
ramesh		103

2. Left Join Output
Logic: Displays all customers from the left table, highlighting non-purchasing users with NULL values (lakshmi and priya).

customer_id	customer_name	order_id	order_date
101		kumar		1001	10/2/2026
101		kumar		1002	20-02-2026
102		senthil		1004	10/3/2026
102		senthil		1005	2/4/2026
103		ramesh		1003	9/3/2026
104		lakshmi		NULL	NULL
105		priya		NULL	NULL

3. Right Join Output
Logic: Displays all orders and their associated customer details.

customer_id	customer_name	order_id	order_date
101		kumar		1001	10-02-2026
101		kumar		1002	20-02-2026
103		ramesh		1003	09-03-2026
102		senthil		1004	10-03-2026
102		senthil		1005	02-04-2026

4. Cross/Conditional Join Output
Logic: Maps products against matching order line items.

product_id	   product_name	order_item_id	item_price
1	   pendrive		2		400
1	   pendrive		5		400
2	   chair		1		200
2	   chair		4		200
4	   mobileholder	3		50

5. Key Insights & Conclusion

Key Insights

Customer Engagement: Customers like kumar (ID 101) and senthil (ID 102) show active purchasing behavior with multiple orders.

Product Performance: Higher-priced items like pendrives (Electronics) and chairs (Furniture) drive significant line-item value, 
while fashion accessories represent a distinct lifestyle category.

Relational Integrity: Using Left and Right joins helps identify inactive users (customers with zero orders), 
which can guide future marketing campaigns and promotional offers.



