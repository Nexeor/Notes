2025-01-17 11:09

Status:

Tags: #Branch #React #WebDev #Framework 

# React Forms
When building apps it is common that we will have to take data from our user, sometimes multiple data points at once. When a user creates an account, submits a post, or makes an advanced query, they are all submitting complex forms. Using [[Bootstrap]], we can quickly create forms for our user. 
## Basic Forms
All forms are essentially three things:
- A set of input boxes that take specific types of data
- Labels for each of those input boxes denoting their type
- A submit button

Using bootstrap, we can create a `"form-control"` element to accept the input, and a `"form-label"` element to label the input box. 
- The second `input` element has a `type` attribute used to restrict the input to numbers only
- The `input` and `label` field are captured inside a custom `mb-3` div
	- `mb-3` (margin, bottom, 3) is a custom bootstrap class meant for adding margin of size 3 to the bottom of this div
- The `form` HTML element means that all `input` elements will be captured for form submission later
- The `button` HTML element is given type `submit`, denoting that it is submit button for the form
```tsx
const Form = () => {
  return (
    <form>
      {" "}
      <div className="mb-3">
        <label htmlFor="name" className="form-label">
          Name
        </label>
        <input id="name" className="form-control"></input>
      </div>
      <div className="mb-3">
        <label htmlFor="age" className="form-label">
          Age
        </label>
        <input id="age" type="number" className="form-control" />
      </div>
        <button className="btn btn-primary" type="submit">
			Submit
	    </button>
    </form>
  );
};
```

 Submitting the form is an event called `onSubmit` (similar to `onClick`), and we can handle it the same way by defining a handleSubmit function
- Default behavior has the page reload after submitting a form, however we can prevent his behavior by calling `preventDefault()` on the event object
	- This allows our log statement to print because the page no longer reloads
```tsx
import React from "react";

const Form = () => {
  return (
    <form
      onSubmit={(event) => {
        event.preventDefault();
        console.log("Submitted");
      }}
    >
      {" "}
      <div className="mb-3">
        <label htmlFor="name" className="form-label">
          Name
        </label>
        <input id="name" className="form-control"></input>
      </div>
      <div className="mb-3">
        <label htmlFor="age" className="form-label">
          Age
        </label>
        <input id="age" type="number" className="form-control" />
      </div>
      <button className="btn btn-primary" type="submit">
        Submit
      </button> 
    </form>
  );
};

export default Form;
```

If the submit logic becomes complex we can split it off into it's own function within the Form component:
```tsx
import React from "react";

const Form = () => {
  const handleSubmit = (event: FormEvent) => {
	event.preventDefault();
	console.log("Submitted");
  }
  return (
    <form onSubmit={handleSubmit}>
	...
```

## Accessing Input Fields
### useRef Hook
We can access our input fields by utilizing the [[useRef Hook]] hook
- We have to do a null check for `nameRef.current` to make sure the corresponding DOM element is loaded in before accessing its value

```tsx
const Form = () => {
  const nameRef = useRef<HTMLInputElement>(null);
  const ageRef = useRef<HTMLInputElement>(null);

  const handleSubmit = (event: FormEvent) => {
    event.preventDefault();
    if (nameRef.current !== null) console.log(nameRef.current.value);
    if (ageRef.current !== null) console.log(ageRef.current.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      {" "}
      <div className="mb-3">
        <label htmlFor="name" className="form-label">
          Name
        </label>
        <input id="name" ref={nameRef} className="form-control"></input>
      </div>
      <div className="mb-3">
        <label htmlFor="age" className="form-label">
          Age
        </label>
        <input id="age" ref={ageRef} type="number" className="form-control" />
      </div>
      <button className="btn btn-primary" type="submit">
        Submit
      </button>
    </form>
  );
};
```

Usually we will want to capture the data as a single object to send to a backend, API, or other function, so its best practice to gather form submissions as a single object:
```jsx
const nameRef = useRef<HTMLInputElement>(null);
const ageRef = useRef<HTMLInputElement>(null);
const person = { name: "", age: 0 };

const handleSubmit = (event: FormEvent) => {
	event.preventDefault();
	if (nameRef.current !== null) person.name = nameRef.current.value;
	if (ageRef.current !== null) person.age = Number(ageRef.current.value);
	console.log(person);
};
```
### useState Hook
We can also use the useState hook to access our fields. This way, each time the user inputs a keystroke in the input field it will update a stateful variable tracking that input field. However, this does result in the component being re-rendered each time a value is changed
- Create a stateful object to represent the form input
- In each `input` object:
	- Set the `onChange` attribute to an arrow function calling the set function for the object
	- Set the `value` attribute to the corresponding field in our object
		- This means that we only rely on our stateful object for the value, not the DOM

**Advantages of useState**
- Cleaner looking code with less boilerplate
- Real-time access to the values stored in the input fields (not just when the user clicks submit)

**Advantages of useRef**
- More performant for large forms
- Doesn't trigger re-renders of the component

