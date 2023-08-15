## Some Hooks

### useRef:

The `useRef` hook in React.js is primarily used to access and manipulate the DOM directly, as well as to maintain mutable values across renders without causing re-renders.

**Example 1: Accessing DOM Elements**

You can use `useRef` to get a reference to a DOM element and then manipulate it.

```jsx
import React, { useRef, useEffect } from "react";

function FocusInput() {
  const inputRef = useRef(null);

  useEffect(() => {
    // Focus the input element when the component mounts
    inputRef.current.focus();
  }, []);

  return <input ref={inputRef} type="text" />;
}

export default FocusInput;
```

**Example 2: Storing and Maintaining Values**

`useRef` can also be used to store and maintain values across renders without causing re-renders.

```jsx
import React, { useRef, useState } from "react";

function Counter() {
  const countRef = useRef(0); // Initialize with initial value
  const [count, setCount] = useState(0);

  const increment = () => {
    countRef.current += 1; // Update without causing re-render
    setCount(countRef.current);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}

export default Counter;
```

**Example 3: Keeping Previous State**

`useRef` can also help in maintaining previous state values without causing re-renders.

```jsx
import React, { useRef, useState, useEffect } from "react";

function PreviousState() {
  const [count, setCount] = useState(0);
  const prevCountRef = useRef();

  useEffect(() => {
    prevCountRef.current = count;
  }, [count]);

  const prevCount = prevCountRef.current;

  return (
    <div>
      <p>Current Count: {count}</p>
      <p>Previous Count: {prevCount !== undefined ? prevCount : "N/A"}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default PreviousState;
```

### useReducer:

The `useReducer` hook in React is used for managing more complex state logic, especially when state transitions involve multiple sub-values or when the next state depends on the previous state. It's often used as an alternative to the `useState` hook when the state logic becomes more intricate.

**Example 1: Simple Counter**

Let's create a simple counter using the `useReducer` hook.

```jsx
import React, { useReducer } from "react";

const initialState = { count: 0 };

const reducer = (state, action) => {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1 };
    case "DECREMENT":
      return { count: state.count - 1 };
    default:
      return state;
  }
};

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>Increment</button>
      <button onClick={() => dispatch({ type: "DECREMENT" })}>Decrement</button>
    </div>
  );
}

export default Counter;
```

**Example 2: Using Actions**

In adition to the last example, you can also define action creators to make your code more organized and maintainable.

```jsx
import React, { useReducer } from "react";

const initialState = { count: 0 };

const reducer = (state, action) => {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1 };
    case "DECREMENT":
      return { count: state.count - 1 };
    default:
      return state;
  }
};

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  const increment = () => {
    dispatch({ type: "INCREMENT" });
  };

  const decrement = () => {
    dispatch({ type: "DECREMENT" });
  };

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
}

export default Counter;
```

**Example 3: Complex State**

Suppose you're managing a form with multiple fields using `useReducer`.

```jsx
import React, { useReducer } from "react";

const initialState = {
  firstName: "",
  lastName: "",
  email: "",
};

const reducer = (state, action) => {
  switch (action.type) {
    case "UPDATE_FIELD":
      return { ...state, [action.field]: action.value };
    default:
      return state;
  }
};

function Form() {
  const [state, dispatch] = useReducer(reducer, initialState);

  const handleChange = (field, value) => {
    dispatch({ type: "UPDATE_FIELD", field, value });
  };

  return (
    <form>
      <input
        type="text"
        placeholder="First Name"
        value={state.firstName}
        onChange={(e) => handleChange("firstName", e.target.value)}
      />
      <input
        type="text"
        placeholder="Last Name"
        value={state.lastName}
        onChange={(e) => handleChange("lastName", e.target.value)}
      />
      <input
        type="email"
        placeholder="Email"
        value={state.email}
        onChange={(e) => handleChange("email", e.target.value)}
      />
    </form>
  );
}

export default Form;
```

### useMemo:

The `useMemo` hook in React is used to optimize performance by memoizing the results of expensive computations so that they are not recalculated on every render unless their dependencies change. This can be particularly useful when dealing with computationally intensive operations or when you want to prevent unnecessary re-renders.

**Example 1: Expensive Calculation**

Let's say you have a component that calculates the factorial of a number. Calculating factorials can be computationally expensive, so you can use `useMemo` to memorize the result and avoid recalculating it on every render.

