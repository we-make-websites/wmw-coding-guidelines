<p align="center"><img src="https://raw.githubusercontent.com/we-make-websites/wmw-coding-guidelines/master/assets/logo.png" alt="We Make Websites" width="170"></p>

# CANVAS Coding Guidelines
Standards, guidelines, and best practice we follow whilst developing at [We Make Websites](https://wemakewebsites.com/) on our CANVAS framework.

## Table of contents

* [HTML & Vue `template`](html/README.md)
* [Liquid](liquid/README.md)
* [CSS/SCSS](css/README.md)
* [Vue & JavaScript](js/README.md)
* [VS Code](vs-code/README.md)

## Why do we need guidelines?

When working on large, long-running projects, with dozens of developers of differing specialities and abilities, it is important that we all work in a unified way in order to:

* Keep the code base maintainable
* Keep code transparent, sane, and readable
* Keep the code base scalable
* Maintain the code base's performance

## Applying the guidelines

When you are writing new code you are expected to maintain the guidelines, if you update an existing feature you are expected to update any non-adhering code that you interact with or change. This means you are not expected to fix the whole file when adding new functionality, just the code you write or change.

Assuming you've followed [setup](https://www.notion.so/wemakewebsites/Setup-CANVAS-WIP-16ca72ba2505444e939e3a30f1525c7f) then your JS and Vue files will be formatted when you save by `eslint --fix`. Other `eslint` and `stylelint` errors will be flagged when committing your changes. You will have to manually apply the guidelines to HTML and Liquid as there is no linter available.

## Can I contribute to the guidelines?

Please branch from `master` and submit a pull request for any suggested changes.

# FRAME Coding Guidelines

These guidelines are for CANVAS projects, for FRAME projects [follow these guidelines](frame/README.md).
