# Configure Jest + Angular Testing Library 🧪 <!-- omit in toc -->

Setup Jest + Angular Testing Library for default Angular project (Jasmine + Karma).

See [blog](https://dev.to/this-is-angular/migrate-from-jasmine-to-jest-and-testing-in-angular-286i).

## Remove Jasmine + Karma

Run

```cmd
npm uninstall -D @types/jasmine jasmine-core karma karma-chrome-launcher karma-coverage karma-jasmine karma-jasmine-html-reporter
```

or delete from `package.json` and then run `npm install`.

```json
"@types/jasmine": "x.y.z",
"jasmine-core": "x.y.z",
"karma": "x.y.z",
"karma-chrome-launcher": "x.y.z",
"karma-coverage": "x.y.z",
"karma-jasmine": "x.y.z",
"karma-jasmine-html-reporter": "x.y.z",
```

Delete `karma.conf.js` and `src/test.ts` files.

```cmd
rm karma.conf.js src/test.ts
```

Remove `test` architect from `angular.json`.

```jsonc
{
  "projects": {
    "my-awesome-project": {
      "architect": {
        "test": {
          // remove
        }
      }
    }
  }
}
```

## Setup Jest

Install dependencies.

```cmd
npm install -D jest jest-preset-angular @types/jest
```

Add `setup-jest.ts` file.

```ts
import 'jest-preset-angular/setup-jest';
```

Add `jest.config.js` file.

```ts
/* eslint-disable */

const esModules = [
  // Add 3rd party libs
].join("|");

module.exports = {
  preset: "jest-preset-angular",
  setupFilesAfterEnv: ["<rootDir>/setup-jest.ts"],
  modulePaths: ["<rootDir>"],
  transformIgnorePatterns: [`node_modules/(?!.*\\.mjs$|${esModules})`],
  coveragePathIgnorePatterns: ["/node_modules/"],
  snapshotSerializers: [
    "jest-preset-angular/build/serializers/no-ng-attributes",
    "jest-preset-angular/build/serializers/ng-snapshot",
    "jest-preset-angular/build/serializers/html-comment",
  ],
};
```

Edit `tsconfig.spec.json` file to set **jest** type.

```jsonc
{
  // ...
  "compilerOptions": {
    // ...
    "types": ["jest"]
  }
}
```

Edit `tsconfig.json` file.

```jsonc
{
  // ...
  "compilerOptions": {
    // ...
    "esModuleInterop": true
  }
}
```

Update npm script `test` in `package.json`.

```json
{
  "scripts": {
    "test": "jest",
    "test:coverage": "jest --coverage",
    "test:ci": "jest --ci --watchAll=false"
  }
}
```

## Setup Angular Testing Library
