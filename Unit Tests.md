# Unit Tests

- [Unit Tests](#unit-tests)
  - [Test Driven Development](#test-driven-development)
  - [Types Of Tests](#types-of-tests)
    - [Unit Tests](#unit-tests-1)
    - [Integration Tests](#integration-tests)
    - [Automation Tests](#automation-tests)
  - [Testing Libraries](#testing-libraries)

## Test Driven Development
  - Write tests before writing code
  - Application code is written to adhere to code suites

## Types Of Tests

### Unit Tests
  - Should cover all small pure functions of an application
  - These do not test the connection between functions / items

### Integration Tests
  - Cross communication between units of codes
  - Use spies to check different side effects
  - Use stubs to fake function calls to test item communication
  - Downside is these are expensive - more dev time to create
  - Brittle - have a lot of moving parts

### Automation Tests
  - End to End Testing
  - UI Tests that run in a browser environment
  - Ensure scenarios work from perspective of the user
  - Take a long time to set up

- #### Automation Testing Libraries
  - Nightwatch
  - ***Web Driver IO*** - *best documentation*
  - ***Test Cafe*** - *all tools in one*
  - ***Nightmare*** - *automate user actions, webscraping*
  - Cypress

## Testing Libraries

| Scaffolding 	| Assertion 	| Test Runner 	| Mock, Spies, Stubs 	| Code Coverage 	|
|:-----------:	|:---------:	|:-----------:	|:------------------:	|:-------------:	|
| Jasmine 	| Jasmine 	| Jasmine 	| Jasmine 	| Istanbul 	|
| Jest 	| Jest 	| Jest 	| Jest 	| Jest 	|
| Mocha 	| Chai 	| Mocha 	| Sinon.js 	| Istanbul 	|
| - 	| - 	| Karma.js 	| - 	| - 	|
| - 	| - 	| Puppeteer 	| - 	| - 	|
| - 	| - 	| JSDOM 	| - 	| - 	|
