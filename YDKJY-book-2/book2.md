## What is Scope and some definitions

**_Scope:_**

Scope refers to the visibility and accessibility of variables, functions, and objects in JavaScript. It defines the part of your code where a particular variable or function can be accessed.

**_Lexical Scope_**

Lexical scope is also known as static scope, is a concept in JavaScript that determines the accessibility and visibility of variables and functions based on their physical location or position in the source code. The term "lexical" refers to the idea that the scope is defined by the structure of the code at the time of lexing or parsing.

In lexical scoping, the scope of a variable or function is determined by its surrounding block or function in the code, where it is physically written or defined. This means that variables and functions have access to variables in their parent or outer scopes, but not to variables in nested or inner scopes.

```js
function outer() {
  var outerVariable = "Hello";

  function inner() {
    var innerVariable = "world";
    console.log(outerVariable + " " + innerVariable);
  }

  inner();
}

outer(); // Output: Hello world
```

**_Nesting of scopes_**

Nesting of scopes: having scopes nested inside one another. It occurs when functions or blocks are defined within other functions or blocks, creating a hierarchical structure of scopes.

When a scope is nested within another scope, the nested scope has access to variables and functions defined in its outer (parent) scope. This is known as lexical scoping or scope chaining.

```js
function outer() {
  var outerVariable = "Hello";

  function inner() {
    var innerVariable = "world";
    console.log(outerVariable + " " + innerVariable);
  }

  inner(); // accessing inner scope within outer scope
}

outer(); // Output: Hello world
```

## Shadowing

Refers to the situation where a variable declared in an inner scope has the same name as a variable declared in an outer scope. The variable in the inner scope "shadows" or temporarily hides the variable in the outer scope, preventing direct access to the outer variable within the inner scope.

When a variable is shadowed, any reference to that variable within the inner scope will refer to the variable declared in the inner scope, not the one in the outer scope. This behavior can sometimes lead to unexpected results or confusion if not handled carefully.

```js
var x = 10;

function outer() {
  var x = 20; // The outer variable x is shadowed by this inner variable

  function inner() {
    var x = 30; // The outer and intermediate x variables are shadowed by this inner variable
    console.log(x); // Output: 30
  }

  inner();
}

outer();
```

> Note: Always use var for globals. Reserve let and const for block scopes.

## Scope of function expressions

**_Function declaration_**

```javascript
function nameOfFunction() {
  // ..
}
```

This type of declaration will create an identifier in its enclosing scope.

**_Function Expression_**

```javascript
var nameOfFunction = function functionIdentifier() {
  // ..
};
```

In this case the function will not hoist, also, the function identifier is declared as an identifier inside of the function itself.

**_Arrow functions_**

```javascript
var nameOfFunction = () => {
  // ..
};
```

Arrow functions follow the same lexical scope rules as regular functions do, they still create an inner scope.

## Some Global scope cases

### Scope of modules (ESM):

In JavaScript, modules introduce their own scope. This means that variables and functions defined within a module are not directly accessible from outside the module unless explicitly exported. Similarly, variables and functions defined outside the module are not directly accessible from within the module unless explicitly imported.

The scope of a module helps to keep the code within the module isolated and prevents naming conflicts with other parts of the program.

```js
// module.js
var moduleVariable = "I'm a module variable";

function moduleFunction() {
  console.log("I'm a module function");
}

export { moduleVariable, moduleFunction };
```

In this example, we have a module named module.js. It defines a variable moduleVariable and a function moduleFunction. By using the export keyword, we make these entities available for use outside the module.

Now, let's say we have another file where we import and use the module:

```js
// main.js
import { moduleVariable, moduleFunction } from "./module.js";

console.log(moduleVariable); // Output: I'm a module variable
moduleFunction(); // Output: I'm a module function
```

In main.js, we import the moduleVariable and moduleFunction from module.js using the import statement. Now, we can access and use these entities within the main.js module.

The scope of the module ensures that the variables and functions defined within it are contained within the module and can only be accessed from outside the module if explicitly exported. This helps in organizing code, preventing global namespace pollution, and promoting modular development practices.

### Global object in Node:

In Node.js, the global object refers to the top-level object in the global scope. It provides a set of globally accessible properties and functions that are available in all modules and scripts running in a Node.js environment.

The global object in Node.js is different from the global object in browsers. In browsers, the global object is typically `window`, whereas in Node.js, it is `global`.

The global object in Node.js provides several built-in properties and functions, including:

1. `console`: Provides methods for printing messages to the console, such as `console.log()` and `console.error()`.
2. `setTimeout()` and `setInterval()`: Functions for scheduling code execution after a certain delay or at regular intervals.
3. `clearTimeout()` and `clearInterval()`: Functions for canceling scheduled code execution.
4. `require()`: Function for importing modules and their functionality into a script.
5. `process`: An object that provides information and control over the current Node.js process.
6. `__filename` and `__dirname`: Strings that contain the file path and directory path of the currently executing script, respectively.
7. `Buffer`: A constructor function for creating binary data buffers.
8. `global`: A reference to the global object itself (i.e., `global.global` refers to the global object).

It's important to note that in Node.js, variables declared without the `var`, `let`, or `const` keywords in the top-level scope of a module are not added as properties to the global object. Unlike in the browser, where variables declared in the global scope become properties of the global object.

Here's an example illustrating the usage of the global object in a Node.js module:

```js
console.log(global.setTimeout); // Output: [Function: setTimeout]
console.log(global.process); // Output: [Object]

console.log(__filename); // Output: /path/to/current/script.js
console.log(__dirname); // Output: /path/to/current
```

In this example, we access the `setTimeout` and `process` properties of the global object. We also use the `__filename` and `__dirname` variables to retrieve the file path and directory path of the current script.

The global object in Node.js provides a set of commonly used properties and functions that are available throughout your Node.js application. However, it's generally recommended to limit the usage of the global object and instead use modules and proper scoping to organize and encapsulate your code.

## Variables

**_Hoisting:_**

Refers to the behavior of moving variable and function declarations to the top of their respective scopes during the compilation phase of JavaScript code execution. This means that you can use a variable or function before it has been declared in your code, but its value will be undefined until it is actually declared.

**_Function Hoisting:_**

Hoisted functions can be used before their declaration. As you can see, the JavaScript interpreter allows you to use the function before the point at which it was declared in the source code. This is extremely useful as the function can be called before defining it. One can then define the function anywhere in the program code. As can be seen from the code below, the `Func_Hoisted()` function is called before it is declared. Function declarations can be hoisted.

```js
Func_Hoisted();

function Func_Hoisted() {
  console.log("This function is hoisted!");
}
```

Output:

`This function is hoisted!`

**_var hoisting:_**

Variables are not hoisted as they cannot be used before their declaration. As shown by the code below, the variable `x` was used before it was declared. At this point, the variable `x` does not exist. Therefore, an error is thrown when you try to access the value of a variable that doesn’t exist. The value returned by `x` is undefined, as illustrated by the output:

```js
console.log(x); // undefined

var x = "Educative is amazing!";

console.log(x); // Educative is amazing!
```

**_let and const hoisting:_**

Variables declared with let and const are also hoisted, but unlike var, they are not initialized and remain in the "temporal dead zone" until the actual declaration is reached.

If you try to access a let or const variable before its declaration, you will encounter a ReferenceError.

```js
console.log(x); // ReferenceError: Cannot access 'x' before initialization
let x = 5;
```

In this example, attempting to access x before its declaration results in a ReferenceError because it is in the temporal dead zone until the line let x = 5; is executed.

## IIFEs:

IIFE stands for Immediately Invoked Function Expression. It is a JavaScript design pattern that involves wrapping a function in parentheses and immediately invoking it. The primary purpose of an IIFE is to create a new scope for the enclosed code, allowing for encapsulation and avoiding polluting the global namespace.

```javascript
// outer scope
(function () {
  // inner hidden scope
})();
// more outer scope
```

In this example, the IIFE contains a function factorial, The function is not accessible from outside the IIFE, as indicated by the ReferenceError when trying to access it in the global scope.

```javascript
var factorial = (function hideTheCache() {
  var cache = {};
  function factorial(x) {
    if (x < 2) return 1;
    if (!(x in cache)) {
      cache[x] = x * factorial(x - 1);
    }
    return cache[x];
  }
  return factorial;
})();
```

### Scoping with blocks

A block only becomes a scope if necessary, to contain its
block-scoped declarations (i.e., let or const)

```javascript
{
  // not necessarily a scope (yet)
  // ..
  // now we know the block needs to be a scope
  let thisIsNowAScope = true;
}
```

## Closures

Closures are found throughout JS. So much so that they can be considered fundamental to the language like variables or functions or loops. The closure allows for the preservation of variables, observing any updates to a variable over time. it essentially keeps variables alive. Though closures can be seen throughout JS, it becomes most important when working with asynchronous code - for example callbacks. Its importance, however, can be noticed most in a function that returns a callback. A definition can be found below:

> _"Closure is when a function remembers and continues to access variables from outside its scope, even when the function is executed in a different scope"_

Some characteristics of closure include:

- It's part of the nature of a function
- Only functions can get closure
- The function must be executed in a different scope than where it originates from for closure to be observed.

```js
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

**_Use closures for:_**

- Efficiency, allowing a function instance to hold on to previous information instead of having to compute it each time.
- Readability, limiting scope-exposure of variables and making functions more especific with out needing all the parameters in every invocation.

## The module pattern

The Module Pattern is a design pattern in JavaScript that allows for encapsulation and organization of code by creating self-contained modules. It provides a way to define private and public members, enabling data privacy and controlling access to the module's functionality.

The Module Pattern typically involves an IIFE (Immediately Invoked Function Expression) to create a new scope for the module. Within the IIFE, you define variables, functions, or objects that are accessible only within the module's scope unless explicitly exposed as public members.

**_Types of encapsulation:_**

- Namespace: Only functions without state.
- Data structures: Data and stateful functions but without limiting visibility of any of it.
- Modules: Grouping, state and control of visibility (private vs. public).

**_Classic Module example:_**

```javascript
var Module = (function () {
  // Private members
  var privateVariable = "I am private";

  function privateFunction() {
    console.log("I am a private function");
  }

  // Public members
  return {
    publicMethod: function () {
      console.log("I am a public method");
    },
    publicVariable: "I am public",
  };
})();

Module.publicMethod(); // Output: I am a public method
console.log(Module.publicVariable); // Output: I am public
console.log(Module.privateVariable); // Output: undefined
Module.privateFunction(); // Error: Module.privateFunction is not a function
```

**_CommonJs and ESM modules:_**

The ESM format shares several similarities with the CommonJS format. ESM is file-based, and module instances are _singletons_, with everything private by default.