```jsx
import React, { useState, useMemo } from "react";

function FactorialCalculator({ number }) {
  const factorial = useMemo(() => {
    console.log("Calculating factorial...");
    let result = 1;
    for (let i = 1; i <= number; i++) {
      result *= i;
    }
    return result;
  }, [number]);

  return (
    <div>
      <p>
        Factorial of {number} is: {factorial}
      </p>
    </div>
  );
}

function App() {
  const [num, setNum] = useState(5);

  return (
    <div>
      <input
        type="number"
        value={num}
        onChange={(e) => setNum(parseInt(e.target.value))}
      />
      <FactorialCalculator number={num} />
    </div>
  );
}

export default App;
```

**Example 2: Memoizing Components**

You can also use `useMemo` to memorize an entire component to prevent unnecessary re-renders.

```jsx
import React, { useState, useMemo } from "react";

function ExpensiveComponent({ data }) {
  // Expensive computations or operations here
  console.log("ExpensiveComponent re-rendered");
  return <div>Expensive Component</div>;
}

function App() {
  const [count, setCount] = useState(0);

  const expensiveComponent = useMemo(() => {
    return <ExpensiveComponent data={count} />;
  }, [count]);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Increment Count</button>
      {expensiveComponent}
    </div>
  );
}

export default App;
```

> The `ExpensiveComponent` is memorized using the `useMemo` hook. This means that it will only re-render when the `count` state changes, preventing unnecessary re-renders due to changes in other parts of the component's state.

**Example 3: Memoizing Complex Objects**

You can also use `useMemo` to memorize complex objects, preventing them from being recreated on every render.

```jsx
import React, { useMemo } from "react";

function App() {
  const complexObject = useMemo(() => {
    return {
      prop1: "value1",
      prop2: "value2",
      // ... other properties
    };
  }, []);

  return (
    <div>
      <p>Complex Object: {JSON.stringify(complexObject)}</p>
    </div>
  );
}

export default App;
```

> The `complexObject` is memorized using the `useMemo` hook with an empty dependency array. This ensures that the object is created only once and remains the same across renders.

### useCallback:

The `useCallback` hook in React is used to memorize functions, which means it helps prevent unnecessary re-creation of functions on every render. This can be beneficial when passing functions down to child components, as it ensures that the child components don't re-render unnecessarily due to changed function references.

**Example 1: Passing Callbacks to Child Components**

Suppose you have a parent component that passes a callback to a child component. Without `useCallback`, the callback would be re-created on every render of the parent component.

```jsx
import React, { useState } from "react";

function ChildComponent({ onClick }) {
  console.log("ChildComponent re-rendered");
  return <button onClick={onClick}>Click me</button>;
}

function ParentComponent() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <ChildComponent onClick={handleClick} />
    </div>
  );
}

export default ParentComponent;
```

> In this example, every time the `ParentComponent` re-renders, a new instance of the `handleClick` function is created. To prevent this unnecessary re-creation, you can use `useCallback`.

```jsx
import React, { useState, useCallback } from "react";

function ChildComponent({ onClick }) {
  console.log("ChildComponent re-rendered");
  return <button onClick={onClick}>Click me</button>;
}

function ParentComponent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount(count + 1);
  }, [count]);

  return (
    <div>
      <p>Count: {count}</p>
      <ChildComponent onClick={handleClick} />
    </div>
  );
}

export default ParentComponent;
```

> By using `useCallback`, the `handleClick` function will be memorized and only re-created if the `count` dependency changes.

**Example 2: Dependency Array**

The dependency array in `useCallback` is crucial. It specifies the values that, when changed, would trigger the re-creation of the memorized function. Omitting the dependency array (like `useCallback(() => {...})`) would result in the function being re-created on every render.

```jsx
const memorizedFunction = useCallback(() => {
  // Function implementation
}, [dependency1, dependency2]);
```

**Example 3: Complex Calculations**

You can also use `useCallback` for memoizing functions that involve complex calculations.

```jsx
import React, { useState, useCallback } from "react";

function ComplexCalculation() {
  // Complex calculation function
  const calculateResult = (a, b) => {
    console.log("Calculating...");
    return a + b;
  };

  const [num1, setNum1] = useState(0);
  const [num2, setNum2] = useState(0);

  const result = useCallback(() => {
    return calculateResult(num1, num2);
  }, [num1, num2]);

  return (
    <div>
      <input
        type="number"
        value={num1}
        onChange={(e) => setNum1(parseInt(e.target.value))}
      />
      <input
        type="number"
        value={num2}
        onChange={(e) => setNum2(parseInt(e.target.value))}
      />
      <p>Result: {result()}</p>
    </div>
  );
}

export default ComplexCalculation;
```

