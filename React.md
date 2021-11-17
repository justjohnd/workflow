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
 4. **Functional Programming** is a pardigm that focuses on use of **pure functions**, and avoids **shared state**, **mutable data**, and **side-effects**. It is **declarative** (vs. imperative)
    - A **pure function** is any function is any a function that given the same input, will always result in the same output. It has no side-effects. It also has **referential transparency**, meaning replacing the function with its expected output will have no effect on the program.
    - A **shared state** is any variable, object, or memory space that exists in a shared scope. This means that functions dependent on shared-state variables are prone to race conditions, meaning the order of execution of functions will affect the output (usually a bug).
 6. A [**promise**](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261) is an object that can be returned synchonously from an asychonous function, with one of three states: Fulfilled, Rejected, or Pending. Only the function that created the promise will have knowledge of its state.
    - A simple example of a promise is:
    ```
    const wait = time => new Promise((resolve) => setTimeout(resolve, time));

    wait(3000).then(() => console.log('Hello!')); // 'Hello!'
    ```
   - The function `wait` takes an argument `time`. `wait` generates a new promise, inside of which another function is passed. This function has the argument `resolve`, which upon exectution is passed, along with `time` to the `setTimeout()` method. 
   - The promise constructor takes two parameters, `resolve()` and `reject()`. The above example does not have a `reject()` parameter defined. Note that technically, the arguments could be called anything. 
   - `resolve()` is called when the `setTimeout` function is finished.
   - A promise must always pass and return a result variable
 8. **Immutable** data is data than never mutates. Mutable data can lead to data loss. Immutability is the core concept of functional programming.
 9. A **side effect** is any application state change observable outside the function call other than its return value. Reducing side effects make an application more easy to understand and debug. Side effect actions need to be isolated from the application. 
 10. **Declarative programming** states, abstractly, what do do. Imperative programming states how to do it. Imperative functions often use statements, such as `for`, `if`, etc., while declarative functions usually call expressions, such as `map()`.
 
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

# State Hook
`useState` is a hook called inside a function to add local state to it. React preserves state between re-renders. It returns a pair: the current state, and the function used to update it. The only argument passed to `useState` is the initial state value. Ex.: `const [userCalAverage, setUserCalAverage] = useState('');`

The variable assigned to the current state, `userCalAverage`, also refers to an object. 'console.log({userCalAverage})' will return the object, while `console.log(userCalAverage)` will return the state. For this reason, when setting state with a variable that is controlled by another state, pass only the name of the variable and do not include curly braces. 

## Passing a function to your state function
The function used to update state can also accept a function when executed. This function can also be passed the previous value of the variable. So for example, if you want to make a form display if it isn't currently displayed, and vice versa, this will work:
```
  function change () {
    setFormVisibility(prevValue => {
      return !prevValue;
    });
  }
```

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

## Forms and State
State can be controlled by form data being entered by applying the `onChange` attribute to an input element. When set to execute a function controlling state, the state will change every time something is changed in the input field.

When the input element triggers the function assigned to `onChange` it also passes an object as an argument to the function. That object corresponds to the event that triggered `onChange`. The object includes a `target` corresponding to the element that triggered the change, and that target contains a `value` which corresponds to the properties assigned to the element. You can also access other `target` properties, such as `placeholder` or `type`.

You can use input data elsewhere in the component by setting state on it.

In HTML forms, `input`, `textarea`, and `select` elements are naturally responsible for handling their own state. the `value` attribute corresponds to what is currently inside the input. In React, you should set the value of the form to the property you are using to control state. This way, we have one source of truth, and that is the state. This makes it a **controlled component**. See [here](https://reactjs.org/docs/forms.html) for details on controlling elements other than `input`. It's best practice to control the state of form input at all times, and not just on `submit`.

```
function App() {

  const [name, setName] = useState("");

  function handleChange(event) {
    setName(event.target.value);
    console.log(event.target.value);
    console.log(event.target.placeholder);
    console.log(event.target.type);
  }
  
    return (
    <div className="container">
      <h1>Hello {headingText}</h1>
      <form>
        <input
          onChange={handleChange}
          type="text"
          placeholder="What's your name?"
          value={name}
        />
        <button type="submit">Submit</button>
      </form>
    </div>
  );
}

export default App;
```
Typing `John` into the input field returns in the console:
```
John
What's your name?
text
```

# Fetch API
Most browsers have a `window.fetch` object built into it, enabling us to make HTTP requests using Javascript promises. 






