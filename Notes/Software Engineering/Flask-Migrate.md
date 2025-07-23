2025-06-03 15:50

Status: #Leaf

Tags: #Python #Flask #PythonLibrary #WebDev  #SQL #Database 

# Flask-Migrate
As our database changes and grows, we will need to *migrate* data between our new and old models. When structured data changes, the data already in that database must be changed to fit that new structure. Flask-Migrate is built on top of Alembic as a database migration tool on top of Flask-SQLAlchemy.

**Install:** `pip install flask-migrate`
## Configuration and Basic Use
Enable by creating a `Migrate` instance with the app and db:
```python
from flask import Flask
from config import Config
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate

app = Flask(__name__)
app.config.from_object(Config)
db = SQLAlchemy(app)
migrate = Migrate(app, db)

from app import routes, models
```

Then create a *migration repository*: a directory that stores records of all migrations
- Create: `flask db init`
- Acts like a Git repository, tracks the changes between versions of the database

Generate a migration: `flask db migrate -m "commit message"`
- `-m` flag adds a short descriptive message to the migration
- This makes a "commit" of the db in the local migration repository
- Doesn't actually modify the database, just generates a migration

Apply a migration: 
- Apply the most recent migrations: `flask db upgrade`
	- Will create the database if it doesn't exist
- Revert the last migration: `flask db downgrade`
## Workflow
Migration is a strategy used to keep the underlying SQL database consistent with the models defined in [[SQLAlchemy]]. 

For example, imagine we have an app on our dev machine, and a copy deployed onto a production server. In our next release, we add a new table to our database and modify some existing ones. This means our production database and our dev database are now different, and would require a custom migration to update the production database, interrupting the production app. 

However, with Flask-Migrate, after we update our dev copy we run `flask db migrate` to generate a new migration script. We then apply these changes to both the dev and production database with `flask db upgrade`. This allows us to easily keep both databases consistent

We can also use `flask db downgrade` to rollback to previous versions of a database. 
## References
