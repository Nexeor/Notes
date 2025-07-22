2024-10-01 23:08

Status:

Tags: #Database #Python #PythonLibrary #SQL #Database 

# SQLAlchemy
SQLAlchemy is a Python library used to work with a [[Relational Database]] in a more pythonic way.

Two Major Components:
- Low level API allows you to work directly with the database using Python
- An [[ORM]] maps database tables directly to Python objects

When using SQLAlchemy with [[Flask API's]], the [[Flask-SQLAlchemy]] extension for Flask changes the syntax to make using SQLAlchemy better suited for the web
- See syntax in [[Flask-SQLAlchemy]]
## Features:
- *Database Abstraction*: Supports multiple database types, such as PostgreSQL, MySQL, SQLite
- *SQL Expression Language*: Allows you to make SQL queries using Python syntax
- *Connection Pooling*: Reuses database connections to improve performance
- *Transaction Management*: Ensures that changes are only applied when necessary 
## Mapping The Database 
### Models
A **model** defines both an SQL table and a python object at the same time
- We can query the model like a table
	- *Ex: We have a "contact" model. We can query that model's table to get all contacts with a certain name, or address, or whatever criteria*
- We can use and modify instances of the model like a python object
	- *Ex: We grab a specific contact from our contact model. We can now use that contact like a python object*
	- ```print(contact.user_id)```
We define models by "mapping" traits to our table. SQLAlchemy has two mapping styles. These styles are for developer preference and do not change the underlying SQL code being generated.
- **Declarative Mapping:** Mapping are generated through in-line annotations in the code 
- **Imperative Mapping**: Mappings are declared manually using the Column() method
### Declarative Mapping
```python
from typing import List, Optional
from sqlalchemy import ForeignKey, String
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column, relationship

class Base(DeclarativeBase):
    pass

class User(Base):
    __tablename__ = "user_account"
    
    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(30))
    fullname: Mapped[Optional[str]]
    addresses: Mapped[List["Address"]] = relationship(
        back_populates="user", cascade="all, delete-orphan"
    )
    def __repr__(self) -> str:
        return f"(id={self.id!r}, name={self.name!r}, fullname={self.fullname!r})"

class Address(Base):
    __tablename__ = "address"
    id: Mapped[int] = mapped_column(primary_key=True)
    email_address: Mapped[str]
    user_id: Mapped[int] = mapped_column(ForeignKey("user_account.id"))
    user: Mapped["User"] = relationship(back_populates="addresses")
    def __repr__(self) -> str:
        return f"(id={self.id!r}, email_address={self.email_address!r})"
```

Mapping begins with a base class (usually referred to as *base*) which we then make subclass of. Each subclass is a unique model that represents both an SQL table and (later) a python object
- Define a name for the table with ```__tablename__ = ```
- Define columns of the table with ```trait: Mapped[data_type]```
- Define primary key with ```= mapped_column(primary_key=True)```
	- See more about primary keys at [[Relational Database]]
- If key is nullable, then indicate with ```Mapped[Optional[data_type]]```
- If linking to a foreign key ```mapped_column(ForeignKey("foreign_table.foreign_trait")```
##### __ init __
The declarative mapping process automatically generates a constructor, but we can define a custom one if we'd like:
```python 
def __init__(self, id, first_name, last_name, email):
	self.id = id
	self.first_name = first_name
	... 
```
##### __ repr __ 
Can also define a string representation within the model
```python
def __repr__(self):
	return f"User is {self.first_name} {self.last_name} with id {self.id}"
```

## Creating the Database
### Engine
An **engine** is what we use to create new entries and connections within the database
- Models without an engine is an empty database that can never be modified

Create a new SQLite engine:
```python
from sqlalchemy import create_engine
engine = create_engine("sqlite+pysqlite:///:memory:", echo=True)
```

**create_engine** requires a **database string**, composed of three parts
- **Database Type**: Here we specify the type of SQL we are using
	- In this case, we're using "sqlite"
- **DBAI:** The third party driver used to interact with the database
	- In this case, we're using "psyqlite"
	- If omitted, the default DBAPI for the selected database type will be selected
- **Database Location**: This is where the databse is located in memory
	- The ```:memory:``` phrase creates an "in-memory-only" database. It is only open during the current session, is not saved to any files, and does not require a server
	- We can also give a URL to a database hosted on a server
		- ```create_engine('postgresql://user:password@localhost/mydatabase')```
	- Can also give the path to a local file database:
		- ```create_engine('sqlite:///example.db')```
If we set ```echo=True``` then SQAlchemy will print to console all SQL operations being performed on the database
### Generating the Database
Once we've created our engine we can use the `create_all` method to build the tables from our metadata base and bind it to our engine we will use to modify it
```python
Base.metadata.create_all(bind=engine)
```

Can also include optional arguments:
- `tables=`, list, only build the tables specified
- `checkFirst=`, bool, only create tables already present in target database
	- Defaults to True, so tables are only built if they don't already exist
	- If set to False, existing tables are rebuilt
## Editing the Database
Now that we have an engine and our metadata is mapped, we can create instances of our classes and add them to the database. We use a **Session** to accomplish this.
- See more about the "with" keyword at [[Python Context Manager (with statement)]]
### Creating Objects
We can now use a session using the "with" keyword to use the engine and interact with the database. We can then create python objects using our models and add those to the session.
1) Use the "with" keyword and Session to access the engine
2) Create the records to be added as pythonic objects
3) Add the records to the session
4) Commit the session to update the underlying database

