## Review
* SELECT
* FROM
* WHERE
* GROUP BY
* HAVING
* ORDER BY
* Comparison operators:
	* =, !=, <>, <, >, <=, >=, IM, NOT IN, BETWEEN, NOT BETWEEN, LIKE, NOT LIKE, IS NULL, IS NOT NULL
* relational operators
* Aggregate functions
 	* COUNT, SUM, AVG, MIN, MAX
 * logical operators
	* AND, OR, NOT
* wild cards

# Objectives
* Calculated fields
* Aggregate functions 

*Data definition language (DDL)* used to create tables, relationships, and other structures

*Data manipulation language (DML)* used for querying, inserting, modifying, and deleting data. Includes SQL Views which are used to create predefined queries.

## Creating tables and relationships
```SQL
CREATE TABLE NewTableName (
	ColumnName	DataType	OptionalColumnConstraints,
	ColumnName	DataType	OptionalColumnConstraints,
	...
	optional table constraints
	...
	);
```
Many data types are available

Column constraints include PRIMARY KEY, FOREIGN KEY, NOT NULL, NULL, UNIQUE, DEFAULT
* NOT NULL means the value is required
* NULL means not required
* UNIQUE means there cannot be duplicated values
* DEFAULT enters content if nothing provided

Defining primary keys
```SQL
-- Two methods:

-- Column constraint:
CREATE TABLE TableName(
	Field1	DataType	PRIMARY KEY,
	Field2	DataType	NOT NULL,
	...);
	
-- Table constraint:
CREATE TABLE TableName(
	Field1	DataType	NOT NULL AUTO_INCREMENT,
	Field2	DataType	NOT NULL,
	Field3	DataType	NOT NULL UNIQUE,
	...
	CONSTRAINT	Table_PK	PRIMARY KEY(Field1)
	);
```

Defining Foreign keys
```SQL
CREATE TABLE TableName(
	Field1	DataType	NOT NULL AUTO_INCREMENT,
	Field2	DataType	NOT NULL,
	Field3	DataType	NOT NULL UNIQUE,
	...
	CONSTRAINT	TableName_PK		PRIMARY KEY(Field1)
	CONSTRAINT	Table1_Table2_FK	FOREIGN KEY(Field2
				REFERENCES	ChildTable(FieldName)
					ON UPDATE CASCADE		-- INSTRUCTIONS FOR WHAT TO DO WITH UPDATES
					-- ON UPDATE NO ACTION
					-- ON DELETE CASCADE
					-- ON DELETE NO ACTION
	);
```

## Data manipulation

Inserting data
```SQL
INSERT INTO TableName VALUES ('char', 'date', integer, numeric)

-- To specify columns

INSERT INTO TableName (Field1, Field2)
VALUES ('char', 'date')

-- Order of data and columns must match, but does not need to match order in table
```
 

## Calculated fields 
Creating new fields to create calculations as an output

Written in SELECT clause and given a name using the following syntax
```SQL
SELECT  field1
	, field2*field3 AS Alias
FROM	table
;
```

_Concatenating fields_
```SQL
SELECT cust_city+', '+cust_state+' '+cust_zip
AS Customer Address
From Customers;

SELECT CONCAT(cust_city, ', ', cust_state, ' ', cust_zip) AS CustomerAddress
FROM Customers
;
```

## Aggregate functions

_Built in functions_
* Count(field or * ) AS FieldName
* MIN (field) AS FieldName
* MAX (field) AS FieldName
* AVG
* SUM
* LEN - length of string (not an aggregate of column)

_Time and Date_
* MONTH - MONTH(fieldname())
* YEAR()
* MINUTE()
* HOUR()
* DATEPART(interval, fieldname()) - returns part of date specified by interval (yyyy, mm, dd)
* DATEDIFF(interval, date1, date2) - returns difference between two dates

## Multiple table queries

### Joins
Joins are one of the most important operations - understanding joins and syntax is extremely important to learning SQL. More on this in coming weeks.

