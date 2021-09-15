## Review - Week 1
* purpose of a database
* potential problems with lists
* what is a database
* what is SQL
* 4 components of a database
* Elements of a database
* Functions of DBMS

## Objectives -Week 2
* conceptual foundation of relational model
* how relations differ from nonrelational tables
* basic relational terminology
* meaning and important of keys, foreign keys, and related
* how FKs represent relationships
* purpose and use of surrogate keys
* meaning of functional dependencies
* apply a process for normalizing relations

### Characteristics of a relation
 * rows contain data about an entity
 * Columns contain data about attributes of entity
 * Cells of the table hold a single value (NA/NULL is ok)
 * All entries in a column are of the same kind
 * Each column has a unique name
 * The order of columns is unimportant
 * The order of rows is unimportant
 * No two rows may hold identical sets of data values

Table/Record/Field
* TABLENAME
* FieldName

Longhand
TABLENAME (Fields, _UnderlinePrimaryKey_, *ItaliciseForeignKey*)

#### Keys
* Primary key
* Foreign key (primary key of another relation that is placed in the current relation to represent a relationship between two tables)
* Surrogate key (unique, DBMS-assigned identifier added to the table, (**ideal is short, numeric, never changes**))
* Composite key

### Integrity constraints
* Entity (Table) integrity constraint - PK must be unique, cannot be null
* Domain (Field) - field properties enforce data integrity (data type, field size, null/Not null, default values, ...)
* Referential (relationship) - ensures FK value exists in primary table data

Enforcing referential integrity happens in the DBMS

NULL values
* missing values
* ambiguous
  * no appropriate value
  * known, not entered
  * unknown
* can eliminate ambiguity by requiring a value

## Normalization - apply a process for normalizing relations, removing redundancies)
* Every determinate must be a candidate key
* Any relation that is not well formed should be broken into two or more relations that are well formed


* PK = determinant
* functional dependence = attribute/field functionally 

### Normal Forms
* 1NF - A relation is in first normal form (1NF) if it 
  * has characteristics of a relation
  * has a defined primary key
  * data does not contain repeating groups

2NF - remove partial dependencies
* fields that are functionally dependent on only part of the CPK
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