In this form we set the `onChange` event of the `input` element to call `setPerson` and update the `person` object
```jsx
  const [person, setPerson] = useState({ name: "", age: "" });

  const handleSubmit = (event: FormEvent) => {
    event.preventDefault();
    console.log(person);
  };

  return (
    <form onSubmit={handleSubmit}>
      {" "}
      <div className="mb-3">
        <label htmlFor="name" className="form-label">
          Name
        </label>
        <input
          id="name"
          onChange={(event) =>
            setPerson({ ...person, name: event.target.value })
          }
          value={person.name}
          className="form-control"
        ></input>
      </div>
      <div className="mb-3">
        <label htmlFor="age" className="form-label">
          Age
        </label>
        <input
          id="age"
          onChange={(event) =>
            setPerson({ ...person, age: Number(event.target.value) })
          }
          value={person.age}
          type="number"
          className="form-control"
        />
      </div>
      <button className="btn btn-primary" type="submit">
        Submit
      </button>
    </form>
  );
```
## Managing Form State with React Hook Form Library
First make sure React Hook Form is installed: `npm i react-hook-form`
### Simplifying Submit Logic
Now we can use React-Hook-Form to write much simpler submit logic:
- Call the `useForm()` hook and get a `register` and `handleSubmit` function
- In each input field, spread the `register` function, providing the name of that input field as a parameter
- Change the `onSubmit` event of the `form` element to call `handleSubmit`, providing our custom `onSubmit` function as a parameter to it

Here is the same form modified to use the library:
```tsx
const Form = () => {
  const { register, handleSubmit } = useForm();

  const onSubmit = (data: FieldValues) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {" "}
      <div className="mb-3">
        <label htmlFor="name" className="form-label">
          Name
        </label>
        <input
          {...register("name")}
          id="name"
          type="text"
          className="form-control"
        ></input>
      </div>
      <div className="mb-3">
        <label htmlFor="age" className="form-label">
          Age
        </label>
        <input
          {...register("age")}
          id="age"
          type="number"
          className="form-control"
        />
      </div>
      <button className="btn btn-primary" type="submit">
        Submit
      </button>
    </form>
  );
};
```
### Data Validation
We can also use this library to apply data validation to our user when they submit the form:
1) Define the shape of the form using an interface with well defined types
2) Get the `errors` value from the `formstate` property derived from `useForm` 
	- This value gives us all the validation rules invalidated
3) Add a list of properties that must be true as a parameter to `register` for each `input` element
	- `required` means this input must be filled
	- `minLength` and `maxLength` set max/min on number of characters

We modify our form to apply data validation to the `name` property. `name` is now a required input and must be at least three characters
- After the `input` element we add a new `paragraph` element that conditionally renders when a validation rule is broken
- `errors.name?.type === 'required' &&` only renders the error message if the `errors` property has a attribute `type` and only if `type === required`
- 
```tsx
import { FieldValues, useForm } from "react-hook-form";

interface FormData {
  name: String;
  age: Number;
}

const Form = () => {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm<FormData>();

  const onSubmit = (data: FieldValues) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {" "}
      <div className="mb-3">
        <label htmlFor="name" className="form-label">
          Name
        </label>
        <input
          {...register("name", { required: true, minLength: 3 })}
          id="name"
          type="text"
          className="form-control"
        />
        {errors.name?.type === "required" && (
          <p className="text-danger">The name field is required</p>
        )}
        {errors.name?.type === "minLength" && (
          <p className="text-danger">Name must be at least 3 characters</p>
        )}
      </div>
      <div className="mb-3">
        <label htmlFor="age" className="form-label">
          Age
        </label>
        <input
          {...register("age")}
          id="age"
          type="number"
          className="form-control"
        />
      </div>
      <button className="btn btn-primary" type="submit">
        Submit
      </button>
    </form>
  );
};

export default Form;

```
### Schema Based Validation With Zod
As our validation rules get more complex, handling it in the markup becomes inconvenient. Using the **Zod** library we can create a **schema** for our validation rules
- Install Zod: `npm i zod`
- Install Zod + ReactHookForm integration: `npm i @hookform/resolvers`

Zod allows us to set a `schema` object that defines our forms fields and validation rules
- Each field is written as a chain of function calls defining its type and rules
	- Name must be a string with a min of three characters
	- Age must be a number that's 18 or greater
- We can provide a custom error message as a parameter to each rule
	- Zod will automatically generate error messages if we don't provide one here
```tsx
const schema = z.object({
  name: z.string().min(3, "Name must be at least three characters"),
  age: z.number({ invalid_type_error: "Age field is required" }).min(18),
});
```

We can combine our interface defining the form with this schema:
```tsx
type FormData = z.infer<typeof schema>;
```

And use the schema to detect formState and errors:
```tsx
const Form = () => {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm<FormData>({ resolver: zodResolver(schema) });
  ...
```

Now after each input we can just list a single dynamic statement to render the error message:
```tsx
{errors.name && <p className="text-danger">{errors.name.message}</p>}
```
### Disabling the Submit Button
We can use our data validation rules to dynamically disable the button if the input is invalid by using the `isValid` attribute of `formState`.

```tsx
const {
	register,
	handleSubmit,
	formState: { errors, isValid },
} = useForm<FormData>({ resolver: zodResolver(schema) });
```

Now set the button's `disabled` property to true when `isValid` is false:
```jsx
<button disabled={!isValid} className="btn btn-primary" type="submit">
	Submit
</button>
```

## References
