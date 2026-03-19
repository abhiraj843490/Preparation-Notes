

# React Lifecycle

React lifecycle = **different phases a component goes through from creation to removal**
## 1️⃣ Mounting (Component Created)

👉 When component is **added to DOM**

### Class methods:

- `constructor()`
- `render()`
- `componentDidMount()`

👉 In functional components:

```js
useEffect(() => {
  console.log("Mounted");
}, []);
```

---

## 2️⃣ Updating (Re-render)

👉 When:

- State changes
- Props change

### Class methods:

- `shouldComponentUpdate()`
- `render()`
- `componentDidUpdate()`

👉 In functional components:

```js
useEffect(() => {
  console.log("Updated");
}, [count]);
```

---

## 3️⃣ Unmounting (Component Removed)

👉 When component is **removed from DOM**
### Class method:

- `componentWillUnmount()`

👉 In functional components:

```js
useEffect(() => {
  return () => {
    console.log("Unmounted");
  };
}, []);
```

---

# 🧠 Lifecycle Flow (Easy to Remember)

👉 Mount → Update → Unmount

---

# 📊 Class vs Functional Mapping

|Phase|Class Component|Functional (Hooks)|
|---|---|---|
|Mount|componentDidMount|useEffect([])|
|Update|componentDidUpdate|useEffect([deps])|
|Unmount|componentWillUnmount|cleanup in useEffect|

---

# 💡 Real Example (Interview Level)

```js
useEffect(() => {
  console.log("Component Mounted or Updated");

  return () => {
    console.log("Cleanup before next render/unmount");
  };
}, [count]);
```

👉 Runs on:

- Mount
- When `count` changes
- Cleanup before next run / unmount

---

👉 “React lifecycle consists of mounting, updating, and unmounting phases, and in functional components, these are handled using the useEffect hook.”

---

# ⚠️ Common Interview Traps

- `useEffect([])` → runs once (mount only)
    
- No dependency → runs every render
    
- Cleanup function → runs before unmount

## React.js Components:

	are the fundamental building blocks for creating user interfaces.  
	It is reusable.

# Real DOM

	The actual DOM is what the browser creates from your HTML.
	It is a Tree structure of elements (`div`, `p`, etc.)
	Any change → browser re-renders UI
	Updates are slow because:
	    - It directly manipulates UI
	    - Reflow & repaint happen
## Virtual dom: 

	Virtual DOM = lightweight copy of Real DOM (in memory)
	 React creates a virtual representation
	Changes happen in memory first
	Then React compares (diffing) and updates only required parts.

## Diffing algorithm:

## Reconciliation process:
	virtual dom -> actual dom

	Reconciliation is the process of updating the DOM to match the structure of the React components`

	useEffect [ count ]: dependency array


## What are props in React?

- Stands for properties
- Props are a mechanism for passing data from one component to another. 
- Props are read-only attributes used to customize the behavior and appearance of components, making them more reusable and modular.
## Context API 
	Allows us to share state across components without passing props explicitly through every level of the component tree.

## Dependencies array: 
	Dependency array is the **second argument** passed to hooks like `useEffect, useMemo, useCallback`
	
## No dependencies array: 
	Runs on every render (both mount and updates).
	
```js
useEffect(() => {
  console.log("Runs every render");
});
```

## Empty dependencies array:
	Runs only on mount.
	
```js
useEffect(() => {
  console.log("Runs once");
}, []);
```

## Specific dependencies: 
	Runs when those specific dependencies change.
	
```js
useEffect(() => {
  console.log("Runs when count changes");
}, [count]);
```

## Callback function
	is a function that is passed as an argument to another function and is executed after some specific event or operation.
	
	is a function passed as a argument(props) to a child component so the child can communicate with the parent.

## HOCs
	are functions that take a component and return a new component with additional props or enhanced functionality.

  

## useCallback 
	is a hook that returns a memoized version of a callback function.

	We use useCallback and React.memo in React to optimize performance by preventing unnecessary re-renders of components.

  

## lazy loading
	is a design pattern. It allows you to load parts of your application on-demand to reduce the initial load time

  

## Custom hook
	is JavaScript function that utilize React's existing hooks (like useState, useEffect, useContext, etc.) to encapsulate and reuse across different components. 

  

## Promises 
	represent the eventual completion or failure of an asynchronous operation.

``` States: Pending, Fulfilled, Rejected```


  

NPM (Node Package Manager) is a tool for the JavaScript developers, which  providing package management, dependency resolution, version control, scripting capabilities, and package publishing.

## Event Loop:

	handles asynchronous operations and manages the execution of code

	 The event loop is responsible for continuously checking the call stack and the event queue. 

	It executes code in the call stack and processes events/callbacks in the event queue when the call stack is empty.

  

## Babel
	is a JavaScript compiler that allows developers to use the latest JavaScript features and syntax in their code

	It is a Transpiler that is used to convert the react code into the .js code to render the page on the browser.

  

## Error boundaries
	are a feature that allows you to catch JavaScript errors anywhere in our component tree

 and display a fallback UI instead of crashing the entire application.

  

```… Rest op accept an indefinite number of arguments as an array ```


  
## useEffect:

	useEffect is a hook used to handle side effects in functional components

	 Side effects mean:
	
	- API calls
	- DOM manipulation
	- Timers (`setTimeout`, `setInterval`)
	- Subscriptions

	Allows you to perform side effects in functional components. It runs after every render and replaces lifecycle methods like componentDidMount, componentDidUpdate, and componentWillUnmount.
	
```js
useEffect(() => {
  // side effect code

  return () => {
    // cleanup code (optional)
  };
}, [dependencies]);
```

  
## useMemo
	is used to memoize a computated value, helping to optimize expensive calculations or data transformations. 
	
```js
const result = useMemo(() => {
  console.log("Calculating...");
  return num * 2;
}, [num]);
```

## useCallback
	is used to memoize callback functions, reducing unnecessary re-creation of functions and optimizing component re-renders.
	
```js
const Parent = () => {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log("Clicked");
  }, []);

  return <Child onClick={handleClick} />;
};
```

Use it when:

- Passing functions to child components
- Using with `React.memo`
- Preventing unnecessary re-renders

## stateUp lifting:
	child to parent data flow by using callback function

It is used during the deserialization process to ensure that a loaded class corresponds exactly to a serialized object.

# Prop Drilling

	Prop drilling means passing data (props) from a parent component to deeply nested child components, even if intermediate components don’t need that data.
	
## Redux

	Redux is a state management library used to manage global state in applications (especially React apps).

	Data flow unidirectional

	Prop drilling (context api, useContext)
	
	Redux have separate store where we store all states of application
	
	global state management tools where we store the states of the components

	Definition: Redux is a pattern and library for managing and updating application state, using events called actions. It serves as a centralized store for state that needs to be used across our entire application, with rules ensuring that the state can only be updated in a predictable fashion.


Main topics

	Store -> Action -> Reducer -> Dispatch
	
	1. Store: Central place where store all states of application.
	
	2. Action: Plain JS object describing 'what happened'
```js
{ type: "INCREMENT" }
```
	
	3. Reducer: Reducers are functions that take the current state and an action as arguments, and return a new state result.

```js
function counterReducer(state = 0, action) {
  switch (action.type) {
    case "INCREMENT":
      return state + 1;
    default:
      return state;
  }
}
```

	4. Dispatcher: Method to send action to reducer
	
```js
dispatch({ type: "INCREMENT" });
```



	It’s important to note that you’ll only have a single store in a Redux application
	Every Redux store has a single root reducer function.

  ``` js
	Import {createStore} from “redux”;
	Const store = createStore(rootReducers);
  ```

Redux principles

- Single source of truth
- State is read-only
- Immutability, one-way data flow, Predictability of outcome
- Changes are made with pure reducer functions

## command:
	npx create-react-app redux
	npm i redux react-redux


	npx create-react-app my-app
	
## How would you copy the content of a <div> element in JavaScript?
Const var = document.querySelector(“divId”)
var.innerHTML/textContent/cloneNode(true)

  

  

What are HTTP status codes?
	numeric codes used by servers to indicate the outcome of a client's request made via HTTP 

Kafka: High through output
  

