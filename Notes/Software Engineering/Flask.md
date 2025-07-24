2025-04-15 14:32

Status: #Trunk

Tags: #Python #PythonLibrary #WebDev #Flask 

# Flask
Follow tutorial-like format here: [[Flask Mega Tutorial Notes]]
## Getting Started
### Minimal Application
A minimal flask application can be built in as few as five lines:
```python
from flask import Flask

app = Flask(__name__) # Create an instance of the class

@app.route("/") # What URL of our server triggers this funtion
def hello_world(): # Run this code when we visit url '/'
    return "<p>Hello, World!</p>"
```

**Run built-in server using:** ```flask --app <.py_file> run```
- If the file is named "app.py", don't need the --app specifier
- We can set the environment variable with `export FLASK_APP=toplevel.py`
	- Can now run flask using `flask run` without specifying the `.py` file
	- `FLASK_APP` must be set to the topmost level of the app
### Optimal Setup
If we split responsibilities across multiple files we can create a more extensible design:
```
microblog/
  venv/
  app/
    __init__.py
    routes.py
  microblog.py
```

The `app` directory contains the logic, while `microblog` is a top-level access point for `flask run`
#### __init__.py
The original work of creating the `app` instance is done in `__init__.py`:
```python 
from flask import Flask

# Create a flask instance 
app = Flask(__name__)

from app import routes  
```
Whenever the `app` package is imported, the `__init__.py` file is run, ensuring that the `app` exists
1) Import the Flask object
2) Create an instance of Flask named `app`
3) Import the `routes` instance after creating `app` to avoid circular imports
	- The `routes` module imports `app` itself, so `app` must exist before we import `routes`
#### routes.py
Routing and view functions are in the `routes.py` file:
```python
from app import app

@app.route('/')
@app.route('/index')
def index():
    return "Hello World!"
```
Whenever the URI indicated with the `@app.route` decorator is visited, the associated view function is run, in this case the `index()` function.
#### microblog.py
This top level file starts the flask server by importing the `app` package. During import, the `__init__.py` file is run, creating the `app` instance and starting the server.
```python
from app import app
```
### Running the Server
**Run Server:** `flask --app microblog.py run`

We can set the *environment variables* so we can quick run with: `flask run`
- Set for single session: `export FLASK_APP=microblog.py`
- Set permanently with `dotenv` package:
	1) `pip install python-dotenv`
	2) Create a `.flaskenv` file 
	3) Inside `.flaskenv`, on a single line, set `FLASK_APP=microblog.py`

## Templates
Attempting to write HTML inside our view function quickly creates messy code:
```python
from app import app

@app.route('/')
@app.route('/index')
def index():
    user = {'username': 'Miguel'}
    return '''
<html>
    <head>
        <title>Home Page - Microblog</title>
    </head>
    <body>
        <h1>Hello, ''' + user['username'] + '''!</h1>
    </body>
</html>'''
```

**Templates** are HTML snippets stored in their own HTML file that we use to render pages
- Flask uses the Jinja2 template engine 

Templates must be stored in a `templates` directory for Flask to recognize them
- If a module, put `templates` at same level as `app.py`:
```
/application.py
/templates
    /hello.html
```
- If a package, put `templates` inside the package:
```
/application
    /__init__.py
    /templates
        /hello.html
```

A template for `/index`:
```html
<!doctype html>
<html>
    <head>
        <title>{{ title }} - Microblog</title>
    </head>
    <body>
        <h1>Hello, {{ user.username }}!</h1>
    </body>
</html>
```

Double braces {{ }} indicates sections that must be provided to the template in the `render_template` function. We can call `render_template` from within our now simplified view function, providing it the template to render and values to fill in. 
```python
def index():
    user = {'username': 'Miguel'}
    return render_template('index.html', title='Home', user=user)
```

Templates also support in-line CSS styling:
```html
<html>
    <head>
        <title>{{ title }} - Microblog</title>
    </head>
    <body>
        <h1 style="color: steelblue;">Hello, {{ user.username }}!</h1>
    </body>
</html>
```
### Conditional Statements
Jinja provides support for Python style conditionals within our templates. The HTML is only rendered if the condition is met. 

