# CodeceptJS

![](https://codecept.io/images/cjs-base.png)

[codecept.io](https://codecept.io)

---

## CodeceptJS

* written in JavaScript
* **end to end** testing framework
* for web & native mobile
* cross-browser support
* high-level **unified APIs** for all backends
* opensource, MIT license

[codecept.io](https://codecept.io)

---

## Purpose of CodeceptJS

* Ideas taken from [Codeception](http://codeception.com)
* High-level BDD-style language
* Run a single test over multiple backends
* Don't worry about asychronity

---

### Sample Scenario

```js
Scenario('todomvc', (I) => {
  I.amOnPage('http://todomvc.com/examples/react/');
  I.waitForElement('.new-todo');
  I.dontSeeElement('.todo-count');
  I.fillField('What needs to be done?', 'Write a guide');
  I.pressKey('Enter');
  I.see('Write a guide', '.todo-list');
  I.see('1 item left', '.todo-count');
  I.fillField('What needs to be done?', 'Write a test');
  I.pressKey('Enter');
  I.see('Write a test', '.todo-list');
  I.see('2 items left', '.todo-count');
  I.fillField('What needs to be done?', 'Write a code');
  I.pressKey('Enter');
  I.see('Write a code', '.todo-list');
  I.see('3 items left', '.todo-count');
});
```

---

![](img/todo.gif)

---

### Architecture

![](img/codeceptjs-backends.svg)

---

## Engines

* WebDriver - classical, cross browser
* Puppeteer - Google Chrome API
* Playwright - cross-browser Puppeteer
* TestCafe - fast cross-browser engine
* Appium, Detox - mobile testing 

[How they work](https://medium.com/@davert/javascript-the-future-of-end-to-end-testing-bfc00e23110b)

---

> Whatever engine you choose it's easy to switch between them

---

## Notable features

* Interactive pause
* Auto retry of a failed steps
* Data Management via REST or GraphQL
* Parallel execution
* Gherkin / Cucumber support
* Reports