Joins retreive data from multiple tables. It can be written in multiple formats (FROM or WHERE).

A CROSS JOIN (Cartesian product) joins every possible combination

```SQL
SELECT 	Field1, Field2, ...
FROM 	Table1, Table2		-- Returns garbage

-- Adding WHERE table1.PK=table2.FK creates an inner join

SELECT 	Field1, Field2, ...
FROM 	Table1, Table2	
WHERE table1.PK=table2.FK
```

When fields from multiple tables are required, you must include the table names in the FROM clause and join the PK and FK

```SQL
-- These do the same thing:

WHERE table1.PK=table2.FK			-- implicit join

FROM table1 INNER JOIN table2 
		ON table1.PK=table2.FK		-- explicit join
```

![image](https://user-images.githubusercontent.com/8172631/136593386-b155e0e9-91e4-49d5-94fc-374126c7caed.png)

### Subqueries

The result of a query is a relation, thus, a query may feed another query. Subqueries are used when more than one returned recordset in needed to evaluate criteria.

They are easier to write and understand than JOINS, 
```SQL
-- What customers placed orders in Feb and May?

SELECT 	cust_name
FROM 	customers 
WHERE 	cust_id IN 
		(SELECT cust_id
		FROM orders
		WHERE Month(OrderDate) = 2
	AND
		(SELECT cust_id
		FROM orders
		WHERE Month(OrderDate) = 5
```
### Self joins
When comparing a recordset to itself

## Data modification and deletion

UPDATE...SET
```SQL
UPDATE	TableName
SET	Field1 = 'new data'
WHERE	PK = KEY

-- WHERE is critical - otherwise will change all!
```

DELETE works the same way, but may fail because of data that must be in other tables

Table and constraint modification
* DROP TABLE
* ALTER TABLE
* TRUNCATE TABLE
 
 
# Class Scripts
```SQL
/* Calculated Fields / Aggregate Functions / GROUP BY clause */

USE TeachSQL;

-- ************** CALCULATED FIELDS ***********************
-- Create a query to display each item's total price 
-- for each item ordered
Select * from OrderItems;

SELECT 	order_num
	, order_item
	, prod_id
	, quantity
	, item_price as ItemPrice
	, quantity*item_price, AS TotalItemPrice
FROM	orderitems;

-- Practice: Using query results above, display only order items 
-- that exceed $750 in total price


SELECT 	order_num
	, order_item
	, prod_id
	, quantity
	, item_price as ItemPrice
	, quantity*item_price AS TotalItemPrice
FROM	orderitems
WHERE	quantity*item_price > 750;	-- Must restate equation (not new name)


-- Practice: Using the query above - add another field that provides the total price
-- with a Discount of 10% on each item
-- example: if buying 10 items that cost $20 each with 10% discount, what is discounted price
-- (10 * 20) * (1-0.1)= 180

SELECT 	order_num
	, order_item
	, prod_id
	, quantity
	, item_price as ItemPrice
	, quantity*item_price AS TotalItemPrice
	, quantity*item_price*(1-.1) AS DiscountPrice
	-- Ensure corroect order of operations
FROM	orderitems;




--  Practice: to format output use FORMAT(expression, 'c') c = Currency



-- ************** CONCATENATING FIELDS using + ***********************
-- Display the Customer Contact and the Address concatenated
-- (City, State, Zip)
SELECT * from customers;

SELECT	cust_name
	, cust_address
	, cust_city + ', ' + cust_state + '  '+ cust_zip 
AS 'City State Zip'
from	customers;


-- remove whitespace on right of field with RTRIM(fName) function

SELECT	cust_name
	, cust_address
	, rtrim(cust_city) + ', ' + cust_state + ' '+ (cust_zip)
AS 'City State Zip'
from	customers;


-- USE the CONCAT(f1, f2, f3) (no +)
SELECT	cust_name
	, cust_address
	, concat(rtrim(cust_city), ', ', cust_state,' ', cust_zip)
AS 'City State Zip' from	customers;



-- ************** AGGREGATE FUNCTIONS (summarizing field data) ***********************
-- COUNT(*) 		-- counts all records returned from a query 
-- count(fieldName)	-- NOTE:  if field is NULL it is EXCLUDED from count
-- Look for difference to see if there are NULL values

-- display the number of orders made to date
SELECT	count(order_num) as NumOrders
FROM	Orders;

-- can also use an count(*) vs an actual field name; NOTE: this will count all records in recordset
SELECT	count(*) as NumOrders
FROM	Orders;


-- ***** Using COUNT() with NULL values - SAMPLE:
-- Display the  total number of vendors and the number vendors that
-- have state values defined

select * from vendors
;


SELECT 	count(*) as NumVendors
	, count(vend_state) as NumVendorsWithStates
from	Vendors;


-- Practice: display the number of customers that have no state value defined

SELECT 	count(*) as NumVendorsWithoutState
from	Vendors
WHERE vend_state IS NULL
;

-- display the number of products currently available, 
-- lowest cost product, highest cost prod and the
-- average cost products currently sold by Teach SQL

select	prod_price from products;

SELECT	COUNT(*) as NumProducts
	, min(prod_price) as LowestPrice
	, max(prod_price) as HighestPrice
	, Avg(prod_price) as AveragePrice			-- one way to do an average
	, sum(prod_price)/count(*) as AvgPrice		-- another way to do an average
FROM	products;	


-- use keyword DISTINCT to display only unique values
SELECT	DISTINCT prod_price as DistinctProdPrice 
FROM products;


-- Practice:  perform above calculations against DISTINCT product price


SELECT	COUNT(DISTINCT prod_price) as NumProducts
	, min(prod_price) as LowestPrice
	, max(prod_price) as HighestPrice
	, Avg(DISTINCT prod_price) as AveragePrice								-- one way to do an average
	, sum(DISTINCT prod_price)/count(DISTINCT prod_price) as AvgPrice		-- another way to do an average
FROM	products;	




-- ************** GROUP BY CLAUSE (grouping aggregate data) *******************************
-- common error is placing a non-aggregate
-- field in the SELECT clause without using
-- GROUP By clause

SELECT	vend_id
	, prod_price
FROM	Products;

-- For each Vendor - display the  number of products they supply, 
SELECT	vend_id			-- field that is NOT within an aggregate function(not being summarized)
	 					-- must be included in GROUP BY clause
		, count(*) as NumProducts
FROM	Products
GROUP BY  VEND_ID;		-- MUST BE INCLUDED SINCE IN THE SELECT STATEMENT & not within aggregate function


-- PRACTICE: add to query above the following columns:
-- lowest cost product, highest cost prod and the
-- average cost products provided by each vendor. 
-- display the number of products with an unique price by vendor

SELECT	vend_id			-- field that is NOT within an aggregate function(not being summarized)
	 					-- must be included in GROUP BY clause
		, count(*) as NumProducts
		, MIN(prod_price) AS LowestPrice
		, MAX(prod_price) AS HighestPrice
		, AVG(prod_price) AS AvgPrice
		, COUNT(DISTINCT prod_price) As DistinctPrice
FROM	Products
GROUP BY  VEND_ID;	


-- PRACTICE: display query above for only vendor DLL01

SELECT	vend_id		
		, count(*) as NumProducts
		, MIN(prod_price) AS LowestPrice
		, MAX(prod_price) AS HighestPrice
		, AVG(prod_price) AS AvgPrice
		, COUNT(DISTINCT prod_price) As DistinctPrice
FROM	Products
WHERE	vend_id='DLL01'
GROUP BY  VEND_ID;	


-- *******************************************************************
-- NOTE - if including a non-aggregate field within SELECT statement
-- the field must include it in the GROUP BY clause!
-- *******************************************************************


-- display the total sum of all products purchased
-- run following to view data!
-- Original query from ABOVE example

SELECT	Prod_id
	, item_Price
	, Quantity
	, item_Price*quantity AS TotalItemPrice
FROM	OrderItems;


-- PRACTICE: edit the query from above to display the TOTAL SUM OF ALL PRODUCTS PURCHASED

SELECT	SUM(item_Price*quantity) AS TotalProductsPurchased
FROM	OrderItems;

-- PRACTICE: display the total sum of all products purchased 

SELECT	prod_id
		, SUM(item_Price*quantity) AS TotalProductsPurchased
FROM	OrderItems
GROUP BY	prod_id
;

-- PRACTICE: display the gross Sales for each product purchased that 
-- cost more than $5

Select	*
From	OrderItems
Where	item_price > 5
order by 5
;


Select	prod_id
		, sum(item_price*quantity) AS TotalProductsPurchasedOver5
From	OrderItems
Where	item_price > 5
Group BY prod_id
;


--- GROUP LEVEL FILTER - use HAVING CLAUSE
-- display products from above query that have more than $500 in purchases

Select	prod_id
		, sum(item_price*quantity) AS TotalProductsPurchasedOver5
From	OrderItems
Where	item_price > 5
Group BY prod_id
;

Select	prod_id
		, sum(item_price*quantity) AS TotalProductsPurchasedOver5
From	OrderItems
Where	item_price > 5		-- row level filter
Group BY prod_id
HAVING	sum(item_price*quantity) > 500		-- filtering aggregated/summarized data
;


-- *************************PRACTICE
-- Query #1 - display the number orders placed by each customer

Select * From orders;

SELECT	cust_id
		, count(order_num  AS NumOrders
FROM	Orders
Group By Cust_id;

-- Query #2 - display only customers that have placed 2 or more orders
		-- based on COUNT so must be HAVING not WHERE

SELECT	cust_id
		, count(order_num) AS NumOrders
FROM	Orders
Group By Cust_id
HAVING	count(order_num) >= 2
;


-- ************** DATETIME functions ************************************
-- sys date time function displays todays date

SELECT	Order_Num
	, order_date
	, Sysdatetime() as TodaysDate
	, getdate() as Today				-- returns same (slightly rounded) date/time
From	orders;

-- YEAR(fieldName), MONTH(fieldName), Day(FieldName) are DATE functions often 
-- Display the current year, the order number, and the year of the order placed
-- FUNCTIONS RETURN INTEGER VALUES, i.e. 2013, 5, 

SELECT	order_date
	, YEAR(order_date) as OrderYear
	, Month(order_date) as OrderMonth
	, Day(order_date) as OrderDay
FROM	Orders

-- Display orders placed in May using a function - MONTH()

SELECT	*
FROM	Orders
WHERE	month(order_date) = 5
;

-- Display orders placed in May using DATEPART function
-- syntax = DATEPart(interval,field)
-- returns requested format for interval selected

select 	datepart(mm, order_date) as OrderMonth
		, DATEPART(YYYY, ORDER_DATE) AS OrderYear
FROM ORDERS;


-- PRACTICE: display orders placed 2013 using DATEPART()

SELECT	*
FROM	Orders
WHERE	DATEPART(yyyy, order_date) = 2013
;

-- Display order Number, the order date and the number of days since  
-- the order was placed DATEDIFF(interval, date1, date2)

SELECT 	Order_num
	, order_date
	, datediff(dd, order_date, SYSDATETIME()) as DaysSinceOrder
FROM	ORDERS;

-- TO SEE IN YEARS - TRUNCATES EXTRA DAYS 

SELECT 	Order_num
	, order_date
	, datediff(YYYY, order_date, SYSDATETIME()) as YearsSinceOrder
FROM	ORDERS;

-- More EXACT displays output in years using the days interval and dividing by 365.25

SELECT	Order_num
	, order_date
	, datediff(dd, order_date, SYSDATETIME())/365.25 as YearsSinceOrder
FROM	ORDERS;

```



