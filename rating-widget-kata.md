# Rating Widget Kata

> A web development kata that introduces TDD style development for Javascript DOM (Document Object Model) interaction

## Kata Aims

The main aim is to become comfortable with the idea of doing taking a TDD (Test-Driven Development) approach to Javasscript that interacts with the browser DOM (Document Object Model). We'll do this by creating a simple five star rating HTML componenent: clicking any of the 'star' buttons will cause the corresponding number of stars to light up and call a callback function that could be used to communicate the state of the rating component.

## Before You Start

* You'll need a Javascript testing framework. For testing code in the browser I tend to use [Karma](http://karma-runner.github.io/0.13/index.html) running on [Grunt](http://gruntjs.com/), but any testing framework that runs in the browser will do. If you're new to unit testing and build tooling, you might find it easier to get started with [QUnit](https://qunitjs.com/).
* You should be familiar with [test-driven development](https://msdn.microsoft.com/en-us/library/aa730844(v=vs.80).aspx)
* Allow at least half an hour for the kata. Depending on your level of expertise you might not get the whole kata finished in an hour, but you'll certainly make good progress.
* Repeat the kata as often as you like – experiment with different ways to write tests and reach a solution

## Kata Rules

* Use a strict [test-driven development](https://msdn.microsoft.com/en-us/library/aa730844(v=vs.80).aspx) approach
* Don't read ahead – complete each step before looking at what's involved in the next task
* If working in groups, programme in pairs. If you're new to pair programming, ping pong-style pairing is a good way to start: one person writes a failing test; the other person then writes the code that causes that test to pass then writes the next test.
* None of your tests should need to simulate user interaction in the browser (eg. triggering events). If you find yourself needing to do this then you probably need to separate the function that handles user input from the event binding. eg:

```js
// not so great: you'll need to trigger the event to test the function associated with the event handler
document.querySelector('button').addEventListener('click', function (event) { // do stuff });

// definitely better: you can test the event handler function without having to trigger the event
// your code is also more readable, flexible and maintainable – result!
function doStuff (event) {
  // do stuff
}

document.querySelector('button').addEventListener('click', doStuff);
```

## Step 1

## Step 2

## Step 3

## Step 4