> In this example, the `calculateResult` function is memorized using `useCallback`, ensuring that it's only re-created when the `num1` or `num2` dependencies change.

### Difference between useCallback and useMemo hooks:

Both `useCallback` and `useMemo` are hooks in React that serve the purpose of optimization, but they are used for slightly different scenarios and have different use cases.

**`useCallback` Hook:**

The `useCallback` hook is used to memorize functions, ensuring that they don't get recreated on every render unless their dependencies change. This is particularly useful when you're passing functions as props to child components or using them as dependencies in other hooks.

Usage:

```jsx
const memorizedFunction = useCallback(() => {
  // Function implementation
}, [dependency1, dependency2]);
```

**`useMemo` Hook:**

The `useMemo` hook is used to memorize the results of expensive computations or operations, ensuring that the result is cached and reused across renders unless the dependencies change. It's useful for optimizing performance when dealing with computationally intensive calculations or when you want to prevent unnecessary re-renders caused by changed values.

Usage:

```jsx
const memorizedValue = useMemo(() => {
  // Expensive computation or operation
}, [dependency1, dependency2]);
```

**Key Differences:**

1. **Purpose:**

   - `useCallback` is used to memorize functions to prevent their re-creation, ensuring consistent references across renders. It's particularly useful when you want to optimize the rendering behavior of child components or hooks that depend on functions.
   - `useMemo` is used to memorize the results of computations or operations, preventing the expensive calculations from being repeated on every render. It's beneficial for optimizing performance when dealing with computationally intensive tasks.

2. **Input and Output:**

   - `useCallback` takes a function as its first argument and returns a memorized version of that function.
   - `useMemo` takes a function (the computation) as its first argument and returns the result of that computation.

3. **Usage Scenarios:**
   - Use `useCallback` when you want to ensure that the same function instance is used across renders, mainly to optimize child component rendering or hooks that depend on functions.
   - Use `useMemo` when you want to optimize performance by caching the results of computations or complex operations and preventing them from being recalculated on every render.

In summary, `useCallback` is used to memorize functions themselves, while `useMemo` is used to memorize the results of function executions or other complex operations.

### useLayoutEffect:

The `useLayoutEffect` hook in React is similar to the `useEffect` hook, but it runs synchronously after all DOM mutations. This makes it suitable for handling tasks that require DOM measurements or other operations that need to be executed before the browser repaints.

**Example 1: Measuring DOM Element**

You can use `useLayoutEffect` to measure a DOM element's dimensions right after it's rendered.

```jsx
import React, { useState, useLayoutEffect, useRef } from "react";

function MeasureBox() {
  const [width, setWidth] = useState(0);
  const boxRef = useRef(null);

  useLayoutEffect(() => {
    if (boxRef.current) {
      setWidth(boxRef.current.offsetWidth);
    }
  }, []);

  return (
    <div>
      <div
        ref={boxRef}
        style={{ width: "200px", height: "100px", background: "lightblue" }}
      >
        Measuring this box
      </div>
      <p>Box Width: {width}px</p>
    </div>
  );
}

export default MeasureBox;
```

In this example, the `useLayoutEffect` hook measures the width of the box after it's rendered and updates the state with the measured value.

**Example 2: Synchronous DOM Updates**

`useLayoutEffect` can be used for synchronous DOM updates before the browser repaints, which can be useful for creating animations or smooth transitions.

```jsx
import React, { useState, useLayoutEffect } from "react";

function AnimatedCounter() {
  const [count, setCount] = useState(0);

  useLayoutEffect(() => {
    if (count > 0) {
      const interval = setInterval(() => {
        setCount((prevCount) => prevCount - 1);
      }, 1000);

      return () => clearInterval(interval);
    }
  }, [count]);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(5)}>Start Countdown</button>
    </div>
  );
}

export default AnimatedCounter;
```

> In this example, the `useLayoutEffect` hook sets up a countdown animation that decrements the count by 1 every second.

**Note:** It's important to use `useLayoutEffect` only when necessary, as it can block the rendering and make the application less responsive. In many cases, using the `useEffect` hook with an empty dependency array is sufficient, as it runs asynchronously after the browser repaints, and any visual updates will be applied.

