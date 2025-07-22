2024-10-01 17:17

Status:

Tags: #Python #React #Flask #WebDev

# Python Flask with React

The Python [[Flask]] framework can be used as a backend [[CRUD API]] used to service a [[React]] frontend
## Setup
1) Create a "frontend" and "backend" folder
2) `npm create vite@latest my-react-app frontend -- --template react`
3) cd into frontend and `npm install` to install needed packages
4) cd into backend and install:
	- `pip3 install Flask`
	- `pip3 install Flask-SQLAlchemy`
	- `pip3 install flas-cors`

## Backend
1) Create three different Python files:
	- `config.py`: Configuration options and imports for the rest of our backend
	- `models.py`: Where we define the attributes of our [[SQLAlchemy]] database
	- `main.py`: Contains the endpoints and routes the API will take
2) Config.py:
```
# Flask handles endpoints
from flask import Flask 

# Import SQLAlchemy to map database tables to classes 
from flask_sqlalchemy import SQLAlchemy

# CORS allows us to accept requests from different domains (the frontend)
from flask_cors import CORS

# Create a Flask instance and enable CORS on it 
app = Flask(__name__)
CORS(app)

# Configure the app to use the database located at 'mydatabase.db'
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///mydatabase.db"

# Disable tracking modifications
app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = False 

# Create an instance of the SQLAlchemy class and bind it to the Flask app
# Can now interact with the database using ORM
db = SQLAlchemy(app)
```
- This config file sets up:
	- Initial [[Flask]] setup
	- [[SQLAlchemy]] to be used as an [[ORM]]
	- [[CORS]] allows for origin sharing between Flask and React (frontend and backend on two different servers)
3) Models.py: Create models according to [[SQLAlchemy#Models in Flask-SQLAlchemy|Flask-SQLAlchemy Models]]
Example: Creating "Contact" model:
```
from config import db

class Contact(db.model):
	id = db.Column(db.Integer, primary_key=True)
	first_name = db.Column(db.String(80), unique=False, nullable=False)
	last_name = db.Column(db.String(80), unique=False, nullable=False)
	email = db.Column(db.String(120), unique=True, nullable=False)
```
## References
