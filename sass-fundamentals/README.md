## 1 - Basics: nesting, scoping and descendants

> Setup: With the `.scss` extension you can take a `.css` valid file, change the extension and start working on it because is valid Sass.

Nesting is placing a declaration in the declaration block of a parent element. Allows to style selector that depend in each other.
Nesting prevents mistyping when you have to manually repeat a parent selector.

Example:

```scss
.parent {
  color: #000;
  .child {
    font-size: 20px;
  }
}
```

### Parental selectors (&) and descendants

The parent selector `&` is useful to declare modifier or seconday classes whitin the same block of the modified selector. (Also for theming as you can see below)

You can use `>` as in css to declare a direct descendant.

Example:

```scss
.parent {
  background-color: #fff;
  &.is-dark {
    background-color: #000;
  }
}
```

### Summary

Here is a summary of the basic selectors:

```scss
.container {
  .some-child {
    /* compiles to .container .some-child */
  }
  > .direct-child {
    /* compiles to .container > .direct-child */
  }
  &.modifier {
    /* compiles to .container.modifier */
  }
  .theme-dark & {
    /* compiles to .theme-dark .container */
  }
  &:disabled {
    /* compiles to .container:disabled */
  }
  &-primary {
    /* compiles to .container-primary */
  }
}
```

## 2 - SASS @imports, partial imports and variables

@imports in css is not performant since it has to make a new request everytime. Sass introduces modularity to styles.

In sass, `@import` allows to import folders, sass files, partials or even css files.

**Partials**:

Partial imports allow you to import only a portion of a file.
Partials (.scss files that start with \_ character) are not transpiled to .css

```scss
// importing an external file
@import "my-sass-file.scss";

// from a library
@import "bootstrap";

// A "partial" in my project
@import "_layout";

// partial import
@import "my-sass-file.scss" partial;
```

**Variables:**

In a variable you can store values that can be reused in your project (such as colors or font sizes.)

```scss
/*
 * global variable with a default value
 * default means "use this value if it hasn't been set elsewhere
 */
$error_color: #f00 !default;

// this declaration has a local variable that can't be used outside the selector
.alert-error {
  $text_color: #ddd;
  background-color: $error_color;
  color: $text_color;
  text-shadow: 0 0 2px darken($text_color, 40%);
}
```

> In big projects, can be a good practice to have a .scss file only dedicated to global variables that you can use through the whole project.

> Some style libraries like bootstrap allow to set a custom variables file that will override the default values, making it very easy to theme the library.

### **Comments:**

Block comments are preserved in the transpiled .css output, one line comments will be removed.

```scss
/**
 * Will remain
 */
.foo {
  color: red; // Will be removed
}
```

You can interpolate variables (and functions) in the comments as such:

```scss
/**
 * Hue is #{hue(green)}
 */
```

## 3 - Mixins and arguments

Mixins are used to create entire reusable blocks of rules that can be used in multiple places.

Mixins are defined with `@mixin` and use with `@include`

```scss
@mixin font-size {
  font-size: 16px;
  font-weight: bold;
}

.section-title {
  @include font-size;
}
```

Mixins with arguments allow parametrization of the generated code.

```scss
// mixin with arguments
@mixin font-size($size, $weight) {
  font-size: $size;
  font-weight: $weight;
}

.section-title {
  @include font-size(16px, bold);
}
```

### Default arguments

You can to set a default value for an argument in a mixin.

```scss
// mixin with default argument
@mixin font-size($size: 16px, $weight: bold) {
   font-size: $size;
   font-weight: $weight;
}

.section-title {
    @include font-size(16px);
}

/**
 * Here the arguments are provided by name, in this case the order of the arguments
 * doesn't matter.
 */
.modal-title {
    @include font-size($weight: bold, $size: 14px);
```

If you use `null` as default value to the mixin, you can omit or skip that specific rule in the generated class:

```scss
@mixin foo($opacity: null) {
  color: #333;
  opacity: $opacity;
}
.btn {
  @include foo();
}
.other-btn {
  @include foo(0.5);
}
```

### Passing a declaration block to the mixin

```scss
@mixin foo($color) {
  color: $color;
  .inner {
    @content;
  }
}

.btn {
  @include foo(#c69) {
    color: red;
  }
}
/**
 * generated css:
 * .btn {
 *   color: #c69;
 * }
 * .btn .inner {
 *   color: red;
 * }
 */
```

## 4 - Built in Functions

Sass provides built in _modules_ that contain commonly used functions and mixins. In recent versions, you need to load the module with the `@use` rule (that is the replacement of @import even though @import is still supported but is going to be gradually phase out).

