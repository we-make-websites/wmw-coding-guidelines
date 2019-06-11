# Liquid Guidelines

The below guidelines cover only specific scenarios and should not be considered exhaustive. For more general HTML guidelines visit [Shopify's Liquid file requirements for themes](https://help.shopify.com/en/themes/development/theme-store-requirements/theme-file-requirements).

**Where there are differences our guidelines take precedence.**

## Shopify Cheatsheet

The [Shopify Cheatsheet](https://www.shopify.co.uk/partners/shopify-cheat-sheet) is no longer up-to-date. For a full and complete list of avaiable objects, tags, and filters see the [Liquid reference](https://help.shopify.com/en/themes/liquid/objects) documentation.

## Table of contents

1. [Characters](#characters)
1. [Commenting (inline)](#commenting-inline)
1. [Commenting (introductory)](#commenting-introductory)
1. [If or Case](#if-or-case)
1. [Indenting](#indenting)
1. [Schema settings](#schema-settings)
1. [Spacing](#spacing)
1. [Variable grouping](#variable-group)
1. [Variable naming](#variable-naming)
1. [Whitespace operators](#whitespace-operators)

## [Characters](#characters)

### Don't
```html
{%- assign variable = "It's time for fun" -%}
```

### Do
```html
{%- assign variable = 'It\'s time for fun' -%}
```

* Use apostrophes `'` in Liquid objects, tags, and filters, not quotations `"`
* Escape apostrophes if they appear inside the string

## [Commenting (inline)](#commenting-inline)

### Don't
```html
<!-- Only display description if tag is present -->
{% if has_tag %}
  <div class="foo">{{ product.description }}</div>
{% endif %}
```

### Do
```html
{%- comment -%} Only display description if tag is present {%- endcomment -%}
{% if has_tag %}
  <div class="foo">{{ product.description }}</div>
{% endif %}
```

## [Commenting (introductory)](#commenting-introductory)

### Do
```html
{% comment %}
------------------------------------------------------------------------------
  Section: Featured link
  - Used to display up to 2 images with text overlays with links.
------------------------------------------------------------------------------
{% endcomment %}
```

* Include an introductory comment at the start of each file
* Describe what folder it's in, the file's name, and list any special features or conditions
* If it's an include show an example include with its variables, e.g.
```html
{%- comment -%}
------------------------------------------------------------------------------
  Snippet: Responsive image
  It creates a style tag and it restricts an image from growing larger than its max resolution.

  // Shortened for brevity

  Usage:
  In your liquid template file, copy the following line
  - {% include 'responsive-image' with image: featured_image, image_class: "css-class", wrapper_class: "wrapper-css-class", max_width: 700, max_height: 800 %}
------------------------------------------------------------------------------
{%- endcomment -%}
```

## [If or Case](#if-or-case)

### Don't
```html
{% if variable == 'Hello' %}
  Content
{% elsif variable == 'Goodbye' %}
  Content
{% elsif variable == 'Good night' %}
  Content
{% else %}
  Content
{% endif %}
```

### Do
```html
{% case variable %}
  {% when 'Hello' %}
    Content
  {% when 'Goodbye' %}
    Content
  {% when 'Good night '%}
    Content
  {% else %}
    Content
{% endcase %}
```

* Do not use `{% if %}` for more than two conditions, use `{% case %}` instead
* `{% case %}` can only be used when exactly matching a condition, you can't use it to test if something `contains`, use `{% if %}` for this purpose

## [Indenting](#indenting)
```html
{% if variable %}
<h1>{{ product.title }}</h1>
{% endif %}
```

### Do
```html
{% if variable %}
  <h1>{{ product.title }}</h1>
{% endif %}
```

* Treat opening Liquid tags the same as HTML elements; indent two spaces inside

## Schema settings

* [`default` & `label`](#default-label)
* [Formatting & order](#formatting-order)
* [`type`](#type)

### [`default` & `label`](#default-label)
```json
{
  "type": "text",
  "id": "storeId",
  "label": "Store ID (enter a unique store ID)",
}
```

#### Do
```json
{
  "type": "text",
  "id": "store_id",
  "label": "Store ID",
  "default": "dev_store",
  "info": "Enter a unique store ID using a snake case naming convention. e.g: gb_store"
}
```

* Don't try to fit everything into `label`, use `info` for more details
* Add a default value where possible, this makes deployment easier

### [Formatting & order](#formatting-order)

#### Don't
```json
{
  "id": "storeId",
  "label": "Store ID (enter a unique store ID)",
  "type": "text"
}
```

#### Do
```json
{
  "type": "text",
  "id": "store_id",
  "label": "Store ID",
  "default": "dev_store",
  "info": "Enter a unique store ID using a snake case naming convention. e.g: gb_store"
}
```

* Use the prescribed order, any other key value pairs should be added below in alphabetical order
* Use snake case for `id`
* Use sentence case for `label` and `info`

### [`Type`](#type)

Specific rules for certain settings of `type`:
* `range` – Use for fixed number choices (such as font size, height, duration etc.)
* `richtext` – If you are changing `text` or `textarea` to `richtext` (or vice versa) you will need to delete any existing settings data as it will throw an error
* `select` – Use for fixed text choices (not for numbers)
* `textarea` – Do not use for raw HTML, use `html` for this
* `url` – Only use for all choices, use one of the limited selections if you are only expecting a certain output (`collection`, `product`, `blog`, `page`, and `article`), do not use for video, use `video_url` for this

## [Spacing](#spacing)

### Don't
```html
{{product.title|append:' text'}}
{%if variable%}Content{% else %}Content{%endif%}
```

### Do
```html
{{ product.title | append: ' text' }}

{% if variable %}
  Content
{% else %}
  Content
{% endif %}
```

* Add a space inside each object and tag
* Add spaces around filters
* If conditions should be on separate lines
* Separate blocks of Liquid code with a newline

## [Variable grouping](#variable-grouping)

### Don't
```html
{%- assign variable_a = 'Hello world' -%}

<h1>{{ product.title }}</h1>
<div>{{ product.description }}</div>
{%- assign variable_b = 'Hello world' -%}
{%- assign variable_c = 'Hello world' -%}

{% for variant in product.variants %}
  <h2>{{ variant.title }}</h2>
  {%- assign variable_d = variant.title -%}
{% endfor %}

{%- assign variable_e = 'Hello world' -%}
{% for product in recommendations.products %}
  <span>{{ product.title | link_to: product.url }}</span>
{% endfor %}
```

### Do
```html
{%- assign variable_a = 'Hello world' -%}
{%- assign variable_b = 'Hello world' -%}
{%- assign variable_c = 'Hello world' -%}
{%- assign variable_e = 'Hello world' -%}

<h1>{{ product.title }}</h1>
<div>{{ product.description }}</div>

{% for variant in product.variants %}
  <h2>{{ variant.title }}</h2>
  {%- assign variable_d = variant.title -%}
{% endfor %}

{% for product in recommendations.products %}
  <span>{{ product.title | link_to: product.url }}</span>
{% endfor %}
```

* Group all variables which are not set inside specific `{% forloop %}` at the top of the file
* If there are lots of variables consider placing them in a variables file, e.g. in the product section you would place them in a _product-variables.liquid_ snippet

## [Variable naming](#variable-naming)

### Don't
```html
{%- assign variableName = 'Hello world' -%}
{%- capture img-var -%}
  This isn't right
{%- endcapture -%}
```

### Do
```html
{%- assign variable_name = 'Hello world' -%}
{%- capture another_name}
  This is correct
{%- endcapture -%}
```

* Use snake case when naming variables
* Do not use abbreviations or shortened words
* Keep the name easy to understand

## [Whitespace operators](#whitespace-operators)

### Don't
```html
{%- if variable_name -%}
  {% assign another_variable = false %}
{%- endif -%}
```

### Do
```html
{% if variable_name %}
  {%- assign another_variable = false -%}
{% endif %}
```

* Only use [whitespace operators](https://help.shopify.com/en/themes/liquid/basics/whitespace_) on the `{% assign %}`, `{% capture %}`, and `{% comment %}` tags
* If you need to trim whitespace from objects then you can use whitespace operators such as `{{- section.settings.body_copy -}}`

> **🗒 Note:** Not all apps support whitespace operators.