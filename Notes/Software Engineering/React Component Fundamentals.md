2025-01-20 22:30

Status: #Branch

Tags: #React #WebDev #JavaScript 

# React Component Fundamentals
**Components** are reusable bits of UI that can be updated live using JavaScript 
- Each component is a [[JavaScript]] function that returns [[JSX]] markup
	- [[JSX]] combines [[HTML]] with JavaScript functionality
	- **[[JSX#JSX Expressions|JSX Expression]]:** JavaScript code embedded into JSX markup
		- Used to pass data dynamically 
		- Indicated by *curly braces*: `{ }`
- Components can contain other components, allowing for complex layouts
	- Referred to as **Parent** and **Child** components
	- The `App` component is the topmost component and contains all other components
	- **props:** Argument used to pass data from parent to child
		- Passing different props will trigger the component to re-render

This component returns a paragraph element containing the `text` passed from `props`
```jsx
function Item(props) { 
	return <p>{props.text}</p>
}

export default Item
```

We can then import this component into `app` as a child component
```jsx
import Item from './Item'
function App() {
	return <div><Item /></div>
}

export default App
```
## Using Bootstrap for Custom Components
[[Bootstrap]] is an open-source framework that provides CSS styles and JavaScript component templates to quickly build good-looking React applications. 

We can use Bootstrap to quickly build components by combining existing templates together. Once such template is the [list-group](https://getbootstrap.com/docs/5.3/components/list-group/). Visiting this link and finding the first HTML snippet we get this:
```html
<ul class="list-group">
  <li class="list-group-item">An item</li>
  <li class="list-group-item">A second item</li>
  <li class="list-group-item">A third item</li>
  <li class="list-group-item">A fourth item</li>
  <li class="list-group-item">And a fifth one</li>
</ul>
```

We can drop this markup into a React component to add a list group to our component. Note how class is changed to className, as class is a reserved keyword 
```javaScript
function MyListGroup() {
  return (
    <ul className="list-group">
      <li className="list-group-item">An item</li>
      <li className="list-group-item">A second item</li>
      <li className="list-group-item">A third item</li>
      <li className="list-group-item">A fourth item</li>
      <li className="list-group-item">And a fifth one</li>
    </ul>
  );
}
export default MyListGroup;
```
## Fragments
JavaScript functions can only return a single parent element from a component. Because of this, the following component is not allowed because it returns two elements, a header and a list group.
```js
function MyListGroup() {
  return (
	<h1> My List </h1>
    <ul className="list-group">
      <li className="list-group-item">An item</li>
      <li className="list-group-item">A second item</li>
      <li className="list-group-item">A third item</li>
      <li className="list-group-item">A fourth item</li>
      <li className="list-group-item">And a fifth one</li>
    </ul>
  );
}
export default MyListGroup;
```

We can fix this with a "div" component acting as a container:
```js
function MyListGroup() {
  return (
	<div>
		<h1> My List </h1>
		<ul className="list-group">
		  <li className="list-group-item">An item</li>
		  <li className="list-group-item">A second item</li>
		  <li className="list-group-item">A third item</li>
		  <li className="list-group-item">A fourth item</li>
		  <li className="list-group-item">And a fifth one</li>
		</ul>
	</div>
  );
}
export default MyListGroup;
```

Or we can use "fragments", empty angle brackets, to avoid adding an unnecessary element to the DOM:
```javaScript
function MyListGroup() {
  return (
	<>
		<h1> My List </h1>
		<ul className="list-group">
		  <li className="list-group-item">An item</li>
		  <li className="list-group-item">A second item</li>
		  <li className="list-group-item">A third item</li>
		  <li className="list-group-item">A fourth item</li>
		  <li className="list-group-item">And a fifth one</li>
		</ul>
	</>
  );
}
export default MyListGroup;
```
## Dynamic Rendering
In the previous examples, our list always displays the same text. We can make that dynamic by adding variables and conditions. Before the return statement, we are free to write JavaScript logic to determine what should appear on the page. 

We can accomplish this with an inline arrow function. 
- Use the `map` function to turn each "color" in "colors" into a list-group-item inside our list-group
- The className attribute lets us know to use the "list-group-item" bootstrap styling 
- The key attribute gives the list item a distinct key that we can use to reference that specific list item
- The dynamic "color" variable is inside curly braces so it renders as "White", "Blue", etc. Without the braces each list item would display the literal word "color"
```javaScript
function MyListGroup() {
  const colors = ["White", "Blue", "Black", "Red", "Green"];

  return (
    <>
      <h1>MTG Colors</h1>
      <ul className="list-group">
        {colors.map((color) => (
          <li className="list-group-item" key={color}>
            {color}
          </li>
        ))}
      </ul>
    </>
  );
}

export default MyListGroup;
```

What if we wanted to display a different list under certain conditions? Similarly to the map function in the previous example, we can also write if statements to render conditionally
- Here we use the "message" constant with an if statement. If the colors array is empty, then "message" is "No colors", otherwise it's null
- We can then display the correct message in the markup with curly braces 
```javaScript
function MyListGroup() {
  let colors = ["White", "Blue", "Black", "Red", "Green"];
  colors = [];
  // Determine message
  const message = colors.length === 0 ? <p>No colors</p> : null; 
  return (
    <>
      <h1>MTG Colors</h1>
      // Display message
      {message}
      <ul className="list-group">
        {colors.map((color) => (
          <li className="list-group-item" key={color}>
            {color}
          </li>
        ))}
      </ul>
    </>
  );
}

export default MyListGroup;
```

If we want the message to be determined by additional input, we can write a function getMessage to determine it instead. We can then call the getMessage function from within the markup.
```javaScript
function MyListGroup() {
  let colors = ["White", "Blue", "Black", "Red", "Green"];

  const getMessage = (num: number) => {
    if (num === 0) {
      return <p>Num is {num}, no colors</p>;
    } else {
      return <p>Here are the colors!</p>;
    }
  };

  return (
    <>
      <h1>MTG Colors</h1>
      {getMessage(1)}
      <ul className="list-group">
        {colors.map((color) => (
          <li className="list-group-item" key={color}>
            {color}
          </li>
        ))}
      </ul>
    </>
  );
}

export default MyListGroup;
```

For more concise code, we can write the constant/function into the markup itself:
```javascript
function MyListGroup() {
  let colors = ["White", "Blue", "Black", "Red", "Green"];
  colors = [];

  return (
    <>
      <h1>MTG Colors</h1>
      {colors.length === 0 ? <p>No colors</p> : null}
      <ul className="list-group">
        {colors.map((color) => (
          <li className="list-group-item" key={color}>
            {color}
          </li>
        ))}
      </ul>
    </>
  );
}

export default MyListGroup;
```

This pattern: "If condition, then display element, otherwise display nothing" is quite common. The best practice for this type of condition is to use the more succinct `&&` logical operator
```js
function MyListGroup() {
  let colors = ["White", "Blue", "Black", "Red", "Green"];
  colors = [];

  return (
    <>
      <h1>MTG Colors</h1>
      {colors.length === 0 && <p>No colors</p>}
      <ul className="list-group">
        {colors.map((color) => (
          <li className="list-group-item" key={color}>
            {color}
          </li>
        ))}
      </ul>
    </>
  );
}

export default MyListGroup;
```
## Events: Handling User Interaction
When the user clicks and interacts with the page, that triggers an event. We can set up our components to handle events using JavaScript functions.

In the previous example, we can create an event for when the user clicks on a list item. We can set the behavior for the "onClick" event by assigning a function to the event. In this snippet we print "Clicked + color" where color is the list item clicked on:
```jsx
function MyListGroup() {
  let colors = ["White", "Blue", "Black", "Red", "Green"];

  return (
    <>
      <h1>MTG Colors</h1>
      {colors.length === 0 && <p>No colors</p>}
      <ul className="list-group">
        {colors.map((color) => (
          <li
            className="list-group-item"
            key={color}
            onClick={() => console.log("Clicked" + color)}
          >
            {color}
          </li>
        ))}
      </ul>
    </>
  );
}

export default MyListGroup;
```

We can also define the function outside the markdown and simply assign in inside the properties of the list item:
```jsx
function MyListGroup() {
  let colors = ["White", "Blue", "Black", "Red", "Green"];
  
  const handleClick = (color: string) => console.log("Clicked" + color);

  return (
    <>
      <h1>MTG Colors</h1>
      {colors.length === 0 && <p>No colors</p>}
      <ul className="list-group">
        {colors.map((color) => (
          <li
            className="list-group-item"
            key={color}
            onClick={() => handleClick(color)}
          >
            {color}
          </li>
        ))}
      </ul>
    </>
  );
}

export default MyListGroup;
```

If we want more details, we can also access the event object itself, and even its properties. In this snippet we access the "clientX" property of event and print in to the console. This property represents the X value of the client's mouse when they clicked on the list item, triggering the event.
```jsx
import { MouseEvent } from "react";

function MyListGroup() {
  let colors = ["White", "Blue", "Black", "Red", "Green"];
  // colors = [];

  const handleClick = (event: MouseEvent) => console.log(event.clientX);

  return (
    <>
      <h1>MTG Colors</h1>
      {colors.length === 0 && <p>No colors</p>}
      <ul className="list-group">
        {colors.map((color) => (
          <li className="list-group-item" key={color} onClick={handleClick}>
            {color}
          </li>
        ))}
      </ul>
    </>
  );
}

export default MyListGroup;
```

## Managing State With the useState Hook
If we want our app to remember past user actions and events, we need to implement state in our components. We do this with the useState hook, which provides a stateful variable and method for changing that variable for us. 

The convention for calling the useState hook is:
```js
const [name, setName] = useState('John Doe') 
```
- name is the state variable
- setName is the setter function for the state variable
- The param given to useState is the default value (in this case "John Doe")

However, this state is only persistent within this individual component. If we created another instance of a component elsewhere, they would have separate instances of their state variables, therefore maintaining their own state. Changing the state of one component will not affect the state of other components of the same type.
#### Example
For example, lets say we wanted to show that a certain listItem is "selected" when the user clicks on it. Bootstrap already has a built in style for this by setting the className to "list-group-item active"
```jsx
className="list-group-item active"
```

However, this sets ALL list items to "active". But we only want it to apply to a single list item. First we need to be able to access the index of individual list items, which we can do by adding an "index" field to the map function:
```jsx
{colors.map((color, index) => (...
```

The className attribute determines whether the list item is "active" or not. We can use a ternary operator to set the className dynamically: either using "list-group-item" or "list-group-item active" depending on the selectedIndex variable
```jsx
className={
	selectedIndex === index ? "list-group-item active" : "list-group-item"
}
```

We also need to modify the onClick event to change the selectedIndex:
```js
onClick={() => { setSelectedIndex(index) }}
```

We define "selectedIndex" and "setSelectedIndex" as stateful by calling the useEffect hook before markup. 
```jsx
import { useState } from "react";

function MyListGroup() {
  let colors = ["White", "Blue", "Black", "Red", "Green"];
  const [selectedIndex, setSelectedIndex] = useState(-1);

  return (
    <>
      <h1>MTG Colors</h1>
      {colors.length === 0 && <p>No colors</p>}
      <ul className="list-group">
        {colors.map((color, index) => (
          <li
            className={
              selectedIndex === index
                ? "list-group-item active"
                : "list-group-item"
            }
            key={color}
            onClick={() => {
              setSelectedIndex(index);
            }}
          >
            {color}
          </li>
        ))}
      </ul>
    </>
  );
}

export default MyListGroup;
```

## Passing Data with Props
Ideally no data should be stored in the component itself, as then we cannot dynamically populate our components using data from parent components. For example, the listGroup example can only display the items and heading written in the component, but we can modify it to take data from elsewhere. 

However, Props are immutable. The value passed in can be read/copied but not modified in any way. If you want the component to change the value, that should be a stateful value with the useState hook, not a prop. 

TypeScript provides the "interface" feature, which allows us to design object patterns that we will pass to our components as "props", our properties. For our listGroup, we want to pass in an array of list items and a string heading:
```typescript
interface ListGroupProps {
	items : string[];
	heading : string;
}

function ListGroup(props: ListGroupProps) {
...
}
```

We can deconstruct the interface in our params for ListGroup to make accessing the props easier:
```jsx
function ListGroup({ items, heading } : listGroupProps) {
...
}
```

We also have to make sure to pass in the props from our parent function:
```jsx
import "./App.css";
import MyListGroup from "./components/MyListGroup";

function App() {
  let colors = ["White", "Blue", "Black", "Red", "Green"];

  return <MyListGroup items={colors} heading="MTG Colors" />;
}

export default App;

```

And now we can replace our listGroups specific "colors" variable to the more generic "items" variable. Our ListGroup is now fully generalized and ready to be dropped in throughout our app.
```jsx
import { useState } from "react";

interface ListGroupProps {
  items: string[];
  heading: string;
}

function MyListGroup({ items, heading }: ListGroupProps) {
  const [selectedIndex, setSelectedIndex] = useState(-1);

  return (
    <>
      <h1>{heading}</h1>
      {items.length === 0 && <p>No colors</p>}
      <ul className="list-group">
        {items.map((item, index) => (
          <li
            className={
              selectedIndex === index
                ? "list-group-item active"
                : "list-group-item"
            }
            key={item}
            onClick={() => {
              setSelectedIndex(index);
            }}
          >
            {item}
          </li>
        ))}
      </ul>
    </>
  );
}

export default MyListGroup;
```

### Typescript: Default and Input Validation
We can set a default value for a prop, so if it's not passed from the parent the default value will be used instead. For example, maybe we want the color of our button to change, but set it to the default primary color otherwise. 

We can indicate a prop is optional in our interface with a `?`, and provide the default value in the method signature:
```jsx 
interface buttonProps {
  color?: string;
  text: string;
  onClickButton: () => void;
}

function Button({ color = "primary", text, onClickButton }: buttonProps) {
	...
}
```

However, this code is flawed because we can set the `color` prop to any string, including an invalid one. We can prevent this by setting color to a set of **string literals**. If the provided value for color is not one of the literals specified, a compile error will be thrown.
```jsx
interface buttonProps {
  color?: 'primary' | 'secondary';
  text: string;
  onClickButton: () => void;
}
```
## Passing Functions with Props
We can pass data through props to pass data from parent component to child components, but we still cannot communicate with the parent component from the child component, we can only return markup. To circumvent this we can write functions in the parent component that we pass through props to the child component, who can then call the function to change data/state in the parent.

First we have to write the function in the parent. This method takes a string "item" and prints it to the console
```jsx
const handleSelectItem = (item: string) => {
	console.log(item);
};
```

We then pass this function down to the child component through props, similar to data. Here we pass the function "handleSelectItem" as the prop "onSelectItem". This convention of "handle" and "on" prefixes help distinguish prop names from function names.
```jsx
return (
	<MyListGroup
	  items={colors}
	  heading="MTG Colors"
	  onSelectItem={handleSelectItem}
	/>
);
```

We then have to modify our interface in the child and unpack the function in the params.
```jsx
interface ListGroupProps {
  items: string[];
  heading: string;
  onSelectItem: (item: string) => void;
}

function MyListGroup({ items, heading, onSelectItem }: ListGroupProps) {
```

And we have to set the onClick function to call the passed function. Now the `onClick` property cals this inline function that calls `setSelectedIndex()` and then `onSelectItem()`
```jsx
onClick={() => {
  setSelectedIndex(index);
  onSelectItem(item);
}}
```
## Passing Nested Data with Children
Sometimes we don't know exactly what data we are passing to a child component to be displayed, and need a "generic box" to use as the input. It allows us to pass any type of data (strings, lists, html, functions, images, etc) in between HTML tags for that content to be rendered no matter what it is. It essentially gives us a more generic way of passing props. 

For example, what if we wanted to be able to pass both text and HTML to be used as our header? We start by changing the interface, method signature, and the markup for heading:
```jsx
import { ReactNode, useState } from "react";

interface ListGroupProps {
  items: string[];
  onSelectItem: (item: string) => void;
  children: ReactNode;
}

function MyListGroup({ items, onSelectItem, children }: ListGroupProps) {
  const [selectedIndex, setSelectedIndex] = useState(-1);

  return (
    <>
      {children}
      ...
    </>
```

Now we can pass children as text, string, list, or even HTML:
```jsx
<MyListGroup items={colors} onSelectItem={handleSelectItem}>
  <h1>MTG Colors</h1>
</MyListGroup>

<MyListGroup items={colors} onSelectItem={handleSelectItem}>
  MTG Colors
</MyListGroup>
```
## References
