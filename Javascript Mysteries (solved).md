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
    - A **pure function** is any a function that given the same input, will always result in the same output. It has no side-effects. It also has **referential transparency**, meaning replacing the function with its expected output will have no effect on the program.
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
 11. **CRUD**: Create, Read, Update, Destroy.

# How does a hash work?
Consider the example (from Elliott Hindman's blog):
```
const arr = [1,2,3,4,5,6,7,7,8,6,10];
const findDupes = (arr) => {
  const observed = {};
  for(let i = 0; i < arr.length; i++) {
    if(observed[arr[i]]) {
      return arr[i]
    } else {
      observed[arr[i]] = arr[i];
    }
  }
  
  return false;
}
console.log(findDupes(arr)); // Returns 7
```
`observed` is an object because its type is defined by `{}`. Therefore any data inside will be in the form of a `key: value` pair. 

`observed[0]` refers to the value with a key of `0`. Note that bracket notation required quotation marks insdie the brackets for non-numeric names. For example `observed['a']` will look up the value for key `a`, however `observed[a]` will return `undefined`

The for loop will look to see if there are any keys assigned the number associated with `[arr[i]]`. If so, the duplicate has been found.

Otherwise a new `key: value` pair will be created: `arr[i]: arr[i]`
