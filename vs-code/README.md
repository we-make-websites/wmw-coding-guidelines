# VS Code Settings

The below settings are recommended to help you follow our other guidelines.

## Table of contents

1. [Extensions](#extensions)
1. [Settings](#settings)

## Extensions

These are the minimum extensions we recommend you have installed.

CANVAS projects can automatically install these extensions in VS Code.

These links will open the Visual Studio Marketplace.

* [Bracket Pair Colorizer 2](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer-2)
* [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)
* [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
* [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
* [GraphQL](https://marketplace.visualstudio.com/items?itemName=GraphQL.vscode-graphql)
* [Highlight Matching Tag](https://marketplace.visualstudio.com/items?itemName=vincaslt.highlight-matching-tag)
* [Import Cost](https://marketplace.visualstudio.com/items?itemName=wix.vscode-import-cost)
* [Liquid](https://marketplace.visualstudio.com/items?itemName=sissel.shopify-liquid)
* [stylelint](https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint)
* [Vetur](https://marketplace.visualstudio.com/items?itemName=octref.vetur)

We do not recommend the Prettier extension as it's _too_ opinionated.

[ꜛ Back to TOC](#table-of-contents)

## Settings

Useful settings to make your life easier.

CANVAS projects can automatically apply these settings in VS Code.

### Changing your settings

1. Press `cmd` + `,` (`ctrl` + `,` on Windows) to open the settings dialog in VS Code
1. Click the arrow pointing to the page icon in top right
1. Paste settings below into the JSON format file this has opened

### Table of contents

* [Character limit](#character-limit)
* [Diff whitespace](#diff-whitespace)
* [Disable file preview](#disable-file-preview)
* [End of line character](#end-of-line-character)
* [File associations](#file-associations)
* [Git autofetch](#git-autofetch)
* [HTML validation](#html-validation)
* [Lint on save](#lint-on-save)
* [Tab size](#tab-size)
* [Trim trailing whitespace](#trim-trailing-whitespace)

### [Character limit](#character-limit)

```json
"editor.rulers": [
  80
],
"workbench.colorCustomizations": {
  "editorRuler.foreground": "#ffffff33"
}
```

* Adds a vertical border in your code editor denoting where the 80 character limit is
* The colour is in the hexadecimal colour with an additional alpha value

### [Diff whitespace](#diff-whitespace)

```json
"diffEditor.ignoreTrimWhitespace": false
```

* Don't ignore whitespace in the diff view of VS Code
* See [trim trailing whitespace](#trim-trailing-whitespace) so you don't have to manually remove trailing whitespace

### [Disable file preview](#disable-file-preview)

```json
"workbench.editor.enablePreviewFromQuickOpen": false,
```

* When opening files from `cmd` + `p` (or `ctrl` + `p` on Windows) file search they won't open in preview mode
* This means that opening another file won't close the previous one

### [End of line character](#end-of-line-character)

```json
"files.eol": "\n"
```

* Prevents issues with end of line characters being different

### [File associations](#file-associations)

```json
"files.associations": {
  "*.scss.liquid": "scss",
  "*.js.liquid": "javascript"
}
```

* This tells VS Code to ignore the `.liquid` extension and open the files with the write sort of highlighting enabled

### [Git autofetch](#git-autofetch)

```json
"git.autofetch": true
```

* Autofetches the Git repo every five minutes

### [HTML validation](#html-validation)

```json
"html.validate.scripts": false,
"html.validate.styles": false
```

* Stops validating `<script>` and `<style>` tags in HTML files
* These are frequently used to pass Liquid variables into global JS
* Because of the presence of Liquid in these tags VS Code's linter flags errors

### [Lint on save](#lint-on-save)

```json
"editor.formatOnSave": false,
"editor.codeActionsOnSave": {
  "source.fixAll.eslint": true,
  "source.fixAll.stylelint": true
}
```

* Runs `eslint --fix` and `stylelint --fix` on file save
* `formatOnSave` is set to `false` to prevent Prettier extension from running (if installed)

### [Tab size](#tab-size)

```json
"editor.tabSize": 2
```

* You should use a tab size of two spaces to follow guidelines

### [Trim trailing whitespace](#trim-trailing-whitespace)

```json
"files.trimTrailingWhitespace": true
```

* Trims any trailing whitespace on file save (makes linting happy)

[ꜛ Back to TOC](#table-of-contents)

[← Back to homepage](../README.md)