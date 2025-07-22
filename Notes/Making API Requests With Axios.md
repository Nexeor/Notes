2025-01-21 17:40

Status: #Branch

Tags: #Database #React #WebDev #API #Axios

# Axios in React
## Basics
There are two main ways of sending [[HTTP]] requests in React:
- `fetch()`, the native solution
- Axios, a popular library

This information is for using **Axios**, to install Axios: `npm i axios`

Axios uses a four part construct for sending and receiving HTTP requests:
- `.get(url)`: Send a *get* request to the provided URL
	- This function returns a *promise*: an object that holds the result/failure of an asynchronous operation
	- We can replace this with other HTTP requests like `delete()`, `post()`, and `patch()`
- `.then(function)`: Run the given function if the promise is successfully resolved
- `.catch(function)`: Run the given function if the promise is unsuccessful
- `.finally(function)`: Always run this code, whether successful or not

Send a `get` request to fetch some `users`:
```tsx
function App() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    axios
      .get("https://jsonplaceholder.typicode.com/users")
      .then((res) => console.log(res))
      .catch((err) => console.log(err));
  });

  return <></>;
}
```
### Using Typescript Interfaces to Define Incoming Data
We can define an interface using TypeScript to define the shape of the incoming data. This way, only properties that match our interface will be recorded, and all others ignored. For example, say we're pulling our users from a complex API that returns the following object:
```json
  {
    "id": 1,
    "name": "Leanne Graham",
    "username": "Bret",
    "email": "Sincere@april.biz",
    "address": {
      "street": "Kulas Light",
      "suite": "Apt. 556",
      "city": "Gwenborough",
      "zipcode": "92998-3874",
      "geo": {
        "lat": "-37.3159",
        "lng": "81.1496"
      }
    },
    "phone": "1-770-736-8031 x56442",
    "website": "hildegard.org",
    "company": {
      "name": "Romaguera-Crona",
      "catchPhrase": "Multi-layered client-server neural-net",
      "bs": "harness real-time e-markets"
    }
```

If we only want the `id` and `name` fields, we only define those properties in our interface:
```tsx
interface User {
  id: number;
  name: string;
}
```

Using this interface we can get and display only the needed data:
```tsx
function App() {
  const [users, setUsers] = useState<User[]>([]);

  useEffect(() => {
    axios
      .get<User[]>("https://jsonplaceholder.typicode.com/users")
      .then((res) => setUsers(res.data));
  }, []);

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```
### Catching Errors with .catch()
The **get** method only returns a *promise*, and sometimes that promise is not fulfilled. To prevent unexpected behavior we can use the **.catch()** function to handle these exceptions. **.catch()** works similarly to **.then()**, except it only executes the function passed as a param if the promise is *unsuccessful*. 

If we want to display an error message if the HTTP request fails:
```tsx
interface User {
  id: number;
  name: string;
}

function App() {
  const [users, setUsers] = useState<User[]>([]);
  const [error, setError] = useState("");

  useEffect(() => {
    axios
      .get<User[]>("https://jsonplaceholder.typicode.com/users")
      .then((res) => setUsers(res.data))
      .catch((err) => setError(err.message));
  }, []);

  return (
    <>
      {error && <p className="text-danger">{error}</p>}
      <ul>
        {users.map((user) => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </>
  );
}
```
### Using AbortController to Cancel Requests
Sometimes we need to cancel an HTTP request before it completes. Maybe the user navigates away before it is loaded, or the request simply took too long to return. In these cases we can use the AbortController to cancel the HTTP request if it hasn't completed by the time we no longer need it.

We can use the `AbortController` by creating one in the `useEffect` hook and setting its signal during the `get` call. The controller is now linked to the HTTP request and we can utilize it to cancel the request with the `controller.abort()` method.