> The primary difference between `useLayoutEffect` and `useEffect` is that `useLayoutEffect` runs synchronously after all DOM mutations, potentially causing a delay in the rendering if not used judiciously.

### useId:

The `useId` hook in React is used to generate unique IDs for elements, The best example of this would be to create an id for an input and have a label point to the same id. For example, if you had an EmailForm component you could write it like so.

```jsx
function EmailForm() {
  return (
    <>
      <label htmlFor="email">Email</label>
      <input id="email" type="email" />
    </>
  );
}
```

This code would work, but if you try to render this form on the same page multiple times you will have multiple input elements with the same id of email. This is obviously bad since every id on a page should be unique and on top of that your labels when clicked on will now all focus the first email input on the page instead of the email input associated with that label. To fix this we can use useId.

```jsx
function EmailForm() {
  const id = useId();
  return (
    <>
      <label htmlFor={id}>Email</label>
      <input id={id} type="email" />
    </>
  );
}
```

> The useId hook is a simple hook that takes no inputs and returns a unique id. This id is unique to each individual component which means we can render this form on our page as many times as we want without having to worry about duplicate ids.

> `useId` should not be used to generate keys in a list. Keys should be generated from your data.

**Getting Elements With querySelector**

One thing to note about the useId hook is that the ids generated by it are invalid selectors to use with the querySelector method. This is intentional since React does not want you to select the elements by their id using something like querySelector. Instead you should use the useRef hook for this.

**Example 1: Using Multiple Ids In One Component**

```js
import React, { useId } from "react";

function LogInForm() {
  const id = useId();
  return (
    <>
      <div>
        <label htmlFor={`${id}-email`}>Email</label>
        <input id={`${id}-email`} type="email" />
      </div>
      <div>
        <label htmlFor={`${id}-password`}>Password</label>
        <input id={`${id}-password`} type="password" />
      </div>
    </>
  );
}
```

## Redux in React.js

### What is Redux?

Redux is a state management library for JavaScript applications, including React. It provides a predictable and centralized way to manage the state of your application, making it easier to handle complex interactions, maintain data consistency, and manage the flow of data throughout your app.

Redux operates on the principles of a single source of truth and immutability. The application state is stored in a single JavaScript object called the "store," and changes to the state are made by dispatching actions. Reducers are pure functions that define how the state changes in response to actions.

### Key concepts in Redux:

- **Store:** The single source of truth that holds the application state.
- **Actions:** Plain JavaScript objects that describe changes in the application state.
- **Reducers:** Pure functions that specify how the state changes in response to actions.
- **Dispatch:** A method used to send actions to the store to trigger state changes.
- **Selectors:** Functions that retrieve specific data from the state.

### How to use Redux in a React application:

**Step 1: Install Redux**

First, you need to install Redux and React-Redux (a library that integrates Redux with React):

```bash
npm install redux react-redux
```

**Step 2: Create Redux Store**

Create a Redux store by combining reducers using the `combineReducers` function.

```jsx
// src/redux/store.js

import { createStore, combineReducers } from "redux";

// Reducers
const counterReducer = (state = 0, action) => {
  switch (action.type) {
    case "INCREMENT":
      return state + 1;
    case "DECREMENT":
      return state - 1;
    default:
      return state;
  }
};

const rootReducer = combineReducers({
  counter: counterReducer,
});

// Create the Redux store
const store = createStore(rootReducer);

export default store;
```

**Step 3: Create Redux Actions**

Define action types and action creators to dispatch actions.

```jsx
// src/redux/actions.js

export const increment = () => {
  return { type: "INCREMENT" };
};

export const decrement = () => {
  return { type: "DECREMENT" };
};
```

**Step 4: Connect Redux to React**

Use the `connect` function from React-Redux to connect your React components to the Redux store.

```jsx
// src/components/Counter.js

import React from "react";
import { connect } from "react-redux";
import { increment, decrement } from "../redux/actions";

function Counter({ count, increment, decrement }) {
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
}

const mapStateToProps = (state) => {
  return {
    count: state.counter,
  };
};

export default connect(mapStateToProps, { increment, decrement })(Counter);
```

**Step 5: Integrate with React**

Finally, integrate your Redux-connected component into your React app.

