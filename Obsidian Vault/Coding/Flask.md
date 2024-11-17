### Setup
To prevent dependency errors when working with different python libraries on different projects, set up a virtual environment:
1) Create a project folder (mkdir) and a .venv folder:
```
$ python3 -m venv .venv
```
2) Activate the virtual environment
```
$ source .venv/bin/activate
```
3) Install FLASK within the environment
```
$ pip install Flask
```

### Minimal Application
```
from flask import Flask

app = Flask(__name__) # Create an instance of the class

@app.route("/") # What URL of our server triggers this funtion
def hello_world(): # Run this code when we visit url '/'
    return "<p>Hello, World!</p>"
```

**Run built-in server using:** ```flask --app <.py_file> run```
- If the file is named "app.py", don't need the --app specifier



