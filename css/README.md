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

### [DRY (Don't Repeat Yourself)](#dry-dont-repeat-yourself)

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

#### Exceptions

* Sometimes it's still better to KISS (Keep It Simple, Stupid) than DRY.

### [Overriding stylelint](#overriding-stylelint)

#### Don't
```scss
// stylelint-disable-next-line
#app {
  // stylelint-disable-next-line
  background-color: $COLOR_BACKGROUND_DARK !important;
  margin: 0;
  // stylelint-disable-next-line
  position: absolute !important;
}
```

#### Do
```scss
// stylelint-disable declaration-no-important
// stylelint-disable selector-max-id

#app {
  background-color: $COLOR_BACKGROUND_DARK !important;
  margin: 0;
  position: absolute !important;
}
```

* If you must override the stylelint then add a document rule at the beginning of file instead of lots of comments
* Add the rule comments at the top of the file just after the intro comment with a newline between the rule comments and the first declaration block
* Add the rule comments in alphabetical order

[ꜛ Back to TOC](#table-of-contents)

## Methods

* [Mobile first](#mobile-first)
* [Responsive banner padding](#responsive-banner-padding)
* [Fullscreen elements](#fullscreen-elements)
* [Magic numbers](#magic-numbers)

### [Mobile first](#mobile-first)

#### Don't
```scss
.foo {
  // Desktop code

  @media (max-width: 768px) {
    // Mobile code
  }
}
```

### Do
```scss
.foo {
  // Mobile code

  @media (min-width: 768px) {
    // Desktop code
  }
}
```

* We should always develop mobile first
* This means we're expanding to fill desktop instead of cramming things in to fit mobile
* If we miss styling something the site will still be usable

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
* `aspect-ratio` is not yet supported in Safari

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

### [Magic numbers](#magic-numbers)

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

> **🗒 Note:** Read more on [CSS tricks](https://css-tricks.com/magic-numbers-in-css/).

[ꜛ Back to TOC](#table-of-contents)

## Declarations

* [#IDs](#ids)
* [!important](#important)
* [Longhand properties](#longhand-properties)
* [Prefixes](#prefixes)
* [Property order](#property-order)
* [Pseudo-elements & -selectors](#pseudo-elements-&--selectors)
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

#### Exceptions

* Sometimes you may have to because you're working with apps or code you can't otherwise edit
* In this case, and only as a last resort, use IDs
* Add a `stylelint-disable selector-max-id` comment at the start of the SCSS file immediately beneath the intro comment to disable linting for it

### [!important](#important)

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
* However before using `!important` consider doubling the specificity (see below) if you're battling code that is loaded in the page after your stylesheet but not actually inline
* Add a `stylelint-disable declaration-no-important` comment at the start of the SCSS file immediately beneath the intro comment to disable linting for it

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

#### Exceptions

* Feel free to use shorthand properties for `border`, `margin`, `padding`, and `transform`

### [Prefixes](#prefixes)

* Prefixes are automatically added to to CSS properties

### [Property order](#property-order)

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
1. Media queries (e.g. `@include mq($from: large)`)

### [Pseudo-elements & -selectors](#pseudo-elements-&--selectors)

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

It is the project lead developer's responsibility to set them up to maintain conformity.

Common properties that benefit from variables are:
* `border-radius`
* `color`
* `font-family`
* `font-weight`
* `margin` (gutters, grid gutters)
* `transition` (duration, easing) – consider a mixin

[ꜛ Back to TOC](#table-of-contents)

## Naming

* [BEM & CSS](#bem-&-css)
* [BEM modifiers](#bem-modifiers)
* [BEM naming](#bem-naming)
* [HTML naming](#html-naming)
* [Descriptive naming](#descriptive-naming)
* [Variable naming](#variable-naming)

### [BEM & CSS](#bem-&-css)

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

### [BEM modifiers](#bem-modifiers)

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

#### Do
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

#### Exceptions

* Feel free to write comments to explain why you've done things strangely if you have had to

### [HTML naming](#html-naming)

#### Don't
```scss
h3.foo {
  font-size: 2rem;
}
```

#### Do
```scss
.header__subtitle {
  font-size: 2rem;
}
```

* Don't use the HTML element name to select an element
* Makes maintenance easier, if you change the HTML element you have to change it in the CSS too
* BEM naming should make it clear what you're selecting

### [Descriptive naming](#descriptive-naming)

#### Don't
```scss
.curley-font {
  font-family: 'Lobster', serif;
}

.red {
  color: $COLOR_RED;
}
```

#### Do
```scss
.blog__subtitle {
  font-family: 'Lobster', serif;
}

.text-highlight {
  color: $COLOR_RED;
}
```

* CSS selectors should describe hierarchy, not the look as the look can change

### [Variable naming](#variable-naming)

#### Don't
```scss
$foo: 20px;

.foo {
  $foo-bar: (2 * $GRID_MARGIN);
  margin-left: $local_margin
}
```

#### Do
```scss
$GRID_MARGIN: 20px;

.foo {
  $local_margin: (2 * $GRID_MARGIN);
  margin-left: $local_margin
}
```

* For global variables use SCREAMING_SNAKE_CASE
* For local variables use snake_case
* Local variables are only available in the declaration they are defined in

[ꜛ Back to TOC](#table-of-contents)

## Spacing

* [Indenting](#indenting)
* [Whitespace](#whitespace)

### [Indenting](#indenting)

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

### [Whitespace](#whitespace)

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
* [Zero values & units](#zero-values-&-units)
* [Parenthesise on @includes](#parenthesise-on-includes)

### [Calculations](#calculation)

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
  left: calc(100% - (#{$GUTTER} * 2));
  width: (100% / 3);
}
```

* Wrap calculations in brackets for readability
* Put spaces between the values and the mathematical operators
* Put multiplying and dividing values after the starting value

#### Also
```scss
.foo {
  $local_margin: (2 * $GLOBAL_MARGIN);

  margin-bottom: $local_margin;
  width: calc(100% + #{$local_margin});
}
```

* Use local variables to define adjusted global variables

### [Capitalisation](#capitalisation)

#### Don't
```scss
.Foo {
  Background: $color_Blue;
}
```

#### Do
```scss
.foo {
  background: $COLOR_CORNFLOWER_BLUE;
}
```

* Use lowercase for selectors and properties
* Refer to [Variable naming](#variable-naming) for global and local variables

### [Commenting (inline)](#commenting-inline)

#### Don't
```scss
.foo {
  background-color: red; // Don't
  /* padding-top: 30px;
  width: 100% */
}
```

#### Do
```scss
.foo {
  // Comment above the line
  background-color: red;
  // padding-top: 30px;
  // width: 100%;
}
```

* Use `//` for commenting not the block level `/* */`
* It's easier to un-comment
* Inline comments should start on a new line preceding the property they're describing
* Use sentence case for your comments
* Do not end with a full stop

### [Commenting (introductory)](#commenting-introductory)

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

### [Zero values & units](#zero-values-&-units)

#### Don't
```scss
.foo {
  animation-delay: 0;
  margin: 0px;
  opacity: .4567;
}
```

#### Do
```scss
.foo {
  animation-delay: 0s;
  margin: 0;
  opacity: 0.4;
}
```

* Do specify units on zero duration times
* Don't specify units on zero length values
* Do add a leading zero for decimal places
* Don't go to more than three decimal places, the fewer the better

### [Parenthesise on @includes](#parenthesise-on-includes)

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
  @include animation(background-color);
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

[ꜛ Back to TOC](#table-of-contents)

## Colours

* [Colour properties](#colour-properties)
* [Colour variables](#colour-variables)

### [Colour properties](#colour-properties)

#### Don't
```scss
.foo {
  color: RED;
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

* Use RGB (or RGBA) colour values as they are more human readable
* These values are often supplied in brand guidelines

#### Exceptions

* If required, use lowercase and shorthand (where possible) hex colour values and names

### [Colour variables](#colour-variables)

#### Don't
```scss
$colour-blue-other: #6195ed;
$colour-dark-grey: #E2E3E4;
$lighter-grey: #d4d7d9;
```

#### Do
```scss
$COLOR_CORNFLOWER_BLUE: rgb(97,149,237);
$COLOR_IRON_1: rgb(226,227,228);
$COLOR_IRON_2: rgb(212,215,217);
```

* All colours should have variables defined for them, this will come from the design
* Use color as the prefix for your variables (not colour, this is consistent with the CSS spec)
* Colours are global variables so they should be in all caps
* Use [this website](http://chir.ag/projects/name-that-color/#6195ED) to automatically generate colour names (unless they're named in the client's brand guidelines)
* If you have two or more colours similar enough to share the same name then suffix them with a number where the 1 is the darkest variant of the colour

[ꜛ Back to TOC](#table-of-contents)

## Nesting

* [Nesting levels](#nesting-levels)
* [Nesting media queries](#nesting-media-queries)

### [Nesting levels](#nesting-levels)

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

  @media (max-width: 700px) {
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

  @media (min-width: 700px) {
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
* Requires an even more specific selector to override it
* Mounts up to a significant maintenance issue
* This does mean that you can't always nest properties the way you normally would
* Media queries are not included when counting levels of nesting
* VS Code can show you how many levels deep you are in its Breadcrumbs feature (View > Toggle Breadcrumbs)

### [Nesting media queries](#nesting-media-queries)

#### Don't
```scss
.foo {

  &__bar {
    // Code

    @media (min-width: 568px) {
      // Code
    }
  }

  &__section {
    // Code

    @media (min-width: 568px) {
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

  @media (min-width: 568px) {
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
* Frame 2+ keeps CSS files small and modular so `@media`/`@include mq()` won't get lost

[ꜛ Back to TOC](#table-of-contents)

## Properties

* [border](#border)
* [font-family](#font-family)
* [font-size](#font-size)
* [letter-spacing](#letter-spacing)
* [line-height](#line-height)
* [margin-top](#margin-top)
* [padding](#padding)
* [transition](#transition)

### [border](#border)

#### Don't
```scss
.foo {
  border: none;
}
```

#### Do
```scss
.bar {
  border: 2px solid rgb(0, 0, 0);

  &:hover {
    border-color: rgb(255, 255, 255);
  }
}
```

* Use 0 instead of none for borders
* When a border is set to 0 it will never display, however if a border is set to none but later overridden by a border-style it will display
* If you're changing a single part of the property then target that specific property rather than setting all the properties again, this is much easier to maintain

### [font-family](#font-family)

#### Don't
```scss
.foo {
  font-family: 'Avenir';
}
```

#### Do
```scss
.foo {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
}
```

* Don't only declare the custom font
* Always provide a font stack which includes web-safe fonts
* This prevents loading blank pages until the font loads
* The `font-family` should be declared in a variable, do not individually declare the `font-family`

### [font-size](#font-size)

#### Don't
```scss
.foo {
  font-size: 12px;
}

.bar {
  font-size: 1em;
}
```

#### Do
```scss
.foo {
  @include ms-respond(font-size, 1);
  // Or
  font-size: rem(18);
  // Or
  font-size: 1rem;
}
```

* Use rem, relative ems. They're easier to understand as they're always relative to the base font-size
* Frame 3 comes with a responsive font `@mixin`, use this

#### Don't
```scss
.foo {
  font-size: 1.263rem;
}
```

* This should be considered at the design phase but keep font sizes relative to each other in rational numbers

### [letter-spacing](#letter-spacing)

#### Don't
```scss
.foo {
  letter-spacing: 0.001rem;
}

.bar {
  letter-spacing: 0.02em;
}
```

#### Do
```scss
.foo {
  letter-spacing: 1px;
}
```

* Use pixel values for `letter-spacing`

### [line-height](#line-height)

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
  line-height: 1.6;
}
```

* Use unit-less relative `line-height`
* This means `line-height` will scale with the font size rather than being fixed
* Another consideration at the design phase is to keep line height relative to the font size in rational numbers

### [margin-top](#margin-top)

#### Don't
```scss
.foo {
  margin-top: 30px;
}
```

#### Do
```scss
.bar {
  &:not(:last-child) {
    margin-bottom: 30px;
  }
}
```

* Don't use margin-top to space elements out as vertical margins collapse
* Use padding-top or margin-bottom on preceding elements

#### Exceptions
* When you need a negative `margin-top`
* If the element is conditionally shown then use `margin-top` instead of a conditional class on the preceding element

### [padding](#padding)

#### Don't
```scss
.foo {
  padding-top: $SPACING_M;
}
```

#### Do
```scss
.foo {
  padding-block-start: $SPACING_M;
}
```

* Consider using [padding block](https://developer.mozilla.org/en-US/docs/Web/CSS/padding-block) and [padding inline](https://developer.mozilla.org/en-US/docs/Web/CSS/padding-inline) constituent properties instead of just `padding` as these reflect the current `writing-mode` of the page
* Note that the properties `padding-block` and `padding-inline` are not yet supported in Edge, you can only use the consistent properties (`padding-block-start`, `padding-block-end`, `padding-inline-start`, and `padding-inline-end`)
* In left to right `writing-mode`, the block properties are vertical (top and bottom) and inline is horizontal (left and right)

### [transition](#transition)

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
  @include transition(transform $TIMING_L $EASING_LINEAR)
}
```

* Do not transition `margin` or positional properties (`top`, `left`, `right`, `bottom`) as these don't perform well and lead to poor frame rates, instead use `padding` or `transform` as they can be hardware-accelerated

[ꜛ Back to TOC](#table-of-contents)