# General Terminology
1. **Arrow Function** - A shorthand form of a function consisting of `name = (parameters) => { operations; }
  - If the function contains a single operation, it doesn't need curly braces and has implicit return. Ex:
  ```hello = () => "Hello World!";```
  - Arrow functions don't bind their own `this`, instead they inherit the one from the parent scope. [See here](https://www.codementor.io/@dariogarciamoya/understanding-this-in-javascript-with-arrow-functions-gcpjwfyuc)
 
 ## React Terminology
 1. [**Functional Components**](https://www.freecodecamp.org/news/functional-components-vs-class-components-in-react/) are basic, stateless JS functions. They can be passed props as parameters though.
 2. **Class Components** are stateful and use the Component class in React. (See above link)

# General Overview
Import the following packages at the top of your `index.js` file:
```
import React from "react";
import ReactDOM from "react-dom";
```
`React` is the entry point to the React library.

When React runs a build process on your JSX, it replaces the first tag in any function return with a Reach.createElement function call.
