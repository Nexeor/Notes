2024-10-29 14:19

Status:

Tags: #Python #PythonLibrary #PythonModule

# Python Libraries and Modules
## Modules
**Module:** A single python file (.py), containing a set of related functions, classes, and variables
- Modules can be imported to different projects
- Allows us to reuse the same code many times
- There are several built in modules: `math`, `datatime`, and `json`
- Any Python file can be used as a module as long as we `import` it

Create a python file named `my_module.py`:
```python
# my_module.py

# Define a function
def greet(name):
    return f"Hello, {name}!"

# Define another function
def add(a, b):
    return a + b

# Define a variable
pi = 3.14159

```

Import to another file:
```python
# main.py
import my_module

# Use functions and variables from the module
print(my_module.greet("Alice"))  # Output: Hello, Alice!
print(my_module.add(5, 3))       # Output: 8
print(my_module.pi)              # Output: 3.14159
```

Import with alias:
```python
import my_module as mm
```

Import specific functions/variables
```python
from my_module import greet, pi
```

## Libraries
**Library**: A collection of related modules packaged together as a bundle
- Popular libraries include `NumPy` and `Pandas`
## References
