## 1. **Functions as First-Class Citizens:**

In functional programming, functions are treated as first-class citizens, which means they can be assigned to variables, passed as arguments to other functions, and returned as values from functions.

```javascript
// Assigning functions to variables
const add = function (a, b) {
  return a + b;
};

const subtract = function (a, b) {
  return a - b;
};

// Passing functions as arguments
function calculate(operation, a, b) {
  return operation(a, b);
}

console.log(calculate(add, 5, 3)); // Output: 8
console.log(calculate(subtract, 10, 4)); // Output: 6

// Returning functions from functions
function createMultiplier(multiplier) {
  return function (number) {
    return number * multiplier;
  };
}

const double = createMultiplier(2);
console.log(double(5)); // Output: 10

const triple = createMultiplier(3);
console.log(triple(5)); // Output: 15
```

## 2. **Pure Functions:**

Pure functions are the cornerstone of functional programming. They always produce the same output for the same input and have no side effects, meaning they don't modify any external state or variables. Pure functions rely only on their input parameters to produce output, making them predictable, testable, and easy to reason about.

```javascript
// Pure function example
function add(a, b) {
  return a + b;
}

// Impure function example (with side effect)
let result = 0;
function addToResult(num) {
  result += num;
}
```

## 3. **Immutable Data:**

Functional programming favors immutability, meaning that once data is created, it cannot be changed. Instead of modifying existing data, functional programs create new data structures with the desired changes. Immutable data ensures that functions don't inadvertently modify shared data and helps avoid bugs related to mutable state.

```javascript
// Creating an immutable array using spread operator
const numbers = [1, 2, 3];
const updatedNumbers = [...numbers, 4]; // New array with an additional element

// Creating an immutable object using spread operator
const person = { name: "John", age: 30 };
const updatedPerson = { ...person, age: 31 }; // New object with an updated property
```

## 4. **Higher-Order Functions:**

Higher-order functions are functions that can take other functions as arguments or return functions as results. They enable powerful abstractions and allow for more flexible and reusable code.

```javascript
// Higher-order function example
function multiplyBy(num) {
  return function (x) {
    return x * num;
  };
}

const multiplyByTwo = multiplyBy(2);
console.log(multiplyByTwo(4)); // Output: 8
```

## 5. **Recursion:**

Instead of using loops for iteration, functional programming often relies on recursion, where a function calls itself until a base condition is met. Recursion is a powerful technique for solving complex problems and working with recursive data structures.

```javascript
// Recursive function to calculate the factorial of a number
function factorial(n) {
  if (n === 0) {
    return 1;
  }
  return n * factorial(n - 1);
}

console.log(factorial(5)); // Output: 120
```

## 6. **Avoiding Mutable State:**

Functional programming encourages minimizing or avoiding mutable state, as it can introduce complexity and bugs. Instead, programs focus on transforming immutable data using functions, which leads to more predictable and maintainable code.

```javascript
// Avoiding mutable state using map() to transform an array
const numbers = [1, 2, 3];
const doubledNumbers = numbers.map((num) => num * 2);

console.log(doubledNumbers); // Output: [2, 4, 6]
```

## 7. **Composition:**

Functional programming promotes composing functions together to create more complex behavior. Functions can be combined by passing the output of one function as the input to another, enabling modular and reusable code.

```javascript
// Function composition example
function add5(num) {
  return num + 5;
}

function multiplyBy2(num) {
  return num * 2;
}

const composedFunction = (num) => multiplyBy2(add5(num));
console.log(composedFunction(3)); // Output: 16
```

## 8. **No Side Effects:**

Functional programming aims to minimize or eliminate side effects, such as modifying external state, performing I/O operations, or relying on mutable global variables. By avoiding side effects, programs become easier to test, understand, and reason about.

```javascript
// Example with no side effects
function calculateSum(numbers) {
  let sum = 0;
  for (let i = 0; i < numbers.length; i++) {
    sum += numbers[i];
  }
  return sum;
}

const numbers = [1, 2, 3, 4, 5];
console.log(calculateSum(numbers)); // Output: 15
```

Remember, functional programming is just one programming paradigm, and understanding it can broaden your perspective and help you write more elegant and maintainable code, but it's not the only approach to programming.
