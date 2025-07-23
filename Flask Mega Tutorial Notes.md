2025-06-09 18:19

Status: #Branch

Tags: #WebDev #Flask #Python 

# Flask Mega Tutorial

## Chapter 5: User Logins
In this chapter we will:
- Setup password hashing to store passwords in the database
- Setup the [[Flask-Login]] extension
- Finish the `login` view function
- Require users to be logged in to view certain pages
- Write a `logout` and `register` view function and corresponding templates
### Password Hashing
To support user logins properly we must implement [[Hashing#Password Hashing|password hashing]]. We can do this in Python using the [[Werkzeug]] library, which contains a method for generating and verifying password hashes.  

We can implement the password verification as two new methods within the *user* model:
```python
from werkzeug.security import generate_password_hash, check_password_hash

# ...

class User(db.Model):
    # ...

    def set_password(self, password):
        self.password_hash = generate_password_hash(password)

    def check_password(self, password):
        return check_password_hash(self.password_hash, password)
```
### Flask-Login
We're going to use the [[Flask-Login]] extension to manage the user's logged-in state. After downloading the extension, create an instance of it within ``__init__.py``:
```python
# ...
from flask_login import LoginManager

app = Flask(__name__)
# ...
login = LoginManager(app)
# ...
```

We then add the `UserMixin` to our `User` model to give it the required properties quickly:
```python
# ...
from flask_login import UserMixin

class User(UserMixin, db.Model):
    # ...
```

We also add the following loader method to `app/models.py` so Flask-Login knows how to load the User model from our database:
```python
from app import login
# ...

@login.user_loader
def load_user(id):
    return db.session.get(User, int(id))
```
### Login Function
Now that Flask-Login is active and we have a load_user function, we can use insert the login functionality into our `login` view function
- The `current_user` variable is provided by [[Flask-Login]] as a way of accessing the user object that represents the client of the request
```python
# ...
from flask_login import current_user, login_user
import sqlalchemy as sa
from app import db
from app.models import User

# ...

@app.route('/login', methods=['GET', 'POST'])
def login():
	# Redirect to home if already authenticated
    if current_user.is_authenticated:
        return redirect(url_for('index'))
    form = LoginForm()
    if form.validate_on_submit():
	    # Query database for submitted username
        user = db.session.scalar(
            sa.select(User).where(User.username == form.username.data))
        # Reload login if user DNE or password wrong
        if user is None or not user.check_password(form.password.data):
            flash('Invalid username or password')
            return redirect(url_for('login'))
        # Password correct, login user and redirect to home
        login_user(user, remember=form.remember_me.data)
        return redirect(url_for('index'))
    return render_template('login.html', title='Sign In', form=form)
```
### Logout Function
We can write a seperate `logout` route in `app/routes`:
```python
# ...
from flask_login import logout_user

# ...

@app.route('/logout')
def logout():
    logout_user()
    return redirect(url_for('index'))
```

We expose this link to the users by dynamically switching the "login" link to a "logout" link in the template (`app/templates/base.html`)
- The `user.is_anonymous` property is `true` when no user is logged in
```html
<div>
	Microblog:
	<a href="{{ url_for('index') }}">Home</a>
	{% if current_user.is_anonymous %}
	<a href="{{ url_for('login') }}">Login</a>
	{% else %}
	<a href="{{ url_for('logout') }}">Logout</a>
	{% endif %}
</div>
```
### Requiring Users to Login
We're gonna user Flask-Login to require the user to be logged in before accessing certain pages. 

First tell Flask-Login what view function handles logins in `app/__init__.py`
```python
# ...
login = LoginManager(app)
login.login_view = 'login'
```

We want the user to be logged in before accessing `/index`, so add the `@login_required` decorator:
```python
from flask_login import login_required

@app.route('/')
@app.route('/index')
@login_required
def index():
    # ...
```

Then modify the final four lines of the `/login` route to redirect to the previously requested page:
```python
from flask import request
from urllib.parse import urlsplit

@app.route('/login', methods=['GET', 'POST'])
def login():
    # ...
    if form.validate_on_submit():
        user = db.session.scalar(
            sa.select(User).where(User.username == form.username.data))
        if user is None or not user.check_password(form.password.data):
            flash('Invalid username or password')
            return redirect(url_for('login'))
        login_user(user, remember=form.remember_me.data)

		# Get the next_page URL from requests
        next_page = request.args.get('next')
        # Redirect to index if no next is provided, or not relative URL
        if not next_page or urlsplit(next_page).netloc != '':
            next_page = url_for('index')
        # Redirect to the original destination
        return redirect(next_page)
    # ...
```
### Templates for Logged-In User
Now that we have real users and they can log in, we have to change the templates to show the logged in user's data.

In the `index.html` template we can pass the current user instead:
```html
{% extends "base.html" %}

{% block content %}
    <h1>Hi, {{ current_user.username }}!</h1>
    {% for post in posts %}
    <div><p>{{ post.author.username }} says: <b>{{ post.body }}</b></p></div>
    {% endfor %}
{% endblock %}
```

And remove the `user` template argument in the view function for `index`, which is now handled by the `current_user` attribute from Flask-Login
```python
@app.route('/')
@app.route('/index')
@login_required
def index():
    # ...
    return render_template("index.html", title='Home Page', posts=posts)
```
### Registration Form
Now we have to build out a registration form for our users. We're going to use [[Flask-WTForms]]
again.
- The `email` field uses the `Email()` validator, which ensures the submitted data matches the structure of an email address
	- Install validator: `pip install email-validator`
- The `password2` field uses the `EqualTo` validator to ensure the password was typed correctly
- `validate_username()` and `validate_email()` are custom validators that check the database and make sure the username/email isn't already present
```python
from flask_wtf import FlaskForm
from wtforms import StringField, PasswordField, BooleanField, SubmitField
from wtforms.validators import ValidationError, DataRequired, Email, EqualTo
import sqlalchemy as sa
from app import db
from app.models import User

# ...

class RegistrationForm(FlaskForm):
    username = StringField('Username', validators=[DataRequired()])
    email = StringField('Email', validators=[DataRequired(), Email()])
    password = PasswordField('Password', validators=[DataRequired()])
    password2 = PasswordField(
        'Repeat Password', validators=[DataRequired(), EqualTo('password')])
    submit = SubmitField('Register')

    def validate_username(self, username):
        user = db.session.scalar(sa.select(User).where(
            User.username == username.data))
        if user is not None:
            raise ValidationError('Please use a different username.')

    def validate_email(self, email):
        user = db.session.scalar(sa.select(User).where(
            User.email == email.data))
        if user is not None:
            raise ValidationError('Please use a different email address.')
```

We also need an HTML template for the registration form:
```html
{% extends "base.html" %}

{% block content %}
    <h1>Register</h1>
    <form action="" method="post">
        {{ form.hidden_tag() }}
        <p>
            {{ form.username.label }}<br>
            {{ form.username(size=32) }}<br>
            {% for error in form.username.errors %}
            <span style="color: red;">[{{ error }}]</span>
            {% endfor %}
        </p>
        <p>
            {{ form.email.label }}<br>
            {{ form.email(size=64) }}<br>
            {% for error in form.email.errors %}
            <span style="color: red;">[{{ error }}]</span>
            {% endfor %}
        </p>
        <p>
            {{ form.password.label }}<br>
            {{ form.password(size=32) }}<br>
            {% for error in form.password.errors %}
            <span style="color: red;">[{{ error }}]</span>
            {% endfor %}
        </p>
        <p>
            {{ form.password2.label }}<br>
            {{ form.password2(size=32) }}<br>
            {% for error in form.password2.errors %}
            <span style="color: red;">[{{ error }}]</span>
            {% endfor %}
        </p>
        <p>{{ form.submit() }}</p>
    </form>
```

Lets also add a link to the registration form from the login form, `app/templates/login.html`
```html
    <p>New User? <a href="{{ url_for('register') }}">Click to Register!</a></p>
```

And finally add a view function for `/register` that displays the template
```python
rom app import db
from app.forms import RegistrationForm

# ...

@app.route('/register', methods=['GET', 'POST'])
def register():
	# Redirect to home if already logged in
    if current_user.is_authenticated:
        return redirect(url_for('index'))
    form = RegistrationForm()
    if form.validate_on_submit():
	    # Create a new User and commit it to the database
        user = User(username=form.username.data, email=form.email.data)
        user.set_password(form.password.data)
        db.session.add(user)
        db.session.commit()
        flash('Congratulations, you are now a registered user!')
        return redirect(url_for('login'))
    return render_template('register.html', title='Register', form=form)
```
## Chapter 6: Profile Page and Avatars
### Profile Page: Setup
Unlike our previous pages, the user profile page will be unique to the client who visits it. As such, we want a unique endpoint for each user. We can accomplish this using Flasks's [[Flask#Dynamic Endpoints|dynamic endpoints]] feature, passing the username as an argument.

Here is a simple `user` endpoint using a dummy list of posts
```python
@app.route('/user/<username>')
@login_required
def user(username):
	# Get user, otherwise throw 404 Not Found
    user = db.first_or_404(sa.select(User).where(User.username == username))
    posts = [
        {'author': user, 'body': 'Test post #1'},
        {'author': user, 'body': 'Test post #2'}
    ]
    return render_template('user.html', user=user, posts=posts)
```

Template: `app/templates/user.html`
```html
{% extends "base.html" %}

{% block content %}
    <h1>User: {{ user.username }}</h1>
    <hr>
    {% for post in posts %}
    <p>
    {{ post.author.username }} says: <b>{{ post.body }}</b>
    </p>
    {% endfor %}
{% endblock %}
```

Add a link to the user profile in the base template:
```html
	<div>
		Microblog:
		<a href="{{ url_for('index') }}">Home</a>
		{% if current_user.is_anonymous %}
		<a href="{{ url_for('login') }}">Login</a>
		{% else %}
		<!-- Use currently logged in user to build url -->
		<a href="{{ url_for('user', username=current_user.username) }}">Profile</a>
		<a href="{{ url_for('logout') }}">Logout</a>
		{% endif %}
	</div>
```
### Avatars
We will use [[Gravatar]] to provide avatar images. Gravatar will handle storing and hosting the images, we only have to provide the gravatar link to our templates to render the image. 

Lets write a method in the `User` class (`app/models`) to fetch any users avatar at the requested size
- `digest` is the MD5 hash of the users email as a hexadecimal string
- All other parts of the app fetch the avatar from here. So if we want to change how we store the avatar, we simply update this function and the rest of the code will still work
```python
from hashlib import md5
# ...

class User(UserMixin, db.Model):
    # ...
    def avatar(self, size):
        digest = md5(self.email.lower().encode('utf-8')).hexdigest()
        return f'https://www.gravatar.com/avatar/{digest}?d=identicon&s={size}'
```

Now modify the `user.html` template (`app/templates/user.html`) to display the images:
- Large avatar at the top next to the username
- Small avatars next to the posts
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
        <table>
            <tr valign="top">
                <td><img src="{{ post.author.avatar(36) }}"></td>
                <td>{{ post.author.username }} says: <b>{{ post.body}}</b></td>
            </tr>
        </table>
    </p>
    {% endfor %}
{% endblock %}
```
### Post Template Using Jinja Sub-Templates
If we want to display posts in multiple places (like on the profile and home page) we need to make a *sub-template* to keep the display consistent across pages.

Modeling the sib-template after the profile page (`app/templates/user.html`):
```html
    <table>
        <tr valign="top">
            <td><img src="{{ post.author.avatar(36) }}"></td>
            <td>{{ post.author.username }} says:<br>{{ post.body }}</td>
        </tr>
    </table
```

Now replace the `<table>` in `user.html` template with the sub-template:
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
        {% include '_post.html' %}
    {% endfor %}
{% endblock %}
```

Later, when we update the `index` template, we can use this sub-template instead to keep posts consistent between the profile and the index page.

### Profile Page: User Customization 
Lets add a few fields to the profile page for the user to customize:
- `about_me`: A user written bio
- `last_seen`: When the user last accessed the site

First update the database `User` model (`app/models/py`) to support the new attributes:
- Remember to use [[Flask-Migrate]] to update the underlying database
```python
class User(UserMixin, db.Model):
    # ...
    about_me: so.Mapped[Optional[str]] = so.mapped_column(sa.String(140))
    last_seen: so.Mapped[Optional[datetime]] = so.mapped_column(
        default=lambda: datetime.now(timezone.utc))
```

Then we modify the `user` template to show this new information if it is set:
```html
{% extends "base.html" %}

{% block content %}
    <table>
        <tr valign="top">
            <td><img src="{{ user.avatar(128) }}"></td>
            <td>
                <h1>User: {{ user.username }}</h1>
                {% if user.about_me %}
	                <p>{{ user.about_me }}</p>
	            {% endif %}
                {% if user.last_seen %}
	                <p>Last seen on: {{ user.last_seen }}</p>
	            {% endif %}
            </td>
        </tr>
    </table>
    ...
{% endblock %}
```

Tracking `last_seen` is simple: each time the user makes a request to a view function we update the `last_seen` field to the current time. We use the [[Flask#Before Request Decorator|@app.before_request]] decorator to run this code before each view function:
```python
from datetime import datetime, timezone

@app.before_request
def before_request():
    if current_user.is_authenticated:
        current_user.last_seen = datetime.now(timezone.utc)
        db.session.commit()
```
### Profile Page: Editor Form
We need a form (again using [[Flask-WTForms]]) for users to change their profile information and write something in the new `about_me` field. 

Make the `EditProfileForm` inside `app/forms.py`
- `about_me` uses the new `TextAreaField`, which displays a text box
- `about_me` uses the new `Length` validator, which restricts the min/max chars
```python
class EditProfileForm(FlaskForm):
    username = StringField("Username", validators=[DataRequired()])
    about_me = TextAreaField("About me", validators=[Length(min=0, max=140)])
    submit = SubmitField("Submit")
```

We also have to make a corresponding template (`app/templates/edit_profile.html`)
```html
{% extends "base.html" %}

{% block content %}
    <h1>Edit Profile</h1>
    <form action="" method="post">
        <p>
            {{ form.hidden_tag() }}<br>
            {{ form.username(size=32) }}<br>
            {% for error in form.username.errors %}
            <span style="color: red;">[{{ error }}]</span>
            {% endfor %}
        </p>
        <p>
            {{ form.about_me.label }}<br>
            {{ form.about_me(cols=50, rows=4) }}<br>
            {% for error in form.about_me.errors %}
            <span style="color: red;">[{{ error }}]</span>
            {% endfor %}
        </p>
        <p>
            {{ form.submit() }}
        </p>
    </form>
```

Now we must write a view function to handle the client request:
- POST: Submitting form
	- Form valid? Save changes and reload page
	- Form invalid? Display default
- GET: Prefill the form with previously submitted data
```python
@app.route("/edit_profile", methods=["GET", "POST"])
@login_required
def edit_profile():
    form = EditProfileForm()
    # Submit: Overwrite username/about_me and commit
    if form.validate_on_submit():
        current_user.username = form.username.data
        current_user.about_me = form.about_me.data
        db.session.commit()
        flash("Your changes have been saved.")
        return redirect(url_for("edit_profile"))
    # If GET, display the username/about_me fetched from db
    elif request.method == "GET":
        form.username.data = current_user.username
        form.about_me.data = current_user.about_me
    return render_template("edit_profile.html", title="Edit profile", form=form)
```

And finally we expose the profile editor through a link in the profile page
```html
{% if user == current_user %}
<p><a href="{{ url_for('edit_profile') }}">Edit Profile</a></p>
{% endif %}
```
## Chapter 7: Error Handling

## References
