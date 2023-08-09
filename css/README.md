# CSS/SCSS Guidelines

## Table of contents

* [General](#general)
* [Methods](#methods)
* [Declarations](#declarations)
* [Naming](#naming)
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
  background-color: var(--color-neutral-3);
  border: 1px solid var(--color-neutral-1);
  color: var(--color-neutral-2);
}

.bar {
  background-color: var(--color-neutral-3);
  border: 1px solid var(--color-neutral-1);
  color: var(--color-neutral-2);
}
```

#### Do

```scss
.btn {
  background-color: var(--color-neutral-3);
  border: 1px solid var(--color-neutral-1);
  color: var(--color-neutral-2);
}
```

* Keep things DRY, aim to use standard classes so you can re-use your code
* SCSS `@include` and `@extend` can encourage poor coding practice, just because it's one line in your code doesn't mean it is when compiled
* If you're applying the same include or styles on lots of selectors, create a class and apply it in HTML to keep things DRY
* Make sure you inspect elements in Figma to find their corresponding classes and CSS variables to avoid repeating them

#### Exceptions

* Sometimes it's still better to KISS (Keep It Simple, Stupid) than DRY

### Overriding stylelint

#### Don't

```scss
.foo {
  // stylelint-disable-next-line
  background-color: var(--color-neutral-1) !important;
  margin: 0;
  // stylelint-disable-next-line
  position: absolute !important;
}

// stylelint-disable-next-line
.barClass {
  color: var(--color-neutral-6)
}
```

#### Do

```scss
// stylelint-disable declaration-no-important

.foo {
  background-color: var(--color-neutral-1) !important;
  margin-block: 0;
  margin-inline: 0;
  position: absolute !important;
}

// Custom class from app
// stylelint-disable-next-line selector-class-pattern
.barClass {
  color: var(--color-neutral-6)
}
```

* Sometimes it's necessary to override stylelint, usually due to app code
* When this happens do not use `stylelint-disable` to disable all linting in the whole file
* Instead use `stylelint-disable-next-line [rule]` to disable just that rule on the next line, then add a preceding comment explaining why it's necessary
* If you find yourself disabling the same rule frequently in a file then add a comment at the top of the file disabling _just_ that rule using `stylelint-disable [rule]
* Add the rule comment at the top of the file just after the intro comment with a newline between the rule comment and the first declaration block
* Add the rule comments in alphabetical order
* Stylelint will tell you which rule you're breaking when hovering over the error
* Note that the stylelint rules listed under [deprecated](https://stylelint.io/user-guide/rules/#deprecated) are supported by prefixing the rule with `stylistic/`, e.g. `stylistic/color-hex-case` instead of just `color-hex-case` as these rules are set to be removed in stylelint 16

[ꜛ Back to TOC](#table-of-contents)

## Methods

* [Fullscreen elements](#fullscreen-elements)
* [Magic numbers](#magic-numbers)
* [Mobile first](#mobile-first)
* [Responsive banner padding](#responsive-banner-padding)
* [RTL](#rtl)

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
  left: calc(var(--layout-gutter) - 10px - (var(--nav-height) / 2));
}
```

#### Or if you have to
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
  padding-block-end: 56.25%;
}
```

#### Or

```scss
@use 'sass:math';

.foo {
  padding-block-end: percentage(math.div(9, 16));
}
```

* When using padding to create a responsive banner always use `padding-block-end`
* Also add a preceding comment explaining the ratio of the banner
* `aspect-ratio` is still not supported in all the browsers that we [support](https://wemakewebsites.com/msa/supported)

### RTL

* Use properties that automatically updated based on writing direction
* See [`margin` & `padding`](#margin--padding) for details on `margin` and `padding`
* [`inset`](#inset) does not have the necessary support yet
* Continue using `top`, `right`, `bottom`, and `left` properties for now
* Use the RTL selector to switch positions in right-to-left writing directions

```scss
.foo {
  left: var(--spacing-m);

  [dir=rtl] & {
    left: unset;
    right: var(--spacing-m);
  }
}
```

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
  background-color: var(--color-neutral-1);
}
```

#### Do

```scss
.foo {
  background-color: var(--color-neutral-1);
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
  color: var(--color-brand-1-default) !important;
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
  background: url('/path/to/image.jpg') no-repeat 50% 50% var(--color-neutral-6);
  font: var(--font-size-m)/var(--line-height-0) var(--font-family-1);
}
```

#### Do

```scss
.foo {
  background-color: var(--color-neutral-6);
  background-image: url('/path/to/image.jpg');
  background-position: 50% 50%;
  background-repeat: no-repeat;
  font-family: var(--font-family-1);
  font-size: var(--font-size-m);
  line-height: var(--line-height-0);
}
```

* Avoid the shorthand `background` and `font` properties
* For these properties the non-described properties automatically are set to `none`/`default`/`0` which causes issues
* It's also recommended that you don't use [`transform`](#transform)

#### Exceptions

* You can use `margin` or `padding` if all the values are the same, or if you're setting the same value for both vertical properties, and both horizontal properties
* Feel free to use shorthand properties for `border` and `transform`

```scss
.foo {
  // This is okay
  margin: var(--spacing-m) var(--spacing-l);
  // This isn't okay as left and right have different values
  margin: var(--spacing-m) var(--spacing-l) var(--spacing-m) var(--spacing-xl);
  // Instead of the above use the following, can't use margin-block as Safari doesn't support it fully
  margin-block-end: var(--spacing-m);
  margin-block-start: var(--spacing-m);
  margin-inline-end: var(--spacing-l);
  margin-inline-start: var(--spacing-xl);
}
```

### Prefixes

* Prefixes are automatically added to to CSS properties
* This also means that unused prefixes are automatically removed, if you definitely need the prefix then use the following:

```scss
/* autoprefixer: ignore next */
-webkit-prefixed-property: value;
```

### Property order

```scss
.foo {
  // Includes
  @include transition(opacity);
  // Extends
  @extend .grid;
  // SASS variables
  $variable: var(--spacing-m);
  // CSS variables
  --variable: var(--spacing-m);
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
    color: var(--color-neutral-4);
  }

  // Direct descendents
  > .baz {
    color: var(--color-neutral-3);
  }

  // Sibling selectors
  + .bam,
  ~ .bam {
    padding-inline-start: 0;
  }

  // Pseudo-selectors
  &:focus,
  &:hover {
    border: 1px solid var(--color-brand-1-default)
  }

  // Modifiers
  &#{&}--big {
    width: 100%;
  }

  // Parents
  .parent & {
    display: none;
  }

  // @media queries
  @media print {}

  // @supports queries
  @supports (display: flex) {}

  // mq media queries
  @include mq($from: l) {}
}
```

The order should be as follows, all items within each group should be sorted alphabetically:
1. Includes (e.g. `@include button-reset()`)
1. Extends (e.g. `@extend %font`)
1. SASS variables (e.g. `$local-margin`)
1. CSS variables (e.g. `--local-padding`)
1. Properties (e.g. `background-color`)
1. Pseudo-elements (e.g. `&::before`, `&::placeholder` etc.)
1. Nested elements (e.g. `&__bar`)
1. Direct descendants (e.g. `> .baz`, `+ .baz`)
1. Pseudo-selectors (e.g. `&:hover`), this way they can change nested elements
1. Modifiers (e.g. `&#{&}--big`), this gives them precedence over all nested elements
1. Parents (e.g. `.parent &`), so that they have the greatest priority
1. Media queries (e.g. `@media print`)
1. Support queries (e.g. `@supports (display: flex)`)
1. MQ media queries (e.g. `@include mq($from: l)`)

### Pseudo-elements & -selectors

#### Don't

```scss
.foo {
  &:after {}

  &:placeholder {}
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

**If you're using Canvas 3.0.0 or newer then use the `design` command in conjunction with the _tokens.json_ file that the designer provided to automatically create your project's variable and classes. [See documentation](https://we-make-websites.gitbook.io/canvas/features/styles/design-tokens)**

When first setting up your project you should go through and define a series of variables and mixins to help with the maintenance of the project, it's a lot easier to change the property of one variable rather than hunting for all appearances of, say, a `font-family`.

It is the project lead developer's responsibility to set them up to maintain conformity.

[ꜛ Back to TOC](#table-of-contents)

## Naming

* [BEM & CSS](#bem-&-css)
* [BEM modifiers](#bem-modifiers)
* [BEM naming](#bem-naming)
* [Descriptive naming](#descriptive-naming)
* [Naming conventions](#naming-conventions)
* [Variable naming](#variable-naming)
* [Naming conventions](#naming-conventions)

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
    &-bez {}
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

  &__bar-bez {}
}
```

* Keeps the BEM naming and nesting of selectors more obvious
* The modifier has interpolation brackets `#{}` because `&&` is invalid SCSS
* The modifier will compile as `.foo__bar.foo__bar--modifier` in CSS
* This means it has greater specificity, without this any later changes to `.foo__bar` would override the modifier (for example in media queries) as `.foo__bar` and `.foo__bar--modifier` would have the same specificity so the properties applied later in the file would trump the modifier's

#### Don't

```scss
.foo {
  &__bar {
    &-bez {}
  }
}
```

#### Do

```scss
.foo {
  &__bar {}
  &__bar-bez {}
}
```

* Do not use concatenation to join hyphenated class names

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
* Class names should be kebab-case

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

### Naming conventions

#### Don't

```scss
@function FunctionName() {}

@mixin mixin_name() {}

@keyframes KEYFRAME_NAME() {}
```

#### Do

```scss
@function function-name() {}

@mixin mixin-name() {}

@keyframes keyframe-name() {}
```

* Functions, mixins, and keyframes should all be kebab-case

### Variable naming

#### Canvas 3.0.0 and newer
* Use the `design` command
* Use kebab-case for CSS and SCSS variables
* Global CSS variables should be set in the `:root {}` declaration
* Global SCSS variables should be set in a SCSS file in the _styles/config/_ folder
* Local CSS and SCSS variables are only available in the declaration they are defined in
* Prefer CSS variables, only use SCSS variables where SCSS functions would break with CSS variables

#### Before Canvas 3.0.0

* For global variables use `$SCREAMING_SNAKE_CASE`
* For local variables use `$snake_case`
* Local variables are only available in the declaration they are defined in

### Naming conventions

#### Don't

```scss
@function FunctionName() {}

@mixin mixin_name() {}

@keyframes KEYFRAME_NAME() {}
```

#### Do

```scss
@function function-name() {}

@mixin mixin-name() {}

@keyframes keyframe-name() {}
```

* Functions, mixins, and keyframes should all be kebab-case

[ꜛ Back to TOC](#table-of-contents)

## Formatting

* [Calculations](#calculation)
* [Capitalisation](#capitalisation)
* [Commenting (inline)](#commenting-inline)
* [Commenting (loud)](#commenting-loud)
* [Commenting (introductory)](#commenting-introductory)
* [Indenting](#indenting)
* [Negative CSS variables](#negative-css-variables)
* [Parenthesise on @includes](#parenthesise-on-includes)
* [Whitespace](#whitespace)
* [Zero values & units](#zero-values--units)

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
  Color: var(--Color-Neutral-2);
}
```

#### Do

```scss
.foo {
  color: var(--color-neutral-2);
}
```

* Use kebab-case for selectors and properties
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

* Use `//` for inline comments not the block level `/* */`
* It's easier to un-comment
* Inline comments should start on a new line preceding the property they're describing
* Use sentence case for your comments
* Do not end with a full stop

### Commenting (loud)

```scss
.foo {
  color: var(--color-neutral-2);
  width: 100%;

  /**
   * Media queries.
   */
  @include mq($from: l) {
    color: var(--color-neutral-1);
  }
}
```

* Loud comments can be used to break up long code and make it easier to navigate
* Use the format above for loud comments
* Use sentence case for your comments
* End each line with a full stop

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

### Negative CSS variables

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
  color: rgb(255 0 0);
  // Or
  color: rgb(255 0 0 / 50%);
}
```

* Use `rgb()` colour functions using space separation as they are more human readable
* Do not use comma separated `rgb()` colour functions
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
--color-brand-1-default: rgb(97 149 237);
--color-neutral-2: rgb(226 227 228);
--color-neutral-3: rgb(212 215 217);
```

* Use the `design` command to generate variables and classes
* All colours should have variables defined for them, this will come from the designs
* Designers will determine the name of colours

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

* [`aspect-ratio`](#aspect-ratio)
* [`border`](#border)
* [`font-family`](#font-family)
* [`font-size`](#font-size)
* [`height` & `width`](#height--width)
* [`inset`](#inset)
* [`line-height`](#line-height)
* [`margin` & `padding`](#margin--padding)
* [`transform`](#transform)
* [`transition`](#transition)

### `aspect-ratio`

* The `aspect-ratio` property does not have the necessary browser support yet, avoid using it
* See [responsive banner padding](#responsive-banner-padding)

### `border`

#### Don't

```scss
.foo {
  border: none;
}

.bar {
  border: 2px solid var(--color-neutral-5);

  &:hover {
    border: 2px solid var(--color-neutral-1);
  }
}
```

#### Do

```scss
.foo {
  border: 0;
}

.bar {
  border: 2px solid var(--color-neutral-5);

  &:hover {
    border-color: var(--color-neutral-1);
  }
}
```

* Use `0` instead of `none` for borders
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
  --font-family-1: 'Avenir', Helvetica, Arial, sans-serif;
}

.foo {
  font-family: var(--font-family-1);
}
```

* Don't just declare the custom font
* Always provide a font stack which includes web-safe fonts
* Edit `fontStacks` in _design.config.js_ in Canvas to add font stacks
* This prevents loading blank pages until the font loads
* The `font-family` should be declared in a global variable

### `height` & `width`

#### Don't

```scss
.foo {
  height: calc(var(--spacing-m) + var(--spacing-2xs));
  width: 32px;
}
```

#### Do

```scss
.foo {
  height: 1.25rem;
  width: 2rem;
}
```

* Don't use spacing variables to build `height` or `width` values
* Don't use `px` units for `height` or `width` values
* Instead use `rem` values
* The spacing variables are only intended to keep spacing relative and not for dimensions
* `rem` units scale relatively when the user zooms in, whereas `px` units may cause issues

#### Exceptions

```scss
.icon {
  height: var(--icon-m);
  width: var(--icon-m);
}
```

* There are existing variables for icon sizes
* Use these for icon element styles

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
  // Before Canvas 3.0.0
  @include ms(0);
}

* Use rem, relative ems
* They're easier to understand as they're always relative to the base font-size
* `em` units change based on the current elements `font-size`
* `px` units are not as responsive to device or zoom level
* Canvas 3.0.0 or newer defines font sizes as global variables
* Before Canvas 3.0.0 we used the font function `ms()`

### `inset`

* The `inset` property (and its longhand versions) do not have full browser support yet, avoid using
* See [RTL](#rtl) for more details

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
  margin-top: var(--spacing-xs);
  margin-right: calc(var(--spacing-s) - var(--spacing-3xs));
  padding-bottom: var(--spacing-m);
  padding-left: var(--spacing-l);
}
```

#### Do

```scss
.foo {
  margin-block-start: var(--spacing-xs);
  margin-inline-end: var(--spacing-s);
  padding-block-end: var(--spacing-m);
  padding-inline-start: var(--spacing-l);
}
```

#### Logical properties
* Use `margin-block-*`, `margin-inline-*`, `padding-block-*`, and `padding-inline-*` properties
* These reflect the current `writing-mode`, `direction`, and `text-orientation` of the page meaning they better support multi-language, especially right-to-left languages
* Be wary of using `margin-block-start` as vertical margins collapse
* Use `margin` and `padding` shorthand properties only when all values match, or if values of properties on the same axis match
* Safari does not support CSS variables when using `margin-block`, `margin-inline`, `padding-block`, and `padding-inline` so use individual styles when using CSS variables

> Read more about [`margin-block`](https://developer.mozilla.org/en-US/docs/Web/CSS/margin-block), [`margin-inline`](https://developer.mozilla.org/en-US/docs/Web/CSS/margin-inline), [`padding-block`](https://developer.mozilla.org/en-US/docs/Web/CSS/padding-block), and [`padding-inline`](https://developer.mozilla.org/en-US/docs/Web/CSS/padding-inline).

#### Variable calculations
* Don't use `calc()` to combine spacing variables to perfectly match the pixel value of designs
* Instead use the closest existing spacing variable if possible
* Where the difference between the desired value and the available variables is too great you can use `calc()`, just remember to add a [magic number comment](#magic-numbers) explaining why

### `transform`

#### Don't

```scss
.foo {
  transform: translateY(var(--spacing-m)) scale(2) rotate(45deg)
}
```

#### Do

```scss
.foo {
  rotate: 45deg;
  scale: 2;
  translate: 0 var(--spacing-m);
}
```

* Do not use the shorthand `transform` property to `rotate`, `scale`, or `translate` as these keywords have their own properties
* This allows you to animate and transition individual properties
* Continue using `transform` to use the `matrix` and `skew` keywords

### `transition`

#### Don't

```scss
.foo {
  transition: ease opacity 0.4s 0.2s;
}
```

#### Do

```scss
.foo {
  @include transition(opacity)
}
```

* Use the `transition` mixin
* See the `get-transition-properties` function for default values used for timing and ease
* You can customise on a per mixin basis

```scss
.foo {
  @include transition(transform var(--timing-slow) var(--easing-normal))
}
```

* Do not transition `margin` or positional properties (`top`, `left`, `right`, `bottom`) as these don't perform well and lead to poor frame rates
* Instead use `padding` or `translate` as these can be hardware-accelerated

[ꜛ Back to TOC](#table-of-contents)
