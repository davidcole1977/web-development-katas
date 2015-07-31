> THIS IS WORK IN PROGRESS AND IS SIGNIFICANTLY FLAWED. PLEASE COME BACK LATER.

# Rating Widget Kata

> A web development kata that introduces TDD style development for Javascript web browser DOM interaction, and also introduces the concept of dummy test doubles and spies.

## Kata Aims

The main aim is to become comfortable with the idea of doing taking a TDD (Test-Driven Development) approach to Javascript that interacts with the browser DOM (Document Object Model) and also get an introduction to using spies and test double dummies. We'll do this by creating a simple five star rating HTML componenent: clicking any of the 'star' buttons will cause the corresponding number of stars to become highlighted and call a function that could be used to communicate the state of the rating component.

Please note that this kata does not necessarily encourage testing best practices, but rather aims to introduce some of the techniques you'll find useful in unit testing that involves the DOM.

## Before You Start

* You'll need a Javascript testing framework and a Javascript library that will let you create spies. For testing code in the browser I tend to use [Karma](http://karma-runner.github.io/0.13/index.html) running on [Grunt](http://gruntjs.com/), but any testing framework that runs in the browser will do. If you're new to unit testing and build tooling, you might find it easier to get started with [QUnit](https://qunitjs.com/). For the spies, [Sinon.js](http://sinonjs.org/) is highly recommended.
* You should have a basic level of familiarity with [test-driven development](https://msdn.microsoft.com/en-us/library/aa730844(v=vs.80).aspx) and unit testing.
* Try to allow around an hour for the kata. Depending on your level of expertise you might not get the whole kata finished in an hour, but you should certainly make good progress.
* Repeat the kata as often as you like – experiment with different ways to write tests and reach a solution.

## Kata Rules

* Use a strict [test-driven development](https://msdn.microsoft.com/en-us/library/aa730844(v=vs.80).aspx) approach.
* Don't read ahead – complete each step before looking at what's involved in the next task.
* Don't worry about input validation or sanitising, or edge cases – stick to the happy path.
* If working in groups, programme in pairs. If you're new to pair programming, ping pong-style pairing is a good way to start: one person writes a failing test; the other person then writes the code that causes that test to pass then writes the next test.
* None of your tests should need to simulate user interaction in the browser (eg. triggering events). If you find yourself needing to do this then you probably need to separate the function that handles user input from the event binding (see the example below).

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

## Step 1
> `getRating()` and fixture set up / tear down

The HTML for the rating widget will look like this:

```html
<fieldset>
  <legend>Rating widget</legend>
  <input type="radio" value="1" name="rating" id="rating-1"/><label for="rating-1">1</label>
  <input type="radio" value="2" name="rating" id="rating-2"/><label for="rating-2">2</label>
  <input type="radio" value="3" name="rating" id="rating-3"/><label for="rating-3">3</label>
  <input type="radio" value="4" name="rating" id="rating-4"/><label for="rating-4">4</label>
  <input type="radio" value="5" name="rating" id="rating-5"/><label for="rating-5">5</label>
</fieldset>
```

* Start by setting up your tests so that the fixture HTML above will be dynamically inserted into the 'fixtures' area of the web page before each test and removed after each test.
* In your Javascript code for the rating widget, create a function called `getRating(DOMelement)` that receives a single radio input DOM element as its argument and returns the rating value associated with that element.
* Remember to do this in TDD fashion, so create the test first before you write any Javascript.
* Use DOM elements from the fixture HTML as the `DOMelement` argument you'll pass to the function in the test(s) you create.

```js
function getRating (DOMelement) {
  // returns the rating value associated with the element
}
```

Hints:
* Your unit testing framework should have hooks to help you set up and remove your fixture HTML – possible called something like `beforeEach` and `afterEach`, or `setup` and `teardown`.

## Step 2
> `displayRating()` and DOM manipulation

* Create rating component fixture in the DOM to refect the rating value.
* This should be done by adding a class 'selected' to the the number of radio button components equivalent to the rating value. For instance, if the rating is three, the first three radio inputs will have a class of 'selected' and the remaining two will not.

```js
function displayRating (rating) {
  // updates the rating component in the DOM to refect the rating value (number 1 - 5)
}
```

### Example HTML fixture state with a rating of 3

```html
<fieldset>
  <legend>Rating widget</legend>
  <input type="radio" value="1" name="rating" id="rating-1" class="selected"/><label for="rating-1">1</label>
  <input type="radio" value="2" name="rating" id="rating-2" class="selected/><label for="rating-2">2</label>
  <input type="radio" value="3" name="rating" id="rating-3" class="selected/><label for="rating-3">3</label>
  <input type="radio" value="4" name="rating" id="rating-4"/><label for="rating-4">4</label>
  <input type="radio" value="5" name="rating" id="rating-5"/><label for="rating-5">5</label>
</fieldset>
```

Hints:
* foo

## Step 3
> `setRating()`, dummies and spies

setRating() is the event handler
spy on displayRating() to determine if it:
* was called
* received the value corresponding to the radio input clicked







* Create an event handler function `setRating(event)` that receives an event object as its argument.
* The function should 
* It's up to you whether or not to create and hook up the event listener at this point, but do not test the listener, only test the event handler function directly.
* You'll need to create a fake (dummy) version of the event object to pass to `onSelectRating()` in your test(s). As you only really need to know the event `target`, one way to do this would be to simply create a Javascript object with a single property `target` that has a DOM element as its value

```js
function onSelectRating (event) {
  // updates the rating component in the DOM to refect the rating value (number 1 - 5)
}
```

Hints:
* foo

## Step 4
> Create function `setRating()` (and spies?)

2. In your Javascript code for the rating widget, create an event handler function `onSelectRating(event)` that receives the event argument that would be passed to it by an event listener. For the purposes of this exercise you don't strictly need to create the actual event listener, as we won't be testing it directly, but you might find it helps you understand the code.
3. Create another function `setRating(value)`.
4. It's time to create your first test. We want to assert that when `onSelectRating()` is called, it in turn calls `setRating()`, passing to it a value that corresponds to the value of the HTML radio button that triggered the event.
  * Remember that we will not actually be simulating any user interaction or triggering any events, so we need a way to fake this part.
  * We also need a way to determine if `setRating()` was called and the value passed to it. As we're testing `onSelectRating()` and not `setRating()`, we'll need some help with this, which is where Sinon's spies come in.

**Event object dummy example**
```js

```

Hints:
* [Sinon's documentation on spies](http://sinonjs.org/docs/#spies)

## Step 3

## Step 4
