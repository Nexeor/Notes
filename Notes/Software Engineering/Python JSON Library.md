2024-08-28 10:25

Status:

Tags: #PythonLibrary #JSON

# Python JSON Library

The built in library JSON allows you to decode and encode JSON objects quickly in Python

### Importing
	import json

### **Encode:** Convert Python type to JSON object/file
- **json.dump()** converts to a JSON file
- **json.dumps()** converts to a JSON string
- Input a Python object and receive a JSON formatted version of it
- Python types are converted to JSON types according to table below

| Python                                 | JSON   |
| -------------------------------------- | ------ |
| dict                                   | object |
| list, tuple                            | array  |
| str                                    | string |
| int, float, int- & float-derived Enums | number |
| True                                   | true   |
| False                                  | false  |
| None                                   | null   |
Example: Python dict converted to JSON string using dumps()

	data = { "name": "John", "age": 30, "city": "New York" } 
	json_data = json.dumps(data) 
	print(json_data) # Output: {"name": "John", "age": 30, "city": "New York"}

### **Decode**: Convert JSON to Python type
- **json.load()** converts a JSON file to a Python object
- **json.loads()** converts a JSON string to a Python object
- Types are converted based on the same table above

## References
Python docs: https://docs.python.org/3/library/json.html