# CSS Guidelines

## styelint & NPM

There is an NPM module available to import our CSS guidelines rules into stylelint. Simply use `yarn add @we-make-websites/stylelint-config` to install.

Visit [@we-make-websites/stylelint-config](https://www.npmjs.com/package/@we-make-websites/stylelint-config) or the [repo](https://github.com/we-make-websites/stylelint-config) for more information.

## Table of contents

1. [General](#general)
1. [Methods](#methods)
1. [Declarations](#declarations)
1. [Naming](#naming)
1. [Spacing](#spacing)
1. [Formatting](#formatting)
1. [Colours](#colours)
1. [Nesting](#nesting)
1. [Properties](#properties)

## General

* [DRY (Do Not Repeat Yourself)](#dry)
* [Notes on Frame](#notes-on-frame)

### [DRY (Do Not Repeat Yourself)](#dry)

#### Don't
```scss
.foo {
  background-color: #fff;
  border: 1px solid #000;
  color: #000;
}

.bar {
  background-color: #fff;
  border: 1px solid #000;
  color: #000;
}
```

#### Do
```scss
.btn {
  background-color: rgb(255, 255, 255);
  border: 1px solid rgb(0, 0, 0);
  color: rgb(0, 0, 0);
}
```

* Keep things DRY, aim to use standard classes to so you can re-use your code
* SASS includes and extends can encourage poor coding practice, just because it's one line in your code doesn't mean it is when exported
* If you're applying the same include or styles on lots of selectors, create a class and apply it in HTML to keep things DRY
* This is where defined h1, h2, p etc. in Sketch files would be really useful so we can re-use established styles

> **Exception:** Sometimes it's still better to KISS (Keep It Simple, Stupid) than DRY.

### [Notes on Frame](#notes-on-frame)

#### Frame 3
You are expected to follow the guidelines to the letter without fail, Frame 3 represents the best that We Make Websites produces so your code should be too. Frame 3 comes with a style lint which will highlight errors, using yarn format will automatically fix most of these errors for you.

#### Frame 2
When building a new project using Frame 2.0 it is expected that if you build any new sections or re-build existing templates you will follow these guidelines.

#### Frame 1 and Old Workflow
Not all of these guidelines will be possible when working with an existing site (or any site built before these guidelines). Frame 2.0 local concatenation of code allows us to use SASS features that are not available with Shopify's old version of SASS, these should be highlighted in the Notes or Exceptions are the end of the page.

## Methods

* [Responsive banner padding](#responsive-banner-padding)
* [Fullscreen elements](#fullscreen-elements)
* [Magic Numbers](#magic-numbers)

### [Responsive banner padding](#responsive-banner-padding)

#### Don't
```scss
.foo {
  padding-top: 60%;
}
```

#### Do
```scss
.foo {
  // 16:9 ratio
  padding-bottom: 56.25%
}
```

* When using padding to create a responsive banner always use padding-bottom
* Also add a preceding comment explaining the ratio of the banner

### [Fullscreen elements](#fullscreen-elements)

#### Don't
```scss
.foo {
  bottom: 0;
  left: 0;
  position: absolute;
  right: 0;
  top: 0;
}
```

#### Do
```scss
.foo {
  height: 100%;
  left: 0;
  position: absolute;
  top: 0;
  width: 100%;
}
```

* Don't define all four positioning properties to set an element to fullscreen as these do not work on button elements
* Instead use `height: 100%` and `width: 100%`

### [Magic Numbers](#magic-numbers)

#### Don't
```scss
.foo {
  left: 23px;
}
```

#### Do
```scss
.foo {
  // 10px because of font height
  left: ($GUTTER - 10px - ($NAV_HEIGHT / 2));
}
```

#### If you have to
```scss
.foo {
  // This values matches the sum of widths above it
  left: 23px;
}
```

* Avoid magic numbers, these are numbers that 'just work' but are set arbitrarily
* Try and use calculations to make the values relative to existing known numbers
* When someone else comes to edit they won't understand why it's this number specifically
* If you have to; leave a comment explaining why that number, it can be just because it works, but explain what will break if it's changed

> **Note:** Read more on [CSS tricks](https://css-tricks.com/magic-numbers-in-css/).

## Declarations

* [#IDs](#ids)
* [!important](#important)
* [Longhand properties](#longhand-properties)
* [Prefixes](#prefixes)
* [Property order](#property-order)
* [Pseudo-elements & -selectors](#pseudo-elements-selectors)
* [Variables](#variables)

### [#IDs](#ids)

#### Don't
```scss
#foo {
  #bar {
     background-color: #000;
  }
}
```

#### Do
```scss
.foo {
  .bar {
     background-color: rgb(0, 0, 0);
  }
}
```

* Don't use IDs, ever. Definitely never use them nested under each other
* They're overly specific and trump classes in the order of CSS rendering

> **Exception:** Sometimes you may have to because you're working with apps or code you can't otherwise edit. In this case, and only as a last resort, use IDs.

### [!important](#important)

#### Don't
```scss
.foo {
  color: red !important;
}
```

* Never use `!important` if it can be avoided
*  Add inline comments above the property explaining why if you have to use `!important`

> **Exception:** Sometimes you have to because of apps though. However before using !important consider doubling the specificity (see below) if you're battling code that is loaded in the page after your stylesheet but not actually inline.

#### Doubling Specificity

```scss
.foo.foo {
  // This style will override .foo styles
}
```

* This works because it's more specific than just the class name once, but won't override inline styles
* [Read this article](https://www.smashingmagazine.com/2007/07/css-specificity-things-you-should-know/) to find out more information about CSS specificity

### [Longhand properties](#longhand-properties)

#### Don't
```scss
.foo {
  background: url('/path/to/image.jpg') no-repeat 50% 50% #fff;
  font: 2rem/1.6 'Helvetica', sans-serif;
}
```

#### Do
```scss
.foo {
  background-color: #fff;
  background-image: url('/path/to/image.jpg');
  background-position: 50% 50%;
  background-repeat: no-repeat;
  font-family: 'Helvetica', sans-serif;
  font-size: 2rem;
  line-height: 1.6;
}
```

* Avoid shorthand for background and font properties
* For these properties the non-described properties automatically are set to none/default/0 which causes issues
* The code is more verbose when making responsive changes
* Do not nest properties (used to be accepted, it no longer is)

> **Exception:** Feel free to use shorthand properties for `margin`, `padding`, `border`, and `transform`.

### [Prefixes](#prefixes)

#### Frame 2 and 3
Thanks to the Frame 2 and 3 watch command you don't need to add prefixes to CSS properties as these are added automatically before upload.

#### Frame 1 and Old Workflow

If you're using the Old Workflow or Frame 1 you will need to add prefixes, you can keep track of browser support using [Can I Use](https://caniuse.com/). The most commonly needed prefixes is for Flex properties, use [Should I Prefix](http://shouldiprefix.com/) to find out what prefixes you need.

You will also need to add prefixes even if you're working on a Frame 2 or 3 project and not working on one of the bundled CSS files (like `checkout.scss.liquid`)

### [Property order](#property-order)

#### Do
```scss
.foo {
  // Extends
  @extend .grid;
  // Includes
  @include transition(0.5s);
  // Properties in alphabetical order
  // Frame 2.0 auto-prefixes so no need to include them
  // Otherwise prefixed versions are listed under the main property
  background-color: transparent;
  border: 0;
  display: flex;
  display: -ms-flexbox;
  display: -webkit-flex;
  padding: 0;

  // Pseudo-element
  &::before {
    content: '';
  }

  // Nested elements
  &__bar {
    margin: 0;
  }

  // Direct descendents
  > .baz {
    padding: 0;
  }

  // Sibling selectors
  + .bam,
  ~ .bam {
    padding-left: 0;
  }

  // Pseudo-selectors
  &:focus, &:hover {
    box-shadow: 0 0 5px color(blue, 0.3);
  }

  // Modifiers
  &#{&}--big {
    width: 100%;
  }

  // Parents
  .parent & {
    display: none;
  }
}
```

The order should be as follows, all items within each group should be sorted alphabetically:
1. Local variables (e.g. `$local_margin`)
1. Extends (e.g. `@extend %font`)
1. Includes (e.g. `@include ms-respond()`)
1. Properties (e.g. `background-color`)
1. Pseudo-elements (e.g. `&::before`, `&::placeholder` etc.)
1. Nested elements (e.g. `&__bar`)
1. Direct descendants (e.g. `> .baz`, `+ .baz`)
1. Pseudo-selectors (e.g. `&:hover`), this way they can change nested elements
1. Modifiers (e.g. `&#{&}--big`), this gives them precedence over all nested elements
1. Parents (e.g. `.parent &`), so that they have the greatest priority
1. Media queries (e.g. `@include mq($from: large) {}`)

### [Pseudo-elements & -selectors](#pseudo-elements-selectors)

#### Don't
```scss
.foo {
  &:placeholder {}

  &:after {}
}
```

#### Do
```scss
.foo {
  &::after {}

  &::placeholder {}

  &:hover {}
}
```

* Pseudo-elements should use `::` (after, before, first-letter, first-line, marker, placeholder, and selection)
* Pseudo-selectors should use `:` (hover, focus, active, nth-child etc.)
* This helps differentiate between an element and a selector

### [Variables](#variables)

When first setting up your project you should go through and define a series of variables and mixins to help with the maintenance of the project, it's a lot easier to change the property of one variable rather than hunting for all appearances of, say, a `font-family`.

> **Note:** On Frame 3 projects you can use the styleguide page template.

As the project's lead it is your responsibility to set them up to maintain conformity.

Common properties that benefit from variables are:
* `border-radius`
* `color`
* `font-family`
* `font-weight`
* `margin` (gutters, grid gutters)
* `transition` (duration, easing) – consider a mixin

## Naming

* [BEM & CSS](#bem-css)
* [BEM Modifiers](#bem-modifiers)
* [BEM naming](#bem-naming)
* [HTML naming](#html-naming)
* [Descriptive naming](#descriptive-naming)
* [Utilities](#utilities)
* [Experimental](#experimental)
* [Variable naming](#variable-naming)

### [BEM & CSS](#bem-css)

#### About

**BEM** stands for Block Element Modifier and is a element class naming convention intended to standardise the way elements are named so everyone is on the same page and can work out the relationship of elements without having to refer back to the HTML constantly. For more information go to [Get BEM](http://getbem.com/).

Below is an ideal example of how BEM would be used across HTML and CSS (technically SCSS).

#### HTML
This HTML example also includes a suggested way of targeting elements in JavaScript (`js-click`).

```html
<div class="product">
  <h1 class="product__title"></h1>
  <h2 class="product__subtitle"></h2>

  <div class="product__image-container">
    <img src="" class="product__image" alt="" js-click="zoom-image">
    <!-- Suggest adding srcset and sizes for responsive images -->
  </div>

  <div class="product__description">
    <ul class="product-icons">
      <li class="product-icons__icon"><li>
      <li class="product-icons__icon"><li>
      <li class="product-icons__icon product-icons__icon--large"><li>
    </ul>
    <!-- Note how this is its own content block as .product__description__product-icons isn't valid BEM -->
    <!-- If you find yourself thinking you need to nest blocks within blocks then instead make a new container -->
    <!-- Also note that when using modifying classes you must still include the base non-modified classes -->
  </div>
</div>

<!-- Separate page -->
<div class="product-card">
  <ul class="product-icons">
    <li class="product-icons__icon"><li>
    <li class="product-icons__icon"><li>
    <li class="product-icons__icon product-icons__icon--large"><li>
  </ul>
</div>
```

#### CSS
The below example will only work when you are using Frame 2 or higher as previous versions do not support interpolation brackets used in the class name.

```scss
// Newlines and spacing removed for brevity

.product {
  &__title {}
  &__subtitle {}
  &__image-container {}
  &__image {}
  &__description {}

  // Even without comments we can see what we're editing
  // It doesn't matter if subtitle becomes another element for example
}

.product-icons {
  &__icon {
    &#{&}--large {
      // This might be over-complicated for this scenario
      // Also we're already at three levels deep so always keep an eye on your nesting
    }
  }

  &__icon--large {
    // This might be simpler alternative than the above
  }

  .product-card & {
    // Keep all the code for .product-icons in one place by qualifying its contex
    // Code here will only apply to .product-icons when inside .product-card
  }
}
```

### [BEM Modifiers](#bem-modifiers)

#### Don't
```html
<div class="bad"></div>
<div class="bad--modifier"></div>
```
```scss
.bad {
  &--modifier {}
}
```

#### Do
```html
<div class="good good--modifier"></div>
```
```scss
.good {
  &#{&}--modifier {}
}
```

* Don't just apply the modifier class and extend the block/element class
* Apply the block/element as a class as well as the modifier class

### [Concatenating](#concatenating)

#### Don't
```scss
.foo {
  .foo__bar {
    .foo__bar--modifier {}
  }
}
```

#### Do (Frame 2+ only)
```scss
.foo {
  &__bar {
    &#{&}--modifier {}
  }
}
```

* Keeps the BEM naming and nesting of selectors more obvious
* The modifier has interpolation brackets `#{}` because && is invalid SCSS
* The modifier would render as `.foo__bar.foo__bar--modifier` in CSS
* This means it has greater specificity, without this any later changes to .foo__bar would override the modifier (for example in media queries) as `.foo__bar` and `.foo__bar--modifier` would have the same specificity so the properties appear later would trump the modifier's

> **Exception:** If you're working on a site using the Old Workflow or Frame 1.0 this can't be done as Shopify's version of SASS doesn't support concatenating or interpolation brackets.

### [BEM naming](#bem-naming)

#### Don't
```scss
.site-search {}
.site-search label {}
.site-search input {}
```

#### Do
```scss
.site-search {}
.site-search__title {}
.site-search__field {}
```

* Elements may change and no longer be a label or input, breaking the CSS, causing maintenance issues so always select with a class
* With decent BEM naming you shouldn't need to comment your CSS to say what things are or where they sit

> **Exception:** Feel free to write comments to explain why you've done things strangely if you have had to.

### [HTML naming](#html-naming)
### [Descriptive naming](#descriptive-naming)
### [Utilities](#utilities)
### [Experimental](#experimental)
### [Variable naming](#variable-naming)

## Spacing

* [Indenting](#indenting)
* [Whitespace](#whitespace)

### [Indenting](#indenting)
### [Whitespace](#whitespace)

## Formatting

* [Calculations](#calculation)
* [Capitalisation](#capitalisation)
* [Commenting](#commenting)
* [Zero values & units](#zero-values-units)
* [Parenthesise on @includes](#parenthesise-on-includes)

### [Calculations](#calculation)
### [Capitalisation](#capitalisation)
### [Commenting](#commenting)
### [Zero values & units](#zero-values-units)
### [Parenthesise on @includes](#parenthesise-on-includes)

## Colours

* [Colour properties](#colour-properties)
* [Colour variables](#colour-variables)

### [Colour properties](#colour-properties)
### [Colour variables](#colour-variables)

## Nesting

* [Nesting levels](#nesting-levels)
* [Nesting media queries](#nesting-media-queries)

### [Nesting levels](#nesting-levels)
### [Nesting media queries](#nesting-media-queries)

## Properties

* [border](#border)
* [font-family](#font-family)
* [font-size](#font-size)
* [letter-spacing](#letter-spacing)
* [line-height](#line-height)
* [margin-top](#margin-top)
* [transition](#transition)

### [border](#border)
### [font-family](#font-family)
### [font-size](#font-size)
### [letter-spacing](#letter-spacing)
### [line-height](#line-height)
### [margin-top](#margin-top)
### [transition](#transition)