Here we set the return function to `controller.abort()`. This way, the HTTP request will be automatically cancelled if this component is unmounted before the request completes. 
```tsx
    axios
      .get<User[]>("https://jsonplaceholder.typicode.com/usersx", {
        signal: controller.signal,
      })
      .then((res) => setUsers(res.data))
      .catch((err) => {
        if (!(err instanceof CanceledError)) setError(err.message);
      });

    return () => controller.abort();
  }, []);
```

We also adjust the `.catch()` call to ignore the `CanceledError`, which is the error thrown when the controller cancels the request. Strict mode will call render this component twice, unmounting it after the first render, which will trigger the `controller.abort()` and cause a `canceled` error.
### Using .finally() to Show a Loading Indicator 
We can use state variables to track whether the request is complete or not

In this block of code we use `isLoading` to track the completion of the HTTP request, manually using `setLoading` to control the status. We then have a bootstrap `spinner-border` element that only renders when the `isLoading` is true, which is when we are sending our HTTP request. 
```tsx
function App() {
  const [users, setUsers] = useState<User[]>([]);
  const [error, setError] = useState("");
  const [isLoading, setLoading] = useState(false);

  useEffect(() => {
    const controller = new AbortController();

    setLoading(true);
    axios
      .get<User[]>("https://jsonplaceholder.typicode.com/users", {
        signal: controller.signal,
      })
      .then((res) => {
        setUsers(res.data);
        setLoading(false);
      })
      .catch((err) => {
        if (!(err instanceof CanceledError)) {
          setError(err.message);
          setLoading(false);
        }
      });

    return () => controller.abort();
  }, []);

  return (
    <>
      {error && <p className="text-danger">{error}</p>}
      {isLoading && <div className="spinner-border"></div>}
      <ul>
        {users.map((user) => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </>
  );
}
```

We can use the `.finally()` construct to avoid code duplication. Code provided in this block will always run, whether the HTTP request is successful or not. However, `.finally()` **is not supported in strict mode**. So be careful when testing it

```tsx
  useEffect(() => {
    const controller = new AbortController();

    setLoading(true);
    axios
      .get<User[]>("https://jsonplaceholder.typicode.com/users", {
        signal: controller.signal,
      })
      .then((res) => {
        setUsers(res.data);
      })
      .catch((err) => {
        if (!(err instanceof CanceledError)) {
          setError(err.message);
        }
      })
      .finally(() => {
        setLoading(false);
      });

    return () => controller.abort();
  }, []);
```

## Using delete(), post(), and patch()
### Deleting Data with delete()
Similarly to how we can use `.get()` to request data from a server, we can also use `.delete()` to remove data from a server. The main choice we need to make is whether this is an [[Optimistic vs Pessimistic Update]]. 

Here we add a "delete" button to our list items from before and perform a **optimistic update**. We call the `setUsers` function to update the UI first, then call `axios.delete()` to perform the update. If the update fails, we revert it within the `.catch()` function:
```tsx
  const deleteUser = (user: User) => {
    const originalUsers = [...users];
    setUsers(users.filter((u) => u.id !== user.id));

    axios
      .delete("https://jsonplaceholder.typicode.com/usersx/" + user.id)
      .catch((err) => {
        setError(err.message);
        setUsers(originalUsers);
      });
  };

  return (
    <>
      {error && <p className="text-danger">{error}</p>}
      {isLoading && <div className="spinner-border" />}
      <ul className="list-group">
        {users.map((user) => (
          <li
            key={user.id}
            className="list-group-item d-flex justify-content-between"
          >
            {user.name}
            <button
              className="btn btn-outline-danger"
              onClick={() => deleteUser(user)}
            >
              Delete
            </button>
          </li>
        ))}
      </ul>
    </>
  );
```

