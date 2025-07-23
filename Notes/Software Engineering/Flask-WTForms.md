2025-06-02 20:11

Status: #Leaf

Tags: #Python #PythonLibrary #Flask #WebDev 

# Flask-WTForms
An extension for [[Flask]] that uses Python classes to represent web forms. Forms are defined as their own class and the fields of that form are defined as class variables. 

We can define a "login" form like so:
```python
from flask_wtf import FlaskForm
from wtforms import StringField, PasswordField, BooleanField, SubmitField
from wtforms.validators import DataRequired

class LoginForm(FlaskForm):
    username = StringField('Username', validators=[DataRequired()])
    password = PasswordField('Password', validators=[DataRequired()])
    remember_me = BooleanField('Remember Me')
    submit = SubmitField('Sign In')
```

From WTF we can import fields for different data types, like `StringField`, `BooleanField`, and `SubmitField`. For each of these "field" objects:
- **Label**: The text label applied to the field
- **Validators (optional):** Array of validation behaviors to be applied to the field 
	- `DataRequired` checks that the submitted field is not empty

### Custom Validators
Any methods added to a form class of the pattern `validate_<field_name>` are treated as custom validators by WTForms and ran as validators. If they execute without raising a `ValidationError`  than the data is valid.

For example, we can use a custom validator to check the database for a certain value. This `validate_username` function raises a `ValidationError` if the username already exists in the database. If the username does not exist, than it passes
```python
def validate_username(self, username):
	user = db.session.scalar(sa.select(User).where(
		User.username == username.data))
	if user is not None:
		raise ValidationError('Please use a different username.')
```

## References