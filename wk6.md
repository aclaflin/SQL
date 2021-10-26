## Review
* Creating database and tables
* Modifying data (insert, update, delete, drop, alter)

# Objectives
* union queries
* views
* case statements

## Union queries
* joins two record sets into one - MUST have same datatypes columns, expressions, or aggregate functions, in the same order
* UNION ALL will include duplicate rows
* UNION removes duplicates

## Views
* Virtual tables
* can save a complex query, reformatting data, frequently used
* CREATE VIEW view.name AS
* Use GO before and after to separate 

## Case statements
* Transact statements, similar to IF..THEN..ELSE
```SQL
SELECT Field1
     , Field4 =
        CASE WHEN expression
          THEN  result1
        ELSE
           result2
        END
 FROM table
 ```
 
 ## Class queries
 ```SQL
 Use TeachSQL;		 
-- ************** UNION queries  - used to join (append) to independent record sents
-- write a query that lists the customer and vendor addresses

SELECT	cust_name as Name
		, cust_address as 'Address'
		, cust_city City
		, cust_state 'State'
		, cust_zip ZIP
FROM	customers
UNION
SELECT	vend_name
		, vend_address 
		, vend_city
		, vend_state
		, vend_zip
FROM	vendors

-- concatenating with NULL fields returns NULL for new field

SELECT	cust_name as Name
		, cust_address as 'Address'
		, cust_city City
		, Concat(cust_state, ', ' , cust_zip) as StateZIP
FROM	customers
UNION
SELECT	vend_name
		, vend_address 
		, vend_city
		, concat(vend_state, ', ', vend_zip)
FROM	vendors

-- **** we can add another field that will indicate whether record represents a customer or vendor

SELECT	cust_name as Name
		, cust_address as 'Address'
		, cust_city City
		, cust_state 'State'
		, cust_zip ZIP
		, 'Customer' as 'Type'
FROM	customers
UNION
SELECT	vend_name
		, vend_address 
		, vend_city
		, vend_state
		, vend_zip
		, 'Vendor' 
FROM	vendors

-- use UNION ALL - to include duplicates

-- UNION removes any duplicate records

 
-- *****************************************************************
-- VIEWS - allows you to store and reuse query syntax; 
-- saved as an object within SQL Server
/*Virtual tables; contain queries that dynamically retrieve data when used.
Contain no actual data Sample Syntax:
	GO 	
	CREATE VIEW name AS
		SELECT	field1, field2, field3	
		FROM 	table1 AS T1, table2 AS T2	
		WHERE	T1.PK=T2.FK	
	Go */
	
-- create a query that will allow you to see the orders a customer 
-- has placed as well inlude order num, date, qty ordered, cost, 
-- extended price 
-- once it is created we will convert it to a view!
go
SELECT	customers.cust_id
	, cust_name
	, orders.order_num
	, order_date
	, products.prod_id
	, prod_name
	, quantity
	, item_price
	, quantity*item_price as ExtPrice
FROM	Customers JOIN ORDERS ON
			Customers.cust_id = Orders.cust_id
		JOIN OrderItems ON
			Orders.order_num = OrderItems.order_num
		JOIN Products ON
			OrderItems.prod_id = Products.prod_id
			
go			
-- make the query above a VIEW called CustomerOrders

go
CREATE VIEW CustomerOrders AS
SELECT	customers.cust_id
	, cust_name
	, orders.order_num
	, order_date
	, products.prod_id		-- must be selected to use from view
	, prod_name
	, quantity
	, item_price
	, quantity*item_price as ExtPrice
FROM	Customers JOIN ORDERS ON
			Customers.cust_id = Orders.cust_id
		JOIN OrderItems ON
			Orders.order_num = OrderItems.order_num
		JOIN Products ON
			OrderItems.prod_id = Products.prod_id
;			
go			

-- Using view display customers that purchased product RGAN01

SELECT *
FROM CustomerOrders
WHERE prod_id = 'RGAN01'

-- Use ALTER VIEW to edit 

go
ALTER VIEW CustomerOrders AS
SELECT	customers.cust_id
	, cust_name
	, orders.order_num
	, order_date
	, products.prod_id		
	, prod_name
	, vend_name
	, quantity
	, item_price
	, quantity*item_price as ExtPrice
FROM	Customers JOIN ORDERS ON
			Customers.cust_id = Orders.cust_id
		JOIN OrderItems ON
			Orders.order_num = OrderItems.order_num
		JOIN Products ON
			OrderItems.prod_id = Products.prod_id
		JOIN Vendors on
			Products.vend_id = Vendors.vend_id
;			
go		

-- using view provide number of orders placed BY each CUSTOMER

SELECT *
FROM CustomerOrders

SELECT	cust_name
		, cust_id
		, Count(distinct(order_num)) as OrderCount
FROM CustomerOrders
GROUP BY	cust_name
			, cust_id			-- two different customers with same name!

-- Note: Remember that one order may have several items on it!


-- CASE STATEMENTS 
/* ID regions 
Midwest = IL, IN, MI
Eastern = NY, OH
Not defined = all others
*/

SELECT	cust_id
		, cust_name
		, cust_state
		, Region =
			CASE WHEN cust_state in ('IL', 'IN', 'MI')
				THEN 'Midwest'
			WHEN cust_state in ('NY', 'OH')
				THEN 'Eastern'
			ELSE 'Not defined'
		END
FROM Customers
Order BY 4
;

Use ReviewSQL;

/* create field to tell if item is 'in stock' or needs to be ordered 'order'
business rule = moer than 25 must be in stock */

SELECT *
FROM	Books

SELECT book_id
		, title
		, price
		, qty_on_hand
		, Status = 
			CASE WHEN qty_on_hand > 25
				THEN 'In Stock'
			ELSE 'Order'
			END
FROM	books
Order by qty_on_hand
 ```
