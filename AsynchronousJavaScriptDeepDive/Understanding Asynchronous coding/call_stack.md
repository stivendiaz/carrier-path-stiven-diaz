Information took form [https://www.freecodecamp.org/news/synchronous-vs-asynchronous-in-javascript/](https://www.freecodecamp.org/news/synchronous-vs-asynchronous-in-javascript/)


# Synchronous JavaScript – How the Function Execution Stack Works

1. When the JavaScript engine invokes a function, it adds it to the stack, and the execution starts.
2. If the currently executed function calls another function, the engine adds the second function to the stack and starts executing it.
3. Once it finishes executing the second function, the engine takes it out from the stack.
4. The control goes back to resume the execution of the first function from the point it left it last time.
5. Once the execution of the first function is over, the engine takes it out of the stack.
6. Continue the same way until there is nothing to put into the stack.

![Call stack](https://www.freecodecamp.org/news/content/images/size/w1000/2021/09/stack.png)

```
function f1() {
  // some code
}
function f2() {
  // some code
}
function f3() {
  // some code
}

// Invoke the functions one by one
f1();
f2();
f3();
```

Behavior:

![first-flow.gif](https://www.freecodecamp.org/news/content/images/2021/09/first-flow.gif)

1. f1() goes into the stack, executes, and pops out. 
2. f2() does the same
3. finally f3() does the same
4. After that, the stack is empty, with nothing else to execute.

[Try it!](https://codesandbox.io/s/carrier-path-callstack1-ve0vo1?file=/src/index.js)

Here is a function f3() that invokes another function f2() that in turn invokes another function f1():

```
function f1() {
  // Some code
}
function f2() {
  f1();
}
function f3() {
  f2();
}
f3();
```
Behavior:

![https://www.freecodecamp.org/news/content/images/2021/09/second-flow.gif](https://www.freecodecamp.org/news/content/images/2021/09/second-flow.gif)

1. f3() gets into the stack, invoking another function, f2(). 
2. So now f2() gets inside while f3() remains in the stack. 
3. The f2() function invokes f1().
4. So, time for f1() to go inside the stack with both f2() and f3() remaining inside.

> f1() finishes executing and comes out of the stack. Right after that f2() finishes, and finally f3().

[Try it](https://codesandbox.io/s/carrier-path-callstack2-hu1mb9)

[NEXT TOPIC -> Event Loop](/event_loop)