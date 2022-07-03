# Linting with ESLint + Prettier 🔎  <!-- omit in toc -->

Configure [ESLint](https://eslint.org/) + [Prettier](https://prettier.io/) in [Angular](https://angular.io/) projects.

## Table of content <!-- omit in toc -->

- [Install Angular ESLint using schematics](#install-angular-eslint-using-schematics)
- [Install and configure Prettier](#install-and-configure-prettier)
  - [Install Prettier](#install-prettier)
  - [Add `.prettierignore` file in root directory](#add-prettierignore-file-in-root-directory)
  - [Add `.prettierrc` file in root directory](#add-prettierrc-file-in-root-directory)
- [Configure Prettier as an ESLint plugin](#configure-prettier-as-an-eslint-plugin)
  - [Install Prettier-ESLint dependencies](#install-prettier-eslint-dependencies)
  - [Replace `.eslintrc.json` file](#replace-eslintrcjson-file)
- [Integrate with VSCode](#integrate-with-vscode)
  - [Add extensions](#add-extensions)
  - [Edit `.vscode/settings.json` file](#edit-vscodesettingsjson-file)
- [Add script in `package.json` (Optional)](#add-script-in-packagejson-optional)

## Install Angular ESLint using schematics

```shell
ng add @angular-eslint/schematics
```

## Install and configure Prettier

### Install Prettier

```shell
npm i -D prettier
```

### Add `.prettierignore` file in root directory

ℹ️ Add patterns defined in `.gitignore` file. Use default patterns:

```pattern
**/coverage
**/.nyc_output
**/.github
**/.vscode
**/node_modules
**/dist
```

### Add `.prettierrc` file in root directory

```json
{
  "tabWidth": 2,
  "useTabs": false,
  "trailingComma": "all",
  "semi": true,
  "singleQuote": true,
  "bracketSpacing": true,
  "arrowParens": "always",
  "singleAttributePerLine": false
}
```

## Configure Prettier as an ESLint plugin

### Install Prettier-ESLint dependencies

```shell
npm i -D prettier-eslint eslint-config-prettier eslint-plugin-prettier
```

### Replace `.eslintrc.json` file

```json
{
  "root": true,
  "ignorePatterns": ["projects/**/*"],
  "overrides": [
    {
      "files": ["*.ts"],
      "parserOptions": {
        "project": ["tsconfig.json"],
        "createDefaultProgram": true
      },
      "extends": [
        "plugin:@angular-eslint/recommended",
        "plugin:@angular-eslint/template/process-inline-templates",
        "plugin:prettier/recommended"
      ],
      "rules": {
        "@angular-eslint/directive-selector": [
          "error",
          {
            "type": "attribute",
            "prefix": "app",
            "style": "camelCase"
          }
        ],
        "@angular-eslint/component-selector": [
          "error",
          {
            "type": "element",
            "prefix": "app",
            "style": "kebab-case"
          }
        ],
        "@angular-eslint/no-attribute-decorator": "error",
        "@angular-eslint/no-lifecycle-call": "error",
        "@angular-eslint/no-pipe-impure": "error",
        "@angular-eslint/no-queries-metadata-property": "error",
        "@angular-eslint/prefer-output-readonly": "error",
        "@angular-eslint/relative-url-prefix": "error",
        "@angular-eslint/sort-ngmodule-metadata-arrays": "error",
        "@angular-eslint/use-component-selector": "error",
        "@angular-eslint/use-component-view-encapsulation": "error"
      }
    },
    {
      "files": ["*.html"],
      "extends": ["plugin:@angular-eslint/template/recommended"],
      "rules": {
        "@angular-eslint/template/accessibility-alt-text": "error",
        "@angular-eslint/template/accessibility-elements-content": "error",
        "@angular-eslint/template/accessibility-label-has-associated-control": "error",
        "@angular-eslint/template/accessibility-table-scope": "error",
        "@angular-eslint/template/accessibility-valid-aria": "error",
        "@angular-eslint/template/no-any": "error",
        "@angular-eslint/template/no-autofocus": "error",
        "@angular-eslint/template/no-call-expression": "error",
        "@angular-eslint/template/no-distracting-elements": "error",
        "@angular-eslint/template/no-duplicate-attributes": "error",
        "@angular-eslint/template/no-positive-tabindex": "error",
        "@angular-eslint/template/use-track-by-function": "error"
      }
    },
    {
      "files": ["*.html"],
      "excludedFiles": ["*inline-template-*.component.html"],
      "extends": ["plugin:prettier/recommended"],
      "rules": {
        "prettier/prettier": ["error", { "parser": "angular" }]
      }
    }
  ]
}
```

ℹ️ Check [Angular ESLint](https://github.com/angular-eslint/angular-eslint) rules:

- [Typescript](https://github.com/angular-eslint/angular-eslint/tree/main/packages/eslint-plugin/docs/rules)
- [Html](https://github.com/angular-eslint/angular-eslint/tree/main/packages/eslint-plugin-template/docs/rules)

## Integrate with VSCode

### Add extensions

```shell
dbaeumer.vscode-eslint
esbenp.prettier-vscode
```

### Edit `.vscode/settings.json` file

```json
{
  "[typescript]": {
    "editor.defaultFormatter": "dbaeumer.vscode-eslint",
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": true
    },
    "editor.formatOnSave": false,
    "editor.formatOnPaste": false
  },
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": true
    },
    "editor.formatOnSave": false,
    "editor.formatOnPaste": false
  },
}
```

## Add script in `package.json` (Optional)

```json
{
  ...
  "scripts": {
    ...
    "lint:fix": "ng lint --fix"
  },
  ...
}
```
