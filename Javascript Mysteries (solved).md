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
