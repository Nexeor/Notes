2025-01-15 11:21

Status: #Branch

Tags: #React #WebDev #Framework #JavaScript

# React State

## The useState() Hook
There are a couple properties of the useState() hook that are important to understand:
1) React updates state asynchronously
2) State is stored outside of any individual component
3) Hooks (including useState) can only be used at the "top level" of a component
### Asynchronous Updates
To minimize re-renders and optimize performance, React *batches* updates together, only 
re-rendering and updating the state variables at the end of the event. 

In this snippet, we may expect the console to log "true" as the state variable was set immediately before. However, it will actually log "false" as the update will be saved until the end of the event:
```tsx
function App() {
	const [isVisible, setVisibility] = useState(false);

	const handleClick = () => {
		setVisibility(true)
		console.log(isVisible) // Will still print "false"
	}
	...
}
```

If we re-rendered the component after each call to a setter function, the page would be reloaded multiple times per event, wasting resources. In more complex apps where events can change many state variables, this becomes important for responsive apps.
### State Stored Outside Components
State variables are not treated like other JS variables declared with `let` or `const`. These JS variables only exist for a small slice of time when the component is being rendered, and then it is reinitialized and essentially reset. Local variables should **never** be used for maintaining state.

In this snippet `count` is update on each click, but will always display `1`, as the `count` variable is reinitialized on each handleClick() event:
```tsx
function App() {
	let count = 0
	const handleClick = () => {
		count += 1
		console.log(count) // Will always print "1" no matter what
	}
	...
}
```
### Hooks at Top Level Only
React stores state variables outside the the component as seen previously. It stores them in an external array, with each state variable's value stored inside. The array doesn't remember the variable names, and instead relies on the ordering of the external array to re-assign the state variables when the component is re-rendered. 

In this snippet, the state variables would be stored as `[false, true]` outside of the `App` component, and be reassigned to `isVisible` and `isApproved` based on the ordering of this external array when `App` is re-rendered
```tsx
// state stored outside as '[false, true]'
function App() {
	const [isVisible, setVisibility] = useState(false);
	const [isApproved, setApproved] = useState(true);

	..
}
```

Because of this ordering reassignment, we must always call make the calls to useState in the same order or the wrong values will be assigned. Essentially, we must always have the same number of useState calls in the same order. Therefore we cannot call the useState hook in the following situations:
- **Conditional Statement**: If the condition is not met, the hook will never be called
	- Not the same number of calls each time
-  **Loops**: Number of iterations may vary
	- Not the same number of calls each time
	- A static loop is fine, but MUST have the same number of iterations every time
- **Nested Functions**: React can't track useState calls made in helper functions
- **After Early Returns**: We must make all useState calls before we return 
	- Returning early before a useState call -> not the same number of calls each time


## State Structure and Objects
When building complex applications we will quickly end up with many state variables. To keep our project simple, follow these two best practices for state structure:
1) Avoid redundant state variables
2) Group related variables inside an object
### Redundant State Variables
We want to keep as few state variables as possible. To do so, we want to minimize state variables whenever we can by combining other state variables together when possible. 

In this snippet, we have a `firstName` and `lastName` state variable. Rather than creating a new `fullName` state variable, we combine the two existing ones. 
```tsx
function App() {
	const [firstName, setFirstName] = useState('Tim');
	const [lastName, setLastName] = useState('Anderson');
	let fullName = firstName + ' ' + lastName

	return (
		<div>
			{fullName}
		<\div>
	);
}
```
### Using Objects and Arrays for Related State Variables
We can define more complex state variables by bundling two or more variables together as an *object*. This creates a single state variable with multiple related properties that we can access.

In this snippet, we group the two state variables `firstName` and `lastName` into a single object:
```tsx
function App() {
	const [fullName, setFullName] = useState({
		firstName : "Tim"
		lastName : "Anderson"
	});

	return (
		<div>
			{fullName}
		<\div>
	);
}
```

