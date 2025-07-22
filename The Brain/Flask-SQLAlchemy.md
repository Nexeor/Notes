2024-10-02 14:44

Status:

Tags: #Flask #Python #PythonLibrary #SQL #Database 

# Flask-SQLAlchemy
Flask-SQLAlchemy is an extension for [[SQLAlchemy]] that changes the syntax to make it more suited to web development.

## Models in Flask-SQLAlchemy
**Model**: A representation of a table as a Pythonic class
- We then write [[CRUD]] methods to add records to the table. which we can access as instantiations of that class
- For each table, create a class to encapsulate its data
	- Each row (record) in the table will later be represented as an instance of this class) 
- Can define a name for the table: ```___tablename___ = "good name" ``` 
#### Column() Method
- Use the Column() method to define each attribute by assigning it to the table column of the same name
	- Can use a different name if an optional first argument is included specifying the desired column name
##### First Argument (Column data type):
| [`Integer`](https://docs.sqlalchemy.org/en/20/core/type_basics.html#sqlalchemy.types.Integer "(in SQLAlchemy v2.0)")         | **an integer**                                                                                                                                |
| ---------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| [`String(size)`](https://docs.sqlalchemy.org/en/20/core/type_basics.html#sqlalchemy.types.String "(in SQLAlchemy v2.0)")     | a string with a maximum length (optional in some databases, e.g. PostgreSQL)                                                                  |
| [`Text`](https://docs.sqlalchemy.org/en/20/core/type_basics.html#sqlalchemy.types.Text "(in SQLAlchemy v2.0)")               | some longer unicode text                                                                                                                      |
| [`DateTime`](https://docs.sqlalchemy.org/en/20/core/type_basics.html#sqlalchemy.types.DateTime "(in SQLAlchemy v2.0)")       | date and time expressed as Python [`datetime`](https://docs.python.org/3/library/datetime.html#datetime.datetime "(in Python v3.11)") object. |
| [`Float`](https://docs.sqlalchemy.org/en/20/core/type_basics.html#sqlalchemy.types.Float "(in SQLAlchemy v2.0)")             | stores floating point values                                                                                                                  |
| [`Boolean`](https://docs.sqlalchemy.org/en/20/core/type_basics.html#sqlalchemy.types.Boolean "(in SQLAlchemy v2.0)")         | stores a boolean value                                                                                                                        |
| [`PickleType`](https://docs.sqlalchemy.org/en/20/core/type_basics.html#sqlalchemy.types.PickleType "(in SQLAlchemy v2.0)")   | stores a pickled Python object                                                                                                                |
| [`LargeBinary`](https://docs.sqlalchemy.org/en/20/core/type_basics.html#sqlalchemy.types.LargeBinary "(in SQLAlchemy v2.0)") | stores large arbitrary binary data                                                                                                            |
#### Example: Creating a model from a table
| ID  | First Name | Last Name | Email        |
| --- | ---------- | --------- | ------------ |
| 01  | John       | Andrew    | 123@mail.com |
| 02  | Sally      | Burns     | 321@mail.com |

```
class User(db.Model):
	id = db.Column(db.Integer, primary_key=True)
	first_name = db.Column(db.String(80), unique=False, nullable=False)
	last_name = db.Column(db.String(80), unique=False, nullable=False)
	email = db.Column(db.String(120), unique=True, nullable=False)

	# Constructor method
	def __init__(self, id, first_name, last_name, email):
		self.id = id
		self.first_name = first_name
		self.last_name = last_name
		self.email = email
	
	# Define string representation
	def __repr__(self):
		return f"User is {self.first_name} {self.last_name} with id {self.id}"
```

We then use [[CRUD]] methods to add "John" and "Sally" to the table
- Can later access them as pythonic objects `x.first_name` or `x.email`

## References
