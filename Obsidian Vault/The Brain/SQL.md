2024-10-01 22:34

Status:

Tags: #Database #SQL 

# SQL (Structured Query Language)
SQL is a database paradigm that organizes data into distinct tables
- **Table:** Each table represents a distinct object type 
	- **Ex:** You would have separate tables for "Users", "Products", and "Orders"
- **Column**: Each column within a table represents a distinct attribute of the object
	- **Ex:** A User table would have separate columns for "Id, name, password" etc.
- **Row:** Each row within a table represents an instance of that object
	- Also called "records"

## Primary and Foreign Keys
**Primary Key:** This column is used for indexing the table, and must be unique for all rows
- **Ex:** We need a "user_id" column to be our primary key for our user table. Each row must have a unique user_id
**Composite Primary Key**: If a single table has multiple primary keys, they form a composite primary key. These columns now form a combined primary key. 
- **Ex:** We have two rows, one a number and one a string, forming a composite primary key. We may have the records (1, A) and (1, B), because the second column is different. We cannot have (1, A) twice. 
**Foreign Key:** When we store the primary key of a table as a column in a different table, creating a relationship between them
- Allows us to access all the data of one table from another and model relationships between them
- **Ex:** We have a table of teams and a table of players, each having an "ID" column as their primary key
	- To represent which team the players are on, we can put the team id as a column in the player table
	- We can now get data about a team from an individual player, or get all the data about all players on a team from the team itself
	- ![[Pasted image 20241008124120.png]]

## Types of Relationships
There are two main types of relationships in SQL:
- One to Many 
- Many to Many
### One to Many Relationships
A relationship where a "parent" table can contain many instances of a "child" table. Each instance of a child table may only belong to a single parent, however.
- Instances of a child table are optional. A parent can have zero children and still be a one to many relationship, because we COULD add children
	- Ex: A customer (parent) had no orders (child)
- The child stores the primary key of the parent
#### Examples:
- One customer can have many orders (but each order belongs to one customer)
- One department can have many employees (but each employee belongs to a single department)
- One product can have many different customizations (but each customization belongs to a single product)

![[Pasted image 20241104160652.png]]

### Many to Many Relationship
A relationship where two tables reference each other many times
#### Examples:
- We have an author table and a book table. An author can write many books, but a book can be written by multiple authors as well. 
- We have a student table and a class table. A student can be in many classes, but a class also contains many students.

These relationships are problematic, as we cannot simply include the related primary keys in the table, as one record could relate to the other table three times, while another record only relates once, leaving tables with unnormalized columns:

![[Pasted image 20241104161426.png]]

#### Joining Table
We must use a separate "joining table" to represent a many to many relationship. This is a third table that contains primary keys from the related tables. 
- These may also be called "association tables"
- Primary key is optional for a joining table, but can still be helpful
- Can include supplementary information that pertains to both tables 
	- Ex: What date did this student enroll in this class?

Example: For the class/student example, we must make a third joining table. Each row contains the primary key of a class and then the primary key of a student. Each row represents a student and a class that student is taking. 
- If we want to see all the classes a student is taking, we can sort the joining table by that student 
- If we want to see all the students in a certain class, we can sort the joining table by that class

![[Pasted image 20241104162131.png]]

## Database Normalization

### First Normal Form
- Row order cannot be used to convey information 
- Data types within the same column must be the same 
	- Ex: Cannot have a string and an int in the same column 
- Each row must have a unique primary key to identify it 
- Repeating groups are not permitted
	- A repeating group is like an array. It contains potentially infinite items that are loosely related
	- Ex: A player inventory can contain many types of items, and different amounts of those items. We cannot represent this inventory as a single column ("5 coins, shield, sword, 3 potions") as this is a repeating group. We cannot represent this as a series of columns (item 1, item 1 quantity...) because this series of columns could repeat a variable amount of times and the number of columns in a table must be constant
### Second Normal Form
