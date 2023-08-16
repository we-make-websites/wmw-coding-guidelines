# Accessibility

These rules apply to all Liquid, HTML, and Vue `<template>` code.

## Table on contents

* Native elements
* Keyboard focus
* Accessible Text
* Element IDs

## Native Elements

### Use `<button>` element for clickable items

#### Don't

```vue
<div
  class="dropdown-menu__input"
  @click="toggleMenu"
>
  <span v-text="label">
  <icon-chevron-down />
</div>

<responsive-image
  @click="handleImageClick"
/>

<a @click="toggleItem">
  Click here
</a>
```

#### Do

```vue
<button
  class="dropdown-menu__input"
  @click="toggleMenu"
>
  <span v-text="label">
  <icon-chevron-down />
</button>


<button @click="handleImageClick">
  <responsive-image />
</button>

```
  
* Prefer using native `<button>` element instead of adding click event handlers to non-interactive elements like `<div>`

#### Why

* The native `<button>` element comes with accessible defaults out of the box such as:
  * Keyboard interaction (buttons can be triggered with `Space` or `Enter` keys)
  * Keyboard focusable
  * Buttons are declared correctly to screen-readers
  * A button's interactivity can be natively disabled with the `disabled` attribute
* Re-creating all of these out of the box defaults on a `<div>` or other element would require additional attributes and event handlers which results in more code to write and more code shipped to the user - Using native elements has performance benefits too.
* E.g. here's a comparison of using a `<div>` element vs a `<button>` element:
```vue
// Bad
<div
  role="button"
  tabindex="0"
  @click="handleClick"
  @keydown="handleKeyDown"
>
  Click me
</div>

// Good
<button @click=:handleClick>
  Click me
</button>
```
  * Note: This `<div>` in the example does not include additional features that the `<button>` comes with, such as indicating a `disabled` state, the code for `handleKeyDown` so that only responds to `Enter` and `Tab` 


### Don't use `<a>` elements where a `<button>` would be better

#### Don't

```vue
<a @click="toggleItem">
  Click here
</a>

<a href="#" @click.prevent="doSomething">
  Do Something
</a>
```

#### Do

```vue
<button @click="toggleItem">
  Click here
</button>

<button @click.prevent="doSomething">
  Do something
</button>
```

* If an element is not linking somewhere, it's better to use a `<button>` element instead
  * `<a>` tags should not be used without a `href` attribute or with blank or javascript `href` attribute

#### Exceptions
* You should consider using a `<a>` element in cases where clicking on the element updates the URL and visiting that URL directly would display a different page
* An example would be Colour Swatches, where clicking on a swatch would update the current page and load in a new product in Vue, but the data is fetched from a separate product with its own URL
* The benefit here is that web crawlers can read these links to discover the other pages

```vue
<a
  :href="`/products/${swatchProduct.handle}`"
  @click.prevent="handleSwatchClick"
  v-text="swatchProduct.color"
/>
```

### Use input events rather than wrapping click events

#### Don't

```vue
<li
  v-for="item in items"
  :key="item.id"
  @click="filter(item.value)"
>
  <input
    :id="item.id"
    :checked="item.active"
    type="checkbox"
    :value="item.value"
  >
  <label
    :for="item.id"
    v-text="item.label"
  >
</li>
```

#### Do

```vue
<li
  v-for="item in items"
  :key="item.id"
>
  <input
    :id="item.id"
    :checked="item.active"
    type="checkbox"
    :value="item.value" 
    @change="filter($event.target.value)"
  >

  <label :for="item.id" v-text="item.label" />
</li>
```

* Instead of adding a click event to an `input` element's parent, use the native `input` or `change` events