# Joins

* WHERE clause joins
* FROM clause joins
  * recommended

``` SQL
-- practice - aggregate function query - YAYA!!

/* Display the vendor ID for vendors that produce more 
than 2 products that cost more than $4 */

Use TeachSQL;

SELECT	vend_id
FROM	Products
WHERE	prod_price > 4		-- row level filter, prior to aggregation
GROUP BY	vend_id
HAVING	COUNT(vend_id) >2	-- aggregate filter


-- ************************** JOINS ***************************************
-- JOINs are used to relate multiple tables using their T1.PK = T2.FK  (common field)
-- relationship
-- There are INNER JOINS, OUTER JOINS and SELF JOINS

--************ INNER JOINS **********************************
-- Can be created either within the WHERE clause or in the FROM Clause

-- ****************WHERE clause JOIN  syntax example: (Slower and cannot use for outer joins)
/*	SELECT	f1, f2, f3
	FROM	table1, table2, table3 -- only the names
	WHERE	table1.pk=table2.fk
		AND	table2.pk=table3.fk
*/

select * from vendors;
select * from products;

-- DISPLAY vendor names and the products produced

SELECT	vendors.vend_id, vend_name, prod_id, 
		prod_price, prod_name
FROM	PRODUCTS, Vendors 
WHERE	products.vend_id = vendors.vend_id; -- JOIN SYNTAX

-- NOTE: must explicitly state table name since field names are identical
-- i.e. vendors.vend_id or products.vend_id

-- Display the Customers (including name) 
-- and the orders they placed
-- WHERE CLAUSE JOIN

SELECT *
FROM	Customers, Orders
WHERE Customers.cust_id = Orders.cust_id

-- using aliases shortens syntax

SELECT *
FROM	Customers as c			--alias
		, Orders o				-- AS is optional, but makes it clear
WHERE c.cust_id = o.cust_id

--*****WHERE JOIN PRACTICE:  write a query to display
-- Cust_id, Cust_name, Order_num
-- Order_date, Order_item, 
-- Prod_name, quantity and 
-- item_price for products ordered

select * from OrderItems;

SELECT	 Customers.Cust_id
		, Cust_name
		, Orders.Order_num
		, Order_date
		, Order_item
		, Prod_name
		, quantity
		, item_price
FROM	Customers
		, Orders
		, OrderItems
		, Products
WHERE	Products.prod_id = OrderItems.prod_id
		AND Customers.cust_id = Orders.cust_id
		AND Orders.order_num = OrderItems.order_num


-- ********* JOINS USING THE FROM CLAUSE 
-- These queries can also be written using a FROM clause join
-- FROM CLAUSE JOIN syntax:
/*		SELECT	f1, f2, f3,
		FROM	table1 JOIN table 2 ON
					table1.pk = table2.fk 
				JOIN table3 ON				-- NOTE: must follow logical path of joins
					table2.PK=table3.fk 
*/

SELECT	Vendors.vend_id, vend_name, prod_id, 
		prod_price, prod_name
FROM	PRODUCTS inner JOIN vendors ON   -- INNER keyword is OPTIONAL
			products.vend_id=vendors.vend_id;  -- JOIN syntax
		
-- Display the Customers (including name) 
-- and the orders they placed
-- FROM CLAUSE JOIN

SELECT	cust_name
		, cust_contact
		, order_num
		, order_date
FROM	Customers JOIN Orders ON
			Customers.cust_id = Orders.cust_id

--*****FROM JOIN PRACTICE:  write a query to display
-- Cust_id, Cust_name, Order_num
-- Order_date, Order_item, 
-- Prod_name, quantity and 
-- item_price for products ordered
/*		SELECT	f1, f2, f3,
		FROM	table1 JOIN table 2 ON
					table1.pk = table2.fk 
				JOIN table3 ON				-- NOTE: must follow logical path of joins
					table2.PK=table3.fk 
*/

SELECT	 Customers.Cust_id
		, Cust_name
		, Orders.Order_num
		, Order_date
		, Order_item
		, Prod_name
		, quantity
		, item_price
FROM	Customers JOIN Orders ON
			Customers.cust_id = Orders.cust_id
		JOIN OrderItems ON								-- Orders already called, just continue joining
			Orders.order_num = OrderItems.order_num
		JOIN Products ON
			Products.prod_id = OrderItems.prod_id
	
-- Joining in FROM is faster because it isn't pulling everything in before the join is made 
-- it only pulls in what is necessary


-- NOTE: FROM CLAUSE JOINS MUST BE USED IN SQL SERVER TO PERFORM OUTER JOINS

-- ********** OUTER JOINS ***************************************
-- Are used to display ALL  records from a table  regardless of whether they have any related records
-- MUST OCCUR IN THE FROM CLAUSE

-- Query to display ALL customer records regardless of whether they placed an order

SELECT * from customers; 
select* from orders;

SELECT	C.cust_id
		, cust_name
		, order_Num
		, order_date AS 'Date'
FROM	CUSTOMERS AS c LEFT JOIN ORDERS AS o
		on	C.CUST_ID=O.cust_id;

-- or the same thing as a right join...
SELECT	C.cust_id
		, cust_name
		, order_Num
		, order_date AS 'Date'
FROM	Orders AS o right JOIN Customers AS c		
			on	C.CUST_ID=O.cust_id;
		
-- display customers that have NOT placed an order 

SELECT	C.cust_id
		, cust_name as CustWithoutOrders	
		, order_Num
		, order_date as 'Date'
FROM	CUSTOMERS AS c LEFT JOIN ORDERS AS o
			on	C.CUST_ID=O.cust_id
WHERE	order_num is NULL;  -- restrict/filter data to 
							-- display only order_num IS NULL

-- display ALL products regardless of if they have been ordered

SELECT	*
FROM	products AS p LEFT JOIN ORDERItemS AS o
			on	p.prod_id = o.prod_id

-- display products that have not been ordered
SELECT	p.prod_name 
FROM	products AS p LEFT JOIN ORDERItemS AS o
			on	p.prod_id = o.prod_id
WHERE	order_num is NULL;


-- display ALL customers and ALL orders 
-- Using a FULL JOIN

SELECT *
FROM	Customers FULL JOIN Orders ON
			customers.cust_id = orders.cust_id

-- display ALL VEndors (vend_id, vend_name, 
-- prod_id, prod_name) 
-- regardless of whether
-- they supply any products

SELECT Vendors.vend_id	
		, vend_name
		, prod_id
		, prod_name
FROM	Vendors LEFT JOIN Products ON
			vendors.vend_id = products.vend_id

-- Display vendor name for vendors that DO NOT supply products

SELECT Vendors.vend_id	
		, vend_name
		, prod_id
		, prod_name
FROM	Vendors LEFT JOIN Products ON
			vendors.vend_id = products.vend_id
WHERE	prod_id is NULL

-- to retrieve EVERYTHING from both tables  - OUTER JOIN

SELECT Vendors.vend_id	
		, vend_name
		, prod_id
		, prod_name
FROM	Vendors FULL JOIN Products ON
			vendors.vend_id = products.vend_id


--***********WHERE CLAUSE JOINS CAN ONLY BE USED FOR INNER JOINS!!
-- 
-- display vendors that don't supply 
-- any products

-- Using aliases for table names - 




-- write a query to display
-- Cust_id, Cust_name, Order_num
-- Order_date, Order_item, 
-- Prod_name, quantity and 
-- item_price for products ordered

-- written with WHERE CLAUSE JOIN


-- written using FROM CLAUSE join

```
