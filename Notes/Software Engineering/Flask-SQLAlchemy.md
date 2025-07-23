2024-10-02 14:44

Status:

Tags: #Flask #Python #PythonLibrary #SQL #Database 

# Flask-SQLAlchemy
Flask-SQLAlchemy is an extension for [[SQLAlchemy]] and [[Flask]]  to make working with database in Flask applications more convenient. These features include:
- Simplified configuration
- Automatically managed session and engine creation/cleanup
- Pre-configured Model base class

**Install:** `pip install flask-sqlalchemy`
## Configuration
Instead of creating the [database engine and session manually]([[SQLAlchemy#Creating the Database]]) we can use Flask-SQLAlchemy to add a new configuration variable to our `Config` class:
```python
basedir = os.path.abspath(os.path.dirname(__file__))

class Config:
    SQLALCHEMY_DATABASE_URI = os.environ.get(
        "DATABASE_URL"
    ) or "sqlite:///" + os.path.join(basedir, "app.db")
```
- Sets the db location to the environment variable (if set), and if that isn't defined, set it to the main directory of the application under `app.db`

We then create an instance of SQLAlchemy inside `__init__`:
- We also import our routes and models here so that the `app` and `db` instance has access
```python
from flask import Flask
from config import Config
from flask_sqlalchemy import SQLAlchemy

# Create a flask instance
app = Flask(__name__)
app.config.from_object(Config)
db = SQLAlchemy(app) # The SQLAlchemy instance

from app import routes, models
```

`db` is now our database instance and can be used to define models without `declareative_base()
```python
class User(db.Model):
	id = db.Column(db.Integer, primary_key=True)
```
## References
