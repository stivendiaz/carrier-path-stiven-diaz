# Vue.js

## Introduction: The good things

Vue.js is a progressive JavaScript framework for building user interfaces. It was developed by Evan You and has gained significant popularity in recent years. Vue.js is designed to be approachable, flexible, and efficient, making it an excellent choice for both small-scale projects and large-scale applications.

aspects of Vue.js:

1. **Reactive Data Binding**: Vue.js utilizes a reactive data binding system, which means that the data and the UI are kept in sync automatically. When the data changes, the corresponding parts of the UI are updated, and vice versa. This makes it easy to build dynamic and interactive applications without manually manipulating the DOM.

2. **Component-Based Architecture**: Vue.js follows a component-based architecture, where the UI is divided into self-contained and reusable components. Each component manages its own template, logic, and styling, which makes development more modular and allows for code reuse. Components can be easily composed to build complex user interfaces.

3. **Declarative Rendering**: Vue.js uses a declarative template syntax that allows you to describe the desired UI structure and behavior. You simply define the desired output, and Vue.js takes care of efficiently updating the DOM based on the data changes. This makes the code more intuitive and easier to understand.

4. **Directives**: Vue.js provides a set of built-in directives, such as `v-if`, `v-for`, and `v-bind`, which allow you to add dynamic behavior to the DOM elements. Directives are prefixed with `v-` and provide powerful features like conditional rendering, list rendering, and event handling.

5. **Vue Router**: Vue.js includes Vue Router, a powerful routing library that enables client-side routing in single-page applications (SPAs). Vue Router allows you to define routes, navigate between different views, and handle route parameters and navigation guards.

6. **Vuex**: Vuex is Vue.js's official state management pattern and library. It provides a centralized store to manage the state of an application, making it easier to share data between components and handle complex application states. Vuex follows a unidirectional data flow, ensuring predictable and scalable state management.

7. **Flexible and Incremental Adoption**: Vue.js is designed to be incrementally adoptable. You can introduce Vue.js into an existing project or use it to build an entire application from scratch. Its flexible architecture allows you to use Vue.js for specific parts of your project while keeping the existing stack intact.

8. **Large Ecosystem**: Vue.js has a vibrant ecosystem with a wide range of libraries, tools, and plugins. This makes it easy to extend Vue.js functionality or integrate it with other frameworks and libraries.

## Quick start:

Step 1: Set up a Development Environment
Before getting started with Vue.js, you need to set up a development environment. Make sure you have Node.js installed on your machine, as it includes npm (Node Package Manager) which we'll use to install Vue.js.

Step 2: Install Vue CLI

```
npm install -g @vue/cli
```

Step 3: Create a New Vue Project
Once Vue CLI is installed, you can create a new Vue project using the following command:

```
> npm init vue@latest
```

This command will install and execute create-vue, the official Vue project scaffolding tool. You will be presented with prompts for several optional features such as TypeScript and testing support:

```
✔ Project name: … <your-project-name>
✔ Add TypeScript? … No / Yes
✔ Add JSX Support? … No / Yes
✔ Add Vue Router for Single Page Application development? … No / Yes
✔ Add Pinia for state management? … No / Yes
✔ Add Vitest for Unit testing? … No / Yes
✔ Add an End-to-End Testing Solution? … No / Cypress / Playwright
✔ Add ESLint for code quality? … No / Yes
✔ Add Prettier for code formatting? … No / Yes

Scaffolding project in ./<your-project-name>...
Done.
```

Step 4: Navigate to the Project Directory
After the project is created, navigate to the project directory using the following command:

```
> cd <your-project-name>
> npm install
> npm run dev
```

# Template Syntax

Vue uses an HTML-based template syntax that allows you to declaratively bind the rendered DOM to the underlying component instance's data. All Vue templates are syntactically valid HTML that can be parsed by spec-compliant browsers and HTML parsers.

## Text Interpolation

```html
<span>Message: {{ msg }}</span>
```

## Raw HTML

```html
<p>Using text interpolation: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

## Attribute Bindings

```html
<div v-bind:id="dynamicId"></div>
```

### Shorthand

```html
<div :id="dynamicId"></div>
```

Attributes that start with `:` may look a bit different from normal HTML, but it is in fact a valid character for attribute names and all Vue-supported browsers can parse it correctly. In addition, they do not appear in the final rendered markup. The shorthand syntax is optional, but you will likely appreciate it when you learn more about its usage later.

> The shorthand syntax is the most common usage for Vue developers.

### Boolean Attributes

```html
<button :disabled="isButtonDisabled">Button</button>
```

### Dynamically Binding Multiple Attributes

```js
const objectOfAttrs = {
  id: "container",
  class: "wrapper",
};
```

You can bind them to a single element by using `v-bind` without an argument:

```html
<div v-bind="objectOfAttrs"></div>
```

## Using JavaScript Expressions

```javascript
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div :id="`list-${id}`"></div>
```

### Expressions Only

```
<!-- this is a statement, not an expression: -->
{{ var a = 1 }}

