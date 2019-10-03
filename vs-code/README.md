# VS Code Settings

The below settings are recommended to help you follow our other guidelines.

## Table of contents

1. [Extensions](#extensions)
1. [Settings](#settings)

## Extensions

These are the minimum extensions we recommend you have installed.

These links will open the Visual Studio Marketplace.

* [Bracket Pair Colorizer 2](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer-2)
* [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
* [Liquid Languages Support](https://marketplace.visualstudio.com/items?itemName=neilding.language-liquid)
* [Import Cost](https://marketplace.visualstudio.com/items?itemName=wix.vscode-import-cost)
* [Shopify Liquid Template Snippets](https://marketplace.visualstudio.com/items?itemName=killalau.vscode-liquid-snippets)
* [stylelint](https://marketplace.visualstudio.com/items?itemName=shinnn.stylelint)
* [Snippet Creator](https://marketplace.visualstudio.com/items?itemName=nikitaKunevich.snippet-creator)

[ꜛ Back to TOC](#table-of-contents)

## Settings

Useful settings to make your life easier.

### Changing your settings

1. Press `cmd` + `,` to open the settings dialog in VS Code
1. Click the arrow pointing to the page icon in top right
1. Paste settings below into the JSON format file this has opened

### Table of contents

* [Character limit](#character-limit)
* [Disable file preview](#disable-file-preview)
* [File associations](#file-associations)
* [Git autofetch](#git-autofetch)
* [Trim trailing whitespace](#trim-trailing-whitespace)

### [Character limit](#character-limit)

```js
"editor.rulers": [
  80
],
"workbench.colorCustomizations": {
  "editorRuler.foreground": "#ffffff33"
}
```

* Adds a vertical border in your code editor denoting where the 80 character limit is
* The colour is in the hexadecimal colour with an additional alpha value

### [Disable file preview](#disable-file-preview)

```js
"workbench.editor.enablePreviewFromQuickOpen": false
```

* When opening files from `cmd` + `p` file search they won't open in preview mode
* This means that opening another file won't close the previous one

### [File associations](#file-associations)

```js
"files.associations": {
  "*.scss.liquid": "scss",
  "*.js.liquid": "javascript"
}
```

* This tells VS Code to ignore the `.liquid` extension and open the files with the write sort of highlighting enabled

### [Git autofetch](#git-autofetch)

```js
"git.autofetch": true
```

* Autofetches the Git repo every five minutes

### [Trim trailing whitespace](#trim-trailing-whitespace)

```js
"files.trimTrailingWhitespace": true
```

* Trims any trailing whitespace on file save (makes linting happy)

[ꜛ Back to TOC](#table-of-contents)

[← Back to homepage](../README.md)