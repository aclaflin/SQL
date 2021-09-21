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

-- Display vendors that do not have NULL values

SELECT	vend_id	
		, vend_name
		, vend_state
FROM	Vendors
WHERE	vend_state IS NOT NULL
;
```