<!-- flow control won't work either, use ternary expressions -->
{{ if (ok) { return message } }}
```

### Calling Functions

It is possible to call a component-exposed method inside a binding expression:

```html
<time :title="toTitleDate(date)" :datetime="date">
  {{ formatDate(date) }}
</time>
```

### Restricted Globals Access

Template expressions are sandboxed and only have access to a [restricted list of globals](https://github.com/vuejs/core/blob/main/packages/shared/src/globalsAllowList.ts#L3). The list exposes commonly used built-in globals such as `Math` and `Date`.

Globals not explicitly included in the list, for example user-attached properties on `window`, will not be accessible in template expressions. You can, however, explicitly define additional globals for all Vue expressions by adding them to [`app.config.globalProperties`](https://vuejs.org/api/application.html#app-config-globalproperties).

## Directives

Directives are special attributes with the `v-` prefix. Vue provides a number of [built-in directives](https://vuejs.org/api/built-in-directives.html), including `v-html` and `v-bind` which we have introduced above.

Directive attribute values are expected to be single JavaScript expressions (with the exception of `v-for`, `v-on` and `v-slot`, which will be discussed in their respective sections later). A directive's job is to reactively apply updates to the DOM when the value of its expression changes. Take [`v-if`](https://vuejs.org/api/built-in-directives.html#v-if) as an example:

```html
<p v-if="seen">Now you see me</p>
```

Here, the `v-if` directive would remove / insert the `<p>` element based on the truthiness of the value of the expression `seen`.

### Arguments

Some directives can take an "argument", denoted by a colon after the directive name. For example, the `v-bind` directive is used to reactively update an HTML attribute:

```html
<a v-bind:href="url"> ... </a>

<!-- shorthand -->
<a :href="url"> ... </a>
```

Here, `href` is the argument, which tells the `v-bind` directive to bind the element's `href` attribute to the value of the expression `url`. In the shorthand, everything before the argument (i.e., `v-bind:`) is condensed into a single character, `:`.

Another example is the `v-on` directive, which listens to DOM events:

```html
<a v-on:click="doSomething"> ... </a>

<!-- shorthand -->
<a @click="doSomething"> ... </a>
```

Here, the argument is the event name to listen to: `click`. `v-on` has a corresponding shorthand, namely the `@` character. We will talk about event handling in more detail too.

### Dynamic Arguments

```html
<!--
Note that there are some constraints to the argument expression,
as explained in the "Dynamic Argument Value Constraints" and "Dynamic Argument Syntax Constraints" sections below.
-->
<a v-bind:[attributeName]="url"> ... </a>

<!-- shorthand -->
<a :[attributeName]="url"> ... </a>
```

Similarly, you can use dynamic arguments to bind a handler to a dynamic event name:

```html
<a v-on:[eventName]="doSomething"> ... </a>

<!-- shorthand -->
<a @[eventName]="doSomething"></a>
```

In this example, when `eventName`'s value is `"focus"`, `v-on:[eventName]` will be equivalent to `v-on:focus`.

#### Dynamic Argument Value Constraints

Dynamic arguments are expected to evaluate to a string, with the exception of `null`. The special value `null` can be used to explicitly remove the binding. Any other non-string value will trigger a warning.

#### Dynamic Argument Syntax Constraints

The following is invalid:

```html
<!-- This will trigger a compiler warning. -->
<a :['foo' + bar]="value"> ... </a>
```

If you need to pass a complex dynamic argument, it's probably better to use a [computed property](https://vuejs.org/guide/essentials/computed.html), which we will cover shortly.

```html
<a :[someAttr]="value"> ... </a>
```

The above will be converted to `:[someattr]` in in-DOM templates. If your component has a `someAttr` property instead of `someattr`, your code won't work. Templates inside Single-File Components are **not** subject to this constraint.

### Modifiers

Modifiers are special postfixes denoted by a dot, which indicate that a directive should be bound in some special way. For example, the `.prevent` modifier tells the `v-on` directive to call `event.preventDefault()` on the triggered event:

```html
<form @submit.prevent="onSubmit">...</form>
```

You'll see other examples of modifiers later, [for `v-on`](https://vuejs.org/guide/essentials/event-handling.html#event-modifiers) and [for `v-model`](https://vuejs.org/guide/essentials/forms.html#modifiers), when we explore those features.

And finally, here's the full directive syntax visualized:

![directive syntax graph](https://vuejs.org/assets/directive.69c37117.png)

# Reactivity Fundamentals

## Declaring Reactive State

### `ref()`

In Composition API, the recommended way to declare reactive state is using the [`ref()`](https://vuejs.org/api/reactivity-core.html#ref) function:

```js
import { ref } from "vue";

