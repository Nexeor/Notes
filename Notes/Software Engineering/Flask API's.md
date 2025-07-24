### Setup2025-02-03 14:52

Status: #Branch 

Tags: #Python #API #WebDev #Flask #PythonLibrary 

# Flask API's

## References

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
We can set up a very basic flask application quickly
```python
from flask import Flask
from flask_restful import Api, Resource

app = Flask(__name__)
api = Api(app)

# Each class represents an endpoint
class HelloWorld(Resource):
	# We can override the methods at each endpoint with our own
    def get(self):
	    # Return a response body and (optionally) a status code
        return {"data" : "Hello World"}, 200

# We add the endpoint as a "resource" to our API, specifying the URL
api.add_resource(HelloWorld, "/helloworld")

# Main method starts up the API
if __name__ == "__main__":
    app.run(debug=True)
```

From here we can add additional endpoint by creating more classes and attaching them with `add_resource`. We can also add `put`, `delete` and other requests within our classes to add additional API commands.

Run the API with: `python main.py` (where `main.py`is the name of your API file)
- Once running, it will provide an API URL to use as a Base URL (ex: `http://127.0.0.1:5000/`)
- We can now make requests to the API (`requests.get(BASE + "helloworld"))
### URL Arguments
Sometimes we want to filter the resources using the API URL itself. We can do this by requiring an additional string/int as an argument. 

Here we must provide a "name" value to access the endpoint
```python
class HelloWorld(Resource):
    def get(self, name):
        return {"data" : "Hello World"}

api.add_resource(HelloWorld, "/helloworld/<string:name>")
```

Here we must provide a "name" and "test" value:
```python
class HelloWorld(Resource):
    def get(self, name, age):
        return {"name" : name, "age" : age}

api.add_resource(HelloWorld, "/helloworld/<string:name>/<int:age>")
```

We then must specify these when making a call to this endpoint
- `response = requests.get(BASE + "helloworld/tim/23")`

Usually the User will provide some sort of filter on what information they want from the server:
```python
employees = { 
	"Tim" : { "age": 23, "id": 0 },
	"Bill" : { "age": 25, "id": 1 },
}

class Employees(Resource):
	def get(self, name):
		return employees[name]

api.add_resource(Employees, "/employees/<string:name>")
```

Which we can request with: `response = requests.get(BASE + "employees/tim")`
### Parsing Request Bodies
When making an API request that modifies the data such as `put`, `patch`, or `post`, we must parse the body of the request to get the required data. 

This `put` request is to the `video/1` endpoint and is replacing the `"likes"` attribute:
```python
response = requests.put(BASE + "video/1", { "likes" : 10 })
```

In our API, we can access the data using `request.form`:
```python
class Video(Resource):
    def get(self, video_id):
        return videos[video_id]
    
    def put(self, video_id):
        print(request.form['likes'])
        return {}
```

**flask_restful** required for reqparse

We can standardize this by specifying *arguments* for our API methods using the `add_argument` function
- First Param: The name of the argument
- **type**: Argument type
- **help:** Send this message if this argument throws a type error
- **required**: Throw an error if this argument isn't provided

```python 
video_put_args = reqparse.RequestParser()
video_put_args.add_argument("name", type=str, help="Name of video", required=True)
video_put_args.add_argument("views", type=str, help="Views of video")
video_put_args.add_argument("likes", type=str, help="Likes of video")
```

We can then use the `parse_args` function to easily access the arguments as a dict
- We update our internal state (the `videos` array) in response to an API request (the `put`)
```python
def put(self, video_id):
	args = video_put_args.parse_args()
	videos[video_id] = args
	return videos[video_id], 201
```
### Validating Requests
We can now `put` videos into our array and `get` them:
```python
videos = {}

class Video(Resource):
    def get(self, video_id):
        return videos[video_id]
    
    def put(self, video_id):
        args = video_put_args.parse_args()
        videos[video_id] = args
        return videos[video_id], 201
```

However, if we attempt a `get` request for a video that doesn't exist, we get errors. To fix this, we must *validate* incoming requests to ensure their data is legal.

We can do this by defining a validation function and calling it before accessing data
- The `abort` function immediately returns with the given message and status code
```python
def validate_video_id(video_id):
    if video_id not in videos:
        abort(404, message="Video id does not exist")

class Video(Resource):
    def get(self, video_id):
        validate_video_id(video_id)
        return videos[video_id]
```
### Delete Requests
 