We could rewrite this as a **pessimistic** function by calling `.delete()` first and updating the UI in the `.then()` function, therefore only updating the UI if the delete request is successful. However, optimistic updates are generally preferred do to their responsiveness. 
```tsx
  const deleteUser = (user: User) => {
    axios
      .delete("https://jsonplaceholder.typicode.com/users/" + user.id)
      .then(() => {
        setUsers(users.filter((u) => u.id !== user.id));
      })
      .catch((err) => {
        setError(err.message);
      });
  };
```
### Creating Data with post()
We can create new data by using `post()` instead of `get()` or `delete()`. In addition to the URL we also pass an object that becomes the payload of our `post` request. 

In this example we create a new "add" button to create new users. In this case we are adding a static user but typically you'd be passing the results of a form or other user input. This is an **optimistic** update where we make the UI changes first and then notify the server. Sometimes the server will also do some operations, like assigning a unique ID. We call `setUsers` within `.then()` to reconcile the returned data with our UI.
```tsx
const addUser = () => {
    const originalUsers = [...users];
    const newUser = { id: 0, name: "Mosh" };
    setUsers([newUser, ...users]);

    axios
      .post("https://jsonplaceholder.typicode.com/usersx/", newUser)
      .then(({ data: savedUser }) => setUsers([savedUser, ...users]))
      .catch((err) => {
        setError(err.message);
        setUsers(originalUsers);
      });
  };
```
### Updating Data with patch()
We can update existing data, changing one or more properties without replacing the original object, by using the `patch()` method. Similarly to `post()` we pass the updated object as a secondary param to the `patch()` method. 

Here we write a function to update a `user` object by adding a "!" to the end of their name. This is an **optimistic** update, where we make the UI changes first before notifying the server. 
```tsx
const updateUser = (user: User) => {
    const originalUsers = [...users];
    const updatedUser = { ...user, name: user.name + "!" };
    setUsers(users.map((u) => (u.id === user.id ? updatedUser : u)));

    axios
      .patch(
        "https://jsonplaceholder.typicode.com/users/" + user.id,
        updatedUser
      )
      .catch((err) => {
        setError(err.message);
        setUsers(originalUsers);
      });
  };
```
## Simplifying Axios Code 
As we add handlers for different HTTP requests, our code will grow in complexity. Axios offers us some tools to prevent code duplication and remove boilerplate.
### Axios Modules
An **Axios Module** allows us to define our **base url** that we send HTTP requests to
- **Axios Module**: A separate file where we can set configuration options for Axios
	- Typical to put in a "services" dir inside of src
- **Base URL**: The base URL that we send HTTP requests to 
	- We can specify an endpoint in our code (/users) to query for specific resources

A basic Axios module where we set out **baseURL** and export the CanceledError
```tsx
import axios, { CanceledError } from "axios";

export default axios.create({
    baseURL: "https://jsonplaceholder.typicode.com"
})

export { CanceledError };
```

We can then import this module into our other classes to use this baseURL. Instead of invoking the call on `Axios`, instead use the imported `apiClient` and only specify the endpoint as the first param, as the baseURL is provided by the Axios module
```tsx
  const deleteUser = (user: User) => {
    apiClient
      .delete("/users/" + user.id)
      .then(() => {
        setUsers(users.filter((u) => u.id !== user.id));
      })
      .catch((err) => {
        setError(err.message);
      });
  };
```
## Services
Ideally we want to completely decouple our components from HTML requests, which we can do with a **service**. A service gives a well defined interface that we can interact with to make HTTP requests to a specific endpoint. This service can be reused throughout our code, allowing multiple different components to use the same interface, preventing code duplication. 

