# Purpose of a database
Keep track of things

unlike list or spreadsheet (flat file), DB stores info that is more complicated than a simple list

DB is backend of all information access, accessed in different ways

# Problems with lists
Data redundancy - duplicates, same data listed multiple times can become innaccurate if updating one and not the other, maintenance,

*data maintenance anomalies (problems) - inserting, updating, deleting*

Multiple themes - different subjects

Relational databases store related data in tables - avoids redundancy, maintenance problems

# Design process
1. Define themes/subjects/subjects/entities/relations
1. Put each attribute (column or field) into respective theme (table)
1. Define a primary key for each theme/table (unique identifier for each record within a table, MUST be unique, CANNOT be NULL, every table MUST have primary key, can use attribute or auto generated surrogate ID)
1. Determine relationships between themes/tables (define or add a foreign key, business rules (how you operate) defines relationships between tables))

Crows foot schema - entity relationship (ER) (picture of design)

one student has one advisor, one advisor has many students 
* advisors is 1-side/primary/parent, students is the MANY-side/related table/child
  * (if on student can have many advisors, MANY-MANY, must create linking table)
* FK resides on MANY side - the primary key from the 1-side is placed into the MANY-side table
* *add PK from parent table to child table as FK, FK is an attribute that links data*

Can be 1-many, 1-1, many-many
* *FK can be on either side of 1-1*
* *FK goes on MANY side of 1-MANY*
* *Must create an intersection or linking table for MANY-MANY*

## Linking table
* Take out direct relationship, create a new table
* Break **MANY-MANY** into **two 1:MANY relationships**
  * one student can relate to multiple advisors
  * one advisor can relate to multiple students
  * one student has one ID, one advisor has one ID
  * New table has each ID related as needed (using each existing PK from 1-side as the FK on the MANY-side)
* Composite primary key (CPK) uses multiple field
  * create a combination of two PKs to uniquely ID each student/advisor combination


## Running SQL Server in Docker

anneclaflin@Annes-Air ~ % docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=sql@Wsxcde3" -e "MSSQL_PID=Express" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest-ubuntu 