Example from [Sass documentation](https://sass-lang.com/documentation/modules):

```scss
@use "sass:color";

.button {
  $primary-color: #6b717f;
  color: $primary-color;
  border: 1px solid color.scale($primary-color, $lightness: 20%);
}
```

Sass provides the following built-in modules (from [Sass documentation](https://sass-lang.com/documentation/modules)):

- sass:math module provides functions that operate on numbers.

- sass:string module makes it easy to combine, search, or split apart strings.

- sass:color module generates new colors based on existing ones, making it easy to build color themes.

- sass:list module lets you access and modify values in lists.

- sass:map module makes it possible to look up the value associated with a key in a map, and much more.

- sass:selector module provides access to Sass’s powerful selector engine.

- sass:meta module exposes the details of Sass’s inner workings.

### Example: Color functions

Color functions allow you to manipulate colors in your project, such as changing the hue or lightening a color.

```scss
// darken color
$primary-color: darken(#000, 10%);

// lighten color
$secondary-color: lighten(#999, 10%);
```

### Custom User functions

Sass functions allow you to create custom functions for manipulating values.

Example:

```scss
// custom sass function
@function add-two-numbers($a, $b) {
  @return $a + $b;
}
```

## 5 - Control flow: @if and @for directives

The `@if` directive allows you to conditionally apply styles based on conditions.

```scss
// We want to apply the line-height value only if the $size is bigger than 20
@mixin foo($size) {
  font-size: $size;
  @if $size > 20 {
    line-height: $size;
  }
}

.small {
  @include foo(14px);
}
.large {
  @include foo(24px);
}

/* 
 * Generated css:
 * .small {
 *   font-size: 14px;
 * }
 * .large {
 *   font-size: 24px;
 *   line-height: 24px:
 *  }
 */
```

`@for` works similar to javascript, loops through a range of items. The following example produces rules for selectors h1 to h5:

```scss
@for $i from 1 through 5 {
  h#{$i} {
    font-size: 5rem - $i * 0.75rem;
  }
}
```

generates:

```css
h1 {
  font-size: 4.25rem;
}
h2 {
  font-size: 3.5rem;
}
h3 {
  font-size: 2.75rem;
}
h4 {
  font-size: 2rem;
}
h5 {
  font-size: 1.25rem;
}
```

## 6 - Data structures: lists, each, nth and maps

Lists are simply sequences of values, tha can be separated by commas, spaces or slashes.

### Example of using @each with a list:

```scss
$sizes: 40px, 50px, 80px;

@each $size in $sizes {
  .icon-#{$size} {
    font-size: $size;
    height: $size;
    width: $size;
  }
}
```

And the generated css:

```css
.icon-40px {
  font-size: 40px;
  height: 40px;
  width: 40px;
}

.icon-50px {
  font-size: 50px;
  height: 50px;
  width: 50px;
}

.icon-80px {
  font-size: 80px;
  height: 80px;
  width: 80px;
}
```

### nth

Function that returns the nth element of a list.

```scss
$gradients: (to left top, blue red), (to left top, blue, yellow);

.foo {
  background-color: linear-gradient(nth($gradients, 2));
}
```

### Maps

Similar to list, allows to use a collection of key-value pairs

```scss
$mymap: (
  dark: #009,
  light: #66f,
);

@mixin theme-button($t) {
  color: map-get($mymap, $t);
}

.btn-dark {
  @include theme-button("dark");
}

.btn-light {
  @incude theme-button('light');
}
```

## 7 - BEM

BEM stands for Block - Element - Modifier. It helps you create reusable and maintainable code.

- Block: Standalone entity, meaningful on its own (header, container, menu, checkpox, input, button).
- Element: Pat of a block that has no standalone meaning, and is semantically tied to its block (menu-item, list-item, checkbox-caption, header-title).
- Modifier: Flag on a block or element, used to change appearance, state or behavior (disabled, highlighted, checked, size-big, collor-yellow).

Example:

```scss
.block {
  display: flex;
  &__element {
    flex: 1;
  }
  &--modifier {
    flex: 2;
  }
}
```

## 8 - Mixins with @extend

@extend allows you to extend a selector and apply the same styles to multiple elements. Works similar to a mixin but with a standard scss selector.

Example:

```scss
.button {
  background-color: #000;
  padding: 8px;
}

.btn-primary {
  @extend .button;
  color: #fff;
}

.btn-secondary {
  @extend .button;
  color: #999;
}
```

### @extend with placeholders

Similar to the previous example but it doesn't create a css selector for the `.button`

```scss
/* It doesn't create .button in the output */
%button {
  background-color: #000;
  padding: 8px;
}

.btn-primary {
  @extend %button;
  color: #fff;
}

.btn-secondary {
  @extend %button;
  color: #999;
}
```

## 9 - Creating own functions

Similar to mixins, but have a return value. Can use internal values, parameters and global variables to make operations.

Example: This example takes a css color, finds out what the dominant RGB channel and returns only that channel, dropping the other two.

First, the function...

```scss
@function biggest_channel_only($color) {
  $r: red($color);
  $g: green($color);
  $b: blue($color);
  @if $r > $g and $r > $b {
    @return rgb($r, 0, 0);
  } @else if $g > $b and $g > $r {
    @return rgb(0, $g, 0);
  } @else {
    @return rgb(0, 0, $b);
  }
}
```

...And this is how to call it...

```scss
.cadetblue {
  color: biggest_channel_only(cadetblue);
}
.peru {
  color: biggest_channel_only(peru);
}
.lawngreen {
  color: biggest_channel_only(lawngreen);
}
```

And finally generates:

```css
.cadetblue {
  color: #0000a0;
}
.peru {
  color: #cd0000;
}
.lawngreen {
  color: #00fc00;
}
```
