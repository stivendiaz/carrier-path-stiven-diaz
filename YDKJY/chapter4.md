## Chapter 4: The Bigger Picture

### Pillar 1: Scope and Closure
####  Scope: 
Refers to the visibility and accessibility of variables and functions within your code. JavaScript has two types of scope: global scope and local scope. Global scope refers to variables and functions that are accessible throughout your entire codebase, while local scope refers to variables and functions that are only accessible within a particular function or code block.

#### Hoisting:
Refers to the behavior of moving variable and function declarations to the top of their respective scopes during the compilation phase of JavaScript code execution. This means that you can use a variable or function before it has been declared in your code, but its value will be undefined until it is actually declared.

#### Temporal Dead Zone (TDZ): 
Refers to the period between the start of a block scope and the point at which a variable is declared within that scope. During this period, the variable exists but is in an uninitialized state, so accessing it will result in a reference error. This is different from hoisting, where variables are moved to the top of their scope and initialized with a value of undefined.

**Closure**  is a natural result of lexical scope when the language has functions as first-class values, as JS does. When a function makes reference to variables from an outer scope, and that function is passed around as a value and executed in other scopes, it maintains access to its original scope variables; this is closure.

### Pillar 2: Prototypes

Prototypes are a mechanism by which objects inherit properties and methods from other objects. Every object in JavaScript has a prototype, which is another object that it inherits from.

When you attempt to access a property or method on an object, JavaScript first looks for that property or method on the object itself. If it doesn't find it there, it looks for it on the object's prototype. If it doesn't find it there, it looks on the prototype's prototype, and so on, until it either finds the property or method or reaches the end of the prototype chain.

Prototypes are created using constructor functions, which are functions that are used to create new objects. When you create a new object using a constructor function, the object's prototype is set to the constructor function's `prototype` property. You can add properties and methods to a constructor function's `prototype` property, and they will be inherited by all objects created using that constructor function.

Here's an example to illustrate how prototypes work:

```
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.sayHello = function() {
  console.log("Hello, my name is " + this.name);
};

var person1 = new Person("John", 30);
var person2 = new Person("Jane", 25);

person1.sayHello(); // Output: Hello, my name is John
person2.sayHello(); // Output: Hello, my name is Jane
```

### Pillar 3: Types and Coercion

Types: In JavaScript, values have types, not variables. The language supports several primitive data types, including numbers, strings, booleans, null, undefined, and symbols, as well as complex types like objects and arrays. Understanding the type of a value is important because it affects how you can interact with that value and what operations you can perform on it.

Coercion: Coercion refers to the automatic conversion of values from one type to another. JavaScript is a loosely typed language, which means that it will attempt to convert values between types as needed in order to perform an operation or comparison. For example, if you try to add a number and a string, JavaScript will automatically convert the number to a string and concatenate the two values.

Coercion can be either implicit or explicit. Implicit coercion occurs when JavaScript automatically converts a value between types without any explicit action by the programmer. Explicit coercion occurs when the programmer explicitly converts a value from one type to another using a conversion function or operator.

```
var x = 5;
var y = "10";

var result = x + y; // result is "510" (implicit coercion)

var num = Number("42"); // num is 42 (explicit coercion)

var bool = Boolean("hello"); // bool is true (explicit coercion)

var value = "42";
if (value == 42) {
  console.log("value is 42"); // this will be executed (implicit coercion)
}
```