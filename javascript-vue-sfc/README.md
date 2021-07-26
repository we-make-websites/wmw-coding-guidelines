# JavaScript & Vue SFC

Our guidelines are predominately based on [@shopify/eslint-plugin](https://github.com/Shopify/web-configs/tree/main/packages/eslint-plugin) with exceptions.

See also [Vue's style guide](https://v3.vuejs.org/style-guide/).

When saving `eslint --fix` should run and automatically fix most issues.

## Table of contents

* [Arrays](#arrays)
* [Comments](#comments)
* [Objects](#objects)
* [Semi-colons](#semi-colons)

[← Back to homepage](../README.md)

## [Arrays](#arrays)

### Don't

```js
const foo = [
  'bar'
]

const bar = ['foo', 'bar' ]
```

### Do

```js
const foo = ['bar']

const bar = [
  'foo',
  'bar',
]
```

* Each item in an array should be on a newline if there are more than two items
* For single item arrays there should be no spaces inside the opening and closing tags
* All items in an array must always have a trailing comma

## Comments

* [General](#general)
* [Long form](#long-form-comments)
* [JSDoc](#jsdoc-comments)
* [Short form](short-form-comments)

### [General comments](#general-comments)

* Add a newline comments, except those starting with `eslint` or `webpack`
* Comments should begin with a capital letter

### [Long form comments](#long-form-comments)

```js
/**
 * [Folder]: [Title]
 * -----------------------------------------------------------------------------
 * [Description].
 *
 */
```

* Use the long form comment for intro comments inside the `<script>` tag in Vue files

### [JSDoc comments](#jsdoc-comments)

```js
/**
 * [Comment].
 * @param {String} [variable] - [Variable description].
 * @returns
 */
```

* Use the JSDoc comment before every function explaining what it does, its expected variables, and what it returns
* See [JSDoc documentation](https://jsdoc.app/) for details

### [Short form comments](short-form-comments)

```js
/**
 * [Comment].
 */
```

* Use the short form comment for general comments

## [Objects](#objects)

### Don't

```js
const foo = {
  'bar': true
}

const bar = {'foo': true, 'bar': false}
```

### Do

```js
const foo = { bar: true }

const bar = {
  foo: true,
  bar: false,
}
```

* Each property in an object should be on a newline if there are more than two properties
* For single property objects there should be a space inside the opening and closing braces
* Keys should not use single quotations (`'`) where the key is a single word
* All properties in an object must always have a trailing comma

## [Semi-colons](#semi-colons)

### Don't

```js
const foo = 'Bar';
```

### Do

```js
const foo = 'Bar'
```

* Do not use semi-colons (`;`) unless not doing so would cause an ASI failure
* E.g. the below would need a semi-colon otherwise when minified it would break:

```js
const array = [];

['foo', 'bar'].forEach((value) => {
  array.push(value))
}
```

[ꜛ Back to TOC](#table-of-contents)