Example:
```python
from sqlalchemy.orm import Session

# Use the "with" keyword to open Session
with Session(engine) as session:
	# Create users as pythonic objects
    spongebob = User(
        name="spongebob",
        fullname="Spongebob Squarepants",
        addresses=[Address(email_address="spongebob@sqlalchemy.org")],
    )
    sandy = User(
        name="sandy",
        fullname="Sandy Cheeks",
        addresses=[
            Address(email_address="sandy@sqlalchemy.org"),
            Address(email_address="sandy@squirrelpower.org"),
        ],
    )
    patrick = User(name="patrick", fullname="Patrick Star")

	# Add users to session
    session.add_all([spongebob, sandy, patrick])
    # Commit changes
    session.commit()
```
## Configuring Relationships
SQLAlchemy has several different relationship types
- See [[SQL]] to learn more about relationships
### One to One: One Parent, One Child
Acts as a One To Many relationship (see below) but indicates there will only be one child row that refers to one parent row. We apply a non-collection type to both sides of the relationship
```python
class Parent(Base):
    __tablename__ = "parent_table"

    id: Mapped[int] = mapped_column(primary_key=True)
    child: Mapped["Child"] = relationship(back_populates="parent")


class Child(Base):
    __tablename__ = "child_table"

    id: Mapped[int] = mapped_column(primary_key=True)
    parent_id: Mapped[int] = mapped_column(ForeignKey("parent_table.id"))
    parent: Mapped["Parent"] = relationship(back_populates="child")
```
### One to Many: One Parent, Many Children
A one to many relationship places the foreign key in the child and a collection of children in the parent. This means that one parent can have many children.
#### Unidirectional One-to-Many: Parent --> Child
**We can access the children through the parent, but not the parent through the children**
```python
class Parent(Base):
    __tablename__ = "parent_table"

    id: Mapped[int] = mapped_column(primary_key=True)
    children: Mapped[List["Child"]] = relationship()


class Child(Base):
    __tablename__ = "child_table"

    id: Mapped[int] = mapped_column(primary_key=True)
    parent_id: Mapped[int] = mapped_column(ForeignKey("parent_table.id"))
```
#### Bidirectional One-to-Many: Parent <--> Child
**We can access the children through the parent, and the parent from the children***
```python
class Parent(Base):
    __tablename__ = "parent_table"

    id: Mapped[int] = mapped_column(primary_key=True)
    children: Mapped[List["Child"]] = relationship(back_populates="parent")


class Child(Base):
    __tablename__ = "child_table"

    id: Mapped[int] = mapped_column(primary_key=True)
    parent_id: Mapped[int] = mapped_column(ForeignKey("parent_table.id"))
    parent: Mapped["Parent"] = relationship(back_populates="children")
```
### Many to One: Many Parents, One Child
A many to one relationship puts the foreign key in the parent table referencing the child. This means that many different parents can have the same child. 
#### Unidirectional Many to One: Parent --> Child
```python
class Parent(Base):
    __tablename__ = "parent_table"

    id: Mapped[int] = mapped_column(primary_key=True)
    child_id: Mapped[int] = mapped_column(ForeignKey("child_table.id"))
    child: Mapped["Child"] = relationship()


class Child(Base):
    __tablename__ = "child_table"

    id: Mapped[int] = mapped_column(primary_key=True)
```
#### Bidirectional Many to One: Parent <--> Child
```python
class Parent(Base):
    __tablename__ = "parent_table"

    id: Mapped[int] = mapped_column(primary_key=True)
    child_id: Mapped[int] = mapped_column(ForeignKey("child_table.id"))
    child: Mapped["Child"] = relationship(back_populates="parents")


class Child(Base):
    __tablename__ = "child_table"

    id: Mapped[int] = mapped_column(primary_key=True)
    parents: Mapped[List["Parent"]] = relationship(back_populates="child")
```
#### Nullable Many to One
Imagine we have an `employee` and `task` table. Many `employee` entries can be assigned to a single `task`, but a `task` may be unassigned, not having any employees. To represent this we need a **nullable many-to-one** table.
```python
from typing import Optional

class Parent(Base):
    __tablename__ = "parent_table"

    id: Mapped[int] = mapped_column(primary_key=True)
    child_id: Mapped[Optional[int]] = mapped_column(ForeignKey("child_table.id"))
    child: Mapped[Optional["Child"]] = relationship(back_populates="parents")


class Child(Base):
    __tablename__ = "child_table"

    id: Mapped[int] = mapped_column(primary_key=True)
    parents: Mapped[List["Parent"]] = relationship(back_populates="child")
```

