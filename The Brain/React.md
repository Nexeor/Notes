2024-10-01 17:29

Status: #Trunk

Tags: #Framework #WebDev #JavaScript 

# React
React is a [[JavaScript]] library used to create UI components for websites

Core Principles:
- **Virtual DOM**: React uses a **Virtual DOM** that is updated first and compared to the previous version, then only updating the parts of the real DOM that changed
	- Don't need to interact directly with the DOM
	- Faster updates 
- **JSX:** Gives access to [[JSX]], allowing us to write [[HTML]] within [[JavaScript]]
- **Unidirectional Data Flow:** Data is passed from parent components to child components via props
- **Hooks:** Hook functions allow for components to become functional and maintain state
## Creating a React Project
There are two typical ways of creating a new project:
- `npm create-react-app` also called "CRA"
- `npm create vite@latest`

The vite option is more commonly used as it is faster and less opinionated about configuration. Vue will then prompt you to select a framework (choose React of course) and typescript vs javascript.  

Then do the following to finish setup and install libraries:
1) `cd react-app` (or whatever the app dir is called)
2) `npm install`

We also have to make sure to install whatever CSS library we're using. For example, if we're using Bootstrap: 
- Download bootstrap extension with `npm install bootstrap`
- Import bootstrap inside `main.tsx` by replacing `import ./index.css` with `import "bootstrap/dist/css/bootstrap.css";` 
## Project Structure
There are several common features to a React project:
- `node_modules` directory: contains all the 3rd party libaries, don't touch this!
- `public` directory: contains all public assets (images, videos, etc.)
- `src` directory: contains the source code of our app
	- `app.tsx`: The main component of the app that all other components build upon
	- `main.tsx`: The entry point of the app that initializes `app.tsx`
	- `index.css`: Global style sheet
	- `app.css`: Styling for app.tsx
	- `assets` directory: assets optimized for direct use in components
- `index.html`: The html container for our app
- `package.json`: Metadata about the app and dependencies
- `tsconfig.json`: Configuration settings for typescript
- `vite.confid`: Configuration settings for vite  
## [[React Component Fundamentals]]
## [[React State]]
## [[React Forms]]
## [[Making API Requests With Axios]]
## [[Optimistic vs Pessimistic Update]]

## Styling Components with CSS
There are several different ways to style our React components, with some being more popular than others. The most common ways are:
- Vanillas CSS Files 
- CSS Modules 
- CSS-in-JS
### Vanilla CSS Files
For simple projects, using a plain [[CSS]] file can be a good option. In this case, create a plain CSS file with the same name as our React component and import it 
- If our component is "ListGroup.tsx", then add a CSS file called "ListGroup.css" and import it into the tsx file
- It's best practice to create a folder for each component that contains the txt and css files
- Vanilla CSS wont work if we're importing a CSS Library like bootstrap, so make sure to remove those first

For example, we can style our ListGroup component using Vanilla CSS instead of Bootstrap to create different list styles:
```css
.list-group {
    list-style: decimal
}
```
![[Pasted image 20250113214242.png]]

Or, if we wanted a bulleted list: 
```css
.list-group {
    list-style: circle
}
```
![[Pasted image 20250113214331.png]]
### CSS Modules
Vanilla CSS files are scoped globally, so any styling that we create for a particular CSS class will be applied to all HTML elements with that class name. This can be problamatic, because if we have two CSS classes that we wish to style differently, we will run into a name conflict. 

For example, we have two components: a "Header" and a "Sidebar". In both of these we have a HTML element with the class name "title". If we create a CSS file and attempt to style the "title" class name, then both the Header and Sidebar components will be styled the same way. 

**CSS Modules** solve this issue by creating locally scoped CSS files. We create two different CSS modules, in each of them specifying how to style "title". We then import the specific module we need into the component to style its HTML elements correctly. 
#### Creating a CSS Module
You can convert any CSS file into a module:
1) Rename the CSS file to add "module", ex: `list.css` becomes `list.module.css`
2) Update the import statements to `import styles from "./list.modules.css"`

