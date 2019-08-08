# HTML Guidelines

The below guidelines cover only specific scenarios and should not be considered exhaustive. For more general HTML guidelines visit [Google's HTML Style guide](https://google.github.io/styleguide/htmlcssguide.html).

**Where there are differences our guidelines take precedence.**

## Table of contents

1. [Attribute order](#attribute-order)
1. [Characters](#characters)
1. [Commenting](#commenting)
1. [`<div>` or `<span>`](#div-or-span)
1. [Indenting](#indenting)
1. [Spacing & line character limits](#spacing--line-character-limits)
1. [Self-closing element](#self-closing-elements)

[← Back to homepage](../README.md)

## [Attribute order](#attribute-order)

### Don't
```html
<textarea js-email="textarea" type="email" name="contact[email]" class="newsletter-modal__textarea" id="NewsletterModal-Email" value="{{ customer.email }}" placeholder="{{ 'general.newsletter_form.email_placeholder' | t }}" autocorrect="off" autocapitalize="off" data-show="true">{{ 'general.pages.contact_placeholder' | t }}</textarea>
```

### Do
```html
<textarea
  id="NewsletterModal-Email"
  class="newsletter-modal__textarea"
  autocapitalize="off"
  autocorrect="off"
  name="contact[email]"
  placeholder="{{ 'general.newsletter_form.email_placeholder' | t }}"
  type="email"
  value="{{ customer.email }}"
  data-show="true"
  js-email="textarea"
>
  {{ 'general.pages.contact_placeholder' | t }}
</textarea>
```

* When an element has more than two attributes they should be stacked on newlines
* They should follow this order:
  * `id`
  * `class`
  * {native}
  * `data-`
  * `js-`
* {native} covers everything else, items within {native} should be in alphabetical order
* This makes it easier for managing merges in Git
* Make sure you trim trailing spaces after each line

> **🗒 Note:** Be sure to follow the formatting; the trailing `>` should be on a newline with the attributes indented in two spaces.

### Also
```html
<div class="hero template-article__hero-image lazyload" data-bgset="{% include 'responsive-bg-image', image: article.image %}"></div>

<div
  class="hero template-article__hero-image lazyload"
  data-bgset="{% include 'responsive-bg-image', image: article.image %}"
>
</div>
```

* Use multi line attributes if as a single line it would exceed 80 characters

[ꜛ Back to TOC](#table-of-contents)

## [Characters](#characters)

### Don't
```html
<div class='foo'></div>
```

### Do
```html
<div class="foo"></div>
```

* Use quotations `"` HTML elements, not apostrophes `'`

[ꜛ Back to TOC](#table-of-contents)

## [Commenting](#commenting)

### Don't
```html
<!-- Comment goes here -->
```

### Do
```html
{% comment %} Comment goes here {% endcomment %}
```

* Do not use HTML comments, use the `{% comment %}` filter with whitespace operators
* Liquid `{% comment %}` will not be rendered in the HTML
* Use spaces inside the `{% comment %}` tag

[ꜛ Back to TOC](#table-of-contents)

## [`<div>` or `<span>`](#div-or-span)

`<div>` and `<span>` are often used interchangeably but there are specific use cases for each:

* If the content is going to be displayed inline, use `<span>`
* If you want the element to display block, on its own line, use `<div>`
* If you're using CSS to change its `display` property, consider using the other

[ꜛ Back to TOC](#table-of-contents)

## [Indenting](#indenting)

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

## [Spacing & line character limits](#spacing--line-character-limits)

### Don't
```html
<div id="Product" class="product" data-id="{{ product.id }}" js-product="container">
  <h1 class="product__title">{{ product.title }}</h1>
  <h2 class="product__subtitle">{{ product.type }}</h2>
  <button class="product__button product-button"><span class="product-button__icon">{% include 'icon-misc' with icon: 'plus' %}</span><span class="product-button__text">{{ 'products.product.add_to_cart' | t }}</span></button>
  <div class="product__content product-content">
    <p class="product-content__description">{{ product.description }}</p>
    <p class="product-content__shipping">{{ pages['shipping'].content | strip_html }}</p>
  </div>
  <div class="footer"><div class="footer__inner"><span class="footer__copy">{{ 'footer.copyright' | t }}</span><span class="footer__name">{{ shop.name }}</span><img src="{{ settings.logo | img_url: '300x' }}" alt="{{ settings.logo.alt }}"></div></div>
</div>
```

### Do
```html
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
      {% include 'icon-misc' with icon: 'plus' %}
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
* Multi line elements should have a newline between them
* This includes empty block-level elements, e.g. the following is correct:

```html
<div
  class="article__image"
  data-bgset="{% include 'responsive-bg-image', image: article.image %}"
  js-article="image"
>
</div>
```

### Exceptions

* Short block-level elements like `<h1>`, `<li>` or `<title>` can be displayed on a single line unless they're greater than 80 characters long
* Inline-level elements greater than 80 characters long should follow multi line rules

[ꜛ Back to TOC](#table-of-contents)

## [Self-closing elements](#self-closing-elements)

### Don't
```html
<img src="image.png" alt=""/>
<input class="foo" placeholder="Content"/>
```

### Do
```html
<img src="image.png" alt="">
<input class="foo" placeholder="Content">
```

* The `/` in self-closing tags is obsolete, do not use it

[ꜛ Back to TOC](#table-of-contents)