const count = ref(0);
```

`ref()` takes the argument and returns it wrapped within a ref object with a `.value` property:

```js
const count = ref(0);

console.log(count); // { value: 0 }
console.log(count.value); // 0

count.value++;
console.log(count.value); // 1
```

To access refs in a component's template, declare and return them from a component's `setup()` function:

```js
import { ref } from "vue";

export default {
  // `setup` is a special hook dedicated for the Composition API.
  setup() {
    const count = ref(0);

    // expose the ref to the template
    return {
      count,
    };
  },
};
```

```html
<div>{{ count }}</div>
```

Notice that we did **not** need to append `.value` when using the ref in the template. For convenience, refs are automatically unwrapped when used inside templates (with a few [caveats](https://vuejs.org/guide/essentials/reactivity-fundamentals.html#caveat-when-unwrapping-in-templates)).

You can also mutate a ref directly in event handlers:

```html
<button @click="count++">{{ count }}</button>
```

For more complex logic, we can declare functions that mutate refs in the same scope and expose them as methods alongside the state:

```js
import { ref } from "vue";

export default {
  setup() {
    const count = ref(0);

    function increment() {
      // .value is needed in JavaScript
      count.value++;
    }

    // don't forget to expose the function as well.
    return {
      count,
      increment,
    };
  },
};
```

Exposed methods can then be used as event handlers:

```html
<button @click="increment">{{ count }}</button>
```

Here's the example live on [Codepen](https://codepen.io/vuejs-examples/pen/WNYbaqo), without using any build tools.

### `<script setup>`

Manually exposing state and methods via `setup()` can be verbose. Luckily, it can be avoided when using [Single-File Components (SFCs)](https://vuejs.org/guide/scaling-up/sfc.html). We can simplify the usage with `<script setup>`:

```html
<script setup>
  import { ref } from "vue";

  const count = ref(0);

  function increment() {
    count.value++;
  }
</script>

<template>
  <button @click="increment">{{ count }}</button>
</template>
```

Top-level imports, variables and functions declared in `<script setup>` are automatically usable in the template of the same component. Think of the template as a JavaScript function declared in the same scope - it naturally has access to everything declared alongside it.

You might be wondering why we need refs with the `.value` instead of plain variables. To explain that, we will need to briefly discuss how Vue's reactivity system works.

When you use a ref in the template, and changes the ref's value later, Vue automatically detects the change and updates the DOM accordingly. This is made possible with a dependency-tracking based reactivity system. When a component is rendered for the first time, Vue **tracks** every ref that was used during the render. Later on, when a ref is mutated, it will **trigger** re-render for components that are tracking it.

In standard JavaScript, there is no way to detect the access or mutation of plain variables. But we can intercept a property's get and set operations.

The `.value` property gives Vue the opportunity to detect when a ref has been accessed or mutated. Under the hood, Vue performs the tracking in its getter, and performs triggering in its setter. Conceptually, you can think of a ref as an object that looks like this:

```js
// pseudo code, not actual implementation
const myRef = {
  _value: 0,
  get value() {
    track();
    return this._value;
  },
  set value(newValue) {
    this._value = newValue;
    trigger();
  },
};
```

Another nice trait of refs is that unlike plain variables, you can pass refs into functions while retaining access to the latest value and the reactivity connection. This is particularly useful when refactoring complex logic into reusable code.

The reactivity system is discussed in more details in the [Reactivity in Depth](https://vuejs.org/guide/extras/reactivity-in-depth.html) section.

### Deep Reactivity

Refs can hold any value type, including deeply nested objects, arrays, or JavaScript built-in data structures like `Map`.

A ref will make its value deeply reactive. This means you can expect changes to be detected even when you mutate nested objects or arrays:

```js
import { ref } from "vue";

const obj = ref({
  nested: { count: 0 },
  arr: ["foo", "bar"],
});