For example, we can set a default value for the page title if one isn't provided by modifying the `<head>` element:
```html
<head>
	{% if title %}
	<title>{{ title }} - Microblog</title>
	{% else %}
	<title>Welcome to Microblog!</title>
	{% endif %}
</head>
```
### Loops
Jinja provides support for Python-style loops within templates. Loops are used to repeat blocks of HTML with different content. 

For example, if we want to display a list of posts, we can write a single HTML block and use Jinja to `for` loop over the set of all posts:
```html
<body>
	<h1>Hi, {{ user.username }}!</h1>
	{% for post in posts %}
	<div><p>{{ post.author.username }} says: <b>{{ post.body }}</b></p></div>
	{% endfor %}
</body>
```
### Template Inheritance: Blocks and Extends
Most websites have repeated elements that are common across multiple URLs. We can avoid repeating HTML code across multiple files by *inheriting* from a base template. 
- Base template contains repeated code with sections where child templates will be inserted
- Child templates contain unique code and are inserted into the base template

In the base template:
- `{% block blockname %}`: The start of a block (named 'blockname') supplied by a child template 
- `{% endblock %}`: The end of the previously opened block
	- Any HTML between the start and end of the block is *default content* and only rendered if a child template is not provided to fill the block

In the child template:
- `{% extends "base.html" %}`: The base template we are filling in
- `{% block content %}`: The start of a block (name 'blockname')
	- A base template can contain multiple blocks. The blockname is needed to insert the correct HTML into the correct blocks 
- `{% endblock %}`: The end of the previously opened block
- HTML between "block" and "endblock" is the content tha't will be inserted into the base-template in between the matching "block" and "endblock"

Websites often have a "navbar" that is shared across pages. We can implement this in our *base* template, and then write each page as a *child* template of that template.

**Base:**
```html
<!doctype html>
<html>
    <head>
      {% if title %}
      <title>{{ title }} - Microblog</title>
      {% else %}
      <title>Welcome to Microblog</title>
      {% endif %}
    </head>
    <body>
        <div>Microblog: <a href="/index">Home</a></div>
        <hr>
        {% block content %}{% endblock %}
    </body>
</html>
```

**Child:** Previously "index" 
```html
{% extends "base.html" %}

{% block content %}
    <h1>Hi, {{ user.username }}!</h1>
    {% for post in posts %}
    <div><p>{{ post.author.username }} says: <b>{{ post.body }}</b></p></div>
    {% endfor %}
{% endblock %}
```

Additional pages can be created as children of this base template, and thus will all be changed when we make modifications.

### Template Inheritance: Include
Include is used for HTML fragments or individual components. It lets us reuse individual components, like a header, footer, or sidebar. 
- In the child template: Put the HTML fragment
- In the parent template: Put `{% include '<fragment.html> %}`
- Include can be used alongside blocks and extends

For example, if we have a 'Post' component, we can make a sub-template for it:
```html
<table>
    <tr valign="top">
        <td><img src="{{ post.author.avatar(36) }}"></td>
        <td>{{ post.author.username }} says: <b>{{ post.body}}</b></td>
    </tr>
</table>
```

And reuse that post wherever we want. For example, in the profile page we render each post using the same sub-template:
```html
{% extends "base.html" %}

{% block content %}
    <table>
        <tr valign="top">
            <td><img src="{{ user.avatar(128) }}"></td>
            <td><h1>User: {{ user.username }}</h1></td>
        </tr>
    </table>
    <hr>
    {% for post in posts %}
    <p>
        {% include '_post.html' %}      
    </p>
    {% endfor %}
{% endblock %}
```

## Configuration and Extensions
Flask is intentionally not opinionated about many things. This allows the developer to fill in the gaps with what works best for them. You can do this through **extensions** and **configuration**:
- **Extensions** are downloadable packages that add more features to Flask
	- Extensions are installed using `pip`: `pip install flask-wtf`
- **Configuration** are options we can set to modify Flask's behavior
	- **SECRET_KEY** is an important configuration variable that is used by Flask and its extensions as a cryptographic key for generating signatures/tokens.

