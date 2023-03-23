# Understanding Asynchronous coding in Javascript

This is a mix between Asynchronous JavaScript Deep Dive course and some internet posts, in order to complement the course's content.

![took from https://www.freecodecamp.org](https://www.freecodecamp.org/news/content/images/size/w2000/2021/09/freeCodeCamp-Cover-2.png)

## So, What is JavaScript?

> JavaScript is a single-threaded, non-blocking, asynchronous, concurrent programming language.

So... How a single-threaded language can be an asynchronous language at the same time?... Lest take a look.

## Synchronous code

| Advantage                         | Disadvantage             |
| --------------------------------- | ------------------------ |
| Easy to write and to reason about | May create blocking code |
|                                   | Less performant          |

### Code example:

Input:
```
console.log('First');
console.log('Second');
console.log('Third');
```

Output:
```
First
Second
Third
```

Every instruction runs once after the previous instruction gets executed.

## Asynchronous code

| Advantage                | Disadvantage                     |
| ------------------------ | -------------------------------- |
| Very performant          | It can be harder to reason about |
| Eliminates code blocking | Harder to write                  |

### Code example:
Input:
```

console.log('First');

// Set timeout for 2 seconds
setTimeout(() => console.log('Second'), 2000);

console.log('Third');
```

Output:
```
First
Third
Second
```

Third gets printed before Second, because of the asynchronous execution of the code. Here setTimeout() function waits for 2 seconds, and in the meantime, the next instruction gets executed without waiting for the previous one to complete the execution.

[NEXT TOPIC -> Call stack](/call_stack.md)
