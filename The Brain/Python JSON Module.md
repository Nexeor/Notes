2024-10-29 15:59

Status:

Tags: #Python #PythonModule 

# Python JSON Module
The JSON module is used to quickly encode and decode json objects and uses two primary function: `dump()` and `load()`

## Encoding Objects as JSON: dump
The json.dump method encodes a Python object (typically a dictionary) and converts it to JSON formatting. It has two variants: 
- `json.dump()`: Convert python object to JSON file
- `json.dumps()`: Convert python object to JSON formatted string
### json.dump()
Takes a python object and a file-like object. It encodes the python object in JSON format and writes it directly to the file.

Ex: Convert "data" dict to JSON and write directly to the file
```python
import json

data = {'name': 'Alice', 'age': 25}
with open('data.json', 'w') as file:
    json.dump(data, file)
```
### json.dumps()
Takes a python object and encodes it as a JSON formatted string. 

Ex: Convert "data" dict to JSON formatted string and print it
```python
import json

data = {'name': 'Alice', 'age': 25}
json_string = json.dumps(data)
print(json_string)  # Output: {"name": "Alice", "age": 25}
```

## Decoding JSON into Objects: load
The json.load method decodes a JSON formatted object into a Python readable object. It has two variants:
- `json.load()`: Convert JSON file to Python object
- `json.loads()`: Convert JSON formatted string to Python object

### json.load()
Takes a file-like object containing JSON data and deserialize it into a Python object (usually a dict)

Ex: Convert a file containing JSON data into a Python dict
```python
import json

with open('data.json', 'r') as file:
    data = json.load(file)
print(data)  # Output: {'name': 'Alice', 'age': 25}
```

Ex: Convert a JSON formatted string into a Python dict 
```python
import json

json_string = '{"name": "Alice", "age": 25}'
data = json.loads(json_string)
print(data)  # Output: {'name': 'Alice', 'age': 25}
```
## References
