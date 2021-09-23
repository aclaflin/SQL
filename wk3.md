# Objectives
* SQL
* SELECT
* FROM
* WHERE

SQL is not a programming language, but is a data sublanguage
* data definition language - define structures (DDL)
* data manipulation language - definition, updating, retreival (DML)
* SQL/persistent stored modules
* transaction control language
* data control language

```SQL

-- ******************* Retrieve, filter and sort records

-- to create single line comments

/* multi-line comments
use */

-- ***** define correct database to write queries against
-- use the drop list from the tool bar - select TeachSQL

-- write code to define DB to use
USE TeachSQL;

-- ##### SQL COMMANDS #####
/*
SELECT - field(s) to be returned
FROM - table(s)that contain fields
WHERE - filters the record set based on criteria provided
ORDER BY - sort data
*/

-- ***** Database diagrams *****
-- option to automatically create diagram based on data

-- ************* retrieve data
-- Display the prod_name column from the Products table
SELECT	prod_name
FROM	products;

-- SQL is NOT sace sensitive - case is used for reafability of code/syntax
-- White space (tabs, line returns, extra spaces) are ignored



-- ********** returning multiple columns
-- Display the prod_id, prod_name and prod_price column 
-- from the Products table

SELECT	prod_id
		, prod_name	
		, prod_price
FROM	Products;



-- *************display all fields within a table with 
-- least amount of code use *
-- Select all fields from Products table

SELECT	* 
FROM	Products;



-- ********************** sorting records use ORDER BY
-- to add sort order add ORDER BY
-- Display the prod_name column from the Products table
-- sort ascending by prod_name
 
 SELECT	prod_name
 FROM	Products
 ORDER BY	prod_name DESC;

-- default is ascending order
-- descending order you key word DESC


-- multiple column sort - primary, secondary
-- Display the prod_id, prod_name and prod_price column 
-- from the Products table sort by prod_price descending and name

SELECT 	prod_id
		, prod_price
		, prod_name
FROM 	Products
ORDER BY	prod_price DESC
			, prod_name
;

-- ^^ so products sorted by price high to low, and alpha when products have the same price


-- sorting by SELECT statement column position
SELECT 	prod_id
		, prod_price
		, prod_name
FROM 	products
ORDER BY	2	desc	-- represents column position
			, 3			-- result is same as previous (less obvious to read, less typing)





-- ********************** WHERE clause used to filter row data
-- filter to display products that cost $3.49
SELECT 	prod_id as "Product ID"
		, prod_price
		, prod_name
FROM 	products
WHERE	prod_price = 3.49

-- ^^ relabels column name 


-- relational operator filters >, <, >=, <=, <>, != 
-- result in a BOOLEAN expression (True or False, 1 or 0)


-- products that cost less than 10 
SELECT 	prod_id as "Product ID", prod_price, prod_name
FROM 	products
WHERE prod_price<	10


-- products that cost 10 or less
SELECT 	prod_id as "Product ID", prod_price, prod_name
FROM 	products
WHERE prod_price<=	10


-- Display products not provided by vend_id DLL01
SELECT 	vend_id, prod_name
FROM 	products
WHERE	vend_id <> 'dll01'

SELECT * FROM products		-- displays entire products table

-- OR could be written ...
SELECT 	vend_id, prod_name
FROM 	products
WHERE	vend_id!= 'dll01'

-- another option using NOT to negate the relational operator 
SELECT 	vend_id, prod_name
FROM 	products
WHERE	vend_id NOT IN ('dll01')




-- ****************** Logical Operators - AND OR

-- AND criteria between two expressions - both must be TRUE
-- OR criteria between two expressions - on must be true

/* Display products and their price
produced that cost $5 and less than 
or equal to $10. */

SELECT	* 
FROM	Products
WHERE	prod_price >= 5 
AND		prod_price <= 10		-- be sure to repeat column name ( must be full espression
;



-- ********** BETWEEN operator is inclusive of number defined
-- price between $5 and $9.49
SELECT 	prod_name
		,  prod_price
FROM 	products
WHERE	prod_price BETWEEN 5 and 9.49
;


-- ************************* IS NULL as criteria to return null values
-- find records with no values NULLS
-- Display vendors that do not have a State defined

SELECT	vend_id	
		, vend_name
		, vend_state
FROM	Vendors
WHERE	vend_state IS NULL
;

-- WHERE	vend_state = ' '	-- does not work b/c looking for space
-- WHERE	vend_state = 'NULL'	-- does not work b/c looking for the word NULL


-- Display vendors that do not have NULL values

SELECT	vend_id	
		, vend_name
		, vend_state
FROM	Vendors
WHERE	vend_state IS NOT NULL
;



-- Combining WHERE clauses

/* Display the product ID, product name and  
price produced by vendor DLL01 and cost $4 or less. */

SELECT	prod_id
		, prod_name					-- parameter in WHERE clause does not have to be in SELECT clause
		, prod_price
FROM	Products
WHERE	vend_id = 'dll01'			-- both criteria must be met
	AND	prod_price <= 4
;


/* Display the product ID, product name 
and  price produced by vendor DLL01 OR cost $4 or less. */

SELECT	prod_id
		, prod_name
		, prod_price
FROM	Products
WHERE	vend_id = 'dll01'			-- one criteria must be met
	or	prod_price <= 4
;

/* Display products and their price produced 
by vendor DLL01 or BRS01 */

SELECT	prod_id
		, prod_name
		, prod_price
FROM	Products
WHERE	vend_id = 'DLL01'			
	or	vend_id = 'BRS01'
;

-- or

SELECT	prod_id
		, prod_name
		, prod_price
FROM	Products
WHERE	vend_id IN ('DLL01', 'BRS01')
;


/* Display products and their price 
produced by vendor DLL01 or BRS01 and 
cost $10 or more. */

SELECT	prod_id					-- INCORRECT
		, prod_name
		, prod_price
FROM	Products
WHERE	vend_id = 'DLL01'			
	OR	vend_id = 'BRS01'
	AND prod_price >= 10		-- incorrect results b/c OR criteria is evaluated first
;


SELECT	prod_id
		, prod_name
		, prod_price
FROM	Products
WHERE	(vend_id = 'DLL01'			
	OR	vend_id = 'BRS01')		-- expressions within parenthesis will occur first - one must be true
	AND prod_price >= 10		-- after vendors are returned, AND criteria will be returned
;

-- or

SELECT	prod_id
		, prod_name
		, prod_price
FROM	Products
WHERE	vend_id IN ('DLL01', 'BRS01') 
	AND prod_price >= 10
;



-- **************** IN operator with criteria containing
-- 2 or more OR comparisons
SELECT 	vend_id
		, prod_name
		,  prod_price
FROM 	products
WHERE	vend_id IN ('DLL01', 'BRS01') 
	AND prod_price >= 10
;

-- same if flipped

SELECT 	vend_id
		, prod_name
		,  prod_price
FROM 	products
WHERE	prod_price >= 10
	AND vend_id IN ('DLL01', 'BRS01') 
;


-- DISPLAY all customers from either Michigan, Minnesota or Ohio

SELECT	*
FROM	Customers
WHERE	cust_state = 'MI'
	OR	cust_state = 'MN'
	OR	cust_state = 'OH'
;

-- or

SELECT	*
FROM	Customers
WHERE	cust_state IN ('MI', 'MN', 'OH')			-- more efficient
;


-- ******* wild cards used for Pattern match in WHERE 
-- ******* must use LIKE with wild card 
--              % - represents any number of characters:  
--			WHERE Prod_Name LIKE 'P%'
--			% means any number of characters before/after what is given (P% means anything starting with P)
--
--              _ - one character
-- 			WHERE title LIKE '201_ Yearly Report'
--		
--		[] - used for multiple characters 
--			WHERE l_name LIKE '[ad]%'
--				returns Anderson, Anders, Dane, Downey
--
--			[a-d]	or a range of characters
--				WHERE l_name LIKE '[a-d]%'
--				returns Anderson, Anders, Bailey, Chandler, Dane, Downey
--
--		[^] - any character NOT in the specified range or set

/* Display ID and product name for products that start with ‘fish’. */
SELECT 	prod_id
	,  prod_name
FROM 	products
WHERE	prod_name LIKE 'fish%' 		-- wildcard character included in the string (very literal about spaces, 'fish %' would not include fishing)


/* Display ID and product name for products that include the words with ‘bean bag’. */
SELECT 	prod_id
	,  prod_name
FROM 	products
WHERE prod_name LIKE '%bean bag%'

		
/* Display ID and product name for products that start with ‘f’ and end with ‘y’. */
SELECT 	prod_id
	,  prod_name
FROM 	products
WHERE 	prod_name LIKE 'f%y'  		-- doesn't work because data was imported in a fixed width column so there are actual spaces in column 

-- Have to use RTRIM  to remove whitespace
-- functions are built in code that can be called and executed
-- NAME(argument1, argument 2)

SELECT 	prod_id
	,  prod_name
FROM 	products
WHERE 	rtirm(prod_name) LIKE 'f%y'



/* Display ID and product name for products that end with ‘X inch teddy bear’. */
SELECT 	prod_id,  prod_name
FROM 	products
WHERE	rtrim(prod_name) LIKE '_ inch teddy bear'

-- syntax to get X or XX inch
-- '__ inch teaddy bear' returns XX inch...
-- could use %
-- could use or clause '_
		
/* Display customer contacts that do not start with the letter ‘J’ or ‘M’. */
-- ^ will negate selection

SELECT 	cust_contact
FROM 	customers
WHERE 	cust_contact LIKE '[^JM]%'

-- or

SELECT 	cust_contact
FROM 	customers
WHERE 	cust_contact NOT LIKE '[JM]%'

```

wildcard tips

* don't over use
* don't use at start of search unless necessary (slow to process)
* pay attn to placement (it is exact pattern matching)
