# Deploy to GitHub Pages workflow <!-- omit in toc -->

Create the following tree structure:

```tree
├─ .github
  ├─ workflow
    └─ deploy-gh-pages.yml
```

Setup `deploy-gh-pages.yml` file.

```yml
name: Build and Deploy

on:
  push:
    branches: [main]

permissions:
  contents: write

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [18.x]

    steps:
      - name: Checkout 🛎️
        uses: actions/checkout@v3

      - name: Deploy 🚀
        uses: AhsanAyaz/angular-deploy-gh-pages-actions@v1.4.0
        with:
          github_access_token: ${{ secrets.GITHUB_TOKEN }}
          build_configuration: production
          base_href: /advice-generator/
          deploy_branch: gh-pages
          angular_dist_build_folder: dist/advice-generator

```
