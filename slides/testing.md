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


in Browser testing

```php
// what if HTML changes?
$I->click('//body/div[3]/p[1]/div[2]/div/span');

// what if browser will render it longer?
$I->wait(1);
```

# 😱 

---


### How to write stable tests

* **Don't mix specification with implementation**
* Focus on result, not on the path
* Use interfaces for tests

---

### How to write stable tests

* Don't mix specification with implementation
* **Focus on result, not on the path**
* Use interfaces for tests

---

### Focus On Result

* Will the test have to duplicate exactly the application code?
* Will assertions in the test duplicate any behavior covered by library code?
* Is this detail important, or is it only an internal concern?

[Blogpost: The Right Way To Test React Components](https://medium.freecodecamp.org/the-right-way-to-test-react-components-548a4736ab22)


---

### Anchor Tests To Stable Parts:

* Web Interface
* Public API (REST, GraphQL, SOAP)
* PHP Interfaces
* Public Methods in Domain

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

