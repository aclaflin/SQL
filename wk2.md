## Review - Week 1
* purpose of a database
* potential problems with lists
* what is a database
* what is SQL
* 4 components of a database
* Elements of a database
* Functions of DBMS

# Objectives - Week 2
* conceptual foundation of relational model
* how relations differ from nonrelational tables
* basic relational terminology
* meaning and important of keys, foreign keys, and related
* how FKs represent relationships
* purpose and use of surrogate keys
* meaning of functional dependencies
* apply a process for normalizing relations

## Characteristics of a relation
 1. Rows contain data about an entity
 1. Columns contain data about attributes of entity
 1. Cells of the table hold a single value (NA/NULL is ok)
 1. All entries in a column are of the same kind
 1. Each column has a unique name
 1. The order of columns is unimportant
 1. The order of rows is unimportant
 1. No two rows may hold identical sets of data values

Table/Record/Field
* TABLE_NAME (always singular, join words with underscore)
* FieldName

Longhand
TABLE_NAME (Fields, _UnderlinePrimaryKey_, *ItaliciseForeignKey*)

## Keys
* Candidate keys (any keys that uniquely identify each row in a relation and could be used as primary key)
	* Every determinate must be a candidate key
* Primary key (candidate key that is chosen based on most meaningful/important functional dependency) 
	* **ideal primary key is short, numeric, never changes**
	* The requirement that no two rows be identical *implies* that a primary key can be defined for each relation. 
	* Can be defined as one or more attributes that functionally determine all other attributes of the relation
* Alternate key (candidate keys not chosen)
* Foreign key (primary (inc. composites) key of another relation that is placed in the second relation to represent a relationship between two tables)
* Surrogate key (unique, DBMS-assigned identifier added to the table, useful when candidate keys are long and/or nonnumeric)
* Composite key (contains two or more attributes)

Primary and foreign keys do not need to have the same field names, only required to have the same values (referential integrity)

Functional dependencies are the foundations for building keys

## Integrity constraints
* Entity (Table) integrity constraint - PK must be unique, cannot be null
* Domain (Field) - field properties enforce data integrity (data type, field size, null/Not null, default values, ...)
* Referential (relationship) - ensures FK value exists in primary table data

Enforcing referential integrity happens in the DBMS

### NULL values
* missing values
* ambiguous
  * no appropriate value
  * known, not entered
  * unknown
* can eliminate ambiguity by requiring a value

## Functional dependencies

* The only reason for having relations is to store instances of functional dependencies
* If I tell you a specific fact, can you respond with a unique associated fact?

```r
CookieCost = NumberOfBoxes x $5
NumberOfBoxes -> CookieCost
```

* CookieCost is functionally dependent on NumberOfBoxes
* NumberOfBoxes is the determinant, determines CookieCost
* Multiple fields can be functionally dependent on a determinant
* You cannot always determine functional dependencies from sample data - must talk to expert users who create data

* PK = determinant
* functional dependence = attribute/field functionally defined by determinant
* Every determinate must be a candidate key

## Normalization 
Apply a process for breaking a table or relation with more than one theme into set of tables where each has one theme, normalizing relations, removing redundancies

1. Identify all the candidate keys of the relation
2. Identify all the functional dependencies in the relation
3. Examine the determinants of the functional dependencies. If any determinant is not a candidate key, the relation is not well formed
	* Place columns of functional dependency in their own new relation
	* Make determinant of the functional dependency the primary key of the new relation
	* Leave a copy of the determinant in the original relation as foreign key
	* Create referential integrity constraint between new and original relations
4. Repeat until every determinant of every relation is a candidate key

*Multivalued dependencies (shown with double arrow ->->) 
* *Determinant of multivalued dependencies can never be the primary key
* *pose no problems as long as they are in their own table
* *Primary key and sole candidate key is the composite of the two columns
* *When isolated, table is 4NF

### Well-formed relation design principles
1. Every determinant must be a candidate key
2. Any relation that is not well formed (functional dependency doesn't involve primary key, determinant of a functional dependency is not a candidate key, ...) should be broken into two or more relations that are well formed

Well-formed relations are in Boyce-Codd Normal Form (BCNF)

* 1NF - A relation is in first normal form (1NF) if it 
  * has characteristics of a relation
  * has a defined primary key
  * data does not contain repeating groups (multivalued dependencies)

* 2NF - remove partial dependencies (fields that are functionally dependent on only part of the CPK)
	* Is table 1NF and are all non-key attributes determined only by the entire primary key?
		* Problem solved by 2NF can only occur in a table with a composite primary key
		* If the table has a single column PK, this problem cannot occur and if 1NF, then also 2NF

	* If only one PK per relation (table) - is 2NF
  * Cannot have partial dependencies if no composite primary key 

3NF - remove determinants (redundancies)
* remove determinants (fields that are defined by something other than PK)

BCNF - Boyce Codd Normal Form

4NF

5NF

## Steps to DB design
1. Gather data to be stored - Fields
2. Put fields together within their proper themes/subjects (characteristics of the table) - Tables
3. Define PK for each table defined
4. Define DIRECT relationship between tables
 * Add FK


## ER diagrams

![image](https://user-images.githubusercontent.com/8172631/133635357-51ce2305-25b1-4d02-8a48-10334a62af9b.png)

![image](https://user-images.githubusercontent.com/8172631/133635470-86afe9bd-88f8-41a0-9297-3740ba5de915.png)