For example, lets say we have an API that serves our user data. There are many different components that may need to use this user information, like a bio, profile settings, or a list of users. Instead of explicitly writing HTTP requests within these components, they can instead call the generic **user service**, giving them an interface to get/modify user data. 
### Example User Service
We can define a service like so. Inside the `UserService` class we define our interface. Each of these interface methods return a promise that is resolved in our component using Axios `.then()` and `.catch()`.  We can also define and return a cleanup method from this interface. We can also define a literal TS interface here to control the shape of our data. 
```ts
import apiClient from "./api-client";

// Define the shape of user
export interface User {
    id: number;
    name: string;
  }

class UserService {
    getAllUsers() {
	    // Handle abort controller here
        const controller = new AbortController();
        const request = apiClient.get<User[]>("/users/", {
            signal: controller.signal,
        })
        // Pass back a tuple of { promise, cleanup function }
        return { request, cancel: () => controller.abort()}
    }

    deleteUser(id : number) {
        return apiClient.delete("/users/" + id)
    }

    addUser(toAdd : User) {
        return apiClient.post("/users/", toAdd)
    }

    updateUser(toUpdate: User) {
        return apiClient.patch("/users/" + toUpdate.id, toUpdate)
    }
}

export default new UserService();
```

We then import the `UserService` and `User` interface into our component and call the methods from within this service. Here we capture the promise (`resquest`) and the cleanup method `cancel` and use them explicitly: 
```tsx
  useEffect(() => {
    setLoading(true);
    const { request, cancel } = userService.getAllUsers();
    request
      .then((res) => {
        setUsers(res.data);
      })
      .catch((err) => {
        if (!(err instanceof CanceledError)) {
          setError(err.message);
        }
      })
      .finally(() => {
        setLoading(false);
      });

    return cancel;
  }, []);
```

The other methods in the service only return a promise so we can work on them directly:
```ts
  const deleteUser = (user: User) => {
    userService
      .deleteUser(user.id)
      .then(() => {
        setUsers(users.filter((u) => u.id !== user.id));
      })
      .catch((err) => {
        setError(err.message);
      });
  };
```
### Creating a Generic HTTP Service
Almost all services we create will be the exact same logic, but just to different endpoints. We can avoid repeating this logic by creating a generic HTTP service.

```ts
import apiClient from "./api-client";

interface Entity {
    id: number;
}

class HttpService {
    endpoint: string

    constructor(endpoint: string) {
        this.endpoint = endpoint;
    }

    getAll<T>() {
        const controller = new AbortController();
        const request = apiClient.get<T>(this.endpoint, {
            signal: controller.signal,
        })
        return { request, cancel: () => controller.abort()}
    }

    delete(id : number) {
        return apiClient.delete(this.endpoint + "/" + id)
    }

    create<T>(entity : T) {
        return apiClient.post(this.endpoint, entity)
    }

    update<T extends Entity>(entity: T) {
        return apiClient.patch<T>(this.endpoint + "/" + entity.id, entity)
    }
}

const create = (endpoint : string) => new HttpService(endpoint);

export default create;
```

This service exports the "create" method, which other services can call with an endpoint to use the code here. For example, we can simplify the users service into just the interface and a constructor call: 
```ts
import create from "./http-service"

export interface User {
    id: number;
    name: string;
  }

export default create("./users");
```
## Using Custom Hooks to Manage State


## Using async() and await()
Using `then()` and `catch()` is succinct, but is difficult to use as our project grows more complex. We can instead write *synchronous* code with a *try-catch* block to handle errors in a more robust manner. 

Using the `async` keyword we can define an **asynchronous function**:
- Asynchronous functions always return a promise
- We can use the `await` keyword to store the result of a HTTP request in a variable
	- This function pauses the function's execution until the Promise is resolved
	- Using `await` forces the code to behave like synchronous code
- Inside the async function we create a `try` and a `catch` block to explicitly handle the risky code
```tsx
useEffect(() => {
	// Define the async function
    const fetchUsers = async () => {
      // Run this code until we encounter an error
      try {
        const res = await axios.get<User[]>(
          "https://jsonplaceholder.typicode.com/users"
        );
        setUsers(res.data);
	  // Handle that error
      } catch (err) {
        setError((err as AxiosError).message);
      }
    };

	// Call the async funtion define here
	fetchUsers()
  }, []);
```
## References
