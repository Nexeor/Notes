2025-01-17 13:00

Status: #Leaf

Tags: #React #WebDev #Framework #HTML 

# useRef Hook
useRef is an important React hook that gives us a mutable reference to either:
- A DOM Element: Accessing the values of DOM elements
- A value: Create a mutable value that persists across re-renders but doesn't trigger re-renders
## Accessing DOM Elements
The useRef hook allows us to get a direct reference to a DOM element. If that DOM element has a value, we then have access to that value.

In this example we use useRef to get a reference to the `input` HTML element. From there we can access the `value` field of current to get the value stored in the DOM
- `nameRef` is given the HTML element that it will reference in angle brackets
- We pass `null` as a param to useRef because useRef will be created before the DOM is created. When the `input` field is created and displayed on screen React will automatically set `nameRef` to the corresponding DOM element
- We pass `nameRef` as the `ref` attribute for `input`
```jsx
const Form = () => {
  const nameRef = useRef<HTMLInputElement>(null);

  const handleSubmit = (event: FormEvent) => {
    event.preventDefault();
    if (nameRef.current !== null) console.log(nameRef.current.value);
  };

  return (
    <form onSubmit={handleSubmit}>
	  <label htmlFor="name" className="form-label">
	    Name
	  </label>
	  <input id="name" ref={nameRef} className="form-control"></input>
      <button className="btn btn-primary" type="submit">
        Submit
      </button>
    </form>
  );
};
```

## Storing Mutable Values
The `useState` hook is most common for persistent values, but any updates to a `useState` value causes a re-render. If we want to store a mutable value in between renders, but not have it trigger a re-render itself when it updated, we can use the `useRef` hook. HOWEVER, it is much preferred to choose the `useState` hook when possible.

```jsx
const countRef = useRef(0);

const increment = () => {
  countRef.current += 1; // Updates ref value without re-rendering
};
```

## References
