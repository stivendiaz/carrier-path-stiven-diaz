### Chapter 2: Each File is a Program

A file is considered as a program that can be executed independently, this does not mean that it cannot depend on another file, or "Module".

### Values

It is the fundamental way to handle data in the application, or to maintain certain states when programming. They are also called primitive values and are the following:


- Objets 
- Numbers				
- Arrays 
- Strings			
- Booleans 
- Undefined 
- Symbol 

### Declaring and Using Variables
- var : Lets declare a variable to be used in this portion of code, and it may or may not have an initial value.   
- let : Lets declare a variable to be used, but in a smaller context than the var has, it is known as block-scoped and may or may not have an initial value.
- const : Lets declare a variable to be used in the same context that uses the let keyword, unlike let if or if it needs an initial value. This initial word does not allow the reassignment of values, but it does admit the mutation of the value in question.
- Parameter: Deja declares a variable when using a function or class, in order to refer to the value with which this function will work.
        
### Functions
    

In JS, we should consider “function” to take the broader meaning of another related term: “procedure.”
A procedure is a collection of statements that can be invoked one or more times, may be provided some inputs, and may give back one or more outputs. functions can be assigned to variables and thus also to Key within objects.

### Comparison

In JavaScript, comparison refers to the process of comparing two values to determine whether they are equal or not. There are two types of comparisons: equality comparisons and relational comparisons.

Equality comparisons are used to compare whether two values are equal or not. There are two equality operators in JavaScript: the double equals (==) and the triple equals (===) operators. The double equals operator compares two values for equality, but it performs type coercion if the two values are of different types. On the other hand, the triple equals operator compares two values for equality without performing type coercion, which means that if the two values are of different types, they will always be considered unequal.

Relational comparisons are used to compare the values of two variables to determine their relative order. There are three relational operators in JavaScript: the greater than (>) operator, the less than (<) operator, and the greater than or equal to (>=) operator, and the less than or equal to (<=) operator.


###  How we Organize in JS
    
#### Classes: 
Were introduced in ECMAScript 2015, the “class” keyword is used to create a new class with a constructor and its corresponding methods. When implementing the classes in JS, they tried to simulate the OOP, they also added the reserved words "extends", which refers to the class in question inheriting the attributes of the parent class, "super", which is used to refer to the parent class.
        
#### Modules:
In JavaScript, a module is a self-contained piece of code that encapsulates related functionality such as variables, functions, classes, and objects. Modules help organize code and make it more maintainable by allowing developers to break down large code bases into smaller, more manageable pieces. In JavaScript, there are two types of modules: the CommonJS module (Classic Module) format and the ES6 module format.

-   Classic Modules: The CommonJS module format is used in Node.js and allows developers to export and import modules using the `require` and `module.exports` statements, respectively. For example, to export a function from a module, we can use the following syntax:

```
// module.js
function add(x, y) {
  return x + y;
}

module.exports = add;
```

```
/ main.js 
const  add = require('./module'); 
console.log(add(2, 3)); // 5
```
            
-   ES Modules: The ES6 module format is a newer module format that is built into modern browsers and allows developers to export and import modules using the `export` and `import` statements, respectively. For example, to export a function from a module, we can use the following syntax:

```
// module.js
export function add(x, y) {
  return x + y;
}
```
```
// main.js
import { add } from './module';

console.log(add(2, 3)); // 5
```