### Many to Many: Many Parents, Many Children 
Many to many requires the use of an additional "association table"
- Can be set up unidirectionally or bidirectionally 
- Learn more about association/join tables at [[SQL]]
#### Unidirectional Many-to-Many: Parent --> Child
Only the parent contains a collection, while the child remains a single record. We can access the child from the parent, but not vice-versa

It is recommended to have both columns as primary keys, to prevent duplicate relationships.

```python
association_table = Table(
    "association_table",
    Base.metadata,
    Column("left_id", ForeignKey("left_table.id"), primary_key=True), 
    Column("right_id", ForeignKey("right_table.id"), primary_key=True),
)

class Parent(Base):
    __tablename__ = "left_table"

    id: Mapped[int] = mapped_column(primary_key=True)
    
    children: Mapped[List[Child]] = relationship(secondary=association_table)

class Child(Base):
    __tablename__ = "right_table"

    id: Mapped[int] = mapped_column(primary_key=True)
```
#### Bi-Directional Many-to-Many: Parent <--> Child
Both the parent and the child contain a collection. We can access both the parent through the child, and the child through the parent. This is done by using the `back_populates` parameter in `relationship`. Other Syntax is similar to unidirectional.

```python
association_table = Table(
    "association_table",
    Base.metadata,
    Column("left_id", ForeignKey("left_table.id"), primary_key=True),
    Column("right_id", ForeignKey("right_table.id"), primary_key=True),
)

class Parent(Base):
    __tablename__ = "left_table"

    id: Mapped[int] = mapped_column(primary_key=True)
    children: Mapped[List[Child]] = relationship(
        secondary=association_table, back_populates="parents"
    )

class Child(Base):
    __tablename__ = "right_table"

    id: Mapped[int] = mapped_column(primary_key=True)
    parents: Mapped[List[Parent]] = relationship(
        secondary=association_table, back_populates="children"
    )
```
### The Association Object
Sometimes we need to store additional information about a relationship. What time did the student sign up for a class? How many times has a client visited a specific office? 

Instead of using a simple table, we instead use a distinct ORM mapped class. By writing the association as an ORM object we can give it additional fields. We then end up with three ORM objects: Parent, Child, and Association.  

When working with association patterns, we must explicitly create the association object. Child objects must also be associated with their association instance before being appended to the parent:
```python
# Create a Parent and an Association object with extra data
p = Parent()
a = Association(extra_data="some data")
a.child = Child()  # Link the child through the association object
p.children.append(a)
```

Similarly, to access the child through the parent we must go through the association object:
```python
# iterate through child objects via association, printing attributes
for assoc in p.children:
    print(assoc.child, assoc.extra_data)
```

**TLDR:** We can access the parents directly from the children, but must go through an association object to access the children from the parents. 
##### Unidirectional Association
```python
class Association(Base):
    __tablename__ = "association_table"
    left_id: Mapped[int] = mapped_column(
	    ForeignKey("left_table.id"), primary_key=True)
    right_id: Mapped[int] = mapped_column(
        ForeignKey("right_table.id"), primary_key=True
    )
	child: Mapped["Child"] = relationship()
    extra_data: Mapped[Optional[str]]


class Parent(Base):
    __tablename__ = "left_table"
    id: Mapped[int] = mapped_column(primary_key=True)
    children: Mapped[List["Association"]] = relationship()


class Child(Base):
    __tablename__ = "right_table"
    id: Mapped[int] = mapped_column(primary_key=True)
```
##### Bidirectional Association
```python
class Association(Base):
    __tablename__ = "association_table"
    left_id: Mapped[int] = mapped_column(
	    ForeignKey("left_table.id"), primary_key=True)
    right_id: Mapped[int] = mapped_column(
        ForeignKey("right_table.id"), primary_key=True
    )
    extra_data: Mapped[Optional[str]]
    child: Mapped["Child"] = relationship(back_populates="parents")
    parent: Mapped["Parent"] = relationship(back_populates="children")


class Parent(Base):
    __tablename__ = "left_table"
    id: Mapped[int] = mapped_column(primary_key=True)
    children: Mapped[List["Association"]] = relationship(back_populates="parent")


class Child(Base):
    __tablename__ = "right_table"
    id: Mapped[int] = mapped_column(primary_key=True)
    parents: Mapped[List["Association"]] = relationship(back_populates="child")
```
### Handling Multiple Join Paths
It's common for one table to have multiple foreign keys in another table. For example, imagine a `user` table that contains both a `billing_address` and a `shipping_address`. Both `billing address` and `shipping_address` reference the `address` table. 

