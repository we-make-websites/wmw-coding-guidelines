<p align="center"><img src="https://raw.githubusercontent.com/we-make-websites/wmw-coding-guidelines/master/assets/logo.png" alt="We Make Websites" width="170"></p>

# Coding Guidelines
Standards, guidelines, and best practice we follow whilst developing at [We Make Websites](https://wemakewebsites.com/) on our Frame framework.

## Table of contents

1. [HTML](html/README.md)
1. [Liquid](liquid/README.md)
1. [CSS/SCSS](css/README.md)
1. [JavaScript](https://github.com/Shopify/javascript#import-javascript-from-shopify)
1. [VS Code](vs-code/README.md)

## Why do we need guidelines?

When working on large, long-running projects, with dozens of developers of differing specialities and abilities, it is important that we all work in a unified way in order to:

* Keep the code base maintainable
* Keep code transparent, sane, and readable
* Keep the code base scalable
* Maintain the code base's performance

## Applying the guidelines

When you are writing new code you are expected to maintain the guidelines, if you update an existing feature you are expected to update any non-adhering code that you interact with or change. This means you are not expected to fix the whole file when adding new functionality, just the code you write or change.

### Frame 1 & 2
You must apply all guidelines manually; there is no automatic linting available.

### Frame 3
Use `yarn format` to automatically check your CSS and JS code. You will have to manually apply the guidelines to HTML and Liquid as there is no linter available.

> **🗒 Note:** Older Frame 3 projects will not adhere to the new HTML and Liquid guidelines as the guidelines were written after Frame 3 was launched.

## Can I contribute to the guidelines?

Please branch from `master` and submit a pull request for any suggested changes.