Configuration options can be set as keys in `app.config`:
```python
app = Flask(__name__)
app.config['SECRET_KEY'] = 'the-secret-key'
# ... more configuration variables can be set as needed
```
We can also access those keys and read their values:
```python
>>> from microblog import app
>>> app.config['SECRET_KEY']
'you-will-never-guess'
```

While setting config options manually is ok, we can use a `config` object to better enforce separation of concerns
- **OR EXPRESSION:** If `SECRET_KEY` is defined in the environment, use that. Otherwise use the hardcoded string.
```python
import os

class Config:
    SECRET_KEY = os.environ.get('SECRET_KEY') or 'you-will-never-guess'
    # ... more configuration variables can be set as needed
```

We then tell the Flask app to use the Config object:
```python
from flask import Flask
from config import Config

app = Flask(__name__)
app.config.from_object(Config)

from app import routes
```
## Before Request Decorator
Sometimes we want to execute a bit of generic code before each view function. Rather than putting a call to this code in every view function, we can instead use the `before_request` decorator. 

`@app.before_request`: Execute this method right before every view function

For example, we can use `@app.before_request` to track the last time an authenticated user visited the site:
```python 
from datetime import datetime, timezone

@app.before_request
def before_request():
	if current_user.is_authenticated:
		current_user.last_seen = datetime.now(timezone.utc)
		db.session.commit()
```

## Dynamic Endpoints
We can have the same view function display different content by using *dynamic endpoints*. These view functions allow us to pass data as part of the URL, essentially letting us use one view function for many different endpoints. 

Use angle brackets `<>` to define a dynamic endpoint: `@app.route('/user/<username>)``
- If we visit `/user/tim`, Flask passes `tim` as the `username` argument
- Can also visit `/user/melis`, Flask passes `melis` as the `username` argument

Typing can be enforced as well: `@app.route('/post/<int:post_id>')`
- Common type converters include: `string`, `int`, `float`, `path`, and `uuid`
- If the wrong type is provided, Flask returns a `404 Not Found` error, as the URL is treated as invalid
## Extension: [[Flask-WTForms]]
We can utilize the WTForms extension to implement forms inside our templates. If we define our form as a WTForm class, we can provide an instance of that class to our view function and utilize a template to display the form to the user. WTForms takes care of converting form objects into HTML to be inserted into the template.
### Form Template
Once we have a [form class for our login page](obsidian://open?vault=The%20Hivemind&file=Flask-WTForms), we must create a Flask template that displays that handles converting the Python code into an HTML page:
```html
{% extends "base.html" %}

{% block content %}
    <h1>Sign In</h1>
    <form action="" method="post" novalidate>
        {{ form.hidden_tag() }}
        <p>
            {{ form.username.label }} 
            <br>
            {{ form.username(size=32) }}
            <br>
            {% for error in form.username.errors %}
            <span style="color: red;">[{{ error }}]</span>
            {% endfor %}
        </p>
        <p>
            {{ form.password.label }}
            <br>
            {{ form.password(size=32) }}
            {% for error in form.password.errors %}
            <span style="color: red;">[{{ error }}]</span>
            {% endfor %}
        </p>
        <p>{{ form.remember_me() }} {{form.remember_me.label }} </p>
        <p>{{ form.submit() }}</p>
    </form>
{% endblock %}
```
- `form` is passed in as an object from the view function
- The `<form>` element is used as a container for the form attributes
	- `action`: What URL to submit the form to
	- `method`: What HTTP request to use when submitting the form
	- `novalidate`: Tell web browser not to apply validation
		- Used when we validate data server-side rather than within the browser
- `form.hidden_tag()` generates a hidden token to protect against CSRF attacks
- `form.<field_name>.label` is the label string, but `form.<field_name>()` is the actual input field
- If the user submits illegal data, those errors are store in `form.<field_name>.errors`
	- We use a `for` loop to display each of these errors below the field
### View Function
We have to setup a view function for the login template:
```python
from app.forms import LoginForm

...