This imports the styles as a Javascript object that we can access the fields of. So, given a css file:
```css
.list-group {
	list-style: none;
	padding: 0;
}
```

We can now provide the style we want to use as the className, accessing the style like an object in a set. This will create an HTML object with className "list-group" and use the provided styling:
```tsx
<ul className={styles['list-group']}>
```

Dashes (-) are reserved characters, but if we use camelcase we can access our classes a bit easier:
```css
.listGroup {
	list-style: none;
	padding: 0;
}
```
```tsx
<ul className={styles.listGroup}>
```
#### Multiple Stylings 
We can also provide multiple stylings for an element, which will combine the stylings together. The later stylings provided in the array determine which stylings take precedence. In this example, the final HTML element will be styled to have padding 10px, margin 15px, and color red (as `.container` appears later in the className array) 
```css
.listGroup { 
	padding: 10px; 
	color: blue; 
}

.container { 
	margin: 15px; 
	color: red; 
}
```
```jsx
<ul className={[styles.listGroup, styles.container].join(' ')}>
```
### CSS-in-JS
CSS-in-JS is a quick and dirty way of styling components where we put our CSS stylings directly within the markup for our components. This keeps our styles scoped locally, and keeps all of our CSS and JS code in the same file.
#### Setup
CSS-in-JS is not native to React so we need to do some setup first:
1) `npm install styled-components`
2) `npm install @types/styled-components` to install types (otherwise TypeScript gets mad)
3) `import styled from 'styled-components'` in the JS file we're using CSS-in-JS
#### Using CSS-in-JS
Styled works by writing custom CSS stylings for HTML components within the JS file and then invoking them in the markdown. For example, we can define one or more stylings in our JS file:
```tsx
const List = styled.ul`
	list-style: none;
	padding: 0;
`;

const ListItem = styled.li`
	padding: 5px 0;
`;
```

We are essentially defining our style as a JS object. We start with a base HTML element from the styled library (`styled.ul` or `styled.li`), and then provide the stylings we wish to apply. 

We can now replace our HTML elements with our new custom JS objects, and all the stylings will be automatically applied:
```tsx
<List>
{items.map((item, index) => (
	<ListItem
		key={item}
	>
		{item}
	</ListItem>
))}
```
#### Styling with Props and State
If our styling changed depending on passed props or state, we can use CSS-in-JS to manage it. In the previous example, what if we wanted to change the background color of a list item if we click and "select" it. 

Lets start by adding an "active" attribute to our ListItem and a corresponding state variable:
```tsx
function ListGroup({ items, heading, onSelectItem }: Props) {
  const [selectedIndex, setSelectedIndex] = useState(-1);

  return (
    <>
      <h1>{heading}</h1>
      {items.length === 0 && <p>No item found</p>}
      <List>
        {items.map((item, index) => (
          <ListItem
            active={index === selectedIndex}
            key={item}
            onClick={() => {
              setSelectedIndex(index);
              onSelectItem(item);
            }}
          >
            {item}
          </li>
        ))}
      </ul>
    </>
  );
}
```

This is a **prop** that we will pass to our list item to determine our styling. However, because this is a custom prop, we need to define our own interface for ListItems. Then, we need to change CSS-in-JS styling to take this interface as its input. In the styling, we can use that prop to dynamically determine the background color:
```tsx
interface ListItemProps {
	active: boolean;
}

const ListItem = styled.li<ListItemProps>`
  padding: 5px 0;
  background: ${(props) => (props.active ? "blue" : "none")};
`;
```


### React Icons
The popular library **React-Icons** gives us a massive selection of different icons and symbols we can drop into our website. 
- Install with: `npm install react-icons`

Using icons in our code is simple:
1) Find the icon you want on [react-icons](https://react-icons.github.io/react-icons/)
2) Click on the icon you want to copy its import
3) Import and use the icon like any other React component
	1) You can also provide props like color and size to style it


## References
