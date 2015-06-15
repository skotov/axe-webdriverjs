# axe-webdriverjs Integration README

This example demonstrates how to use aXe to run web accessibility tests with Selenium's WebDriverJS browser automation tool.
The Webdriver integration allows for automatic testing of full pages and sites.

## Requirements
* WebDriver must be installed; follow the directions at http://webdriver.io/guide/getstarted/install.html
* Google Chrome must be installed; follow the directions https://www.google.com/chrome/

## Getting Started

Install the WebDriver module: `npm install axe-webdriverjs`

## Usage

This module uses a chainable API to assist in injecting, configuring and analyzing web pages using aXe with Selenium WebDriverJS.  As such, it is required to pass an instance of WebDriver.

Here is an example of a script that will drive Selenium to this repository, perform analysis and then log results to the console:
```
var AxeBuilder = require('axe-webdriverjs'),
  WebDriver = require('selenium-webdriver');

var driver = new WebDriver.Builder()
  .forBrowser('firefox')
  .build();

driver
  .get('https://github.com/dequelabs/axe-webdriverjs')
  .then(function () {
    AxeBuilder(driver)
      .analyze(function (results) {
        console.log(results);
      });
  });
```

### `AxeBuilder(driver:WebDriver)`

This is the constructor for the AxeBuilder helper. You must pass an instance of selenium-webdriver as the first and only argument.  Can be called with or without the `new` keyword.

```javascript
var builder = AxeBuilder(driver);
```

### `AxeBuilder#include(selector:String)`

This function adds a CSS selector to the list of elements to include in the analysis; the example below includes the `.results-panel` selector.

```javascript
AxeBuilder(driver)
  .include('.results-panel');
```

### `AxeBuilder#exclude(selector:String)`

This functions adds a CSS selector to the list of elements to be excluded from analysis; the example below excludes the `.results-panel h2`selector.

```javascript
AxeBuilder(driver)
  .include('.results-panel')
  .exclude('.results-panel h2');
```

### `AxeBuilder#options(options:Object)`

This function call specifies options to be used by `axe.a11yCheck`.  Be careful, because **it will override any other configured options, including calls to `withRules` and `withTags`.** See [axe-core API documentation](https://github.com/dequelabs/axe-core/blob/master/doc/API.md) for information on its structure.

```javascript
AxeBuilder(driver)
  .options({ checks: { "valid-lang": ["orcish"] }});
```

### `AxeBuilder#withRules(rules:Mixed)`

This function limits analysis only to sections with the specified rule IDs.  The function call accepts a String of a single rule ID or an Array of multiple rule IDs. **Note that subsequent calls to `AxeBuilder#options`, `AxeBuilder#withRules` or `AxeBuilder#withRules` will override specified options.**

```javascript
AxeBuilder(driver)
  .withRules('html-lang');
```

```javascript
AxeBuilder(driver)
  .withRules(['html-lang', 'image-alt']);
```

### `AxeBuilder#withTags(tags:Mixed)`

Limits analysis to only those with the specified rule IDs.  Accepts a String of a single tag or an Array of multiple tags.  **Subsequent calls to `AxeBuilder#options`, `AxeBuilder#withRules` or `AxeBuilder#withRules` will override specified options.**

```javascript
AxeBuilder(driver)
  .withTags('wcag2a');
```

```javascript
AxeBuilder(driver)
  .withTags(['wcag2a', 'wcag2aa']);
```


### `AxeBuilder#analyze(callback:Function)`

Performs analysis and passes the result object to the provided function.  **Does not chain as the operation is asynchronous**

```javascript
AxeBuilder(driver)
  .analyze(function (results) {
    console.log(results);
  });
```

## Examples

This project has a couple integrations that demonstrate the ability and use of this module. You can access them here:

1. [test/integration/doc-lang.js](test/integration/doc-lang.js)
2. [test/integration/frames.js](test/integration/frames.js)


## Contributing

This repository is available for download and enhancement. We will accept pull requests once we publish our Contributor agreement.
