# General Terminology
1. **Arrow Function** - A shorthand form of a function consisting of `name = (parameters) => { operations; }
  - If the function contains a single operation, it doesn't need curly braces and has implicit return. Ex:
  ```hello = () => "Hello World!";```
  - Arrow functions don't bind their own `this`, instead they inherit the one from the parent scope. [See here](https://www.codementor.io/@dariogarciamoya/understanding-this-in-javascript-with-arrow-functions-gcpjwfyuc)
 2. [**Destructuring**](https://www.freecodecamp.org/news/javascript-object-destructuring-spread-operator-rest-parameter/) allows you to easily create variables from object properties or array values. 
 3. **Object destructuring** key/value pairs is achieved by setting the variable name to a key name--inside curly braces--and making it equal to the object name.
    - New variables can be added directly to the curly braces, and be given a default value. The default value will only be assigned though, if the key does not already exist in the object.
       ```
      const user = { 
          'name': 'Alex',
          'address': '15th Park Avenue',
          'age': 43
      }
      const { name, age, salary=123455 } = user;
      
      console.log(name, age, salary); // Output, Alex 43 123455
      ```
    - You can also destructure a function parameter:
      ```
      function logDetails({name, age}) {
      console.log(`${name} is ${age} year(s) old!`)
      }

      logDetails(user); 
      ```
    - [**Array Destructuring**](https://zellwk.com/blog/es6/#destructuring) uses brackets to assign variable names to index values according their index placement.
    ```
    let [one, two] = [1, 2, 3, 4, 5]
    console.log(one) // 1
    console.log(two) // 2
    ```
    
 
 ## React Terminology
 1.  2. **Elements** Are the smallest units of React. They are objects and describe what will be shown on the screen. They are immutable. 
 2. Components accept arbitrary inputs--or arguments--(props) and return elements describing what should appear on the screen. React passes JSX attributes and children to components as a single object called **props**
    - [**Functional (or "Function") Components**](https://www.freecodecamp.org/news/functional-components-vs-class-components-in-react/) are basic, stateless JS functions. They can be passed props as parameters though.
    - **Class Components** are stateful and use the Component class in React. (See above link)
 3. **React** is the entry point to the React library. It uses reusable, containable components to create a UI. Typically, components are built in JSX. JSX is just sytactic sugar, replacing the first tag in any function return with a React.createElement function call. Import the following packages at the top of your `index.js` file:
    ```
    import React from "react";
    import ReactDOM from "react-dom";
    ```
 4. **root** is the root DOM node, under which everything will be managed by React DOM. To render a React element or component into a Root DOM node, use the following:
    ```
    import App from './components/App';
    ReactDOM.render(<App />, document.getElementById('root'));
    ```
 5. **State** is private and fully controlled by the component. **Hooks** are functions that let you use state and access React features without writing a class.

# State Hook
`useState` is a hook called inside a function to add local state to it. React preserves state between re-renders. It returns a pair: the current state, and the function used to update it. The only argument passed to `useState` is the initial state value. Ex.: `const [userCalAverage, setUserCalAverage] = useState('');`

## Passing functions as props
You may need to access and manipulate data from one component and pass it to another one. In this case, you could preserve that data state in a parent component that shares two children. Example:

 - I want to get an array from child component chain `Usersection` -> `UserCards`, calculate the average of array values, then pass it to another child component `MealPlans`. Both child coponents `UserCards` and `MealPlans` share a parent `App`. Basic structure:
```
function App() {
  return (
    <div>
      <UserSection/>
      <MealPlan/>
    </div>
  );
}
```
 - `UserCards` array data is updated in the child, but `MealPlans` will display the average, so `MealPlans` state will need to be changed. Therefore state must be controlled in the parent and state value passed down as a prop to `MealPlan`.
```
const [userCalAverage, setUserCalAverage] = useState('');
```
 - A function will need to caluculate the average and render a new state whenver the array changes:
 ```
   function addUserCal(userCalArray) {
    const calAverage = (
      userCalArray.reduce((acc, cur) => acc + cur) / userCalArray.length
    ).toFixed(0);
    setUserCalAverage(calAverage);
  }
  ```
  - The function will need to be passed as a prop down to the child `UserCards` (`<UserSection addUserCal={addUserCal} />`). In the child component, the function will be executed:
```
{props.addUserCal(userCalArray)}
```
where the argument `userCalArray` is declared in the child component. 
- The child triggers the function in the parent. From here, the state-controlled value will be sent to the other childe `MealPlan` via props: `<MealPlan userCalAverage={userCalAverage} />`

So the `App` component in its entirity looks like this:
```
function App() {
  const [userCalAverage, setUserCalAverage] = useState('');

  function addUserCal(userCalArray) {
    const calAverage = (
      userCalArray.reduce((acc, cur) => acc + cur) / userCalArray.length
    ).toFixed(0);
    setUserCalAverage(calAverage);
  }

  return (
    <div>
      <UserSection addUserCal={addUserCal} />
      <MealPlan userCalAverage={userCalAverage} />
    </div>
  );
}

export default App;
```

# Props
Props are always passed down from a parent component to a child component. In the following example, `Welcome` is a child component of `element`, and `element` passes the attribute `name` as a  prop value of `"Sara"` down to `Welcome`
```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```



