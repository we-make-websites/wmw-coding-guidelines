# Accessibility

These rules apply to all Liquid, HTML, and Vue `<template>` code.

The examples given here will mainly be written in Vue, but the same rules apply for vanilla JS code-bases.

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

## Keyboard Support

### Test keyboard focus
* Ensure that interactive elements can be focussed and activated using a keyboard
* Ensure that functionality that supports mouse interactions (click, mousedown, mouseleave etc) have suitable keyboard-accessible alternatives

### Test keyboard tab order
* Ensure that keyboard focus order is correct
  * The order that items are focussed when tabbing should match the visual order of the elements on the page
  * Try an ensure that the order elements in the DOM match the visual order on the site
* Exceptions can be made here when the order changes responsively, e.g. when the order is correct on mobile screen sizes 

### Focus Trap

* Use a focus trap when opening overlays so that a keyboard user is only able to tab between elements inside the overlay
* Ensure that the focus trap is cleared when an overlay is closed
* Ensure that the focus trap is updated when the overlay state changes
  * Example: if you have a multi-tiered navigation, ensure that the focus trap updates as the user navigates between tiers
  * Example: if you have a search overlay and the results update as you type, ensure that focus trap updates as the content changes
* Focus trapping can be achieved in Canvas projects with the `focusTrap` and `previousElement` helpers in the `accessibility.js` store - see [Canvas Gitbook - accessibility.js](https://we-make-websites.gitbook.io/canvas/features/vuex-stores/accessibility.js)
* Focus trapping can be achieved in Frame 3 projects by using the `@shopify/theme-a11y` package - see [@shopify/theme-ally docs](https://github.com/Shopify/theme-scripts/blob/master/packages/theme-a11y/README.md)

#### Why

* Using a focus trap prevents users from tabbing to elements outside of the overlay element, preventing an issue where the user might not know which element they are focussed on

## Accessible Text

### Ensure that `input` elements have corresponding `label` elements

#### Don't

```html
<input
  id="newsletter-email"
  name="email" 
  type="email"
  placeholder="Enter your email"
>
```

#### Do

```html
<label for="newsletter-email">
  Enter your email
</label>

<input
  id="newsletter-email"
  name="email" 
  type="email"
  placeholder="Enter your email"
>
```

* Ensuring that you have that an `input` has a corresponding `label` element helps to describe the purpose and can provide instructions for the input for users
* A `label` should be used even when the input has a `placeholder` attribute.
  * Assistive technology, such as screen-readers do not treat placeholder text as labels, so a `placeholder` should be used alongside a label, not instead of a label
* In some cases, a site's design may show inputs without visible labels. Sometimes this may be because other elements on the page give the input some context - for example a search text input placed directly next to a "Search" button.
  * If a input does not have a visible label, a `<label>` element should still be added for the benefit of screen readers
  * A label can be hidden visually by adding the `visually-hidden` class to the `label`
  ```html
  <!-- Label will be available to assistive technology, but visually hidden -->
  <label class="visually-hidden" for="search">
    Search the site
  </label>

  <input id="search" type="text" name="search">
  ```

### Ensure interactive elements have accessible text

#### Don't

```vue
<a class="site-logo" href="/">
  <brand-logo />
</a>

<button
  type="button"
  @click="openMenuDrawer"
>
  <icon-menu />
</button>

<a href="/account">
  <icon-account />
</a>

<a href="/sale">
  <!-- Image containing embedded text about a 10% off sale -->
  <img src="10-off-promo-image.jpg">
</a>


```

#### Do

```vue
<a class="site-logo" href="/">
  <brand-logo />
  
  <span class="visually-hidden">
    My Shop Name
  </span>
</a>

<button
  type="button"
  @click="openMenuDrawer"
>
  <icon-menu />
  
  <span class="visually-hidden">
    Toggle Menu
  </span>
</button>

<a href="/account">
  <icon-account />
  
  <span class="visually-hidden">
    Account
  </span>
</a>

<a href="/sale">
  <!-- Image containing embedded text about a 10% off sale -->
  <img src="10-off-promo-image.jpg" alt="Get 10% off">
</a>
```

* When using `<button>` or `<a>` elements containing SVG elements, ensure that accessible text is also included
  * If the text should not be visible, the `visually-hidden` class can be used
* If a `<button>` or `<a>` contains a `<img>` element - ensure that there is either appropriate `alt` text on the image, or add a supporting `visually-hidden` span with appropriate text

### Ensure correct usage of `aria-labelledby` and `aria-label` 

* The `aria-labelledby` and `aria-label` attributes provide another method for adding labels to elements
* These are alternatives to using a `visually-hidden` element showcased in the examples above

#### `aria-labelledby`

* `aria-labelledby` can be used to label an element based on the text content of another existing element by it's unique `id`
* If a heading is present inside a region, this could be a good candidate for `aria-labelledby`

```html
<section aria-labelledby="template-title">
  <h1 id="template-title">
    Title for the section
  </h1>

  <!-- other content -->
</section>
```
* Ensure that the element referenced by `aria-labelledby` exists on the page.
  * If the referenced element is missing, screen readers will still read out the the other text content contained in the element, but users may be missing out on the additional context that the referenced element might be adding
* When using `aria-labelledby` in a component, consider whether the element you are referencing existing inside the component's template code.
  * If the element you are targeting with `aria-labelledby` is stored in another component or snippet, you may not be able to guarantee that it will always be on the page.
  * In this case, opt to use `<span class="visually-hidden">` or `aria-label` instead.
* Additional documentation: [MDN - aria-labelledby](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-labelledby)

#### `aria-label`
* `aria-label` is another approach for labelling elements, and can be used when there is no visible element in the DOM that can be used directly as a label or referenced with `aria-labelledby`
* Examples of using this are similar to adding a `<span class="visually-hidden">` element

```vue
<button
  type="button"
  aria-label="Toggle Menu"
  @click="openMenuDrawer"
>
  <icon-menu />
</button>

<a href="/account" aria-label="Account">
  <icon-account />
</a>
```


# Additional Resources
https://www.powermapper.com/tests/screen-readers/aria/index.html
https://webaim.org/techniques/keyboard/