However, avoid nested structures. If we wanted to add a "contact" field, which itself is an object containing more information, it's best to create a new object to represent that. It gets awkward to access deeply nested properties, so it's best to avoid using them.
#### Updating Objects
State objects, similar to props, should be treated as *read only*. We can only update the object by creating a new one with the modified fields and assigning it to the same variable. 

For example, in this snippet we have a `drink` object with a `title` and `price` field. We want to change the `price` of our `drink` object when we click on a button.  To do this we have to create a new `drink` object with the new `price`.
```tsx
function App() {
	const [drink, setDrink] = useState({
		title: "Americano",
		price: 5,
	});

	const handleClick() = () => {
		const newDrink = {
			title: drink.title
			price: drink.price + 1
		}
		setDrink(newDrink)
	}
}
```

Copying every property can get annoying, so we can use the **spread** operator to copy all existing properties. In this snippet we create and update the `drink` all in one line using this syntax:
```tsx
function App() {
	const [drink, setDrink] = useState({
		title: "Americano",
		price: 5,
	});

	const handleClick() = () => {
		setDrink({...drink, price : 6})
	}
}
```
#### Updating Nested Objects 
The **spread** operator is a shallow copy, and works by simply setting new values to the values stored in the old object. If we have a object nested inside another object and use the spread operator, the nested object in the new object is still a reference to the old object, rather than being its own object. This can cause unintended behavior where changing a nested object in one object could change the nested object in one or more other objects. 

In this example we have a `customer` object with a nested `address` object. However, if we use the "spread" operator as shown, the new `customer` object will actually point to the *old* value of address.
```tsx
function App() {
	const [customer, setCustomer] = useState({
		name: 'John'
		address: {
			city: 'San Francisco',
			zipCode: 94111,
		}
	})

	const handleClick = () => {
		setCustomer({ ...customer })
	}
}
```

To overcome this, we have to create a *new* instance of `address` with the spread operator. Then we can change whatever values we wish inside either `customer` or `address`:
```tsx
const handleClick = () => {
	setCustomer({ ...customer, address: {...customer.address, zipCode: 94112}})
}
```

This is why we want to *avoid* nested objects. Updating them quickly becomes tedious and difficult to read. 
#### Updating Arrays
Similarly to objects, when we update a stateful array we have to create a new one and set it to the original variable. We can also use the **spread** operator to make copying values easier.

In this snippet, we're adding the "excited" value to `tags`
```tsx
function App() {
	const [tags, setTags] = useState('happy', 'cheerful']);

	const handleClick = () => {
		setTags([...tags, 'exciting']);
	} 
}
```

Removing a specific value (delete happy):
```tsx
const handleClick = () => {
	setTags(tags.filter(tag => tag !== 'happy'));
}
```

Update values (Change 'happy' to 'super happy'):
```tsx
const handleClick = () => {
	setTags(tags.map(tag => tag === 'happy' ? 'super happy' : tag));
}
```
#### Updating Arrays of Objects
Combining the rules from before we can have stateful arrays with several objects inside. We have to create a new array, which we fill with the modified objects and then copy over unmodified objects. We can use the `map` function to update only specific objects and copy the rest. 

In this snippet, we have an array of `bug` objects. When we click, we want to update the list of bugs such that the first `bug` has its `status` changed to 'fixed':
```tsx
function App() {
	const [bugs, setBugs] = useState([
		{ id: 1, title: 'Bug 1', fixed: false },
		{ id: 2, title: 'Bug 2', fixed: false },
	]);

	const handleClick = () => {
		setBugs(bugs.map(bug => bug.id === 1 ? {...bug, fixed: true } : bug))
	}
}
```


#### Using immer Library to Update
We can take a more JS native approach to updating objects using the **immer** library. First make sure it is installed: `npm i immer`. Using immer we can create a `draft` object inside a `produce` function that acts as a proxy of the object we want to modify. We then use JS to modify that object however we wish, and at the end of the `produce` function immer will automatically copy over the newly modified object without us having to copy the properties that remains the same.  

