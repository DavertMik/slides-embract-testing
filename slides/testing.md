## Testing Business Expectations

---

### Why Do We Test 

* To ensure software works as expected
* To discover bugs in software (before users)
* To measure performance 
* To seek for security issues

---

> We automate tests to execute them at any time

---

### What Tests We Can Automate

* **To ensure software works as expected**
* ~~To discover bugs in software (before users)~~
* **To measure performance**
* ~~To seek for security issues~~

---

### Automated Testing

* To establish trust
* For constant changes
* To stabilize current codebase

---

### No Automated Testing When

* Features need to be delivered as fast as possible
* Project won't be changed in future
* Management is not interested in automated testing

---

### What Is Hard To Test

* Asynchronous interactions (JavaScript, Queues)
* 3rd party APIs
* Real data
* Captchas

---

## What to Test

---

### Priority First

* Crucial **business scenarios**
* Algorithms, functions with complex logic
* Everything that is hard to test manually

---

### Tests Should Be

* Independent - not affect each other
* Atomic - concentrated on one feature

---

### Start With General


```gherkin
Feature: customer registration

Background:
  Given I am unregistered customer

Scenario: registering successfully
  When I register
  Then I should be registered
```

---

## Add Details

```gherkin
Scenario: registering successfully
  When I register with
    | Name      | davert              |
    | Email     | davert@sdclabs.com  |
    | Password  | 123456              |

  Then I should be registered
  And I receive confirmation email

```

---

## Qualities of a Test

1. Readability
1. Stability
1. Speed

---

## Readability

* Test should be easy to follow
* Test should be simple to update 
* Code can be reused to test all similar cases

---

# 😞

```php
$request = $this->getRequest()
    ->setRequestUri('/user/profile/1')
    ->setParams(array('user_id'=>1));

$controller = $this->getMock(
    'UserController',
    array('render'),
    array($request, $response, $request->getParams())
);
$controller->expects($this->once())
         ->method('render')
         ->will($this->returnValue(true));

$this->assertTrue($controller->profileAction());
$this->assertTrue($controller->view->user_id == 1);         
```

---

## Stability

* Test should be stable by execution
* Test should be stable to code changes

---

# 😱 

Codeception + WebDriver

```php
// what if HTML changes?
$I->click('//body/div[3]/p[1]/div[2]/div/span');

// what if browser will render it longer?
$I->wait(1);
```

---

# 😧

```php
$mock = $this->getMock('Client', array('getInputFilter'));
$mock->expects($this->once()) // is it important?
    ->method('getInputFilter') // hardcoded method name
    ->will($this->returnValue($preparedFilterObject));

$formFactory = $this->getMock('Symfony\Component\Form\FormFactoryInterface');
$formFactory
    ->expects($this->once())
    ->method('create')
    ->will($this->returnValue($form))    
```

---


### How to write stable tests

* Don't mix specification with implementation
* Use public interfaces (API) for tests

[Blogpost: Expectation vs Implementation](http://codeception.com/12-21-2016/writing-better-tests-expectation-vs-implementation.html)

---

### Questions To Be Asked

* Will the test have to duplicate exactly the application code?
* Will assertions in the test duplicate any behavior covered by library code?
* Is this detail important, or is it only an internal concern?

[Blogpost: The Right Way To Test React Components](https://medium.freecodecamp.org/the-right-way-to-test-react-components-548a4736ab22)

---

## Speed

* For one test case:
  * fast enough for instant feedback
  * < 20 s
* For all tests
  * should be run on CI
  * easy to split into parallel thereads
  * < 20 min

---

## Questions To Be Asked

* Should we sacrifice readability for speed?
* If so, why do you develop in PHP and not in C?

---

## Test Infrastructure

Let's talk about implementation

---

### Outer and Inner testing

* **Outer**: test from the public interface
* **Inner**: test from the source code

---

> Think how you can test a feature with minimal effort

---

![](/img/pyramid.png)

---

## Test Types

* Outer
  * Acceptance: Browser-based UI tests
  * Characterization: CURL-based request/response
* Inner
  * Functional: Request/response emulation
  * Integration: Service with its dependencies
  * Unit: Service in pure isolation

---


![](/img/pros-cons.svg)


---

![](/img/test-layers.png)

---

## How To Test

* Write down specification
* Write examples for specification
* Choose the testing layer


---

> The more specific example we need to test the more detailed layer we choose.

---

## New Project. How to test?

* Domain Layer should have unit / integration tests
* Application layer should have integration / functional tests
* UI should have acceptance tests with positive scenarios

---

## Early Stages Startup. How to test?

* Uncertanity Problem:
  * We don't have strict requirements
  * We can do pivot any day
  * We are unsure of EVERYTHING 😨
* Solution:
  * Test only when you stablizie the code
  * Start with Public API, Domain Logic

---

## Legacy Project. How to test?

* Detect the critical parts of a system
* Write acceptance tests for them
* Refactor old code
* Cover the new code with unit tests

---

# How about?...

---

## Data Management & Cleanup

* create unique data per test
* clean up database between tests
* create data for test and revert afterwards

---

## Unit vs Integration Tests

* Unit Tests for
  * pure functions
  * algorithms
  * complex data
* Integration tests for
  * everything else

---

## Mocks

* Are dangerous:
  * Affect readability 
  * Affect stability
* Should be used for
  * Async services
  * 3rd-party services
  * Remote services

---

## TDD || !TDD

* Is a personal preference
* Hard to start (nothing is reliable)
* Use TDD to discover specifications

---

## Conclusions

* Discover what to test
* Find a suitable level of testing
* Write readable+stable+fast tests!

---

## Questions!

... or holywars 😇

* **Michael Bodnarchuk** @davert
* Author of Codeception Testing Framework
* Consultant & Trainer at SDCLabs