# Callback Function in Javascript

## Functions are Objects

The first thing we need to know is that in Javascript, functions are first class objects. As such, we can work with them in the same way that we work with other objects, such as assigning them to variables and passing them as arguments to other functions. This is important, because this last technique allows us to extend the functionality of our applications.

## Callback functions
A callback function is one that is passed as an argument to another function to be "called back" at a later time. A function that accepts other functions as arguments is called a High-Order function, and contains the logic to determine when the callback function is executed. It is the combination of these two that allows us to extend our functionality.

Short example:

```Js
function createQuote(quote, callback){ 
  var myQuote = "As I always say " + quote;
  callback(myQuote); // 2
}

function logQuote(quote){
  console.log(quote);
}

createQuote("eat your vegetables!", logQuote); // 1
```

[Try here](https://codesandbox.io/s/carrier-path-callback-example-1jksoz?file=/src/index.js:139-219)


Exercises:

- [Callback example 1](/AsynchronousJavaScriptDeepDive/Callbacks/Exercises/Callback1/)
- [Callback example 2](/AsynchronousJavaScriptDeepDive/Callbacks/Exercises/Callback2/)
- [Exercise 0](/AsynchronousJavaScriptDeepDive/Callbacks/Exercises/Exercise0/)
- [Problems](/AsynchronousJavaScriptDeepDive/Callbacks/Exercises/Problems/)

