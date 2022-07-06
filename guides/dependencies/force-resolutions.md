# Force resolutions ⛓️ <!-- omit in toc -->

Force the installation of specific version of a transitive dependency (dependency of dependency).

## Npm version 7 or greater (7.x+)

1. Check npm docs for [overrides](https://docs.npmjs.com/cli/v8/configuring-npm/package-json#overrides).
2. Update dependencies [👇](#update-dependency-tree).

## Npm older versions (6.x)

⚠️ Important: [Read warning](https://www.npmjs.com/package/npm-force-resolutions).

## Steps

Add `npm-force-resolutions` package

```shell
npm i -D npm-force-resolutions
```

Add `preinstall` script to *package.json* file

```json
{
  ...
  "scripts": {
    "preinstall": "npm install --package-lock-only --ignore-scripts && npx npm-force-resolutions",
    ...
  },
  ...
}
```

Add `resolutions` field with dependencies version to fix to *package.json* file

```json
{
  ...
  "resolutions": {
    "<package-name>": "<fix-version>",
    "minimist": "^1.2.6"
    ...
  },
  ...
}
```

## Update dependency tree

Run `npm install`

```shell
npm install
```

Confirm installed package version

```shell
npm ls <package-name>
```
