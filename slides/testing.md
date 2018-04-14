## Testing Business Expectations

---

### Why Do We Test 

* To ensure software works as expected
* To discover bugs in software (before users)
* To measure performance 
* To seek for security issues

---

### What Tests We Can Automate

* **To ensure software works as expected**
* ~~To discover bugs in software (before users)~~
* **To measure performance**
* ~~To seek for security issues~~

---

> We automate tests to execute them at any time

---

### Automated Testing

* To establish trust
* For constant changes
* To stabilize current codebase

---

### Testing is Told Us to Be Like This:

![](img/owl.jpg)

---

We talk about how to test but we don't say

## What to Test

---

### Priority First

* Crucial **business scenarios**
* Security cases
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

# Readability

---

* #### Test should be easy to follow
* #### Test should be simple to update 
* #### Code can be reused to test similar cases

---


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

# 😞

---

# Stability

---

* #### Test should be stable by execution
* #### Test should be stable to changes

---


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

# 😧

---


Codeception + WebDriver

```php
// what if HTML changes?
$I->click('//body/div[3]/p[1]/div[2]/div/span');

// what if browser will render it longer?
$I->wait(1);
```

# 😱 

---


### How to write stable tests

* Don't mix specification with implementation
* Use interfaces for tests
* Focus on result, not on the path

[Blogpost: Expectation vs Implementation](http://codeception.com/12-21-2016/writing-better-tests-expectation-vs-implementation.html)

---

### Interfaces???


<img src="img/webinterface.png" style="float: left; width: 45%;">
<img src="img/interface.png" style="float: right; width: 45%;">

---

### What are Interfaces

* Interface define rules to get things done
* Interfaces considered stable
* Interface is not just a keyword

---

### 5 stages of interface change

![](img/grief.png)

---

### Anchor Tests To Stable Parts:

* Web Interface
* Public API (REST, GraphQL, SOAP)
* PHP Interfaces
* Public Methods in Domain

---

## Consider What Is Stable For You

---

## Can we test private methods?

* **Technically**: yes
* **Ideally**: no
* **Practically**: yes, if you consider them stable

---

### Focus On Result

* Will the test have to duplicate exactly the application code?
* Will assertions in the test duplicate any behavior covered by library code?
* Is this detail important, or is it only an internal concern?

[Blogpost: The Right Way To Test React Components](https://medium.freecodecamp.org/the-right-way-to-test-react-components-548a4736ab22)

---

# Speed

---

* #### For one test case:
  * fast enough for instant feedback
  * < 20 s
* #### For all tests
  * should be run on CI
  * easy to split into parallel processes
  * < 20 min

---

## Questions To Be Asked

* Should we sacrifice readability for speed?
* If so, why do you develop in PHP and not in C?

---

> Think how you can test a feature with minimal effort

---

## Test Infrastructure

Let's talk about implementation

---

### Outer and Inner testing

* **Outer**: test from the public interface
* **Inner**: test from the source code

---

![](img/pyramid.png)

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


![](img/pros-cons.svg)


---

![](img/test-layers.png)

---

# How to build test architecture?

---

## What To Test

* Write down specifications
* Choose specifications which should be tested
* Write examples for specification
* Choose the testing layer

---

> The more specific example we need to test the more detailed layer we choose.


---

### Acceptance vs Functional vs Unit

1. Choose a testing layer where test would be
  * Readable
  * Stable
  * Fast enough
1. Write a test
1. Repeat
1. Refactor!

---

## Unit vs Integration Tests

* Unit Tests for
  * pure functions
  * algorithms
  * complex data
  * dozen execution paths
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

> Even you can write a unit test with mocks it doesn't mean you should

---

## TDD || !TDD

* Is a team choice
* Hard to start (nothing is stable)
* Use TDD to discover specifications
* Plays nicely with outer and inner testing

---

## BDD || !BDD

* Writing tests in English is not about BDD at all
* BDD has its cost
* Use BDD when non-technical mates involved
  * *(when management is actually going to read your tests)*

---

# Test Architecture Templates

---

## New Project. How to test?

* Domain Layer should have unit / integration tests
* Application layer should have integration / functional tests
* UI should have acceptance tests with positive scenarios

---

## Early Stages Startup. How to test?

* Uncertainty Problem:
  * We don't have strict requirements
  * We can do pivot any day
  * We are unsure of EVERYTHING 😨
* Solution:
  * Test only when you stabilize the code
  * Start with Public API, Domain Logic

---

## Legacy Project. How to test?

* Detect the critical parts of a system
* Write acceptance tests for them
* Refactor old code
* Cover the new code with unit tests

---

## Conclusions

* Discover what to test
* Find a suitable level of testing
* Write readable+stable+fast tests!

---

## Questions!

... or holywars 😇

* **Michael Bodnarchuk** @davert
* Author of [Codeception](http://codeception.com) Testing Framework
* Consultant & Trainer at **[SDCLabs](http://sdclabs.com)**