<p align="center"><img src="https://raw.githubusercontent.com/we-make-websites/wmw-coding-guidelines/master/assets/logo.png" alt="We Make Websites" width="170"></p>

# FRAME Coding Guidelines
Standards, guidelines, and best practice we follow whilst developing at [We Make Websites](https://wemakewebsites.com/) on our FRAME framework.

## Table of contents

* [HTML](frame/html/README.md)
* [Liquid](frame/liquid/README.md)
* [CSS/SCSS](frame/css/README.md)
* [JavaScript](https://github.com/Shopify/javascript#import-javascript-from-shopify)
* [VS Code](frame/vs-code/README.md)

## Why do we need guidelines?

When working on large, long-running projects, with dozens of developers of differing specialities and abilities, it is important that we all work in a unified way in order to:

* Keep the code base maintainable
* Keep code transparent, sane, and readable
* Keep the code base scalable
* Maintain the code base's performance

## Applying the guidelines

When you are writing new code you are expected to maintain the guidelines, if you update an existing feature you are expected to update any non-adhering code that you interact with or change. This means you are not expected to fix the whole file when adding new functionality, just the code you write or change.

### FRAME 3
When committing your changes the SCSS and JS files will be automated linted due to `husky` and `lint-staged`. You will have to manually apply the guidelines to HTML and Liquid as there is no linter available.

> **🗒 Note:** The oldest FRAME 3 projects will not adhere to the new HTML and Liquid guidelines as the guidelines were written after FRAME 3 was launched.

### FRAME 1 & 2
You must apply all guidelines manually; there is no automatic linting available.

# CANVAS Coding Guidelines

These guidelines are for FRAME projects, for CANVAS projects [follow these guidelines](/README.md).
