# General Terminology
1. **Arrow Function** - A shorthand form of a function consisting of `name = (parameters) => { operations; }
  - If the function contains a single operation, it doesn't need curly braces and has implicit return. Ex:
  ```hello = () => "Hello World!";```
  - Arrow functions don't bind their own `this`, instead they inherit the one from the parent scope. [See here](https://www.codementor.io/@dariogarciamoya/understanding-this-in-javascript-with-arrow-functions-gcpjwfyuc)
 
 ## React Terminology
 1. Components accept arbitrary inputs--or arguments--(props) and return elements describing what should appear on the screen.
    - [**Functional (or "Function") Components**](https://www.freecodecamp.org/news/functional-components-vs-class-components-in-react/) are basic, stateless JS functions. They can be passed props as parameters though.
    - **Class Components** are stateful and use the Component class in React. (See above link)
 2. **Elements** Consist of user-defined components and props. React passes JSX attributes and children to the attribute as a single object called **props**
 3. **React** is the entry point to the React library. It uses reusable, containable components to create a UI. Typically, components are built in JSX. JSX is just sytactic sugar, replacing the first tag in any function return with a React.createElement function call. Import the following packages at the top of your `index.js` file:
```
import React from "react";
import ReactDOM from "react-dom";
```
 5. **Hooks** let you use state and access React features without writing a class.
 6. **root** is the root DOM node, under which everything will be managed by React DOM

# General Overview