```jsx
// src/App.js

import React from "react";
import { Provider } from "react-redux";
import store from "./redux/store";
import Counter from "./components/Counter";

function App() {
  return (
    <Provider store={store}>
      <div className="App">
        <Counter />
      </div>
    </Provider>
  );
}

export default App;
```

## Server side rendering in React.js

### What is Server Side Rendering?

Server Side Rendering (SSR) is the process of rendering a web page on the server and sending the HTML to the client instead of rendering it in the browser. This is useful for improving performance and SEO, as the client receives a fully rendered page instead of an empty HTML page that's rendered in the browser.

### How to implement Server Side Rendering in React?

There are some options to implement SSR in React, actually you can create an Express.js app and implement it, this is an example:

(Frontent master's express-vite example)[https://github.com/btholt/citr-v8-project/tree/main/server-side-rendering]

but the most popular one is Next.js. Next.js is a React framework that provides built-in support for SSR, making it easy to implement and use.

### Key Features of Next.js

1.  **Server-Side Rendering and Static Site Generation:** Next.js simplifies the process of server-side rendering and static site generation, allowing you to pre-render React components on the server for improved performance and SEO.
2.  **Automatic Code Splitting:** Next.js automatically splits JavaScript code into smaller chunks, loading only the required components for each page, which enhances performance by reducing initial load times.
3.  **CSS Support:** Next.js provides built-in support for CSS modules, styled-jsx, and CSS-in-JS libraries, making it easier to handle styling within your React components.
4.  **Powerful Routing:** Next.js offers a file-based routing system, allowing you to create routes simply by placing components in the correct folder structure.

### How to create a Next.js App?

**Step 1: Create a Next.js Project**

```bash
npx create-next-app my-ssr-app
cd my-ssr-app
```

**Step 2: Create a Server-Side Rendering Page**

Create a new file called `ssr.js` in the `pages` directory:

```jsx
// pages/ssr.js

import React from "react";

function SSRPage({ serverRenderedData }) {
  return (
    <div>
      <h1>Server-Side Rendering Example</h1>
      <p>Server-rendered data: {serverRenderedData}</p>
    </div>
  );
}

export async function getServerSideProps() {
  // Simulate fetching data from an API or database
  const serverRenderedData = "Data fetched on the server";

  return {
    props: {
      serverRenderedData,
    },
  };
}

export default SSRPage;
```

**Step 3: Start the Development Server**

Start the development server using the following command:

```bash
npm run dev
```

Visit `http://localhost:3000/ssr` in your browser to see the server-side rendered page.

**Step 4: Build and Deploy**

When you're ready to deploy your app, build it using:

```bash
npm run build
```

## Next.js Fundamentals

### 1. Routing and Navigation in Next.js:

Next.js provides built-in routing and navigation capabilities using the `Link` component.

```jsx
// pages/index.js
import Link from "next/link";

function Home() {
  return (
    <div>
      <h1>Welcome to Next.js!</h1>
      <Link href="/about">Go to About</Link>
    </div>
  );
}

export default Home;
```

```jsx
// pages/about.js
import Link from "next/link";

function About() {
  return (
    <div>
      <h1>About Us</h1>
      <p>This is the about page.</p>
      <Link href="/">Go back to Home</Link>
    </div>
  );
}

export default About;
```

### 2. Dynamic Routes and Query Parameters:

Next.js allows you to create dynamic routes by using square brackets `[]`.

For example, a blog could include the following route pages/blog/[id].js where [slug] is the Dynamic Segment for blog posts.

```jsx
// pages/post/[id].js
import { useRouter } from "next/router";

function Post() {
  const router = useRouter();
  const { id } = router.query;

  return (
    <div>
      <h1>Post {id}</h1>
    </div>
  );
}

export default Post;
```

### 3. Linking Between Pages:

The `Link` component in Next.js makes it easy to create links between pages.

```jsx
import Link from "next/link";

function Navigation() {
  return (
    <nav>
      <Link href="/">Home</Link>
      <Link href="/about">About</Link>
      <Link href="/contact">Contact</Link>
    </nav>
  );
}
```

### 4. Using Layouts and Shared Components:

You can create a layout and share components across pages by creating a custom `_app.js` file.

```jsx
// pages/_app.js
import Layout from "../components/Layout";

function MyApp({ Component, pageProps }) {
  return (
    <Layout>
      <Component {...pageProps} />
    </Layout>
  );
}

export default MyApp;
```

### 5. Styling and CSS-in-JS with Next.js:

Next.js supports various CSS-in-JS libraries for styling, such as styled-components and emotion.

```jsx
import styled from "styled-components";

const StyledDiv = styled.div`
  background-color: lightblue;
  padding: 1rem;
`;

function StyledComponent() {
  return <StyledDiv>This is a styled component.</StyledDiv>;
}

export default StyledComponent;
```

### 6. Managing Assets and Images:

Next.js makes it easy to manage assets and images. Place your assets in the `public` directory.

```jsx
function ImageComponent() {
  return (
    <div>
      <img src="/logo.png" alt="Next.js Logo" />
    </div>
  );
}

export default ImageComponent;
```

## Server-Side Rendering (SSR) in Next.js

### 1. Introduction to Server-Side Rendering:

Server-side rendering (SSR) involves rendering React components on the server before sending them to the client.

```jsx
// pages/ssr.js
import React from "react";

function SSRPage({ serverRenderedData }) {
  return (
    <div>
      <h1>Server-Side Rendering Example</h1>
      <p>Server-rendered data: {serverRenderedData}</p>
    </div>
  );
}

export async function getServerSideProps() {
  // Simulate fetching data from an API or database
  const serverRenderedData = "Data fetched on the server";

  return {
    props: {
      serverRenderedData,
    },
  };
}

export default SSRPage;
```

### 2. `getServerSideProps`: Fetching Data on the Server:

The `getServerSideProps` function fetches data on the server before rendering the component.

```jsx
// pages/post/[id].js
import { useRouter } from "next/router";

function Post({ post }) {
  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
    </div>
  );
}

export async function getServerSideProps(context) {
  const { id } = context.query;
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/posts/${id}`
  );
  const post = await response.json();

  return {
    props: {
      post,
    },
  };
}

