2024-10-01 17:29

Status: #Trunk

Tags: #React #WebDev #Frontend

# React
React is a [[JavaScript]] library used to create UI components for websites

Core Principles:
- **Virtual DOM**: React uses a **Virtual DOM** that is updated first and compared to the previous version, then only updating the parts of the real DOM that changed
	- Don't need to interact directly with the DOM
	- Faster updates 
- **JSX:** Gives access to [[JSX]], allowing us to write [[HTML]] within [[JavaScript]]
- **Unidirectional Data Flow:** Data is passed from parent components to child components via props
- **Hooks:** Hook functions allow for components to become functional and maintain state
## Starting Local Dev Server With Vite
 [[Vite]] is a performant tool for creating local dev servers. It can also be used to bundle the app for production. 

**JavaScript** React project:
```bash
npm create vite@latest my-app --template react
```

**TypeScript** React project:
```bash
npm create vite@latest my-app --template react-ts
```

[[Vite]] will then open a [[CLI]] menu where we can select what type of project we would like to create (select `React`) and then ask whether we want JavaScript/TypeScript and SWC.
- **Speedy Web Compiler (SWC)**: A replacement for the TypeScript compiler that is significantly faster, but doesn't support type checking

Finally, install the needed `node.js` modules and start the dev server:
```bash
cd <app_name>
npm install
npm run dev
```
## React Core:
- ### [[React Component Fundamentals]]
- ### [[React State]]
## React Libraries:
- ### [[Making API Requests With Axios]]
- ### [[React Forms]]
- ### [[Bootstrap]]
## References
