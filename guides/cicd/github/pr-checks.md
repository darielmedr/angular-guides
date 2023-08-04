# PR check workflow <!-- omit in toc -->

Create the following tree structure:

```tree
├─ .github
  ├─ workflow
    └─ pr-checks.yml
```

Setup `pr-checks.yml` file.

```yml
name: PR checks

on:
  pull_request:
    types: [opened, reopened, synchronize]
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [18.x]

    steps:
      - name: Checkout ✅
        uses: actions/checkout@v3

      - name: Setup Node.js ${{ matrix.node-version }} 🏗
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      - name: Clean install ⚙️
        run: npm ci

      - name: Lint 🔎
        run: npm run lint

      - name: Build 🛠
        run: npm run build:prod

      - name: Test 🧪
        run: npm run test:ci
```
