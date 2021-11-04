# Data modeling and the entity-relational model

## Entity class v entity instance
* Class is a collection of entities and provides the structure (table)
* Attributes are fields
* Identifiers (primary keys)
* Instance is a record

## Relationships
Binary relationships
* one-to-one
* one-to-many
* many-many

## Cardinality 

* names and classifies relationships (min and max count of relationship)
* Each type of binary relationship has a maximum cardinality and minimum cardinality
* optional (O) or mandatory (|) and one/many

A supplier does not have to supply an item (O) and an item must have a supplier (|)
* Item -O--  <M:N>  --|- (Supplier
* Maximum cardinality on outer edge, minimum oninner edge
* Department -|-O-----|-<- Employee

![image](https://user-images.githubusercontent.com/8172631/139866533-c1e49082-d3e2-4f10-9751-3b0f71ccc8f0.png)

Weak entity cannot exist without another entity (solid line). Strong entities can exist on their own (represented by dashed connecting line)

ID-dependent weak entities
* A building can exist without apartments. An apartment cannot exist without a building. Apartment is ID-Dependent on Building
* A book can exist without editions, editions cannot exist with a book. Edition is ID-Dependent on Book

Recursive relationships (self joins)
* ex: customer can be referred by another customer
  * can refer many, can only be referred by one, optional on both sides
* recursive because we don't want duplicated information

![image](https://user-images.githubusercontent.com/8172631/139880386-a234b6c9-6b0d-43bc-ba7a-4f5e127823ac.png)

![image](https://user-images.githubusercontent.com/8172631/139881016-a63c7fe1-db53-45d7-b0f5-0b49c2c70504.png)

![image](https://user-images.githubusercontent.com/8172631/139881832-c4036295-2552-499b-b099-09004dca8470.png)

# Database design
Business needs determine design

Data model is just business view (no primary/foreign keys named)

Add foreign keys to get to logical/ER diagram

![image](https://user-images.githubusercontent.com/8172631/139905290-73cc6536-eafe-4c27-b90f-ae8fcc95f3b0.png)


business needs might give constraints table

![image](https://user-images.githubusercontent.com/8172631/139905199-44e4b361-cd55-4962-870c-1f4c538af26b.png)
