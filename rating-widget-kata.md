# Rating widget kata

> A web development kata that introduces TDD style development for Javascript web browser DOM interaction, and also introduces the concepts of dummy test doubles and spies.

## Kata Aims

The main aim is to become comfortable with the idea of doing taking a TDD (Test-Driven Development) approach to Javascript that interacts with the web browser DOM and also get an introduction to using dummy test doubles and spies in unit testing.

We'll do this by creating a simple five star rating HTML & Javascript componenent / module – `MyRating`. Clicking any of the 'star' buttons will cause the corresponding number of stars to become highlighted and communicate the state to an external model that holds the ratings for a range of movies (or anything else ratable). We won't create the model as part of this kata, but it would be easy enough to stub this out if you wish.

We won't be doing any CSS in the kata. In fact we won't be looking at it in the browser at all – all development work will be done through unit tests.

Please note that this kata does not necessarily demonstrate testing or development best practices, but rather aims to introduce some of the techniques you'll find useful in unit testing that involves the DOM.

## Before You Start

* You'll need a Javascript testing framework that runs in the browser and a Javascript library that will let you create spies. For testing code in the browser I tend to use [Karma](http://karma-runner.github.io/0.13/index.html) with [Mocha](http://mochajs.org/) running on [Grunt](http://gruntjs.com/), but any unit testing framework that runs in the browser will do. If you're new to unit testing and build tooling, you might find it easier to get started with [QUnit](https://qunitjs.com/). For the spies, [Sinon.js](http://sinonjs.org/) is highly recommended.
* You should have a basic level of familiarity with [test-driven development](https://msdn.microsoft.com/en-us/library/aa730844(v=vs.80).aspx) and unit testing.
* Try to allow around an hour for the kata. Depending on your level of expertise you might not get the whole kata finished in an hour, but you should certainly make good progress.

## Kata Rules

* Use a strict test-driven development approach.
* Don't read ahead – complete each step before looking at the next one.
* Don't worry about input validation or sanitising, or edge cases – stick to the happy path.
* If working in groups, program in pairs. If you're new to pair programming, ping pong-style pairing is a good way to start: one person writes a failing test; the other person then writes the code that causes that test to pass then writes the next test.
* None of your tests should need to simulate user interaction in the browser (eg. triggering events). If you find yourself needing to do this then you probably need to separate the event handler function from the event binding (see the example below).
* Feel free to use jQuery or a similar library for DOM traversal, element selection and getting or setting element properties, but don't use the library for anything else.

```js
// not so great: you'll need to trigger the event to test the function associated with the event handler
document.querySelector('button').addEventListener('click', function (event) {
  // do stuff
});

// definitely better: you can test the event handler function without having to trigger the event
// your code is also more readable, flexible and maintainable – result!
function doStuff (event) {
  // do stuff
}

document.querySelector('button').addEventListener('click', doStuff);
```

### If you repeat the kata

* Try skipping straight to step 4 and experiment with different ways to write tests and solve the kata.
* You could also try removing all tests apart from those you wrote for step 4 and then refactor your previous solution to work in a different way while still passing the tests.

### Discussion points

* What are the advantages and disadvantages of simulating user interaction / triggering DOM events in your tests? 
* Do you need to unit test each individual function, or can you can determine a function is working correctly by testing other functions that call the function. What are the advantages and drawbacks to this approach?
* What about only testing public API functions and not private functions?
* How could [code coverage](https://en.wikipedia.org/wiki/Code_coverage) help you decide what functions to test? What other factors might be involved?

## Step 1: `getRating()` and fixture set up / tear down

The HTML for the rating widget will look like this. It uses radio inputs (for potential non-javascript compatibility), but has been coded using data attributes in such a way that you could easily use just about whatever HTML tags you like as long as you use the data attributes correctly – the data attributes should be the only hooks your Javascript needs.

```xml
<fieldset data-myrating-key="human-nature" data-myrating-rating="1" data-myrating-role="container">
  <legend>Rate Human Nature, by Michel Gondry</legend>
  
  <input type="radio" value="1" name="rating-human-nature" data-myrating-value="1" data-myrating-key="human-nature" data-myrating-role="input-segment" id="rating-human-nature-1"/>
  <label for="rating-human-nature-1" data-myrating-role="output-segment" data-myrating-value="1" data-myrating-key="human-nature">1</label>
  
  <input type="radio" value="2" name="rating-human-nature" data-myrating-value="2" data-myrating-key="human-nature" data-myrating-role="input-segment" id="rating-human-nature-2"/>
  <label for="rating-human-nature-2" data-myrating-role="output-segment" data-myrating-value="2" data-myrating-key="human-nature">2</label>
  
  <input type="radio" value="3" name="rating-human-nature" data-myrating-value="3" data-myrating-key="human-nature" data-myrating-role="input-segment" id="rating-human-nature-3"/>
  <label for="rating-human-nature-3" data-myrating-role="output-segment" data-myrating-value="3" data-myrating-key="human-nature">3</label>
  
  <input type="radio" value="4" name="rating-human-nature" data-myrating-value="4" data-myrating-key="human-nature" data-myrating-role="input-segment" id="rating-human-nature-4"/>
  <label for="rating-human-nature-4" data-myrating-role="output-segment" data-myrating-value="4" data-myrating-key="human-nature">4</label>
  
  <input type="radio" value="5" name="rating-human-nature" data-myrating-value="5" data-myrating-key="human-nature" data-myrating-role="input-segment" id="rating-human-nature-5"/>
  <label for="rating-human-nature-5" data-myrating-role="output-segment" data-myrating-value="5" data-myrating-key="human-nature">5</label>
</fieldset>
```

* Start by setting up your tests so that the fixture HTML above will be dynamically inserted into the 'fixtures' area of the web page before each test and removed after each test.
* Remember, use a strict TDD approach, so always write your tests before writing any code (but treat the HTML fixture as part of the test).
* Create a Javascript module called `MyRating`
* Create a module function called `extractRating()`
	* `extractRating()` should accept one argument, an HTML element and should return the value of the element's `data-myrating-value` attribute, converted to an integer
	* For your tests, use the HTML fixture element(s) with the data attribute `data-myrating-role="input-segment"` as the function argument
* Create a similar method `extractKey()` that returns the value of the element's data attribute `data-myrating-key`

```js
function extractRating (DOMelement) {
  // returns the rating value associated with the element as an integer
}
```

```js
function extractKey (DOMelement) {
  // returns the rating value associated with the element as an integer
}
```

Hints:
* Your unit testing framework should have hooks to help you set up and remove your fixture HTML – possible called something like `beforeEach` and `afterEach`, or `setup` and `teardown`.
* For tests that are easier to maintain and read, also use the setup hook to create a new instance of the `Rating` constructor before each test.


## Step 2: `displayRating()` and DOM manipulation

* Create a new function `displayRaying()`
* This should accept a single argument, the rating value
	* The method should update the value of the data attribute `data-myrating-rating` on the HTML fixture element


## Step 3: `setModelCallback()`, `updateModel()` and spies

* Create a new function: `setModelCallback()`
	* `setModelCallback()` receives a single argument: a callback function
* Create a new function `updateModel()`
	* `updateModel()` receives two arguments: `key` (the key name of the thing being rated – eg. `'human-nature'`) and `rating`, the rating value
	* When `updateModel()` is called, the callback function defined using `setModelCallback()` should be called, and should be passed the `key` and `rating` values
	* Use a Sinon `Spy` to help you assert that the function has been called and has received the correct arguments

Hints:
* [Sinon's documentation on spies](http://sinonjs.org/docs/#spies)


## Step 4: `onInputUpdate()` and putting it all together

* Create an event handler function `onInputUpdate()` that receives an event object as its argument.
* This is the function that you would associate with an event listener (but don't create the event listener)
* When called, `onInputUpdate()` should use the functions you already created to:
	* Get the rating value and key name associated with the event `target` DOM object
	* Update the display (HTML DOM / view) to indicate the new rating
	* Call the associated model's update function
* You'll need to create a fake (dummy) version of an event object to pass to `onInputUpdate()` in your test(s). As you only really need to know the event's `target` property, one way to do this would be to simply create a Javascript object with a single property `target` that has a DOM element as its value.
* As you've tested the various individual functions already, you shouldn't need to fully test them all again – only create enough new tests to gain confidence this function does what it should.
* Once you've verified it's working for the HTML fixture, add several new HTML ratings components with different film names to the fixture and udpate your tests to ensure the correct HTML components and model attributes are being updated when `onInputUpdate()` is called with different event target elements. You'll probably need to do a little code (and test) refactoring.

```js
function onSelectRating (event) {
  // Get the rating value and key name associated with the event `target` DOM object
  // Update the display (view) to indicate the new rating
  // Call the associated model's update function
}
```

Hints:
* [A concise Stack Overflow description of the different between dummies, fakes, mocks and stubs](http://stackoverflow.com/questions/3459287/whats-the-difference-between-a-mock-stub#answer-17810004)
