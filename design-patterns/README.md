## 1. **Singleton Pattern:**

The Singleton pattern ensures that only one instance of a class is created and provides a global point of access to that instance. It is useful when you want to restrict the instantiation of a class to a single object. Examples include managing global configuration settings or creating a logger instance.

```javascript
const Singleton = (function () {
  let instance;

  function createInstance() {
    // Private members
    const privateVariable = "I am private";

    return {
      // Public methods and variables
      publicMethod: function () {
        console.log("I am a public method");
      },
      publicVariable: "I am a public variable",
      privateVariable: privateVariable,
    };
  }

  return {
    getInstance: function () {
      if (!instance) {
        instance = createInstance();
      }
      return instance;
    },
  };
})();

const singletonInstance1 = Singleton.getInstance();
const singletonInstance2 = Singleton.getInstance();

console.log(singletonInstance1 === singletonInstance2); // Output: true
```

## 2. **Factory Pattern:**

The Factory pattern provides an interface for creating objects without specifying the exact class of the object that will be created. It encapsulates object creation logic and allows flexibility in object creation based on different conditions or parameters. It promotes loose coupling and separates object creation from the client code.

```javascript
function Circle(radius) {
  this.radius = radius;
}

Circle.prototype.getArea = function () {
  return Math.PI * Math.pow(this.radius, 2);
};

function Square(side) {
  this.side = side;
}

Square.prototype.getArea = function () {
  return Math.pow(this.side, 2);
};

function ShapeFactory() {}

ShapeFactory.prototype.createShape = function (type, ...args) {
  if (type === "circle") {
    return new Circle(...args);
  } else if (type === "square") {
    return new Square(...args);
  }
};

const factory = new ShapeFactory();
const circle = factory.createShape("circle", 5);
console.log(circle.getArea()); // Output: 78.53981633974483

const square = factory.createShape("square", 4);
console.log(square.getArea()); // Output: 16
```

## **Observer Pattern:**

The Observer pattern establishes a relationship between objects where one object (the subject) maintains a list of its dependents (observers) and notifies them of any state changes. It enables objects to communicate and stay in sync without having direct dependencies on each other. This pattern is commonly used in event-driven systems or to implement publish-subscribe systems.

```javascript
class Subject {
  constructor() {
    this.observers = [];
  }

  addObserver(observer) {
    this.observers.push(observer);
  }

  removeObserver(observer) {
    this.observers = this.observers.filter((obs) => obs !== observer);
  }

  notifyObservers() {
    this.observers.forEach((observer) => observer.update());
  }
}

class Observer {
  constructor(name) {
    this.name = name;
  }

  update() {
    console.log(`${this.name} has been notified.`);
  }
}

const subject = new Subject();

const observer1 = new Observer("Observer 1");
const observer2 = new Observer("Observer 2");

subject.addObserver(observer1);
subject.addObserver(observer2);

subject.notifyObservers();
// Output:
// Observer 1 has been notified.
// Observer 2 has been notified.
```

## 4. **Module Pattern:**

The Module pattern enables encapsulation and information hiding by organizing code into self-contained modules with private and public methods and properties. It helps prevent naming conflicts and provides a clean way to expose only necessary functionality to other parts of the code. Modules can be implemented using IIFE (Immediately Invoked Function Expression) or using modern JavaScript module syntax (ES Modules).

```javascript
const CounterModule = (function () {
  let count = 0;

  function increment() {
    count++;
  }

  function decrement() {
    count--;
  }

  function getCount() {
    return count;
  }

  return {
    increment: increment,
    decrement: decrement,
    getCount: getCount,
  };
})();

CounterModule.increment();
console.log(CounterModule.getCount()); // Output: 1

CounterModule.decrement();
console.log(CounterModule.getCount()); // Output: 0
```

## 5. **Prototype Pattern:**

The Prototype pattern creates objects by cloning or copying existing objects, known as prototypes. It avoids the need for creating objects from scratch by utilizing a prototype as a blueprint. This pattern is particularly useful when creating multiple similar objects with slight variations or when object creation is expensive.

```js
function Shape() {}
Shape.prototype.getArea = function () {
  return "Unknown shape";
};

function Circle(radius) {
  this.radius = radius;
}
Circle.prototype = Object.create(Shape.prototype);
Circle.prototype.constructor = Circle;
Circle.prototype.getArea = function () {
  return Math.PI * Math.pow(this.radius, 2);
};

function Square(side) {
  this.side = side;
}
Square.prototype = Object.create(Shape.prototype);
Square.prototype.constructor = Square;
Square.prototype.getArea = function () {
  return Math.pow(this.side, 2);
};

const circle = new Circle(5);
console.log(circle.getArea()); // Output: 78.53981633974483

const square = new Square(4);
console.log(square.getArea()); // Output: 16
```

## 6. **Decorator Pattern:**

The Decorator pattern allows adding new functionality or modifying behavior of an existing object dynamically, without changing its structure. It involves wrapping the original object with one or more decorator objects, which provide additional features or modify existing ones. This pattern promotes flexibility and avoids the need to create subclasses for every possible combination of features.

```js
function Pizza() {
  this.getDescription = function () {
    return "Pizza";
  };
  this.getCost = function () {
    return 10;
  };
}

function ExtraCheese(pizza) {
  this.getDescription = function () {
    return pizza.getDescription() + ", Extra Cheese";
  };
  this.getCost = function () {
    return pizza.getCost() + 2;
  };
}

function Jalapenos(pizza) {
  this.getDescription = function () {
    return pizza.getDescription() + ", Jalapenos";
  };
  this.getCost = function () {
    return pizza.getCost() + 1.5;
  };
}

let myPizza = new Pizza();
myPizza = new ExtraCheese(myPizza);
myPizza = new Jalapenos(myPizza);

console.log(myPizza.getDescription()); // Output: Pizza, Extra Cheese, Jalapenos
console.log(myPizza.getCost()); // Output: 13.5
```