function mutateDeeply() {
  // these will work as expected.
  obj.value.nested.count++;
  obj.value.arr.push("baz");
}
```

### DOM Update Timing

To wait for the DOM update to complete after a state change, you can use the [nextTick()](https://vuejs.org/api/general.html#nexttick) global API:

```js
import { nextTick } from "vue";

async function increment() {
  count.value++;
  await nextTick();
  // Now the DOM is updated
}
```

## `reactive()`

There is another way to declare reactive state, with the `reactive()` API. Unlike a ref which wraps the inner value in a special object, `reactive()` makes an object itself reactive:

```js
import { reactive } from "vue";

const state = reactive({ count: 0 });
```

```html
<button @click="state.count++">{{ state.count }}</button>
```

### Reactive Proxy vs. Original

It is important to note that the returned value from `reactive()` is a [Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) of the original object, which is not equal to the original object:

```js
const raw = {};
const proxy = reactive(raw);

// proxy is NOT equal to the original.
console.log(proxy === raw); // false
```

Only the proxy is reactive - mutating the original object will not trigger updates. Therefore, the best practice when working with Vue's reactivity system is to **exclusively use the proxied versions of your state**.

To ensure consistent access to the proxy, calling `reactive()` on the same object always returns the same proxy, and calling `reactive()` on an existing proxy also returns that same proxy:

```js
// calling reactive() on the same object returns the same proxy
console.log(reactive(raw) === proxy); // true

// calling reactive() on a proxy returns itself
console.log(reactive(proxy) === proxy); // true
```

This rule applies to nested objects as well. Due to deep reactivity, nested objects inside a reactive object are also proxies:

```js
const proxy = reactive({});

const raw = {};
proxy.nested = raw;

console.log(proxy.nested === raw); // false
```

### Limitations of `reactive()`

The `reactive()` API has a few limitations:

1.  **Limited value types:** it only works for object types (objects, arrays, and [collection types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects#keyed_collections) such as `Map` and `Set`). It cannot hold [primitive types](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) such as `string`, `number` or `boolean`.
2.  **Cannot replace entire object:** since Vue's reactivity tracking works over property access, we must always keep the same reference to the reactive object. This means we can't easily "replace" a reactive object because the reactivity connection to the first reference is lost:

    ```js
    let state = reactive({ count: 0 });

    // the above reference ({ count: 0 }) is no longer being tracked
    // (reactivity connection is lost!)
    state = reactive({ count: 1 });
    ```

3.  **Not destructure-friendly:** when we destructure a reactive object's property into local variables, or when we pass that property into a function, we will lose the reactivity connection:

    ```js
    const state = reactive({ count: 0 });

    // count is disconnected from state.count when destructured.
    let { count } = state;
    // does not affect original state
    count++;

    // the function receives a plain number and
    // won't be able to track changes to state.count
    // we have to pass the entire object in to retain reactivity
    callSomeFunction(state.count);
    ```

Due to these limitations, we recommend using `ref()` as the primary API for declaring reactive state.

## Additional Ref Unwrapping Details

### As Reactive Object Property

A ref is automatically unwrapped when accessed or mutated as a property of a reactive object. In other words, it behaves like a normal property :

```js
const count = ref(0);
const state = reactive({
  count,
});

console.log(state.count); // 0

state.count = 1;
console.log(count.value); // 1
```

If a new ref is assigned to a property linked to an existing ref, it will replace the old ref:

```js
const otherCount = ref(2);

state.count = otherCount;
console.log(state.count); // 2
// original ref is now disconnected from state.count
console.log(count.value); // 1
```

Ref unwrapping only happens when nested inside a deep reactive object. It does not apply when it is accessed as a property of a [shallow reactive object](https://vuejs.org/api/reactivity-advanced.html#shallowreactive).

### Caveat in Arrays and Collections

Unlike reactive objects, there is **no** unwrapping performed when the ref is accessed as an element of a reactive array or a native collection type like `Map`:

```js
const books = reactive([ref("Vue 3 Guide")]);
// need .value here
console.log(books[0].value);

