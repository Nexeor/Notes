2025-06-09 18:57

Status: #Leaf

Tags: #Flask #Flask-Extension #WebDev #Python #PythonModule 

# Flask-Login

## Setup
To use, install with `pip install flask-login` and create an instance in `__init__` func:
```python
from flask_login import LoginManager

app = Flask(__name__)
login = LoginManager(app)
```

We use the module by adding certain properties to out `user` model. The model can be based on any database system as long as it has the following Python properties:
- `is_authenticated():` True if user has valid credentials, False otherwise
- `is_active()`: `True` if account is active, `False` otherwise
- `is_anonymous():` `False` for regular users, `True` for for special anonymous users
- `get_id():` Method that returns a unique identifier for this user as a string

These properties are fairly generic and can be added quickly using the provided `UserMixin` class:
```python
# ...
from flask_login import UserMixin

class User(UserMixin, db.Model):
    # Additional properties/methods here...
```
## Using Flask-Login
Flask-Login tracks the logged in user by keeping its unique identifier (retrieved from `get_id()`) stored in the *user session*, a storage space assigned to each user who connects. Each time the user navigates the a new page, Flask-Login checks the user session and loads the stored user into memory. 

Flask-Login isn't opinionated about our database, so we need to provide it with a method to load the requested user from the database. We can do this by writing a method that returns the User instance and prefixing it with the `@login.user_loader` decorator:
- Flask-Login passes the id as a `string`, so we convert it to an `int` first
```python
from app import login
# ...

@login.user_loader
def load_user(id):
    return db.session.get(User, int(id))
```

`current_user`: variable representing the client making the request
- `load_user` tells Flask-Login how to acess the underlying database model

**Properties**:
- `is_authenticated`: Bool, whether user is logged in or not
	- If the user is not logged in yet, `current_user` will instead be a special *anonymous* user

**Methods:** 
- `login_user(user, remember)`: Logs in the provided user
	- `user`: The user object to load in
	- `remember`: `bool`, remember the user after the session expired
		- Default `false`
- `logout_user()`: Log out the currently logged in user
### Required Login
We can use Flask-Login to require users to login before they can view certain pages. If the user attempts to view a protected page while not logged in, Flask-Login will automatically redirect them to the login form instead. 

First we tell Flask-Login which view function handles logins inside our `__init__` module:
```Python
login = LoginManager(app)
login.login_view = 'login'
```

Now we can specify protected functions with the `@login_required` decorator:
```python
from flask_login import login_required

@app.route('/')
@app.route('/index')
@login_required
def index():
    # ...
```

When the anonymous user attempts to access a protected page they are redirected to login, but the `next` query string argument in the URL is set to the original URL. This way we can redirect the user back to their original destination after they login.
 
We can redirect to the `next` argument like so:
- `if not next_page` redirects to `index` if no `next` is given
- `urlsplit(next_page).netloc != ''` redirects to `index` if `next` is set to a full URL instead of a relative URL
	- A full URL can be used to redirect to an external site
	- Allowing relative URLs only means we only redirect using `next` within the application
```python
next_page = request.args.get('next')
if not next_page or urlsplit(next_page).netloc != '':
	next_page = url_for('index')
```
## References
