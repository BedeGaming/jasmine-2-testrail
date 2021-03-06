[![TestRail v4.1](https://img.shields.io/badge/TestRail%20API-v2-green.svg)](http://docs.gurock.com/testrail-api2/start)
# Jasmine2TestRail

This package allows you to use [Protractor](https://www.protractortest.org) in conjunction with [TestRail](http://www.gurock.com/testrail/).

* It can automatically create a new test run on TestRail.
* It can automatically send test results to TestRail - after they've been run.

## Install
```code
npm i jasmine-2-testrail
```

## Example - Protractor **conf.js**
The Reporter must be imported and declared outside of the config
<br>and included in the **onPrepare** section.
<br>The *createRun()* method is called for creating run in the **afterLaunch** section of the config file,<br>with the first parameter being your corresponding TestRail project ID
<br>and the second parameter being the suite ID in which you want to put the newly created run.
<br>*(The third parameter is undefined by default, check **Additional parameters** below)*
```javascript
const Reporter = require('jasmine-2-testrail')
const reporter = new Reporter({
});

exports.config = {
  onPrepare: () => {
    jasmine.getEnv().addReporter(reporter);
  },

  afterLaunch: () => {
    // The first parameter is the project ID, and the second is the suite ID
    reporter.createRun(1, 1, browser.params.runName)
    // afterLaunch needs to return a promise in order
    // to execute asynchronous code (used the most basic promise)
    return new Promise(() => true)
  },
}

```
## Additional parameters
### sendResultsToTestRail
You can also add a parameter if you don't want to send results to TestRail every time.
<br>In the config file, add:
```javascript
params: {
  sendResultsToTestRail: true,
},
```
You also need to add everything in the **afterLaunch** section in an *if* statement, like this:
```javascript
afterLaunch: () => {
    if (browser.params.sendResultsToTestRail) {
      reporter.createRun(1, 1, browser.params.runName)
      return new Promise(() => true)
    }
  }
```
This enables sending results by default, and if you want, you can disable it by running Protractor like this:
```javascript
protractor conf --params.sendResultsToTestRail=false
```
You can also invert the values, if you don't want to send results by default (sendResultsToTestRail: false).

### runName
There is also a parameter you can add for naming a run in TestRail.
```javascript
params: {
  runName: false,
},
```

```javascript
protractor conf --params.runName=TestRun1
```
(If you don't specify a run name, it defaults to the current date and time)


## Example - tests
The Case ID from TestRail must be added to the start of each *it()* description, <br>and separated from the test name by a colon - ":".
```javascript
describe('Login Page', () => {
  // "1:" this is Case ID from Test Rail
  it('1: Login success', async () => {
    expect(1).toBe(1)
  })

  it('2: Login fail', async () => {
    expect(1).toBe(0)
  })

  xit('3: Registration', async () => {
    expect(1).toBe(1)
  })
})
```
**Note:** The Case ID is a unique and permanent ID of every test case (e.g. C125),
<br>and shoudn't be confused with a Test Case ID, which is assigned to a test case<br> when a new run is created (e.g. T325).

## Example - **.env** file
This file needs to be created in the same directory as the **conf** file.
<br> It must contain the URL of your TestRail, username (email address) and password (or API key).
<br> This file needs to have all 3 parameters correctly filled.
```javascript
NETWORK_URL = https://<YourProjectURL>.testrail.io
USERNAME = email address
PASSWORD = password or API key
```
## Enable TestRail API
In order to use TestRail API, it needs to be enabled by an administrator
<br>in your own TestRail Site Settings.
Also if you want to use API authentication instead of your password,
<br>enable session authentication for API  in the TestRail Site Settings,
<br>and add an API key in your User settings *(This is recommended)*.

## Authors
| [<img src="https://avatars.githubusercontent.com/Slobo989" width="100px;"/><br /><sub><b>Slobodan Dušanić</b></sub>](https://github.com/Slobo989)| [<img src="https://avatars.githubusercontent.com/zeljkosimic95" width="100px;"/><br /><sub><b>Željko Simić</b></sub>](https://www.github.com/zeljkosimic95) |
|---|---|

## Special thanks

[<img src="https://avatars.githubusercontent.com/markoarsenal" width="100px;"/>
<br /><sub><b>Marko Rajević</b></sub>](https://github.com/markoarsenal)<br />

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE.md) file for details.
