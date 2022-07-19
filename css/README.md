# CSS/SCSS Guidelines

## Table of contents

* [General](#general)
* [Methods](#methods)
* [Declarations](#declarations)
* [Naming](#naming)
* [Spacing](#spacing)
* [Formatting](#formatting)
* [Colours](#colours)
* [Nesting](#nesting)
* [Properties](#properties)

[← Back to homepage](../README.md)

## General

* [DRY (Don't Repeat Yourself)](#dry-dont-repeat-yourself)
* [Overriding stylelint](#overriding-stylelint)

### DRY (Don't Repeat Yourself)

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
* Make sure you inspect elements in Figma to find their text styles to avoid repeating them

#### Exceptions

* Sometimes it's still better to KISS (Keep It Simple, Stupid) than DRY.

### Overriding stylelint

#### Don't

```scss
// stylelint-disable-next-line
#app {
  // stylelint-disable-next-line
  background-color: var(--color-background-dark) !important;
  margin-block: 0;
  margin-inline: 0;
  // stylelint-disable-next-line
  position: absolute !important;
}
```

#### Do

```scss
// stylelint-disable declaration-no-important
// stylelint-disable selector-max-id

#app {
  background-color: var(--color-background-dark) !important;
  margin-block: 0;
  margin-inline: 0;
  position: absolute !important;
}
```

* If you must override stylelint then add a document rule at the beginning of file instead of lots of comments
* Add the rule comments at the top of the file just after the intro comment with a newline between the rule comments and the first declaration block
* Add the rule comments in alphabetical order

[ꜛ Back to TOC](#table-of-contents)

## Methods

* [Mobile first](#mobile-first)
* [Responsive banner padding](#responsive-banner-padding)
* [Fullscreen elements](#fullscreen-elements)
* [Magic numbers](#magic-numbers)

### Mobile first

#### Don't

```scss
.foo {
  // Desktop code

  @include mq($until: l) {
    // Mobile code
  }
}
```

### Do
```scss
.foo {
  // Mobile code

  @include mq($from: l) {
    // Desktop code
  }
}
```

* We should always develop mobile first
* This means we're expanding to fill desktop instead of cramming things in to fit mobile
* If we fail to add desktop styles then the site will still be usable

### Responsive banner padding

#### Don't

```scss
.foo {
  padding-block-start: 60%;
}
```

#### Do

```scss
.foo {
  // 16:9 ratio
  padding-block-end: 56.25%
}
```

* When using padding to create a responsive banner always use `padding-block-end`
* Also add a preceding comment explaining the ratio of the banner
* `aspect-ratio` is not yet supported in the latest two whole versions of Safari

### Fullscreen elements

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

* Don't define all four positioning properties to set an element to fullscreen as these do not work on all elements
* Instead use `height: 100%` and `width: 100%`

### Magic numbers

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
  left: calc(var(--gutter) - 10px - (var(--nav-height) / 2));
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
* If you have to use magic numbers then leave a comment explaining why that number, it can be just because it works, but explain what will break if it's changed

> Read more about [magic numbers](https://css-tricks.com/magic-numbers-in-css/).

[ꜛ Back to TOC](#table-of-contents)

## Declarations

* [#IDs](#ids)
* [!important](#important)
* [Longhand properties](#longhand-properties)
* [Prefixes](#prefixes)
* [Property order](#property-order)
* [Pseudo-elements & -selectors](#pseudo-elements-&--selectors)
* [Variables](#variables)

### #IDs

#### Don't

```scss
#foo {
  background-color: #000;
}
```

#### Do

```scss
.foo {
  background-color: rgb(0, 0, 0);
}
```

* Don't use IDs, ever, definitely never use them nested under each other
* They're overly specific and trump classes when it comes to CSS specificity

#### Exceptions

* Sometimes you may have to because you're working with apps or code you can't otherwise edit
* In this case, and only as a last resort, use IDs
* Add a `// stylelint-disable selector-max-id` comment at the start of the SCSS file immediately beneath the intro comment to disable linting for it

### `!important`

#### Don't

```scss
.foo {
  color: red !important;
}
```

* Never use `!important` if it can be avoided
*  Add inline comments above the property explaining why if you have to use `!important`

#### Exceptions

* Sometimes you have to because of apps though
* However before using `!important` consider doubling the specificity if you're battling code that is loaded in the page after your stylesheet but not actually inline (see below)
* Add a `// stylelint-disable declaration-no-important` comment at the start of the SCSS file immediately beneath the intro comment to disable linting for it

#### Doubling Specificity

```scss
.foo.foo {
  // This style will override .foo styles
}
```

* This works because it's more specific than just the class name once, but won't override inline styles

> Read more about [CSS specificity](https://www.smashingmagazine.com/2007/07/css-specificity-things-you-should-know/).

### Longhand properties

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

* Avoid shorthand for `background` and `font` properties
* For these properties the non-described properties automatically are set to `none`/`default`/`0` which causes issues
* Don't use `margin` and `padding` shorthand properties, instead using `margin-block`, `margin-inline`, `padding-block`, and `padding-inline`

#### Exceptions

* Feel free to use shorthand properties for `border` and `transform`

### Prefixes

* Prefixes are automatically added to to CSS properties
* This also means that unused prefixes are automatically removed, if you definitely need the prefix then use the following:

```scss
/* autoprefixer: ignore next */
-webkit-prefixed-property: value;
```

### Property order

#### Do

```scss
.foo {
  // Extends
  @extend .grid;
  // Includes
  @include transition(opacity);
  // Properties in alphabetical order
  background-color: transparent;
  border: 0;
  display: flex;

  // Pseudo-element
  &::before {
    content: '';
  }

  // Nested elements
  &__bar {
    color: var(--color-text-light);
  }

  // Direct descendents
  > .baz {
    color: var(--color-text-secondary);
  }

  // Sibling selectors
  + .bam,
  ~ .bam {
    padding-inline-start: 0;
  }

  // Pseudo-selectors
  &:focus,
  &:hover {
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
1. Local variables (e.g. `$local-margin`)
1. Extends (e.g. `@extend %font`)
1. Includes (e.g. `@include ms-respond()`)
1. Properties (e.g. `background-color`)
1. Pseudo-elements (e.g. `&::before`, `&::placeholder` etc.)
1. Nested elements (e.g. `&__bar`)
1. Direct descendants (e.g. `> .baz`, `+ .baz`)
1. Pseudo-selectors (e.g. `&:hover`), this way they can change nested elements
1. Modifiers (e.g. `&#{&}--big`), this gives them precedence over all nested elements
1. Parents (e.g. `.parent &`), so that they have the greatest priority
1. Media queries (e.g. `@include mq($from: large)`)

### Pseudo-elements & -selectors

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

### Variables

**If you're using Canvas 3.0.0 or newer then use the `design` command in conjunction with the _tokens.json_ file that the designer provided to automatically create your project's variable and classes.**

When first setting up your project you should go through and define a series of variables and mixins to help with the maintenance of the project, it's a lot easier to change the property of one variable rather than hunting for all appearances of, say, a `font-family`.

It is the project lead developer's responsibility to set them up to maintain conformity.

[ꜛ Back to TOC](#table-of-contents)

## Naming

* [BEM & CSS](#bem-&-css)
* [BEM modifiers](#bem-modifiers)
* [BEM naming](#bem-naming)
* [Descriptive naming](#descriptive-naming)
* [Variable naming](#variable-naming)

### BEM & CSS

#### About

**BEM** stands for Block Element Modifier and is a element class naming convention intended to standardise the way elements are named so everyone is on the same page and can work out the relationship of elements without having to refer back to the HTML constantly.

> For more information go to [Get BEM](http://getbem.com/).

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
```

```html
<div class="product-card">
  <ul class="product-icons">
    <li class="product-icons__icon"><li>
    <li class="product-icons__icon"><li>
    <li class="product-icons__icon product-icons__icon--large"><li>
  </ul>
</div>
```

#### CSS

```scss
// product.scss
.product {
  &__title {}
  &__subtitle {}
  &__image-container {}
  &__image {}
  &__description {}

  // Newlines and spacing removed for brevity
  // Even without comments we can see what we're editing
  // It doesn't matter if subtitle becomes another element for example
}
```
```scss
// product-icons.scss
.product-icons {
  &__icon {
    &#{&}--large {
      // We're already at three levels deep so always keep an eye on your nesting
    }
  }

  .product-card & {
    // Keep all the code for .product-icons in one place by qualifying its context
    // Code here will only apply to .product-icons when inside .product-card
  }
}
```

### BEM modifiers

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
* Using interpolated brackets means the CSS will compile as `.good.good--modifier {}`

### Concatenating

#### Don't

```scss
.foo {
  .foo__bar {
    .foo__bar--modifier {}
  }
}
```

#### Do

```scss
.foo {
  &__bar {
    &#{&}--modifier {}
  }
}
```

* Keeps the BEM naming and nesting of selectors more obvious
* The modifier has interpolation brackets `#{}` because `&&` is invalid SCSS
* The modifier will compile as `.foo__bar.foo__bar--modifier` in CSS
* This means it has greater specificity, without this any later changes to `.foo__bar` would override the modifier (for example in media queries) as `.foo__bar` and `.foo__bar--modifier` would have the same specificity so the properties applied later in the file would trump the modifier's

> Read more about [CSS specificity](https://www.smashingmagazine.com/2007/07/css-specificity-things-you-should-know/).

### BEM naming

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

* Always use classes to select elements
* Elements may change and no longer be a `<label>` or `<input>`, breaking the CSS, causing maintenance issues
* With decent BEM naming you shouldn't need to comment your CSS to say what things are or where they sit

#### Exceptions

* Feel free to write comments to explain why you've done things strangely if you have had to

### Descriptive naming

#### Don't

```scss
.curley-font {
  font-family: 'Lobster', serif;
}

.red {
  color: var(--color-red);
}
```

#### Do

```scss
.blog__subtitle {
  font-family: 'Lobster', serif;
}

.text-highlight {
  color: var(--color-red);
}
```

* CSS selectors should describe hierarchy, not the look as the look can change

### Variable naming

#### Canvas 3.0.0 and newer
* Use kebab-case for CSS and SASS variables
* Global CSS variables should be set in the `:root {}` declaration
* Global SASS variables should be set in a SCSS file in the _styles/config/_ folder
* Local CSS and SASS variables are only available in the declaration they are defined in
* Prefer CSS variables, only use SASS variables where SASS functions would break with CSS variables

#### Canvas 2.3.0 or older

* For global variables use `$SCREAMING_SNAKE_CASE`
* For local variables use `$snake_case`
* Local variables are only available in the declaration they are defined in

[ꜛ Back to TOC](#table-of-contents)

## Spacing

* [Indenting](#indenting)
* [Whitespace](#whitespace)

### Indenting

#### Don't

```scss
.foo {
    color: red;

    &_bar {
        // No good
    }
}
```

#### Do

```scss
.foo {
  color: red;

  &_bar {
    // Better
  }
}
```

* Indent with 2 spaces, not tabs or 4 spaces
* Shopify uses 2 spaces
* Using spaces is easier to copy and paste

### Whitespace

#### Don't

```scss
.foo{box-shadow: 0 0 rgba(0,0,0,0.5);color:red;font:{size:1em;weight:700;}}

.foo,.spanner,.foobar{
  color:red;
  .baz{color:blue}}

.foo>.bar {color:red;}
```

#### Do

```scss
.foo {
  box-shadow: 0 0 rgba(0, 0, 0, 0.5);
  color: rgb(255, 0, 0);
  font-size: 1rem;
  font-weight: 700;
}

.foo, .foobar,
.spanner {
  color: rgb(0, 255, 0);

  .baz {
    color: rgb(0, 0, 255);
  }
}

.foo > .bar {
  color: rgb(255, 0, 0);
}
```

* Add whitespace after commas (including property values), after selector name, after property colon and before and after child selector
* Each element selector block should have an empty line above it
* Each property on a new line and a new line for the closing }
* Keep related selectors on the same line, separate selectors go on a new line
* Add spaces between child selectors
* Put a semi-colon after a property, even if it's the last one
* Put a newline after the last selector

#### Why
* It's easier to read
* Whitespace is free
* Code is minified anyway

[ꜛ Back to TOC](#table-of-contents)

## Formatting

* [Calculations](#calculation)
* [Capitalisation](#capitalisation)
* [Commenting (inline)](#commenting-inline)
* [Commenting (introductory)](#commenting-introductory)
* [Negative CSS variables](#negative-css-variables)
* [Parenthesise on @includes](#parenthesise-on-includes)
* [Zero values & units](#zero-values-&-units)

### Calculations

#### Don't

```scss
.foo {
  left: calc(100% - 30px * 2);
  width: 100% / 3;
}
```

#### Do

```scss
.foo {
  left: calc(100% - (var(--gutter) * 2));
  width: (100% / 3);
}
```

* Wrap calculations in brackets for readability
* Put spaces between the values and the mathematical operators
* Put multiplying and dividing values after the starting value
* Use variables in calculations to keep them relative

#### Also
```scss
.foo {
  --local-margin: (2 * var(--global-margin));
  margin-block-end: var(--local-margin);
  width: calc(100% + var(--local-margin));
}
```

* Use local variables to define adjusted global variables

### Capitalisation

#### Don't

```scss
.Foo {
  Color: var(--Color-Text-Primary);
}
```

#### Do

```scss
.foo {
  color: var(--color-text-primary);
}
```

* Use lowercase for selectors and properties
* Refer to [Variable naming](#variable-naming) for global and local variables

### Commenting (inline)

#### Don't

```scss
.foo {
  background-color: red; // Don't
  /* padding-block-start: 30px;
  width: 100% */
}
```

#### Do

```scss
.foo {
  // Comment above the line
  background-color: red;
  // padding-block-start: 30px;
  // width: 100%;
}
```

* Use `//` for commenting not the block level `/* */`
* It's easier to un-comment
* Inline comments should start on a new line preceding the property they're describing
* Use sentence case for your comments
* Do not end with a full stop

### Commenting (introductory)

```css
/**
 * Component: Footer Social
 * -----------------------------------------------------------------------------
 * Social icons and links in footer.
 *
 */
```

* Include an introductory comment at the start of each file
* Describe what folder it's in, the file's name, and list any special features or conditions
* All lines except the first one should have a full stop

### Negative variables

#### Don't

```scss
$margin: var(--spacing-m);

.foo {
  margin-block-start: -$margin;
}
```

#### Do

```scss
.foo {
  margin-block-start: calc(var(--spacing-m) * -1);
}
```

* To use negative values for CSS variables you have to times them by `-1` in a `calc()` function

### Parenthesise on @includes

#### Where

```scss
@mixin animation($property: color) {
  // Code
}

@mixin visually-hidden() {
  // Code
}
```

#### Don't

```scss
.foo {
  @include visually-hidden();
}
```

#### Do

```scss
.foo {
  @include animation(background-color);
  @include visually-hidden;
}
```

* Do not include parenthesise on argument-less mixins

### Zero values & units

#### Don't

```scss
.foo {
  animation-delay: 0;
  margin-block: 0px;
  opacity: .4567;
}
```

#### Do

```scss
.foo {
  animation-delay: 0s;
  margin-block: 0;
  opacity: 0.4;
}
```

* Do specify units on zero duration times
* Don't specify units on zero length values
* Do add a leading zero for decimal places
* Don't go to more than three decimal places, the fewer the better

[ꜛ Back to TOC](#table-of-contents)

## Colours

* [Colour properties](#colour-properties)
* [Colour variables](#colour-variables)

### Colour properties

#### Don't

```scss
.foo {
  color: red;
  // Or
  color: #FF0000;
  // Or
  color: hsl(0, 100%, 50%);
}
```

#### Do

```scss
.foo {
  color: rgb(255, 0, 0);
  // Or
  color: rgba(255, 0, 0, 0.5);
}
```

* Use rgb (or rgba) colour values as they are more human readable
* These values are often supplied in brand guidelines

#### Exceptions

* If required, use lowercase and shorthand (where possible) hex colour values

### Colour variables

#### Don't

```scss
$colour-blue-other: #6195ed;
$colour-dark-grey: #E2E3E4;
$LIGHT_GREY: #d4d7d9;
```

#### Do

```scss
--color-primary: rgb(97, 149, 237);
--color-text-primary: rgb(226, 227, 228);
--color-border-light: rgb(212, 215, 217);
```

* All colours should have variables defined for them, this will come from the designs
* Designers will determine the name of colours
* Colours will generally be named for what they are used for, e.g. `color-text-primary`
* If you're using Canvas 3.0.0 or newer than use the `design` command to generate variables and classes

[ꜛ Back to TOC](#table-of-contents)

## Nesting

* [Nesting levels](#nesting-levels)
* [Nesting media queries](#nesting-media-queries)

### Nesting levels

#### Don't

```scss
.foo {
  .this {
    .is {
      .very {
        .bad {

        }
      }
    }
  }

  @include mq($from: l) {
    .this .is .very .bad {

    }
  }
}
```

#### Do

```scss
.foo {
  // Base level (not included)

  &__bar {
    // Level one

    &:hover {
      // Level two

      &.bar {
        // Level three
      }
    }
  }

  @include mq($from: l) {
    // Base level (not included)

    &__bar {
      // Level one

      &:hover {
        // Level two

        &.bar {
          // Level three
        }
      }
    }
  }
}
```

* Don't nest more than 3 levels
* As styles would require an even more specific selector to override them
* This would quickly add up to a significant maintenance issue
* This does mean that you can't always nest properties the way you normally would
* Media queries are not included when counting levels of nesting
* VS Code can show you how many levels deep you are in its Breadcrumbs feature (View > Toggle Breadcrumbs)

### Nesting media queries

#### Don't

```scss
.foo {

  &__bar {
    // Code

    @include mq($from: m) {
      // Code
    }
  }

  &__section {
    // Code

    @include mq($from: m) {
      // Code
    }
  }
}
```

#### Do

```scss
.foo {

  &__bar {
    // Code
  }

  &__section {
    // Code
  }

  /**
   * Media queries.
   */
  @include mq($from: m) {
    &__bar {
      // Code
    }

    &__section {
      // Code
    }
  }
}
```

* Keep media queries at the root of the declaration, not nested inside each selector
* This way it's easier to find media queries

[ꜛ Back to TOC](#table-of-contents)

## Properties

* [`border`](#border)
* [`font-family`](#font-family)
* [`font-size`](#font-size)
* [`line-height`](#line-height)
* [`margin` & `padding`](#margin--padding)
* [`transition`](#transition)

### `border`

#### Don't

```scss
.foo {
  border: none;
}

.bar {
  border: 2px solid var(--border-light);

  &:hover {
    border: 2px solid var(--border-dark);
  }
}
```

#### Do

```scss
.foo {
  border: 0;
}

.bar {
  border: 2px solid var(--border-light);

  &:hover {
    border-color: var(--border-dark);
  }
}
```

* Use `0` instead of none for borders
* When a border is set to `0` it will never display, however if a border is set to `none` but later overridden by a `border-style` it will display
* If you're changing a single part of the property then target that specific property rather than setting all the properties again, this is much easier to maintain

### `font-family`

#### Don't

```scss
.foo {
  font-family: 'Avenir';
}
```

#### Do

```scss
:root {
  --font-family-body: 'Avenir', Helvetica, Arial, sans-serif;
}

.foo {
  font-family: var(--font-family-body);
}
```

* Don't just declare the custom font
* Always provide a font stack which includes web-safe fonts
* This prevents loading blank pages until the font loads
* The `font-family` should be declared in a global variable

### `font-size`

#### Don't

```scss
.foo {
  font-size: 12px;
  // Or
  font-size: 1em;
}
```

#### Do

```scss
:root {
  --font-size-m: 1rem;
}

.foo {
  // Canvas 3.0.0 or newer
  font-size: var(--font-size-m);
  // Canvas 2.3.0 or older
  @include ms(0);
}
```

* Use rem, relative ems
* They're easier to understand as they're always relative to the base font-size
* `em` units change based on the current elements `font-size`
* `px` units are not as responsive to device or zoom level
* Canvas 3.0.0 or newer defines font sizes as global variables, use them
* Canvas 2.3.0 or older comes with the font function `ms()`, use this

### `line-height`

#### Don't

```scss
.foo {
  line-height: 18.5px;
}

.bar {
  line-height: 2.35rem;
}
```

#### Do

```scss
.foo {
  line-height: 160%;
}
```

* Use `%` units for relative `line-height`
* This means `line-height` will scale with the font size rather than being fixed
* Another consideration at the design phase is to keep `line-height` relative to the `font-size` in rational numbers

### `margin` & `padding`

#### Don't

```scss
.foo {
  margin-top: var(--spacing-m);
  margin-right: var(--spacing-m);
  padding-bottom: var(--spacing-m);
  padding-left: var(--spacing-m);
}
```

#### Do

```scss
.foo {
  margin-block-start: var(--spacing-m);
  margin-inline-end: var(--spacing-m);
  padding-block-end: var(--spacing-m);
  padding-inline-start: var(--spacing-m);
}
```

* Use `margin-block-*`, `margin-inline-*`, `padding-block-*`, and `padding-inline-*` properties
* These reflect the current `writing-mode`, `direction`, and `text-orientation` of the page meaning they better support multi-language, especially right-to-left languages
* Be wary of using `margin-block-start` as vertical margins collapse
* Avoid using `margin` or `padding` as they're not the shorthand properties of these new selectors

> Read more about [`margin-block`](https://developer.mozilla.org/en-US/docs/Web/CSS/margin-block), [`margin-inline`](https://developer.mozilla.org/en-US/docs/Web/CSS/margin-inline), [`padding-block`](https://developer.mozilla.org/en-US/docs/Web/CSS/padding-block), and [`padding-inline`](https://developer.mozilla.org/en-US/docs/Web/CSS/padding-inline).

### `transition`

#### Don't

```scss
.foo {
  transition: ease transform 0.4s 0.2s;
}
```

#### Do

```scss
.foo {
  @include transition(transform)
}
```

* Use the `transition` mixin
* See the `get-transition-properties` function for default values used for timing and ease
* You can customise on a per mixin basis:

```scss
.foo {
  @include transition(transform var(--timing-slow) var(--easing-normal))
}
```

* Do not transition `margin` or positional properties (`top`, `left`, `right`, `bottom`) as these don't perform well and lead to poor frame rates
* Instead use `padding` or `transform` as they can be hardware-accelerated

[ꜛ Back to TOC](#table-of-contents)
