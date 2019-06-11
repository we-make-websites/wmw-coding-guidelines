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
<textarea js-email="textarea" type="email" name="contact[email]" class="newsletter-modal__textarea" id="NewsletterModal-Email" value="{{ customer.email }}" placeholder="{{ 'general.newsletter_form.email_placeholder' | t }}" autocorrect="off" autocapitalize="off" data-show="true"></textarea>
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
  // Content
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
<input class='foo'></input>
```

### Do
```html
<input class="foo"></input>
```

* Use quotations `"` HTML elements, not apostrophes `'`

## [Commenting](#commenting)

### Don't
```html
<!-- Comment goes here -->
```

### Do
```html
{%- comment -%} Comment goes here {%- endcomment -%}
```

* Do not use HTML comments, use the `{% comment %}` filter with whitespace operators
* Liquid `{% comment %}` will not be rendered in the HTML
* Use spaces inside the `{% comment %}` tag

## [Indenting](#indenting)

### Don't
```html
<div class="foo">
Content
</div>
```

### Do
```html
<div class="foo">
  Content
</div>
```

* Indent two spaces inside each opening HTML element

## [Spacing](#spacing)

### Don't
```html
<span class="foo">Content</span>
<span class="foo">Content</span>
<div class="bar">
  Content
  More content
</div>
<div class="foobar">
  <span class="foobar__inner">Content</span>
  <img src="image.png" alt="">
  <img src="image.png" alt="">
</div>
```

### Do
```html
<span class="foo">Content</span>
<span class="foo">Content</span>

<div class="bar">
  Content
  More content
</div>

<div class="foobar">
  <span class="foobar__inner">Content</span>

  <img src="image.png" alt="">
  <img src="image.png" alt="">
</div>
```

* Use spacing to improve the readability of your code
* Single line elements can be placed next to each other
* Multi-line elements should be padding with a newline before and after
* If more than two single line elements are next to each other consider grouping them how they appear visually or by type

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