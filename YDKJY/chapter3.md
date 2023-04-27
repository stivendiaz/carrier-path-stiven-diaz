## Chapter 3: Digging to the Roots of JS

### Iteration

An iterator provides a standardized approach to how data are consumed. It allows you to handle the data one at a time instead of all at once. These typically include loops that iterate over iterables such as arrays, strings, maps, sets, and others. This creates cleaner and easier-to-understand code.

So an iterable is a data structure or value that can be iterated over.

```
var arr = [1, 2, 3, 4];

for (let i of arr) {
  console.log(i);
}

// 1
// 2
// 3
// 4
```

### Closure

Closures are found throughout JS. So much so that they can be considered fundamental to the language like variables or functions or loops. The closure allows for the preservation of variables, observing any updates to a variable over time. it essentially keeps variables alive. Though closures can be seen throughout JS, it becomes most important when working with asynchronous code - for example callbacks. Its importance, however, can be noticed most in a function that returns a callback. A definition can be found below:

> _"Closure is when a function remembers and continues to access variables from outside its scope, even when the function is executed in a different scope"_

Some characteristics of closure include:

-   It's part of the nature of a function
-   Only functions can get closure
-   The function must be executed in a different scope than where it originates from for closure to be observed.

 ```
function outerFunction() {
  var outerVariable = "Hello ";

  function innerFunction(name) {
    var innerVariable = "world";
    console.log(outerVariable + name + " " + innerVariable);
  }

  return innerFunction;
}

var myFunction = outerFunction();
myFunction("John"); // Output: Hello John world
```


### _this_  keyword

The  _this_  keyword means different things depending on the context that it is used. When used inside a function,  _this_  exposes the function's execution context. Which refers to an actual object whose properties are available to a function while it executes. The value of  _this_  always changes depending on how the function is called. This is the biggest advocate for using  _this_. It allows you to have the ability to reuse functions with data from different objects.


```
const person = {
  name: "Bob",
  aga: 20,
  walk: () => {...}
};

function personSaysHi() {
  return `Hi ${this.name}, nice to meet you`;
}
``` 

### Prototypes

A prototype is characteristic of an object. More frankly it can be described as a way to link two objects together. And can chain these objects together (prototype chain).