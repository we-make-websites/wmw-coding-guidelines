# JavaScript & Vue SFC

Our guidelines are predominately based on [@shopify/eslint-plugin](https://github.com/Shopify/web-configs/tree/main/packages/eslint-plugin) with exceptions.

See also [Vue's style guide](https://v3.vuejs.org/style-guide/).

When saving `eslint --fix` should run and automatically fix most issues.

## Table of contents

* [Arrays](#arrays)
* [Comments](#comments)
* [If conditions](#if-conditions)
* [Indents](#indents)
* [Newlines](#newlines)
* [Objects](#objects)
* [Order](#order)
* [Passing parameters](#passing-parameters)
* [Semi-colons](#semi-colons)
* [Variables](#variables)
* [Whitespace](#whitespace)

[← Back to homepage](../README.md)

## Arrays

### Don't

```js
const foo = [
  'bar'
]

const bar = ['foo',
  'bar'
]
```

### Do

```js
const foo = ['bar']

const bar = [
  'foo',
  'bar',
]
```

* Single item arrays should be on a single line with no spaces inside the opening and closing tags
* Multiple item arrays should have each item on a newline with a trailing comma

[ꜛ Back to TOC](#table-of-contents)

## Comments

* [Comments general](#comments-general)
* [Long form](#long-form-comments)
* [JSDoc](#jsdoc-comments)
* [Short form](short-form-comments)

### General comments

* Add a newline after comments, except those starting with `eslint` or `webpack`
* Comments should begin with a capital letter

### Long form comments

```js
/**
 * Folder: Title
 * -----------------------------------------------------------------------------
 * Description.
 *
 */
```

* Use the long form comment for intro comments inside the `<script>` tag in Vue files and at the start of JS files

### JSDoc comments

```js
/**
 * [Comment].
 * @param {String} variable - Variable description.
 * @returns {String}
 */
```

* Use the JSDoc comment before every function explaining what it does, its expected variables, and what it returns
* Wrap the variable in square brackets to denote that it's optional, e.g. `[variable]`
* See [JSDoc documentation](https://jsdoc.app/) for details

### Short form comments

```js
/**
 * Comment.
 */
```

* Use the short form comment for general comments

[ꜛ Back to TOC](#table-of-contents)

## If conditions

* [Early return](#early-return)
* [Ternary operator](#ternary-operator)

### Early return

#### Don't

```js
if (condition) {
  runFunctionA()
} else {
  runFunctionB()
}
```

#### Do

```js
if (condition) {
  runFunctionA()
  return
}

runFunctionB()
```

* Use an early return instead of `if ... else` where possible to make the amount of code the browser has to run shorter
* Where both parts of the condition lead to the same outcome you should continue using `if ... else`, e.g.

```js
let foo = ''

if (condition) {
  foo += 'Foo'
} else {
  foo += 'Bar'
}

foo += 'Baz'

return foo
```

### Ternary operator

#### Don't

```js
let foo = 'Baz'

if (condition) {
  foo = 'Bar'
}

const bar = condition ? 'When the ternary would exceed 80 characters then split onto newlines' : 'Foo'
```

#### Do

```js
const foo = condition ? 'Bar' : 'Baz'

const bar = condition
  ? 'When the ternary would exceed 80 characters then split onto newlines'
  : 'Foo'
```

* When assigning a variable it's preferrable to use a ternary operator, e.g.
* Split the ternary operator onto newlines when it exceeds 80 characters

[ꜛ Back to TOC](#table-of-contents)

## Newlines

### Don't

```js
const foo = 'Bar'
const bar = [
  true,
  false,
  true,
]
bar.forEach((item) => {
  if (item) return
  runFunction(item)
})
```

## Do

```js
const foo = 'Bar'

const bar = [
  true,
  false,
  true,
]

bar.forEach((item) => {
  if (item) {
    return
  }

  runFunction(item)
})
```

* Newlines are free, use them to break up blocks of code
* Multi-line objects should have newlines before and after them
* Do not use single line if conditions

[ꜛ Back to TOC](#table-of-contents)

## Objects

### Don't

```js
const foo = {
  'bar': true
}

const bar = {'foo': true, 'bar-baz': false}
```

### Do

```js
const foo = { bar: true }

const bar = {
  foo: true,
  'bar-baz': false,
}
```

* Each property in an object should be on a newline if there are multiple properties
* For single property objects there should be a space after the opening brace and before the closing brace
* Single word keys should not use an apostrophes/single quotations (`'`)
* All properties in a multi line object must always have a trailing comma

[ꜛ Back to TOC](#table-of-contents)

## Order

* [Attributes](#attributes)
* [Vue SFC order](#vue-sfc-order)

### Attributes

* As per [Vue's style guide](https://v3.vuejs.org/style-guide/#component-instance-options-order-recommended) your Vue `<script>` export should follow this order:
  * `name`
  * `components`
  * `props`
  * `emits`
  * `setup`
  * `data`
  * `computed`
  * `watch`
  * Lifecycle events in the order they're called
  * `methods`

### Vue SFC order

* Vue SFC files should be in the following order:
  * `<template>`
  * `<script>`
  * `<style>`

[ꜛ Back to TOC](#table-of-contents)

## Indents

* [Indents general](#indents-general)
* [Vue SFC indentation](#vue-sfc-indentation)

### Indents general

* When saving your file `eslint --fix` is automatically run, if there are items which need to break onto multiple lines (such as arrays) then `eslint` will do this for you, however the indenting may be incorrect
* Apply common sense when saving a file which `eslint` then fixes, if the indenting doesn't look right it probably isn't, `eslint` doesn't lint indenting correctly at all times

### Vue SFC indentation

#### Don't

```html
<template>
<div class="example">
  <!-- Code -->
</div>
</template>

<script>
  export default {
    name: 'Example',
  }
</script>

<style lang="scss">
  @import './example';
</style>
```

#### Do

```html
<template>
  <div class="Example">
    <!-- Code -->
  </div>
</template>

<script>
export default {
  name: 'Example',
}
</script>

<style lang="scss">
@import './example';
</style>
```

* Only indent the contents of the `<template>` tag
* `<script>` and `<style>` tags should not be indented
* This gives more character length to write your JS in

[ꜛ Back to TOC](#table-of-contents)

## Passing parameters

### Don't

```js
array.forEach(item => {
  // Code
})
```

### Do

```js
array.forEach((item) => {
  // Code
})
```

* Wrap the passed parameters in brackets `()`

[ꜛ Back to TOC](#table-of-contents)

## Semi-colons

### Don't

```js
const foo = 'Bar';
```

### Do

```js
const foo = 'Bar'
```

* Do not use semi-colons (`;`) unless not doing so would cause an ASI error
* E.g. the below needs a semi-colon on the first line otherwise when minified it would break:

```js
const array = [];

['foo', 'bar'].forEach((value) => {
  array.push(value))
}
```

[ꜛ Back to TOC](#table-of-contents)

## Variables

### Don't

```js
var foo = true

let bar = [
  'One',
  'Two',
  'Three',
]

const baz = 'String'
baz += ', extra string'
```

### Do

```js
const foo = true

const bar = [
  'One',
  'Two',
  'Three',
]

let baz = 'String'
baz += ', extra string'
```

* Do not use `var`, use `let` or `const` as the situation dictates
* Use `const` for variables which are _never_ reassigned
* Use `let` for variables which are reassigned

[ꜛ Back to TOC](#table-of-contents)

## Whitespace

### Don't

```js
const foo='Bar'

array.forEach((item)=>{
  // Code
})

for(const item of array){
  // Code
}

if(condition==='foo'){
  // Code
}else{
  // Code
}
```

### Do

```js
const foo = 'Bar'

array.forEach((item) => {
  // Code
})

for (const item of array) {
  // Code
}

if (condition === 'foo') {
  // Code
} else {
  // Code
}
```

* Whitespace is free, use it
* It makes the code more readable

[ꜛ Back to TOC](#table-of-contents)
