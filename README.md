<p align="center"><img src="https://raw.githubusercontent.com/we-make-websites/wmw-coding-guidelines/master/assets/logo.png" alt="We Make Websites" width="170"></p>

# 📏 Coding Guidelines

Standards, guidelines, and best practice when working on our Canvas framework.

Updates to Basis and projects using Basis Adapter must also follow these guidelines.

For Frame projects [follow these guidelines](frame/README.md).

## Table of contents

* [HTML & Vue `template`](html-vue-template/README.md)
* [Liquid](liquid/README.md)
* [CSS/SCSS](css/README.md)
* [JavaScript & Vue SFC](javascript-vue-sfc/README.md)
* [Accessibility](accessibility/README.md)
* [VS Code](vs-code/README.md)

## Why do we need guidelines?

When working on large, long-running projects, with dozens of developers of differing specialities and abilities, it is important that we all work in a unified way in order to:

* Keep the code base maintainable
* Keep code transparent, sane, and readable
* Keep the code base scalable
* Maintain the code base's performance

## Applying the guidelines

When you are writing new code you are expected to maintain the guidelines, if you update an existing feature you are expected to update any non-adhering code that you interact with or change. This means you are not expected to fix the whole file when adding new functionality, just the code you write or change.

Assuming you've followed [project setup](https://we-make-websites.gitbook.io/canvas/guides/project-setup) then your JS, SCSS, and Vue files will be formatted by `eslint --fix` and `stylelint --fix` when you save a supported file. Other `eslint` and `stylelint` errors will be flagged when committing your changes.

You will have to manually apply the guidelines to HTML and Liquid as there is no linter available. Certain Vue and SCSS rules must also be applied manually as their respective linters don't support them.

## Can I contribute to the guidelines?

Please branch from `main` and submit a pull request to the relevant code owner for review.

See [CONTRIBUTING.md](.github/CONTRIBUTING.md) for more details on process and code quality when contributing.