Using the example from before, we can replace the `map` logic with immer. 

```tsx
function App() {
	const [bugs, setBugs] = useState([
		{ id: 1, title: 'Bug 1', fixed: false },
		{ id: 2, title: 'Bug 2', fixed: false },
	]);

	const handleClick = () => {
		setBugs(produce(draft => {
			const bug = draft.find(bug => bug.id === 1);
			if (bug) bug.fixed = true;
		}) 
	}
}
```
## Component Purity
A **Pure Function** is a function that, given the same inputs, will always produce the same output. For React Components, a **Pure Component** is one that, given the same props, will always render the same component. React is designed around this concept and expects all of our components to be pure. Because of this, React only re-renders a component if it is passed different props. 

![[Pasted image 20250115122705.png]]

To keep components pure, we need to **keep changes out of the render phase**. A component's rendering should be determined only by the props passed to it and no external factors.

In this snippet we have an **impure** `Message` component. The count variable is updated each render, causing `Message` to render differently each time it is called despite taking no props.
```tsx
let count = 0;

const Message = () => {
	count++;
	return <div>Message {count}</div>
};
```
```tsx
function App() {
	return (
		<div>
			<Message />
			<Message />
			<Message />
		</div>
	)
}
```
```
Output:
Message 2
Message 4
Message 6
```

This snippet can be easily made pure by moving the `let count = 0` line inside the Message function. This would make it a local variable, forcing it to re-initialize on each render.
### Strict Mode
To help us detect impure components, React runs dev builds in **Strict Mode**, a special debugging mode. In strict mode, all components are rendered **twice**. This is why the example component above prints `2, 4, 6`:
![[Pasted image 20250115124336.png]]

If we modify the above snippet to print message to console the message ranges from 1-6:
```
let count = 0;

const Message = () => {
	console.log('Message Called')
	count++;
	return <div>Message {count}</div>
};
```
```tsx
function App() {
	return (
		<div>
			<Message />
			<Message />
			<Message />
		</div>
	)
}
```
```
Output:
Message 2
Message 4
Message 6

Log: 
Message 1
Message 2
Message 3
Message 4
Message 5
Message 6
```
## The useEffect() Hook
**Side Effect**: Any operations or interactions that affect something outside the scope of the component being rendered
- Interacting with external systems, like API's
- Updating the external state
- Operations not directly related to rendering the UI element

Side effects don't relate to JSX markup or rendering components, so having side effects in our components makes them **impure**. However, we can fix this by using the **useEffect** hook.

**useEffect**: A hook that tells React to run a piece of code *after* a component has rendered
- **Function**: The code (side effect) we want to run
	- **Cleanup Function:** If we *return* a function within the side effect, that function will be run on unmount
- **Dependency Array**: A list of dependencies that, when they change, trigger the side effect
	- **None**: Runs on mount and  render
	- **Empty**: Runs on mount only
	- **Filled**: Runs on mount  and whenever the specified variables are updated

This example uses useEffect to set the `title` of the document to "My App":
```jsx
useEffect(() => {
	document.title = 'My App';
})
```
### Dependency Array
In this example we *do not* provide a dependency array. The default behavior is now for this side effect to run each time the component renders. If we want to change the state within the side effect (storing data fetched from an array to a state variable), this will cause an infinite loop:
- The component renders for the first time, triggering the side effect
- The side effect runs, updating a state variable
- The state variable is changed, triggering a re-render
- The component renders, triggering the side effect...

To avoid this, we need to provide a **dependency array**. If any variables are included in the dependency array (state variables, props, etc.), then the side effect will instead only trigger when one of those values are updated. If we pass an empty array instead, the side effect will only be run once, when the component is first mounted. 

Here we use an empty useEffect hook to "fetch" a list of dummy products from the server when this component is mounted:
```jsx
const ProductList = () = {
	const [products, setProducts] = useState([]);

	useEffect(() => {
		console.log('Fetching Products')
		setProducts(['Clothing', 'Household'])
	}, [])
}
```