export default Post;
```

### 3. `getStaticProps`: Pre-rendering Static Pages:

`getStaticProps` pre-renders static pages at build time.

```jsx
// pages/index.js
function Home({ posts }) {
  return (
    <div>
      <h1>Latest Posts</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}

export async function getStaticProps() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts");
  const posts = await response.json();

  return {
    props: {
      posts,
    },
  };
}

export default Home;
```

### 4. Incremental Static Generation (ISG):

Next.js supports Incremental Static Generation (ISG), allowing you to re-generate static pages at runtime.

```jsx
// pages/greeting.js
function Greeting({ message }) {
  return (
    <div>
      <h1>Greeting</h1>
      <p>{message}</p>
    </div>
  );
}

export async function getStaticProps() {
  const response = await fetch("https://api.example.com/random-greeting");
  const { message } = await response.json();

  return {
    props: {
      message,
    },
    revalidate: 10, // Regenerate page every 10 seconds
  };
}

export default Greeting;
```

## Routing and Navigation in Next.js

### 1. Creating Nested Routes:

Nested routes allow you to create a hierarchical structure of pages.

```jsx
// pages/blog/index.js
import Link from "next/link";

function Blog() {
  return (
    <div>
      <h1>Blog</h1>
      <ul>
        <li>
          <Link href="/blog/post-1">Post 1</Link>
        </li>
        <li>
          <Link href="/blog/post-2">Post 2</Link>
        </li>
      </ul>
    </div>
  );
}

export default Blog;
```

```jsx
// pages/blog/post-1.js
function Post1() {
  return (
    <div>
      <h1>Post 1</h1>
      <p>This is the content of Post 1.</p>
    </div>
  );
}
export default Post1;
```

### 2. Working with Dynamic Routes:

Dynamic routes allow you to create pages based on route parameters.

```jsx
// pages/blog/[slug].js
import { useRouter } from "next/router";

function BlogPost() {
  const router = useRouter();
  const { slug } = router.query;

  return (
    <div>
      <h1>Blog Post: {slug}</h1>
      {/* Render post content */}
    </div>
  );
}

export default BlogPost;
```

### 3. Customizing Route Behavior:

You can customize route behavior using the `router` object and hooks.

```jsx
// pages/about.js
import { useRouter } from "next/router";

function About() {
  const router = useRouter();

  const handleRedirect = () => {
    // Redirect to a different page
    router.push("/");
  };

  return (
    <div>
      <h1>About Us</h1>
      <button onClick={handleRedirect}>Go to Home</button>
    </div>
  );
}

