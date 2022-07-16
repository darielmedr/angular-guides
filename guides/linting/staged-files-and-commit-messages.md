# Linting staged files and commit messages 🔎  <!-- omit in toc -->

## Add Git Hooks

Install [Husky 🐶](https://www.npmjs.com/package/husky).

```shell
npm install -D husky
```

Add Husky configuration to enable Git Hook.

```shell
npm pkg set scripts.prepare="husky install"
npm run prepare
```

## Add `pre-commit` hook

```shell
npx husky add .husky/pre-commit "npx lint-staged"
npx husky add .husky/pre-commit "npm run test:pre-commit"
```

### Add configuration for linting staged files

Install `lint-staged` package.

```shell
npm install -D lint-staged
```

Add file `.lintstagedrc.json` with configuration:

```json
{
  "src/**/*.{html,ts}": ["prettier --write", "eslint --fix"],
  "src/**/*.{scss,css}": ["prettier --write"]
}
```

⚠️ This config requires [ESLint + Prettier](eslint-prettier.md).

### Add npm script `test:pre-commit`

```shell
"test:pre-commit": "ng test --watch=false --browsers=ChromeHeadless"
```

## Add `commit-msg` hook with `commilint`

```shell
npx husky add .husky/commit-msg "npx --no-install commitlint --edit $1"
```

Install `commilint` packages.

```shell
npm install -D @commitlint/config-conventional @commitlint/cli
```

Add `commilint` config file `.commitlintrc.json`.

```js
{
  "extends": ["@commitlint/config-conventional"],
  "rules": {
    "type-enum": [
      2,
      "always",
      [ "feat", "fix", "docs", "style", "refactor", "perf", "test", "build", "ci", "chore", "revert" ]
    ]
  }
}
```
