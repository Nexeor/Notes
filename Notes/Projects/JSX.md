2025-01-07 17:47

Status: #Leaf

Tags: #WebDev #JavaScript #HTML

# JSX
JSX, or JavaScript XML, is a markup language designed to mesh [[HTML]] structure with [[JavaScript]] dynamism. It is primarily used in the [[React]] framework.
- [[HTML]] is static. If we want components to be displayed dynamically we need a real coding language
- [[JavaScript]] is verbose and repetitive. Components created with only JavaScript are unreadable and difficult to maintain. 

JSX combines the best of both worlds, using the structure and component of [[HTML]] rendering them dynamically with [[JavaScript]]. However, JSX is simply just fancy JavaScript, and even is compiled down to JavaScript after being returned from a component. 

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
## References
