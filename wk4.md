## Review
* Select
* From
* Where
* relational operators
* logical operators
* wild cards

# Objectives
* Calculated fields
* Aggregate functions 

## Calculated fields 
Creating new fields to create calculations as an output

Written in SELECT clause and given a name using the following syntax
(Find and finish writing out)
```r
SELECT  field1


```

_Concatenating fields_
```r
SELECT cust_city+', '+sut_state+' '+cust_zip
AS Customer Address
From Customers;
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
* 

```SQL
/* April 4, 2017 - Calculated Fields / 
Aggregate Functions / GROUP BY clause */


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



