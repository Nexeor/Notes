2025-01-07 17:47

Status: #Leaf

Tags: #WebDev #JavaScript #HTML 

# JSX
JSX, or JavaScript XML, is a markup language designed to mesh [[HTML]] structure with [[JavaScript]] dynamism. It is primarily used in the [[React]] framework.
- [[HTML]] is static. If we want components to be displayed dynamically we need a real coding language
- [[JavaScript]] is verbose and repetitive. Components created with only JavaScript are unreadable and difficult to maintain. 

JSX combines the best of both worlds, using the structure and component of [[HTML]] but rendering them dynamically with [[JavaScript]].  JSX compiles to [[JavaScript]] after being returned from the component.

```js
// JSX:
return(
	<Header />
	<Main />
)

// Decompiled into JS:
/*#__PURE__*/_jsxs(_Fragment, {
  children: [/*#__PURE__*/_jsx(Header, {}), /*#__PURE__*/_jsx(Main, {})]
});
```
## JSX Expressions
Indicated with a pair of curly braces `{ }`, **JSX Expressions** embed [[JavaScript]] code directly inside  JSX snippets. evaluate to a value and render as part of the JSX output. 

JSX expressions can render primitive values, variables, conditionals, lists, and function calls:
```jsx
{1 + 2}                // → 3
{user.name}            // → evaluates to user's name
{isLoggedIn ? "Yes" : "No"}  // → conditional rendering
{items.map(item => <li>{item}</li>)} // → lists

function getGreeting(name: string) {
  return `Hello, ${name}`;
}

const name = "Tim";
return <h1>{getGreeting(name)}</h1>;  // → "Hello, Tim"
```
## References