export default About;
```

### 4. Handling Route Transitions and Animations:

You can use route transitions and animations to improve the user experience.

```jsx
// pages/_app.js
import { AnimatePresence, motion } from "framer-motion";

function MyApp({ Component, pageProps, router }) {
  return (
    <AnimatePresence exitBeforeEnter>
      <motion.div
        key={router.route}
        initial="pageInitial"
        animate="pageAnimate"
        exit="pageExit"
        variants={{
          pageInitial: { opacity: 0 },
          pageAnimate: { opacity: 1 },
          pageExit: { opacity: 0 },
        }}
      >
        <Component {...pageProps} />
      </motion.div>
    </AnimatePresence>
  );
}

export default MyApp;
```

### 5. Using Client-Side Navigation:

Next.js provides the `Link` component and `useRouter` for client-side navigation.

```jsx
// pages/index.js
import Link from "next/link";

function Home() {
  return (
    <div>
      <h1>Welcome to Next.js!</h1>
      <Link href="/about">Go to About</Link>
    </div>
  );
}

export default Home;
```

## Data Fetching and API Integration

### 1. Fetching Data from APIs with SWR:

**Basic Data Fetching:**

SWR (Stale-While-Revalidate) is a popular data fetching library for Next.js. Here's an example of fetching and displaying data using SWR:

```jsx
import useSWR from "swr";

function Posts() {
  const { data, error } = useSWR("/api/posts", fetch);

  if (error) return <div>Error loading data</div>;
  if (!data) return <div>Loading...</div>;

  return (
    <div>
      <h1>Posts</h1>
      <ul>
        {data.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}

export default Posts;
```

**Fetching with Pagination:**

Using SWR to fetch paginated data and display it with "Load More" functionality:

```jsx
import useSWR, { useSWRPages } from "swr";

function Posts() {
  const getKey = (pageIndex) => `/api/posts?page=${pageIndex + 1}`;
  const { pages, loadMore, isReachingEnd } = useSWRPages(
    "posts",
    getKey,
    fetch
  );

  return (
    <div>
      <h1>Posts</h1>
      <ul>
        {pages.map((page, index) => (
          <React.Fragment key={index}>
            {page.data.map((post) => (
              <li key={post.id}>{post.title}</li>
            ))}
          </React.Fragment>
        ))}
      </ul>
      <button onClick={loadMore} disabled={isReachingEnd}>
        Load More
      </button>
    </div>
  );
}

export default Posts;
```

**Handling Mutations (POST/PUT):**

Using SWR to handle data mutations (e.g., creating or updating data):

```jsx
import useSWR, { mutate } from "swr";

function CreatePostForm() {
  const [title, setTitle] = useState("");

  const handleSubmit = async (event) => {
    event.preventDefault();
    const response = await fetch("/api/posts", {
      method: "POST",
      body: JSON.stringify({ title }),
    });

    if (response.ok) {
      mutate("/api/posts"); // Update cache
      setTitle(""); // Clear input field
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
      />
      <button type="submit">Create Post</button>
    </form>
  );
}

export default CreatePostForm;
```

### 2. Serverless Functions and API Routes:

Next.js allows you to create serverless functions using API routes. Here's an example of creating an API route:

```jsx
// pages/api/posts.js
export default function handler(req, res) {
  const posts = [
    { id: 1, title: "Post 1" },
    { id: 2, title: "Post 2" },
  ];

  res.status(200).json(posts);
}
```

You can then fetch data from this API route as shown in the previous example.

### 3. Authentication and Authorization:

Next.js can handle authentication and authorization by protecting routes. Here's an example using a higher-order component (HOC) for authentication:

```jsx
// components/AuthenticatedRoute.js
import { useRouter } from 'next/router';

function AuthenticatedRoute({ children }) {
  const router = useRouter();
  const isAuthenticated = /* Check if user is authenticated */;

  if (!isAuthenticated) {
    router.push('/login');
    return null;
  }

  return children;
}

export default AuthenticatedRoute;
```

### 4. Caching and Revalidation Strategies:

SWR provides caching and revalidation strategies out of the box. Here's an example of customizing caching and revalidation intervals:

```jsx
import useSWR from "swr";

function Posts() {
  const { data, error } = useSWR("/api/posts", fetch, {
    refreshInterval: 5000,
  });

  // ...
}
```

> In this example, data will be revalidated every 5 seconds.
