2024-10-24 18:16

Status:

Tags: #Python #ProgrammingConcepts

# Python Context Manager (with statement)
When interacting with system resources (files, locks, network sockets) we must acquire and release those resources at the appropriate times. Context managers allow us to abstract away resource management using the "with" keyword. It also allows us to define custom protocols for certain resources when they are acquired and released.

## The With Statement
```python
with open('hello.txt', 'w') as f:
	f.write('hello, world!')
```

When we use the "with" keyword to open the file, it ensures that the file will be properly released, even if there is an exception. We know the file will be closed properly without having to pay attention to how.

If we wrote a similar statement without the with keyword it would look something like this:
```python
f = open('hello.txt', 'w')
try: 
	f.write('hello world!')
finally:
	f.close()
```
The "with" statement abstracts away this try finally block into a repeatable pattern

## Context Managers
We can extend the "with" statement to be used with our own objects if we implement a **context manager** within the class. 

A context manager requires two components:
- An `__enter__` function
- An `__exit__` function

Example Context Manager:
```python
class ManagedFile:
	def __init__(self, name):
		self.name = name
	def __enter__(self):
		self.file = open(self.name, 'w')
		return self.file
	def __exit__(self, type, value, traceback):
		if self.file:
			self.file.close()

# Can use the context manager on our custom object
with ManagedFile('demo.txt') as open_file:
	open_file.write("Hello world!")
```

### Using Decorators and Generators
The "contextlib" module can be used to automatically generate a context manager from a try finally block

```python 
from contextlib import contextmanager

@contextmanager
def open_file(name):
    f = open(name, 'w')
    try:
        yield f
    finally:
        f.close()
```

We can now use the "with" keyword for this object as implemented by this contextmanager:
```python
with open_file('some_file') as f:
    f.write('hola!')
```
## References
Python Tips: https://book.pythontips.com/en/latest/context_managers.html
YT Video: https://www.youtube.com/watch?v=iba-I4CrmyA
