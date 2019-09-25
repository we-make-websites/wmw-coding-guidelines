# Liquid Guidelines

The below guidelines cover only specific scenarios and should not be considered exhaustive. For more general HTML guidelines visit [Shopify's Liquid file requirements for themes](https://help.shopify.com/en/themes/development/theme-store-requirements/theme-file-requirements).

**Where there are differences our guidelines take precedence.**

## Shopify Cheatsheet

The [Shopify Cheatsheet](https://www.shopify.co.uk/partners/shopify-cheat-sheet) is no longer up-to-date. For a full and complete list of available objects, tags, and filters see the [Liquid reference](https://help.shopify.com/en/themes/liquid/objects) documentation.

## Table of contents

1. [Commenting](#commenting)
1. [Conditional statements](#conditional-statements)
1. [DRY (Don't Repeat Yourself)](#dry-dont-repeat-yourself)
1. [`{% include %}`](#-include-)
1. [Formatting](#formatting)
1. [Language strings](#language-strings)
1. [Naming](#naming)
1. [Schema settings](#schema-settings)
1. [Snippets](#snippets)
1. [Split characters](#split-characters)
1. [Variables](#variables)
1. [Whitespace controls](#whitespace-controls)

[← Back to homepage](../README.md)

## Commenting

* [Inline](#inline)
* [Introductory](#introductory)

### [Inline](#inline)

#### Don't
```html
<!-- Only display description if tag is present -->
{% if has_tag %}
  <div class="foo">{{ product.description }}</div>
{% endif %}
```

#### Do
```html
{% comment %} Only display description if tag is present {% endcomment %}
{% if has_tag %}
  <div class="foo">
    {{ product.description }}
  </div>
{% endif %}
```

* Use the `{% comment %}` tag for inline comments, not HTML comments
* For `.js.liquid` or `.scss.liquid` file use the language appropriate comment

### [Introductory](#introductory)

#### Do
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
{% comment %}
------------------------------------------------------------------------------
  Snippet: Responsive image
  It creates a style tag and it restricts an image from growing larger than its max resolution.

  Usage:
  In your liquid template file, copy the following line
  - {% include 'responsive-image' with image: featured_image, image_class: "css-class", wrapper_class: "wrapper-css-class", max_width: 700, max_height: 800 %}
------------------------------------------------------------------------------
{% endcomment %}
```

[ꜛ Back to TOC](#table-of-contents)

## Conditional statements

* [Conditional settings](#conditional-settings)
* [Conditional spacing](#conditional-spacing)
* [`{% if %}` or `{% case %}`](#-if--or--case-)
* [Inline `{% if %}` statements](#inline--if--statements)

### [Conditional settings](#conditional-settings)

#### Don't
```html
{% for block in section.blocks %}
  <div class="block">
    <img src="{{ block.settings.image | img_url: '2000x' }}" alt="{{ block.settings.image.alt }}">

    <h2 class="block__title">{{ block.settings.title }}</h2>
    <div class="block__text rte">{{ block.settings.text }}</div>

    <a class="block__button" href="{{ block.settings.button_url }}">
      {{ block.settings.button_text }}
    </a>
  </div>
{% endfor %}
```

#### Do
```html
{% for block in section.blocks %}
  <div class="block">
    {% if block.settings.image != '' %}
      <img
        alt="{{ block.settings.image.alt }}"
        src="{{ block.settings.image | img_url: '2000x' }}"
      >
    {% endif %}

    {% if block.settings.title != '' %}
      <h2 class="block__title">{{ block.settings.title }}</h2>
    {% endif %}

    {% if block.settings.text != '' %}
      <div class="block__text rte">
        {{ block.settings.text }}
      </div>
    {% endif %}

    {% if block.settings.button url != '' and block.settings.button_text != '' %}
      <a class="block__button" href="{{ block.settings.button_url }}">
        {{ block.settings.button_text }}
      </a>
    {% endif %}
  </div>
{% endfor %}
```

* In areas where the client can customise the content do not assume that they will want to display every part of a section's settings
* Wrap each setting in a `{% if %}` to hide it if no content is entered
* Use `!= ''` rather than `{% if condition %}` or `!= blank` as it is more reliable, it is possible for a setting to exist if it previously had a value
* When testing make sure the section does not appear visually broken, the client will expect it to work with missing settings

### [Conditional spacing](#conditional-spacing)

#### Don't
```html
{% if condition %}
  <!-- Multiple lines of code -->
  <!-- Multiple lines of code -->
  <!-- Multiple lines of code -->
{% else %}
  <!-- Multiple lines of code -->
  <!-- Multiple lines of code -->
  <!-- Multiple lines of code -->
{% endif %}
```

#### Do
```html
{% if condition %}
  <!-- Multiple lines of code -->
  <!-- Multiple lines of code -->
  <!-- Multiple lines of code -->

{% else %}
  <!-- Multiple lines of code -->
  <!-- Multiple lines of code -->
  <!-- Multiple lines of code -->
{% endif %}
```

* When an `{% if %}` tag spans multiple lines add a newline before the `{% else %}` tag
* This helps visually separate the code and make it easier to scan

### [`{% if %}` or `{% case %}`](#-if--or--case-)

#### Don't
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

#### Do
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

### [Inline `{% if %}` statements](#inline--if--statements)

#### Don't
```html
<div class="row {% if section.settings.width == 'full' or section.settings.display == 'full' %}row--full-width{% endif %} {% if is_hidden %}is-hidden{% endif %}"></div>
```

#### Do
```html
{% if section.settings.width == 'full' or section.settings.display == 'full' %}
  {% assign row_class = 'row--full-width' %}
{% endif %}

<div
  class="
    row
    {{ row_class }}
    {% if is_hidden %} is-hidden{% endif %}
  "
>
  <!-- Content -->
</div>
```

* Do not put long `{% if %}` tags inline, assign them separately
* If the `{% if %}` tag is short you can put it inline
* Include the space inside the `{% if %}` tag so it is only outputted when the if condition is met

[ꜛ Back to TOC](#table-of-contents)

## [DRY (Don't Repeat Yourself)](#dry-dont-repeat-yourself)

### Don't
```html
<!-- A custom page slider so four slides have been hard-coded in section settings -->
<!-- This means we can't use {% for block in section.blocks %} -->
<div class="slide slide--1">
  <div
    class="slide__background"
    style="background-image: url({{ block.settings.slide_1_image | img_url: '2000x' }});"
  ></div>

  <div class="slide__text">
    <h2 class="slide__title">{{ block.settings.slide_1_text }}</h2>
    <p class="slide__copy">{{ block.settings.slide_1_text }}</p>

    <div class="slide__buttons">
      <a class="slide__button" href="{{ block.settings.slide_1_button_1_url }}">
        {{ block.settings.slide_1_button_1_text }}
      </a>

      <a class="slide__button" href="{{ block.settings.slide_1_button_2_url }}">
        {{ block.settings.slide_1_button_2_text }}
      </a>
    </div>
  </div>
</div>

<!-- All of the code is repeat for each hard-coded slide instance -->
<div class="slide slide--2">...</div>
<div class="slide slide--3">...</div>
<div class="slide slide--4">...</div>
```

### Do
```html
{% for i in (1..4) %}
  {% assign slide_image = 'slide_#_image' | replace: '#', i %}
  {% assign slide_heading = 'slide_#_heading' | replace: '#', i %}
  {% assign slide_text = 'slide_#_text' | replace: '#', i %}

  <div class="slide slide--{{ i }}">
    <div
      class="slide__background"
      style="background-image: url({{ block.settings[slide_image] | img_url: '2000x' }});"
    ></div>

    <div class="slide__text">
      <h2 class="slide__heading">{{ block.settings[slide_heading] }}</h2>

      <p class="slide__copy">
        {{ block.settings[slide_text] }}
      </p>

      <div class="slide__buttons">
        {% for j in (1..2) %}
          {% assign slide_button_url = 'slide_#_button_%_url' |
            replace: '#', i |
            replace: '%', j
          %}

          {% assign slide_button_text = 'slide_#_button_%_text' |
            replace: '#', i |
            replace: '%', j
          %}

          <a class="slide__button" href="{{ block.settings[slide_button_url]}}">
            {{ block.settings[slide_button_text] }}
          </a>
        {% endfor %}
      </div>
    </div>
  </div>
{% endfor %}
```

* Use `{% for i in (1..#) %}` in combination with `{% assign %}` and replace to remove repetitive code
* Use the iteration to set variables which are then used to call the block's settings
* Use Liquid's built in tags to avoid repeating code

[ꜛ Back to TOC](#table-of-contents)

## Formatting

* [`{% include %}`](#-include-)
* [Characters](#characters)
* [Indenting](#indenting)
* [Spacing & line character limits](#spacing--line-character-limits)

### [`{% include %}`](#-include-)

#### Don't
```html
{% include 'icon-misc', icon: 'search', colour: 'red' %}
{% include 'social-sharing' with share_title: product.title, share_permalink: product.url, share_image: product.featured_image %}
```

#### Do
```html
{% include 'icon-misc' with icon: 'search', colour: 'red' %}

{% include 'social-sharing' with
  share_image: product.featured_image,
  share_permalink: product.url,
  share_title: product.title,
%}
```

* When setting variables on your `{% include %}` always use `with` instead of starting with a comma
* After the first variable declaration you must use a comma `,`
* If there are more than two variables and it goes over 80 characters then break it into a multi-line include
* Sort the variables alphabetically and end each line with a comma `,`

### [Characters](#characters)

#### Don't
```html
{% assign variable = "It's time for fun" %}
```

#### Do
```html
{% assign variable = 'It\'s time for fun' %}
```

* Use apostrophes `'` in Liquid objects, tags, and filters, not quotations `"`
* Escape apostrophes if they appear inside the string

### [Indenting](#indenting)

#### Don't
```html
{% if variable %}
<h1>{{ product.title }}</h1>
{% endif %}
```

#### Do
```html
{% if variable %}
  <h1>{{ product.title }}</h1>
{% endif %}
```

* Treat opening Liquid tags the same as HTML elements; indent two spaces inside

### [Spacing & line character limits](#spacing--line-character-limits)

#### Don't
```html
{% if template contains 'search' or template contains 'account' or template contains 'customer' or template contains 'cart' %}<meta name="robots" content="noindex, nofollow">{% endif %}
{% assign sanitized_var = string|downcase|split: '/'|last|remove:'<p>'|remove:'</p>'|money_with_currency %}
{%if variable%}<h1 class="product__title">{{ product.title }}</h1>
  <div class="product__price product__price--large money subtitle-1">{{ product.price | money }}</div>{% else %}<h2 class="product__title">{{ product.title }}</h2>{%endif%}
```

#### Do
```html
{% if
  template contains 'search' or
  template contains 'account' or
  template contains 'customer' or
  template contains 'cart'
%}
  <meta content="noindex, nofollow" name="robots">
{% endif %}

{% assign sanitized_variable = string |
  downcase |
  split: '/' |
  last |
  remove:'<p>' |
  remove:'</p>' %} |
  money_with_currency
%}

{% if variable %}
  <h1 class="product__title">{{ product.title }}</h1>

  <div
    class="
      product__price
      product__price--large
      money
      subtitle-1
    "
  >
    {{ product.price | money }}
  </div>

{% else %}
  <h2 class="product__title">{{ product.title }}</h2>
{% endif %}
```

* Add spaces inside each object and tag
* Add spaces around filters
* Separate blocks of Liquid code with a newline
* Opening `{% if %}` tags should be on separate lines
* `{% if %}` tags with more than two filters and exceeding 80 characters should be split onto multiple lines
* If the contents of an `{% if %}` tag spans more than two lines add a newline before any following `{% elsif %}` or `{% else %}` tags
* `{% assign %}` tags with more than two filters and exceeding 80 characters should be broken up over multiple lines and indented
* Once you are writing something over multiple lines, each line should only have one attribute or value on it
* For details on HTML spacing see the [HTML](../html/README.md) rule for [Spacing & line character limit](../html/README.md#spacing--line-character-limits)

[ꜛ Back to TOC](#table-of-contents)

## [Language strings](#language-strings)

### Don't
```html
<div class="product-card">
  <h2 class="product-card__title">{{ product.title }}</h2>

  <span class="product-card__stock">
    {% if product.available %}
      In stock
    {% else %}
      Out of stock
    {% endif %}
  </span>
</div>
```

### Do
```html
<div class="product-card">
  <h2 class="product-card__title">{{ product.title }}</h2>

  <span class="product-card__stock">
    {% if product.available %}
      {{ 'products.product.in_stock' | t }}
    {% else %}
      {{ 'products.product.out_of_stock' | t }}
    {% endif %}
  </span>
</div>
```

* Never hard-code text in your template files
* Always use Shopify's [translation filter](https://help.shopify.com/en/themes/development/theme-store-requirements/internationalizing/translation-filter)
* Most of our clients are multi-lingual so it is expected that they will be able to translate their store without requiring additional development

[ꜛ Back to TOC](#table-of-contents)

## Naming

1. [Section template naming](#section-template-naming)
1. [Snippet naming](#Snippet-naming)
1. [Tag naming](#tag-naming)

### [Section template naming](#section-template-naming)

```html
template-[section].liquid
```

* If a section replaces the template's content it should be prefixed with `template-`
* E.g. `template-product.liquid` or `template-collection.liquid`

### [Snippet naming](#snippet-naming)

```html
icon-[snippet].liquid
```

* Snippets used to contain inline SVGs should be prefixed with `icon-`
* E.g. `icon-payment.liquid`

```html
section-[snippet].liquid
```

* Snippets that contain a section's content should be prefixed with `section-`
* E.g. `section-hero.liquid` or `section-featured-collection.liquid`

### [Tag naming](#tag-naming)

```html
tag_name
tag_name: [value]
tag_name: [value1]_[value2] (etc.)
```

* Use the naming convention `tag_name: [value]` for admin tags (products, orders, customers) with a value and `tag_name` for tags without a value
* In most cases `[value]` should be in lowercase to make comparisons easier. However in instances where the value is outputted on the front-end in a specific case (e.g. Title Case) make sure this is clear in the tech spec
* If you need to store separate values in the same tag then separate them using `_`  such as `type_modal: [Model]_[Year]` (this isn't snake_case)
* Do not use boolean values (e.g. `has_addon: true`) as simply having the tag in the first place is enough to know that it is `true` (e.g. `has_addon`)
* If the tag is to be used in a search string (and therefore needs to be handlelised) then use the naming convention `tag_name--[value]`, this allows the value to be kebab-cased (e.g. `build_date--2019-07-03`)
* Never put formatted money into the tag (e.g. `monthly_cost: £10`) as tags do not support all the formatting standards required by international stores (e.g. `monthly_cost: €12,34` will not be allowed because the comma ends the tag), instead use `monthly_cost: 1234` and format the cost using Liquid or JavaScript

[ꜛ Back to TOC](#table-of-contents)

## Schema settings

* [`default` & `label`](#default-&-label)
* [Formatting & order](#formatting-&-order)
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

### [Formatting & order](#formatting-&-order)

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

[ꜛ Back to TOC](#table-of-contents)

## Snippets

* [Section snippets](#section-snippets)
* [Snippets general](#snippets-general)

### [Section snippets](#section-snippets)

* Place the content for a section inside a snippet (see [snippet naming](#snippet-naming) for advice on naming)
* This allows us to use a section anywhere as we can place the snippet inside a `{% case %}` block setup

#### Example

##### Section
```html
{% comment %}
----------------------------------------------------------------------------
  Section: Featured text
  – Large text banner.
----------------------------------------------------------------------------
{% endcomment %}
<section
  class="featured-text-section"
  data-section-id="{{ section.id }}"
  data-section-type="featured-text"
>
  {% include 'section-featured-text' with object: section %}
</section>

{% schema %}
  {
    "name": "Featured Text",
    <!-- Section settings -->
  }
{% endschema %}
```

##### Snippet
```html
<!-- Code removed for brevity -->
<div class="featured-text">
  {% if object.settings.title != '' %}
    <p class="featured-text__copy body-1">
      {{ object.settings.title }}
    </p>
  {% endif %}
</div>
```

* With this setup you would then be able to include `{% include 'section-featured-text' with object: block %}` on a page other than the homepage

### [Snippets general](#snippets-general)

* Use Liquid snippets to keep files small and manageable
* In the same way a block gets its own SCSS and JS file, consider splitting it into its own snippet
* Snippets are especially useful for repeating content

[ꜛ Back to TOC](#table-of-contents)

## [Split characters](#split-characters)

Split characters are used to effectively provide multiple description fields on the product page. It is useful when you need to output long form content in multiple locations.

### Example
```html
{% if product.description != '' %}
  {% assign full_description = product.description |
    remove: '<meta charset="utf-8">' |
    remove: '<meta charset="utf-8" />' |
    remove: '<span>' |
    remove: '</span>' |
    remove: '<p>&nbsp;</p>' |
    remove: '<p> </p>' |
    remove: '<p></p>'
  %}

  {% if full_description contains '---DESCRIPTION---' %}
    {% assign description = full_description |
      split: '<p>---DESCRIPTION---</p>' |
      last |
      split: '<p>---' |
      first
    %}
  {% endif %}

  {% if full_description contains '---SIZE GUIDE---' %}
    {% assign size_guide = full_description |
      split: '<p>---SIZE GUIDE---</p>' |
      last |
      split: '<p>---' |
      first
    %}
  {% endif %}
{% endif %}

{% if description %}
  <div class="product__description rte">
    {{ description }}
  </div>
{% endif %}

{% if size_guide %}
  <div class="product__size-guide rte">
    {{ size_guide }}
  </div>
{% endif %}
```

* Use split characters in the format `---[NAME]---` where `[NAME]` is the identifying name of the split content
* `[NAME]` can have spaces, e.g. `---COMPATIBLE INFO---`
* Assign the split description to variables at the top of the file then output the variable as and when you need it
* Use split characters over metafields as they don't require any special apps to access and content can be automatically imported using apps like Excelify
* Shopify tends to add extra HTML so in the example this has been stripped off, including empty line returns

[ꜛ Back to TOC](#table-of-contents)

## Variables

1. [Assigning](#variable-assigning)
1. [Grouping](#variable-grouping)
1. [Naming](#variable-naming)

### [Variable assigning](#variable-assigning)

#### Don't
```html
{% for product in collection.products %}
  {% if product.tags contains 'has_shipping' %}
    {% assign has_shipping_tag = true %}
  {% endif %}
{% endif %}
```

#### Do
```html
{% for product in collection.products %}
  {% assign has_shipping_tag = false %}

  {% if product.tags contains 'has_shipping' %}
    {% assign has_shipping_tag = true %}
  {% endif %}
{% endif %}
```

* Assign the default state of a variable before you first use it
* This avoids false positives in `{% for %}` loops as it resets the variable at the start of each item
* In the Don't example above once one product had the tag the variable is set to `true`, but nothing resets it meaning all products after it will have the variable set to `true`

### [Variable grouping](#variable-grouping)

#### Don't
```html
{% assign variable_a = 'Hello world' %}

<h1>{{ product.title }}</h1>
<div>{{ product.description }}</div>
{% assign variable_b = 'Hello world' %}
{% assign variable_c = 'Hello world' %}

{% for variant in product.variants %}
  <h2>{{ variant.title }}</h2>
  {% assign variable_d = variant.title %}
{% endfor %}

{% assign variable_e = 'Hello world' %}
{% for product in recommendations.products %}
  <span>{{ product.title | link_to: product.url }}</span>
{% endfor %}
```

#### Do
```html
{% assign variable_a = 'Hello world' %}
{% assign variable_b = 'Hello world' %}
{% assign variable_c = 'Hello world' %}
{% assign variable_e = 'Hello world' %}

<h1>{{ product.title }}</h1>
<div>{{ product.description }}</div>

{% for variant in product.variants %}
  <h2>{{ variant.title }}</h2>
  {% assign variable_d = variant.title %}
{% endfor %}

{% for product in recommendations.products %}
  <span>{{ product.title | link_to: product.url }}</span>
{% endfor %}
```

* Group all variables which are not set inside specific `{% forloop %}` at the top of the file
* If there are lots of variables consider placing them in a variables file, e.g. in the product section you would place them in a _product-variables.liquid_ snippet

### [Variable naming](#variable-naming)

#### Don't
```html
{% assign variableName = 'Hello world' %}
{% capture img-var %}
  This isn't right
{% endcapture %}
```

#### Do
```html
{% assign variable_string = 'Hello world' %}
{% capture another_string %}
  This is correct
{% endcapture %}
```

* Use snake case when naming variables
* Do not use abbreviations or shortened words
* Keep the name easy to understand

#### More examples
```html
{% assign has_shipping_tag = true %}
{% assign is_catalogue_product = false %}

{% assign additional_handle_array = 'Item 1,Item 2,Item 3' | split: ',' %}

{% assign installation_search_string = '' %}
```

* If a variable is a boolean then name it as a question, 'has the shipping tag' becomes `has_shipping_tag`
* If the variable is an array, then append `_array` to the name
* If it's a string than append `_string`
* This makes it obvious what you're dealing with when you have a lot of variables

[ꜛ Back to TOC](#table-of-contents)

## [Whitespace controls](#whitespace-controls)

### Don't
```html
{%- if variable_name -%}
  {%- assign another_variable = false -%}
{%- endif -%}
```

### Do
```html
{% if variable_name %}
  {% assign another_variable = false %}
{% endif %}
```

* Do not use [whitespace control](https://help.shopify.com/en/themes/liquid/basics/whitespace_) on tags
* If you need to trim whitespace from objects then you can use whitespace controls, e.g. `{{- section.settings.body_copy -}}`

> **🗒 Note:** Not all apps support whitespace controls.

[ꜛ Back to TOC](#table-of-contents)