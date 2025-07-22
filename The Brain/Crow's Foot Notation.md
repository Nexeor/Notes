2024-10-08 15:20

Status:

Tags: #Database #SQL 

# Crow's Foot Notation
Crow's foot notation is used to graphically represent database systems in a readable manner. The notation consists of two parts
- Tables/Objects are represented as "entities" with given attributes
- Relationships between these tables

## Entities
Each distinct object is represented as a name with a set of attributes
- * The uniquely identifying attribute ([[Relational Database|primary key]]) is marked with an asterick
- It is common to also note the type of data adjacent to each attribute
![[Pasted image 20241008152444.png]]

## Cardinality
We can also draw relationships between entities to define how they are related. A line between two entities shows a relationship, and the symbols on either end describe the relationship 

Relationships typically define how two entities relate, but we can also use it to represent relationships between individual attributes as well. 

Cardinality is determined by two symbols:
- The first symbol is the *maximum* number of times this object may relate to the other one, which must be **many** or **one**
	- **many**: For each of the other object, their can be many of this one
	- **one**: For each of the other object, there is at least one of this one
- The second symbol is the *minimum* number of times this object must relate to the other one, which may be **one** or **zero**
	- Describes whether the relationship is optional or not
	- **one**: Not optional. For each of this object their must be one of the other
	- **zero**: Is optional. For each of this object their may be one of the other
### Many
An entity with this relationship symbol can be related to as many of the other entity as needed
 ![[Pasted image 20241008155024.png]]
### One
Can be used in two ways:
- If it is the first symbol, it indicates that this entity can only be related with one instance of the other entity
- If it is the second symbol, it indicates that this entity must be related with at least one of the other entity

![[Pasted image 20241008155200.png]]
### Zero
Indicates that this relationship is optional. This entity can be related to another entity, but doesn't need to.

![[Pasted image 20241008155308.png]]

### Combining Symbols
These symbols can then be combined to represent complex relationships

#### Ex 1: Teacher and Course
In this school, each teacher only teaches one course. However, the same course can be taught by multiple different teachers
- Teacher-Course: Each teacher only teaches a single course, but must teach at least one course, so we use a "one and only one" relationship here
- Course-Teacher: Each lesson can be taught by multiple teachers, and each course must be taught by at least one teacher, so we use a "many or one" relationship
![[Pasted image 20241008155417.png]]

### Ex 2: Customer and Pizza
In our pizzeria we have many different pizza types. When a customer enters, they may purchase a single pizza or many pizzas. Similarly, for each customer, it can be ordered by many customers or no customers. For both of these we use a "zero or many" relationship 
![[Pasted image 20241008160420.png]]

## References
https://vertabelo.com/blog/crow-s-foot-notation/