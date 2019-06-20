# HTML Guidelines

The below guidelines cover only specific scenarios and should not be considered exhaustive. For more general HTML guidelines visit [Google's HTML Style guide](https://google.github.io/styleguide/htmlcssguide.html).

**Where there are differences our guidelines take precedence.**

## Table of contents

1. [Attributes](#attributes)
1. [Characters](#characters)
1. [Commenting](#commenting)
1. [Indenting](#indenting)
1. [Spacing](#spacing)
1. [Self-closing element](#self-closing-elements)

## [Attributes](#attributes)

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

## [Indenting](#indenting)

### Don't
```html
<h1 class="foo">
{{ product.title }}
</h1>
```

### Do
```html
<h1 class="foo">
  {{ product.title }}
</h1>
```

* Indent two spaces inside each opening HTML element

## [Spacing](#spacing)

### Don't
```html
<div id="Product" class="product" data-id="{{ product.id }}"js-product="container">
  <h1 class="product__title">{{ product.title }}</h1>
  <h2 class="product__subtitle">{{ product.vendor }}</h2>
  <div class="product__content product-content">
    <span class="product-content__info">{{ product.price | money }}</span><span class="product-content__info">{{ product.type }}</span>
    <p class="product-content__copy">{{ product.description }}</p>
  </div>
  <div class="footer"><div class="footer__inner"><span class="footer__name">{{ shop.name }}</span></div></div>
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
  <h1 class="product__title">
    {{ product.title }}
  </h1>

  <h2 class="product__subtitle">
    {{ product.vendor }}
  </h2>

  <div class="product__content product-content">
    <span class="product-content__info">
      {{ product.price | money }}
    </span>

    <span class="product-content__info">
      {{ product.type }}
    </span>

    <p class="product-content__copy">
      {{ product.description }}
    </p>
  </div>

  <div class="footer">
    <div class="footer__inner">
      <span class="footer__name">
        {{ shop.name }}
      </span>
    </div>
  </div>
</div>
```

* Use spacing to improve the readability of your code
* All elements should open onto a new line
* All elements should have padding of a newline between them

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