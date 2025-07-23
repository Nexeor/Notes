2024-10-01 17:29

Status:

Tags:

# React
React is a [[JavaScript]] library used to create UI components for websites

Core Principles:
- **Virtual DOM**: React uses a **Virtual DOM** that is updated first and compared to the previous version, then only updating the parts of the real DOM that changed
	- Don't need to interact directly with the DOM
	- Faster updates 
- **JSX:** Gives access to [[JSX]], allowing us to write [[HTML]] within [[JavaScript]]
- **Unidirectional Data Flow:** Data is passed from parent components to child components via props
- **Hooks:** Hook functions allow for components to become functional and maintain state
- 
## Components
**Components** are reusable bits of UI that can be updated live
- Components are defined as [[JavaScript]] functions
- Can be passed a **props** argument, that can be accessed within the function
	- If this argument changes, React will update the component
- Component functions return [[HTML]] written in [[JSX]]
	- Combines JavaScript functionality and HTML formatting into a single function

This component prints a paragraph containing the "text" field inside the props variable
```
function Item(props) {
	return <p>{props.text}</p>
}
```
## useState Hook
Components can be given an internal state with the "useState" hook
- Must be imported: `import { useState } from 'react'`
Can then create a **hook** that defines a value and setter function for that value 
- Define the setter function to determine how to update the value

The Counter component below has a button with corresponding state variable count
```
function Counter() {
	// Declare a state variable called "count" and update function "setCount" 
	const [count, setCount] = useState(0)

	return (
		<div>
			<p> You clicked {count} times </p>
			{/* Button to increment the count */}
			<button onClick={() => setCount(count + 1)}>
				Click Me!
			<\button>
		</div>
	);
}
```

## useEffect Hook
**Side Effect**: Any operations or interactions that affect something outside the scope of the component being rendered
- Interacting with external systems, like API's
- Updating the external state
- Operations not directly related to rendering the UI element

**Pure Function**: Components are defined such that given the same state and set of props, the component should always render the same output without and unexpected results. 
- **Side Effects** interfere with this, as they involve operations not tied to rendering the UI element
#### Component Lifecycle
Components go through three phases during their lifetime:
1) Mount: The component is first created
2) Update: The component is changed or modified
3) Unmount: The component is destroyed
Side Effects are tied to one of these three phases

**useEffect**: A hook that allows us to perform side effects safely

Consists of two components:
1) **Function:** The side effect that we want to perform
	- Can specify another internal "return" function to be executed on unmount of the component
2) **Dependency Array**: What state variables will cause this hook to run
	- No dependency array: Runs on mount, unmount, and every time this component updates
	- Empty dependency array: Runs on mount and unmount
	- Filled dependency array: Runs on mount, unmount, and whenever the specified variables are updated
```
function Counter() {

	const [count, setCount] = useState(0)
	const [loaded, setLoaded] = useState(false)

	// No dependency array will run each time the component is updated
	useEffect(() => {
		// Whenever we update the counter, send an alert
		alert('Updating side effect!')
	})

	// Empty dependency array to only run on mount/unmount
	useEffect(() => {
		// On mount, make an API request and setLoaded to true
		fetch('foo').then(() => setLoaded(true))
	}, [])

	// Filled dependency array runs when "count" changes
	useEffect(() => {
		// On mount, and when count is changed, make an API request
		fetch('foo').then(() => setLoaded(true))
	}, [count])

	// Function with return statement will run on teardown
	useEffect(() => {
		return() => alert('Goodbye!')
	}, [])
	
	return (
		<div>
			<p> You clicked {count} times </p>
			{/* Button to increment the count */}
			<button onClick={() => setCount(count + 1)}>
				Click Me!
			<\button>
		</div>
	);
}
```

## References
