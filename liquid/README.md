# Liquid Guidelines

For details on how to pass Liquid variables to Vue props, see [CANVAS's documentation](https://www.notion.so/wemakewebsites/Passing-Liquid-to-Vue-9367b24ce4ae447bbfc6471794f01b7a).

## Table of contents

* [`{% liquid %}` tag](#-liquid--tag)
* [Commenting](#commenting)
* [Conditional statements](#conditional-statements)
* [DRY (Don't Repeat Yourself)](#dry-dont-repeat-yourself)
* [Formatting](#formatting)
* [Language strings](#language-strings)
* [Naming](#naming)
* [Schema & section settings](#schema--section-settings)
* [Snippets](#snippets)
* [Split characters](#split-characters)
* [Variables](#variables)
* [Whitespace controls](#whitespace-controls)

[← Back to homepage](../README.md)

## [`{% liquid %}` tag](#-liquid--tag)

#### Don't
```html
{% assign variable_a = 'Hello world' %}
{% assign variable_b = 'Hello world' %}
{% assign variable_c = 'Hello world' %}
{% assign variable_e = 'Hello world' %}
```

#### Do
```html
{%- liquid
  assign variable_a = 'Hello world'
  assign variable_b = 'Hello world'
  assign variable_c = 'Hello world'
  assign variable_e = 'Hello world'
-%}
```

* When using multiple Liquid tags replace with a single `{% liquid %}` tag
* Use whitespace controls to remove whitespace from around the statement when rendered
* See [Shopify documentation](https://shopify.dev/api/liquid/tags/theme-tags#liquid) for more details

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
* HTML comments are included in the compiled HTML
* For `.js.liquid` or `.scss.liquid` file use the language appropriate comment format

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
* If it's a snippet then provide a list of parameters that it supports using the [JSDoc format](https://jsdoc.app/)
```html
{% comment %}
-----------------------------------------------------------------------------
  Snippet: Line item (line-item)

  @param {String} [block_class] - Block level class to apply to all elements.
  @param {String} [class] - Additional classes.
  @param {Number} [index] - Index for line item, used for animation delay.
  @param {Boolean|Object} [item] - Line item in cart response or order.
  @param {Boolean} [loading_animation] - Force the display of the loading state.
-----------------------------------------------------------------------------
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
    {% if block.settings.image != blank %}
      <img
        alt="{{ block.settings.image.alt }}"
        src="{{ block.settings.image | img_url: '2000x' }}"
      >
    {% endif %}

    {% if block.settings.title != blank %}
      <h2 class="block__title">{{ block.settings.title }}</h2>
    {% endif %}

    {% if block.settings.text != blank %}
      <div class="block__text rte">
        {{ block.settings.text }}
      </div>
    {% endif %}

    {% if block.settings.button url != blank and block.settings.button_text != blank %}
      <a class="block__button" href="{{ block.settings.button_url }}">
        {{ block.settings.button_text }}
      </a>
    {% endif %}
  </div>
{% endfor %}
```

* In areas where the client can customise the content do not assume that they will want to display every part of a section's settings
* Wrap each setting in a `{% if %}` to hide it if no content is entered
* Use `!= blank` rather than `{% if condition %}` or `!= ''` as it is more reliable
* See [setting states](./setting-states.md) for a full breakdown of what each setting returns when empty or cleared
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
  <!-- Code -->
{% elsif variable == 'Goodbye' %}
  <!-- Code -->
{% elsif variable == 'Good night' %}
  <!-- Code -->
{% else %}
  <!-- Code -->
{% endif %}
```

#### Do
```html
{%- liquid
  case variable
    when 'Hello'
      <!-- Code -->
    when 'Goodbye'
      <!-- Code -->
    when 'Good night'
      <!-- Code -->
    else
      <!-- Code -->
  endcase
-%}
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
{%- liquid
  if section.settings.width == 'full' or section.settings.display == 'full'
    assign row_class = 'row--full-width'
  endif
-%}

<div
  class="
    row
    {{ row_class }}
    {% if is_hidden %}is-hidden{% endif %}
  "
>
  <!-- Content -->
</div>
```

* Do not put long `{% if %}` tags inline, assign them separately
* If the `{% if %}` tag is short you can put it inline
* Only include the space inside the `{% if %}` tag if it's on a single line, otherwise there is no need to include a space

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
  {%- liquid
    assign slide_image = 'slide_#_image' | replace: '#', i
    assign slide_heading = 'slide_#_heading' | replace: '#', i
    assign slide_text = 'slide_#_text' | replace: '#', i
  -%}

  <div class="slide slide--{{ i }}">
    <div
      class="slide__background"
      style="background-image: url({{ block.settings[slide_image] | image_url: width: 2000 }});"
    ></div>

    <div class="slide__text">
      <h2 class="slide__heading">{{ block.settings[slide_heading] }}</h2>

      <p class="slide__copy">
        {{ block.settings[slide_text] }}
      </p>

      <div class="slide__buttons">
        {% for j in (1..2) %}
          {%- liquid
            assign slide_button_url = 'slide_#_button_%_url' | replace: '#', i | replace: '%', j
            assign slide_button_text = 'slide_#_button_%_text' | replace: '#', i | replace: '%', j
          -%}

          <a
            class="slide__button"
            href="{{ block.settings[slide_button_url] }}"
          >
            {{ block.settings[slide_button_text] }}
          </a>
        {% endfor %}
      </div>
    </div>
  </div>
{% endfor %}
```

* Use `{% for i in (1..#) %}` in combination with `{% assign %}` to replace repetitive code
* Use the iteration to set variables which are then used to call the block's settings
* Use Liquid's built in tags to avoid repeating code

[ꜛ Back to TOC](#table-of-contents)

## Formatting

* [`{% render %}`](#-render-)
* [Characters](#characters)
* [Indenting](#indenting)
* [Spacing & line character limits](#spacing--line-character-limits)

### [`{% render %}`](#-render-)

#### Don't
```html
{% include 'icon-misc', icon: 'search', colour: 'red' %}
{% render 'social-sharing' with share_title: product.title, share_permalink: product.url, share_image: product.featured_image %}
```

#### Do
```html
{% render 'icon-misc' with icon: 'search', colour: 'red' %}

{% render 'social-sharing' with
  share_image: product.featured_image,
  share_permalink: product.url,
  share_title: product.title,
%}
```

* When setting variables on your `{% render %}` always use `with` instead of starting with a comma
* After the first variable declaration you must use a comma `,`
* If there are more than two variables and it goes over 80 characters then break it into a multi-line tag
* Sort the variables alphabetically and end each line with a comma `,`

> 📋 `{% include %}` tags have been deprecated by Shopify and replaced with the `{% render %}` tag. [For full details visit this page](https://help.shopify.com/en/themes/liquid/tags/theme-tags#render).

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

{%- liquid
  assign sanitized_variable = string | downcase | split: '/' | last | remove: '<p>'
  assign sanitized_variable = sanitized_variable | remove: '</p>' | money_with_currency
-%}

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
* If the contents of an `{% if %}` condition spans more than two lines add a newline before any following `{% elsif %}` or `{% else %}` tags
* Once you are writing something over multiple lines, each line should only have one attribute or value on it
* For details on HTML spacing see the [HTML](../html/README.md) rule for [Spacing & line character limit](../html/README.md#spacing--line-character-limits)

#### Liquid tags

* Previously we recommended placing Liquid filters on newlines when there are more than two and the Liquid exceeded 80 characters
* This approach is not compatible with the [`{% liquid %}` tag](#-liquid--tag)
* We now recommended keeping filters on the same line and the `{% liquid %}` tag is our preferred approach
* However if there are a large number of filters being used on an `{% assign %}` then consider splitting the Liquid onto multiple lines like in the example above

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
* Most of our clients are multi-language so it is expected that they will be able to translate their store without requiring additional development

[ꜛ Back to TOC](#table-of-contents)

## Naming

* [Section template naming](#section-template-naming)
* [Snippet naming](#Snippet-naming)
* [Tag naming](#tag-naming)

### [Section template naming](#section-template-naming)

```html
main-[section].liquid
```

* If a section replaces the template's content it should be prefixed with `main-` and moved into the _main_ folder in _sections_ (if it isn't a dynamic component)
* E.g. `main-product.liquid` or `main-collection.liquid`

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
* In most cases `[value]` should be in lowercase to make comparisons easier
* However in instances where the value is outputted on the front-end in a specific case (e.g. Title Case) make sure this is clear in the tech spec
* If you need to store separate values in the same tag then separate them using `_`  such as `type_modal: [Model]_[Year]` (this isn't snake_case)
* Do not use boolean values (e.g. `has_addon: true`) as simply having the tag in the first place is enough to know that it is `true` (e.g. `has_addon`)
* If the tag is to be used in a search string (and therefore needs to be handlelised) then use the naming convention `tag_name--[value]`, this allows the value to be kebab-cased (e.g. `build_date--2019-07-03`)
* Never put formatted money into the tag (e.g. `monthly_cost: £10`) as tags do not support all the formatting standards required by international stores (e.g. `monthly_cost: €12,34` will not be allowed because the comma ends the tag), instead use `monthly_cost: 1234` and format the cost using Liquid or JavaScript

[ꜛ Back to TOC](#table-of-contents)

## Schema & section settings

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
  "info": "Enter a unique store ID using a snake case naming convention. e.g: gb_store",
  "default": "dev_store"
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
  "info": "Enter a unique store ID using a snake case naming convention. e.g: gb_store",
  "default": "dev_store"
}
```

* Use the prescribed order:
  * `type`
  * `id`
  * `label`
  * `info`
  * `accept` (video_url)
  * `limit` (collection_list, product_list)
  * `min` (range)
  * `max` (range)
  * `step` (range)
  * `unit` (range)
  * `options` (radio, select)
  * `placeholder` (number, text, textarea, html, video_url)
  * `default`
* Use snake_case for `id`
* Use sentence case for `label` and `info`

### [`type`](#type)

Specific rules for certain settings of `type`:
* `range` – Use for fixed number choices (such as font size, height, duration etc.)
* `richtext` – If you are changing `text` or `textarea` to `richtext` (or vice versa) you will need to delete any existing settings data as it will throw an error
* `select` – Use for fixed text choices (not for numbers)
* `textarea` – Do not use as it doesn't support dynamic content sources
* `url` – Only use for all choices, use one of the limited selections if you are only expecting a certain output (`collection`, `product`, `blog`, `page`, and `article`), do not use for video, use `video_url` for this

[ꜛ Back to TOC](#table-of-contents)

## [Snippets](#snippets-general)

* Use Liquid snippets to keep files small and manageable
* In the same way a block gets its own SCSS and JS file, consider splitting it into its own snippet
* Snippets are especially useful for repeating content

[ꜛ Back to TOC](#table-of-contents)

## [Split characters](#split-characters)

Split characters are used to effectively provide multiple description fields on the product page. It is useful when you need to output long form content in multiple locations.

> 📋 Using Shopify native metafields is now the preferred approach.

### Example
```html
{%- liquid
  if product.description != ''
    assign full_description = product.description | remove: '<meta charset="utf-8">' | remove: '<meta charset="utf-8" />'
    assign full_description = full_description | remove: '<span>' | remove: '</span>'
    assign full_description = full_description | remove: '<p>&nbsp;</p>' | remove: '<p> </p>' | remove: '<p></p>'

    if full_description contains '---DESCRIPTION---'
      assign description = full_description | split: '<p>---DESCRIPTION---</p>' | last | split: '<p>---' | first
    endif

    if full_description contains '---SIZE GUIDE---'
      assign size_guide = full_description | split: '<p>---SIZE GUIDE---</p>' | last | split: '<p>---' | first
    endif
  endif
-%}

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
* Shopify tends to add extra HTML so in the example this has been stripped off, including empty line returns

[ꜛ Back to TOC](#table-of-contents)

## Variables

* [Assigning](#variable-assigning)
* [Grouping](#variable-grouping)
* [Naming](#variable-naming)

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
{%- liquid
  for product in collection.products
    assign has_shipping_tag = false

    if product.tags contains 'has_shipping'
      assign has_shipping_tag = true
    endif
  endif
-%}
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
  {% assign variable_d = variant.title %}
  <h2>{{ variable_d }}</h2>
{% endfor %}

{% assign variable_e = 'Hello world' %}
{% for product in recommendations.products %}
  <span>{{ product.title | link_to: product.url }}</span>
{% endfor %}
```

#### Do
```html
{%- liquid
  assign variable_a = 'Hello world'
  assign variable_b = 'Hello world'
  assign variable_c = 'Hello world'
  assign variable_e = 'Hello world'
-%}

<h1>{{ product.title }}</h1>
<div>{{ product.description }}</div>

{% for variant in product.variants %}
  {% assign variable_d = variant.title %}
  <h2>{{ variable_d }}</h2>
{% endfor %}

{% for product in recommendations.products %}
  <span>{{ product.title | link_to: product.url }}</span>
{% endfor %}
```

* Group all variables which are not set inside a specific `{% for %}` tag at the top of the file

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
{%- liquid
  assign has_shipping_tag = true
  assign is_catalogue_product = false

  assign additional_handle_array = 'Item 1,Item 2,Item 3' | split: ','

  assign installation_search_string = ''
-%}
```

* If a variable is a boolean then name it as a question, 'has the shipping tag' becomes `has_shipping_tag`
* If the variable is an array, then append `_array` to the name
* If it's a string than append `_string`
* This makes it obvious what you're dealing with when you have a lot of variables

[ꜛ Back to TOC](#table-of-contents)

## [Whitespace controls](#whitespace-controls)

* Only use whitespace controls when using a `{% liquid %}` tag
* If you need to trim whitespace from objects then you can use whitespace controls, e.g. `{{- section.settings.body_copy -}}`

> 📋 Not all apps support whitespace controls.

[ꜛ Back to TOC](#table-of-contents)
