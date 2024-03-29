# HTML & Vue `<template>` Guidelines

These rules apply to all Liquid, HTML, and Vue `<template>` code.

See also [Vue's style guide](https://vuejs.org/style-guide/).

## Table of contents

* [Attributes](#attributes)
* [Characters](#characters)
* [Commenting](#commenting)
* [`<div>` or `<span>`](#div-or-span)
* [Indenting](#indenting)
* [Spacing & line character limits](#spacing--line-character-limits)
* [Self-closing element](#self-closing-elements)
* [Vue render tags](#vue-render-tags)

[← Back to homepage](../README.md)

## Attributes

* [Attribute casing](#attribute-casing)
* [Attribute order](#attribute-order)

### Attribute casing

* The general rule is if the attribute is to do with CSS then you use kebab-case for the value
* If the attribute is for JavaScript then use camelCase in the value
* kebab-case
  * `id`
  * `class`
  * `for`
* camelCase
  * `v-` bind
  * Vue `@` events
  * `data-`
  * `js-` selector values

### Attribute order

#### Don't

```html
{% # Liquid file %}
<input id="NewsletterModal-Email" class="newsletter-modal__textarea" autocapitalize="off" autocomplete="newsletter-address" autocorrect="off" name="contact[email]" placeholder="{{ 'general.newsletter_form.email_placeholder' | t }}" type="email" value="{{ customer.email }}" aria-labelledby="NewsletterModal-EmailLabel" data-show="true" js-newsletter="email">
```

#### Do

```html
{% # Liquid file %}
<input
  id="newsletter-modal-email"
  class="newsletter-modal__textarea"
  autocapitalize="off"
  autocomplete="newsletter-address"
  autocorrect="off"
  name="contact[email]"
  placeholder="{{ 'general.newsletter_form.email_placeholder' | t }}"
  type="email"
  value="{{ customer.email }}"
  aria-labelledby="newsletter-modal-email-label"
  data-show="true"
  js-newsletter="email"
>
```

* When an element has more than one attribute then each attribute should be on its own line
* Attributes should follow this order:
  * `is`
  * `v-for`
  * `v-if` / `v-else-if` / `v-else`
  * `v-show`
  * `v-cloak`
  * `id`
  * `ref`
  * `key`
  * `v-model`
  * `class`
  * { native }
  * `aria-`
  * `data-`
  * `js-`
  * `@event`
  * `v-html` / `v-text`
* { native } covers everything else, items within { native } should be in alphabetical order
* This makes it easier for managing merges in Git
* Make sure you trim trailing spaces after each line
* See also [spacing & line character limits](#spacing--line-character-limits)

> Be sure to follow the formatting; the trailing `>` should be on a newline with the attributes indented in two spaces.

## Characters

### Don't

```html
<div class='foo'></div>
```

### Do

```html
<div class="foo"></div>
```

* Use double quotations `"` in HTML elements, not apostrophes/single quotations `'`
* Note that you should use single quotations in Liquid tags and filters

[ꜛ Back to TOC](#table-of-contents)

## `<div>` or `<span>`

`<div>` and `<span>` are often used interchangeably but there are specific use cases for each:

* If the content is going to be displayed inline, use `<span>`
* If you want the element to display block, on its own line, use `<div>`
* If you're using CSS to change its `display` property, consider using the other

[ꜛ Back to TOC](#table-of-contents)

## Indenting

### Don't

```html
<p class="body-1">
{{ product.description }}
</p>
```

### Do

```html
<p class="body-1">
  {{ product.description }}
</p>
```

* Indent two spaces inside each opening HTML element

[ꜛ Back to TOC](#table-of-contents)

## Spacing & line character limits

### Don't

```html
{% # Liquid file %}
<div id="Product" class="product" data-id="{{ product.id }}" js-product="container">
  <h1 class="product__title">{{ product.title }}</h1>
  <h2 class="product__subtitle">{{ product.type }}</h2>
  <button class="product__button product-button"><span class="product-button__icon">{% render 'icon-misc' with icon: 'plus' %}</span><span class="product-button__text">{{ 'products.product.add_to_cart' | t }}</span></button>
  <div class="product__content product-content">
    <p class="product-content__description">{{ product.description }}</p>
    <p class="product-content__shipping">{{ pages['shipping'].content | strip_html }}</p>
  </div>
  <div class="footer"><div class="footer__inner"><span class="footer__copy">{{ 'footer.copyright' | t }}</span><span class="footer__name">{{ shop.name }}</span><img src="{{ settings.logo | img_url: '300x' }}" alt="{{ settings.logo.alt }}"></div></div>
</div>
```

### Do

```html
{% # Liquid file %}
<div
  id="Product"
  class="product"
  data-id="{{ product.id }}"
  js-product="container"
>
  <h1 class="product__title">{{ product.title }}</h1>
  <h2 class="product__subtitle">{{ product.type }}</h2>

  <button class="product__button product-button">
    <span class="product-button__icon">
      {% render 'icon-misc' with icon: 'plus' %}
    </span>

    <span class="product-button__text">
      {{ 'products.product.add_to_cart' | t }}
    </span>
  </button>

  <div class="product__content product-content">
    <p class="product-content__description">
      {{ product.description }}
    </p>

    <div class="product-content__shipping">
      {{ pages['shipping'].content }}
    </div>
  </div>

  <div class="footer">
    <div class="footer__inner">
      <span class="footer__copy">{{ 'footer.copyright' | t }}</span>
      <span class="footer__name">{{ shop.name }}</span>

      <img
        alt="{{ settings.logo.alt }}"
        src="{{ settings.logo | img_url: '300x' }}"
      >
    </div>
  </div>
</div>
```

* Use common sense spacing to improve the readability of your code
* [Block-level elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Block-level_elements) should open onto a new line and indent
* [Inline-level elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Inline_elements#Elements) should be written on a single line
* Single line elements should be grouped together by type
* Multi-line elements should have a newline between them
* This guideline includes empty block-level elements, e.g. the following is correct:

```html
{% # Liquid file %}
<div
  class="article__image"
  data-bgset="{% render 'responsive-bg-image' with image: article.image %}"
  js-article="image"
></div>
```

* Once you are writing something over multiple lines, each line should only have one attribute or value on it
* If an attribute's value exceeds 80 characters then it too should be split in the following format:

```html
{% # Liquid file %}
<div
  class="
    class-1
    class-2
    class-3
    class-4
  "
  data-handle="{{ product.handle }}"
  js-product="content"
>
  {% # Content %}
</div>
```

### Exceptions

* Short block-level elements like `<h1>`, `<li>` or `<title>` can be displayed on a single line unless they're greater than 80 characters long
* Inline-level elements greater than 80 characters long should follow multi-line rules

[ꜛ Back to TOC](#table-of-contents)

## Self-closing elements

### Don't

```html
{% # Liquid file %}
<component-handle />

<img
  alt=""
  src="{{ 'image.png' | file_url: '200x' }}"
/>

<input
  class="foo"
  placeholder="Content"
  type="text"
/>
```

### Do

```html
{% # Liquid file %}
<component-handle></component-handle>

<img
  alt=""
  src="{{ 'image.png' | file_url: '200x' }}"
>

<input
  class="foo"
  placeholder="Content"
  type="text"
>
```

* Vue custom elements in Liquid files should not use a self-closing tag as this is invalid and causes errors
* The `/` in self-closing tags is obsolete in most HTML elements, do not use it

### Exceptions

* Inside a Vue `<template>` tag you should use self-closing tags for elements which don't normally self-close:

```html
<!-- .vue file -->
<h1
  class="main-product__title"
  v-html="product.title"
/>
```

[ꜛ Back to TOC](#table-of-contents)

## Vue render tags

### Don't

```html
<!-- .vue file -->
<h1 class="main-product__title">
  {{ product.title }}
</h1>
```

### Do

```html
<!-- .vue file -->
<h1
  class="main-product__title"
  v-html="product.title"
/>
```

* Use `v-html` or `v-text` to render content in an element instead of using the template render tags

[ꜛ Back to TOC](#table-of-contents)
