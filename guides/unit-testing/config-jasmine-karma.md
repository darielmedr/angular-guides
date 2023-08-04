# Configure Jasmine + Karma 🧪 <!-- omit in toc -->

Setup Jasmine + Karma for development and CI environment.

Add `karma.conf.js` config file.

```js
// Karma configuration file, see link for more information
// https://karma-runner.github.io/1.0/config/configuration-file.html

module.exports = function (config) {
  config.set({
    basePath: '',
    frameworks: ['jasmine', '@angular-devkit/build-angular'],
    plugins: [
      require('karma-jasmine'),
      require('karma-chrome-launcher'),
      require('karma-jasmine-html-reporter'),
      require('karma-coverage'),
      require('@angular-devkit/build-angular/plugins/karma'),
    ],
    client: {
      jasmine: {
        // you can add configuration options for Jasmine here
        // the possible options are listed at https://jasmine.github.io/api/edge/Configuration.html
        // for example, you can disable the random execution with `random: false`
        // or set a specific seed with `seed: 4321`
      },
      clearContext: false, // leave Jasmine Spec Runner output visible in browser
    },
    jasmineHtmlReporter: {
      suppressAll: true, // removes the duplicated traces
    },
    coverageReporter: {
      dir: require('path').join(__dirname, './coverage/advice-generator'),
      subdir: '.',
      reporters: [{ type: 'html' }, { type: 'text-summary' }],
    },
    reporters: ['progress', 'kjhtml'],
    browsers: ['ChromeHeadless', 'ChromeHeadlessCI'],
    customLaunchers: {
      ChromeHeadlessCI: {
        base: 'ChromeHeadless',
        flags: ['--no-sandbox'],
      },
    },
    restartOnFileChange: true,
  });
};
```

Update `angular.json` file.

```json
{
  "scripts": {
    "my-awesome-proyect": {
      "architect": {
        "test": {
          "karmaConfig": "karma.conf.js",
        }
      }
    }
  }
}
```

Add npm scripts in `package.json`.

```json
{
  "scripts": {
    "test": "ng test --browsers=ChromeHeadless",
    "test:ci": "ng test --no-watch --no-progress --browsers=ChromeHeadlessCI",
  }
}
```

Use `npm run test` in dev mode and `npm run test:ci` for the CI.
