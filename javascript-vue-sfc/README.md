# JavaScript & Vue SFC

Our guidelines are predominately based on [@shopify/eslint-plugin](https://github.com/Shopify/web-configs/tree/main/packages/eslint-plugin) with exceptions.

See also [Vue's style guide](https://v3.vuejs.org/style-guide/).

When saving `eslint --fix` should run and automatically fix most issues.

## Table of contents

* [Arrays](#arrays)
* [Comments](#comments)
* [Indents](#indents)
* [Objects](#objects)
* [Order](#order)
* [Semi-colons](#semi-colons)
* [Variables](#variables)

[← Back to homepage](../README.md)

## [Arrays](#arrays)

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

const bar = ['foo', 'bar']
```

* Arrays should be consistently formatted, either they should all be on one line, or they should all be on newlines
* For single item arrays there should be no spaces inside the opening and closing tags
* All items in an array must have a trailing comma

[ꜛ Back to TOC](#table-of-contents)

## Comments

* [Comments general](#comments-general)
* [Long form](#long-form-comments)
* [JSDoc](#jsdoc-comments)
* [Short form](short-form-comments)

### [General comments](#general-comments)

* Add a newline after comments, except those starting with `eslint` or `webpack`
* Comments should begin with a capital letter

### [Long form comments](#long-form-comments)

```js
/**
 * Folder: Title
 * -----------------------------------------------------------------------------
 * Description.
 *
 */
```

* Use the long form comment for intro comments inside the `<script>` tag in Vue files and at the start of JS files

### [JSDoc comments](#jsdoc-comments)

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

### [Short form comments](short-form-comments)

```js
/**
 * Comment.
 */
```

* Use the short form comment for general comments

[ꜛ Back to TOC](#table-of-contents)

## [Objects](#objects)

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

### [Attributes](#attributes)

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

### [Vue SFC order](#vue-sfc-order)

* Vue SFC files should be in the following order:
  * `<template>`
  * `<script>`
  * `<style>`

[ꜛ Back to TOC](#table-of-contents)

## Indents

* [Indents general](#indents-general)
* [Vue SFC indentation](#vue-sfc-indentation)

### [Indents general](#indents-general)

* When saving your file `eslint --fix` is automatically run, if there are items which need break onto multiple lines (such as arrays) then `eslint` will do this for you, however the indenting may be incorrect
* Apply common sense when saving a file which `eslint` then fixes, if the indenting doesn't look right it probably isn't, `eslint` doesn't lint indenting correctly at all times

### [Vue SFC indentation](#vue-sfc-indentation)

#### Don't

```js
<template>
<div class="product-card">
</div>
</template>

<script>
  export default {
    name: 'Product Card',
  }
</script>

<style lang="scss">
  @import '@/config/configuration';
  @import './featured-products';
</style>
```

#### Do

```js
<template>
  <div class="product-card">
  </div>
</template>

<script>
export default {
  name: 'Product Card',
}
</script>

<style lang="scss">
@import './featured-products';
</style>
```

* Only indent the contents of the `<template>` tag
* Scripts and styles should not be indented
* This gives more character length to write your JS in

[ꜛ Back to TOC](#table-of-contents)

## [Semi-colons](#semi-colons)

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

## [Variables](#variables)

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
