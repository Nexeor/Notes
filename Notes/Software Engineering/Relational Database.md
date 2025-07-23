2024-10-01 19:39

Status:

Tags: #Database

# Relational Database
A relational Database is a structured database, as opposed to the more freeform [[Non-Relational Database]]

A relational database consists of several **tables**:
- **Table**: Represents an object type, and encapsulates all members (rows) and attributes (columns) of that type
- **Row**: Each row is a unique instance, or **record**, of the object 
- **Column**: Each column represents an **attribute** of the object
- Tables can be linked between each other using similar columns (attributes) to join the tables
**Primary Key**: The unique identifier of a row
- All tables must have a primary key attribute
- No two rows may have the same primary key (always unique!)
- **Compound Primary Key:** When two columns are used as a primary key
	- Typically used in a "many-to-many" relationship where multiple rows are associated with multiple other rows
**Foreign Key**: A reference to the primary key of another record in a different table
- Used to form relations between tables
###### **Table 1: Customer**
| Customer ID (Primary) | Customer Name | Shipping Address |     |
| --------------------- | ------------- | ---------------- | --- |
| 001                   | Max Johnson   | 200 Foo Lane     |     |
###### **Table 2: Orders**
| Order ID (Primary) | Customer ID (Foreign) | Shipping Address | Order Date | Order Status     |
| ------------------ | --------------------- | ---------------- | ---------- | ---------------- |
| 001                | 001                   | 200 Foo Lane     | 10/01      | Out for Delivery |

Now when ask questions about different orders we can also query the customer the corresponding customer as well. We can find which customers have orders out for delivery, who made a purchase on a specific date, or issue a refund to customers that experienced a delay. 
## Benefits
Relational databases are an intuitive and standardized method of organizing databases, but also offers several other advantages:
- *Flexibility*: It's easy to update, add, or delete records and their fields without needing to change the larger database structure
- *ACID Compliance*: Relational databases support [[ACID]], allowing them to ensure data validity regardless of errors, failures, or other exceptions
- *Ease of Use*: Complex queries can be made in [[SQL]]
- *Collaboration*: Multiple people can operate and access data simultaneously. Built in locks prevent simultaneous access to data. 
- *Built-In Security*: Role-based security ensures data is limited to specific users
- *Normalization*: Design techniques reduces data redundancy and standardizes data types

## References