Here we provide the prop `category` in the dependency array, so we now perform the fetch whenever the "category" prop is changed:
```tsx
function ProductList = ({ category : string} : props) = {
	const [products, setProducts] = useState([]);

	useEffect(() => {
		console.log('Fetching Products in ', category)
		setProducts(['Clothing', 'Household'])
	}, [category])
}
```
### Cleanup Functions 
Sometimes we also need to reverse or cleanup some side effect from earlier. We need to end a session, connection, or do some other sort of cleanup. We can do this by *returning* a cleanup function from within the useEffect() hook.

In this simple example, we call the `connect` method on mount (empty dependency array) and print the "Connecting" message. Then, when the component is unmounted and removed, we call the `disconnect` method and print the "Disconnecting" message.
```tsx
const connect = () => console.log('Connecting');
const disconnect = () => console.log('Disconnectiong');

function App() {
	useEffect(() => {
		connect();
		return () => disconnect();
	}, []);
}
```
## Custom Hooks
As code gets more complex we may call useState() and useEffect() several times in a single component to manage a single aspect of our state. We may also need to repeat these state methods several times across our various components if they rely on the same stateful variables. We can abstract our state by using a **Custom Hook**, which allows us to define React logic outside of a component.

This `GameGrid` component has become quite complex:
- Two different interfaces for managing API
	- See [[Making API Requests With Axios]] for more information on API requests
- Multiple hooks needed to fetch data:
	- A `useEffect()` hook making an API request
	- a `useState()` hook for tracking the API request results
- Data is not reusable, will need to repeat all logic if we want Game info in another component
```tsx
interface Game {
  id: number;
  name: string;
}

interface FetchGamesResponse {
  count: number;
  results: Game[];
}

const GameGrid = () => {
  const [games, setGames] = useState<Game[]>([]);
  const [error, setError] = useState("");

  useEffect(() => {
    apiClient
      .get<FetchGamesResponse>("./games")
      .then((res) => {
        setGames(res.data.results);
        console.log(games);
      })
      .catch((err) => setError(err.message));
  }, []);

	return( 
		// JSX Markup here 
	)
```

Using a **custom hook** we can *bundle* the interfaces, hooks, and API requests into another file. Here we can write a new function that returns state variables and functions, rather than JSX markup.
- Custom Hook files are prefixes with "use", such as "useGames", "useUsers", "useSetComment", etc.

We can move the logic from the `GameGrid` component into a new `useGames` custom hook:
- We define the interfaces here
- Return the state objects the component may need access to 
- We can import this custom hook anywhere we need the Games
```jsx
import apiClient from "@/services/api-client";
import { CanceledError } from "axios";
import { useEffect, useState } from "react";

interface Game {
  id: number;
  name: string;
}

interface FetchGamesResponse {
  count: number;
  results: Game[];
}

const useGames = () => {
      const [games, setGames] = useState<Game[]>([]);
      const [error, setError] = useState("");
    
      useEffect(() => {
        const controller = new AbortController();

        apiClient
          .get<FetchGamesResponse>("./games", { signal : controller.signal})
          .then((res) => {
            setGames(res.data.results);
            console.log(games);
          })
          .catch((err) => {
            if (err instanceof CanceledError) return;
            setError(err.message);
          })
          return () => controller.abort();
      }, []);

      return { games, error }
    }
export default useGames;
```

Using this custom hook we can reduce our component to just a few lines of markup:
```tsx
import useGames from "@/hooks/useGames";
import { Text } from "@chakra-ui/react";

const GameGrid = () => {
  const { games, error } = useGames();
  return (
    <>
      {error && <Text>{error}</Text>}
      <ul>
        {games.map((game) => (
          <li key={game.id}>{game.name}</li>
        ))}
      </ul>
    </>
  );
};

export default GameGrid;
```
## References