const map = reactive(new Map([["count", ref(0)]]));
// need .value here
console.log(map.get("count").value);
```

### Caveat when Unwrapping in Templates

Ref unwrapping in templates only applies if the ref is a top-level property in the template render context.

In the example below, `count` and `object` are top-level properties, but `object.id` is not:

```js
const count = ref(0);
const object = { id: ref(0) };
```

Therefore, this expression works as expected:

```js
{
  {
    count + 1;
  }
}
```

...while this one does **NOT**:

```js
{
  {
    object.id + 1;
  }
}
```

The rendered result will be `[object Object]1` because `object.id` is not unwrapped when evaluating the expression and remains a ref object. To fix this, we can destructure `id` into a top-level property:

```js
const { id } = object;
```

```js
{
  {
    id + 1;
  }
}
```

Now the render result will be `2`.

Another thing to note is that a ref does get unwrapped if it is the final evaluated value of a text interpolation (i.e. a `{{ }}` tag), so the following will render `1`:

```
{{ object.id }}
```

This is just a convenience feature of text interpolation and is equivalent to `{{ object.id.value }}`.

# Conditional Rendering

## `v-if`

The directive `v-if` is used to conditionally render a block. The block will only be rendered if the directive's expression returns a truthy value.

```html
<h1 v-if="awesome">Vue is awesome!</h1>
```

## `v-else`

You can use the `v-else` directive to indicate an "else block" for `v-if`:

```html
<button @click="awesome = !awesome">Toggle</button>

<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```

## `v-else-if`

The `v-else-if`, as the name suggests, serves as an "else if block" for `v-if`. It can also be chained multiple times:

```html
<div v-if="type === 'A'">A</div>
<div v-else-if="type === 'B'">B</div>
<div v-else-if="type === 'C'">C</div>
<div v-else>Not A/B/C</div>
```

Similar to `v-else`, a `v-else-if` element must immediately follow a `v-if` or a `v-else-if` element.

## `v-if` on `<template>`

Because `v-if` is a directive, it has to be attached to a single element. But what if we want to toggle more than one element? In this case we can use `v-if` on a `<template>` element, which serves as an invisible wrapper. The final rendered result will not include the `<template>` element.

```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

`v-else` and `v-else-if` can also be used on `<template>`.

## `v-show`

Another option for conditionally displaying an element is the `v-show` directive. The usage is largely the same:

```html
<h1 v-show="ok">Hello!</h1>
```

The difference is that an element with `v-show` will always be rendered and remain in the DOM; `v-show` only toggles the `display` CSS property of the element.

`v-show` doesn't support the `<template>` element, nor does it work with `v-else`.

## `v-if` vs. `v-show`

`v-if` is "real" conditional rendering because it ensures that event listeners and child components inside the conditional block are properly destroyed and re-created during toggles.

`v-if` is also **lazy**: if the condition is false on initial render, it will not do anything - the conditional block won't be rendered until the condition becomes true for the first time.

In comparison, `v-show` is much simpler - the element is always rendered regardless of initial condition, with CSS-based toggling.

Generally speaking, `v-if` has higher toggle costs while `v-show` has higher initial render costs. So prefer `v-show` if you need to toggle something very often, and prefer `v-if` if the condition is unlikely to change at runtime.

## `v-if` with `v-for`

It's **not** recommended to use `v-if` and `v-for` on the same element due to implicit precedence.

When `v-if` and `v-for` are both used on the same element, `v-if` will be evaluated first. See the [list rendering guide](https://vuejs.org/guide/essentials/list.html#v-for-with-v-if) for details.

# Lifecycle Hooks

## Registering Lifecycle Hooks

For example, the `onMounted` hook can be used to run code after the component has finished the initial rendering and created the DOM nodes:

```vue
<script setup>
import { onMounted } from "vue";

onMounted(() => {
  console.log(`the component is now mounted.`);
});
</script>
```

There are also other hooks which will be called at different stages of the instance's lifecycle, with the most commonly used being [`onMounted`](https://vuejs.org/api/composition-api-lifecycle.html#onmounted), [`onUpdated`](https://vuejs.org/api/composition-api-lifecycle.html#onupdated), and [`onUnmounted`](https://vuejs.org/api/composition-api-lifecycle.html#onunmounted).

When calling `onMounted`, Vue automatically associates the registered callback function with the current active component instance. This requires these hooks to be registered **synchronously** during component setup. For example, do not do this:

```js
setTimeout(() => {
  onMounted(() => {
    // this won't work.
  });
}, 100);
```

Do note this doesn't mean that the call must be placed lexically inside `setup()` or `<script setup>`. `onMounted()` can be called in an external function as long as the call stack is synchronous and originates from within `setup()`.

## Lifecycle Diagram

Below is a diagram for the instance lifecycle. You don't need to fully understand everything going on right now, but as you learn and build more, it will be a useful reference.

![Component lifecycle diagram](https://vuejs.org/assets/lifecycle.16e4c08e.png)