@app.route("/login", methods=["GET", "POST"])
def login():
    form = LoginForm()

    if form.validate_on_submit():
        flash(
            "Login requested for user {}, remember_me={}".format(
                form.username.data, form.remember_me.data
            )
        )
        return redirect(url_for("index"))

    return render_template("login.html", title="Sign In", form=form)
```
- The `methods` attribute of the route decorator sets what HTTP methods can be sent to this endpoint. By default only GET requests are allowed
	- Our template sends a POST request, so we must specify that here to prevent a `METHOD_NOT_ALLOWED` error
- `validate_on_submit()` gathers the data, runs each validator, and returns `true` if the submitted data is valid
- `flash()` shows a message to the user (in this case using data from the submitted form)
	- `flash` stores the message, but it still needs to be displayed in the template (see [[#Updating Base Template]] below)
	- Flash messages are displayed once and then deleted
- `redirect()` automatically navigates the user to a certain page (in this case the home page)
### Sidenote: url_for()
As the scope of our project grows we will need many more pages. We can use hardcoded URL's to navigate, but if these URL's ever change we will need to rename the symbol across our entire project. For example:
1) We access our login page at the `/login` endpoint
2) The `/login` endpoint is defined by the `login` view function
3) At some point, the `/login` endpoint becomes `/users/login`, but is still defined by the same view function
4) Because of the endpoint change, all hardcoded URL's will break, despite the endpoint being handled the exact same way!

`url_for()` is a Flask function that generates the URL for a given view function given its name
1) The `/login` endpoint becomes `/users/login`, but is still defined by the same view function
2) Our links are defined using `url_for('login')`, so all links automatically update!
- NOTE: If we change the name of the view function, the links WILL break. So don't change view function names once they're chosen!

### Updating Base Template
```html
<!DOCTYPE html> 
<html>
    <head>
        {% if title %}
        <title>{{ title }} - Microblog</title>
        {% else %}
        <title>Welcome to Microblog!</title>
        {% endif %}
    </head>
    <body>
        <div>
            Microblog: 
            <a href="{{ url_for('index')}}">Home</a>
            <a href="{{ url_for('login')}}">Login</a>
        </div>
        <hr>
        {% with messages = get_flashed_messages() %}
        {% if messages %}
        <ul>
            {% for message in messages %}
            <li>{{ message }}</li>
            {% endfor %}
        </ul>
        {% endif %}
        {% endwith %}
        {% block content %}{% endblock %}
    </body>
</html>
```
The `<head>` is the same, but the `<body>` has been almost entirely overhauled:
- `href` links now generate the url using `url_for('<view_function_name>')` to avoid using hardcoded links
	- See [[#Sidenote url_for()]] for why this is important
- We use the `with` construct to loop over the messages generated by `flash` in the [[#View Function]] and display them as line items
	- `get_flashed_messages()` is a Flask method for templates to retrieve unwritten flash methods
## Extension: [[Flask-Migrate]]

## Shell Context
The Python shell interpreter is a common tool when testing our Flask application. However, we have to use several verbose imports to get into the app context:
```python
>>> from app import app, db
>>> from app.models import User, Post
>>> import sqlalchemy as sa
>>> app.app_context().push()
```

We can shortcut this with the command `flask shell`, which starts the interpreter in the context of our application:
```python
(venv) $ flask shell
>>> app
<Flask 'app'>
```

This essentially "pre-imports" the `app` instance. We can do the same with other symbols by defining a *shell_context* function within `micro-blog.py`:
```python
import sqlalchemy as sa
import sqlalchemy.orm as so
from app import app, db
from app.models import User, Post

@app.shell_context_processor
def make_shell_context():
    return {'sa': sa, 'so': so, 'db': db, 'User': User, 'Post': Post}
```
- The `@app.shell_context_processor` decorator marks this function as a *shell context function*, which is invoked whenever we run the `flask shell` command
- Each entry in the returned dict is an item that will be registered and accessible to the shell session

Now when we run `flask shell` we can access the objects (db, User, Post, etc.) created in the shell context function:
```python
(venv) $ flask shell
>>> db
<SQLAlchemy sqlite:////home/miguel/microblog/app.db>
>>> User
<class 'app.models.User'>
>>> Post
<class 'app.models.Post'
```
## References