This requires the additional `relationship()` param `foreign_keys`, a list containing the keys that should be considered "foreign" in this relationship. Foreign keys listed in other join paths will not be considered when creating this relationship.
```python
class Customer(Base):
    __tablename__ = "customer"
    id = mapped_column(Integer, primary_key=True)
    name = mapped_column(String)

    billing_address_id = mapped_column(Integer, ForeignKey("address.id"))
    shipping_address_id = mapped_column(Integer, ForeignKey("address.id"))

    billing_address = relationship("Address", foreign_keys=[billing_address_id])
    shipping_address = relationship("Address", foreign_keys=[shipping_address_id])
```
## Querying the Database
Queries are formed using the **select()** function and specifying a table and constraint. 
- `stmt = select(User).where(User.name == 'spongebob)`
This statement is then passed the **execute()** method, which returns a **result** object
- `result = session.execute(stmt)`
We can combine these two into a single statement:
- `result = session.execute(select(User).where(User.name == "spongebob"))`

A **result** returns a list of row objects. In this case, there is a single "User" element per row
```python
result = session.execute(select(User).order_by(User.id))
result.all() =
[(User(id=1, name='spongebob', fullname='Spongebob Squarepants'),),
 (User(id=2, name='sandy', fullname='Sandy Cheeks'),),
 (User(id=3, name='patrick', fullname='Patrick Star'),),
 (User(id=4, name='squidward', fullname='Squidward Tentacles'),),
 (User(id=5, name='ehkrabs', fullname='Eugene H. Krabs'),)]
```
If we want to receive a list of ORM entities (AKA Python Objects), we can use the **scalars()** function instead, which returns a list of single elements, rather than rows
```python
session.scalars(select(User).order_by(User.id)).all() = 
[User(id=1, name='spongebob', fullname='Spongebob Squarepants'),
 User(id=2, name='sandy', fullname='Sandy Cheeks'),
 User(id=3, name='patrick', fullname='Patrick Star'),
 User(id=4, name='squidward', fullname='Squidward Tentacles'),
 User(id=5, name='ehkrabs', fullname='Eugene H. Krabs')]
```
You can also use the **.scalars()** method to convert a Result to a Scalar Result:
- `scalar_user = Result.scalars()

### Using Select Statements
Select statements have two components:
- **FROM**: What table/columns are we searching
- **WHERE**: What are we searching (or not searching) for?
#### Setting Columns and FROM Clause
We first have to set which table(s) or column(s) our query will search. The parameter inside the **select()** statement specifies this. 

We can provide table column names directly:
- Selecting from table: `select(user_table)`
- Selecting from column (must use `.c` accessor): 
	- `select(user_table.c.name, user_table.c.fullname)`
	- `select(user_table.c.["name", "fullname"])`

We can also select ORM entities directly (this is more typical):
- `select(User)`
- `select(User.name, User.fullname)`

### Setting the WHERE Clause
Following the select statement we can specify constraints using the "where" clause and normal python expressions.
- `select(User).where(User.name == "spongebob")`

Can use AND conjunction by specifying where multiple times or giving multiple conditions:
```python
select(Address.email)
	.where(User.name == "spongebob")
	.where(Address.user_id == User.id)

select(Address.email).where(
	where(User.name == "spongebob")						
	where(Address.user_id == User.id)
)
```

Can use OR conjunction by specifying explicitly with the decorators **and_**() **or_**() :
```python
select(Address.email).where(
	and_(
		or_(User.name == "squidward", User.name == "spongebob"),
		Address.user_id == User.id,
	)
)
```

The simple **filter_by()** method allows for comparison against a single entity, and accepts keyword arguments matching ORM entities
```python
select(User).filter_by(name="spongebob")
```

## References
SQAlchemy Docs: https://docs.sqlalchemy.org/en/20/orm/quickstart.html
SQAlchemy YT tuts:
- https://www.youtube.com/watch?v=AKQ3XEDI9Mw&t=283s
- https://www.youtube.com/watch?v=aAy-B6KPld8&t=535s

