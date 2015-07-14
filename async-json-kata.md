# Asynchronous JSON Kata: Getting a little deeper with Mocha and Chai

> This kata is aimed at those getting up to speed with test driven development in Javascript and related tooling, who wish to take a step beyond typical TDD katas, apply TDD to a simple real world web development problem and learn a little more about assertion styles in Chai, testing asynchronous code and hooks in Mocha.

> The kata was created for a specific front end development team using a Grunt / Karma / Mocha / Chai testing set up, but could easily be adapted to other set ups.


## Kata aims

* **Begin to apply test-driven development to real world web development**
* **Become familiar with Chai's expect assertion style and various different assertions**
* **Become familiar with asynchronous testing in Mocha**
* **Become familiar with Mocha's beforeEach() hook**
* **Become familiar with using fixture data in your unit tests**

## JSON fixture data we'll use

**success.json fixture**

```js
{
  "status": "success",
  "content": "foo"
}
```

**failure.json fixture**

```js
{
  "status": "failure"
}
```

## Task 1: Set up the JSON fixture files and make them available during testing

**You don't need to write any unit tests for this part.**

* Create the two fixture json files as above and place them in a fixtures folder within the unit tests folder
* Update the Grunt Karma task configuration so that the files – and any file in the same folder with a .json extension – are served by Karma's webserver, but are not run (included) in your test code.

Hints:

* [http://karma-runner.github.io/0.8/config/configuration-file.html](http://karma-runner.github.io/0.8/config/configuration-file.html)
* [http://karma-runner.github.io/0.8/config/files.html](http://karma-runner.github.io/0.8/config/files.htm)
* [https://github.com/karma-runner/grunt-karma](https://github.com/karma-runner/grunt-karma)
* [https://github.com/isaacs/node-glob](https://github.com/isaacs/node-glob)

## Task 2: Write a function that fetches the JSON data

**Everything from this point on should be done in strict TDD style**

Create a function called `getData()` that's a wrapper / facade for JQuery's `get()` function (or using `XMLHttpRequest`) and fetches a JSON file, passing the fetched data as an argument to a callback function.

An implementation of `getData()` would look something like this:

```js
function callbackHandler (data) {
    // do something with the data
}
  
getData("http://host.com/my-json-location", callbackHandler);
```

* Your first test should verify that the callback handler function receives an `Object` as its argument.
* Your second test should verify the argument received has the same contents as the JSON file returned from the requested URL (use Karma's web server address for `success.json`)
* This must be done asynchronously
* Use Chai's `expect` assertion style

Use the following Chai expect assertions:

* `a` / `an`
* `deep`
* `equal`

Hints:

[http://mochajs.org/#asynchronous-code](http://mochajs.org/#asynchronous-code)
[http://chaijs.com/api/bdd/](http://chaijs.com/api/bdd/)
[https://api.jquery.com/jquery.get/](https://api.jquery.com/jquery.get/)

For bonus points:

* Don't hard code the JSON data object in your test spec – instead make it available by reading the file contents using NodeJS module(s) / function(s)

## Task 3: Update your function to return an `error` argument if the JSON has a status of "failure"

An updated implementation of `getData()` would look something like this:

```js
function callbackHandler (error, data) {
    // if needed, handle the error
    // do something with the data
}
  
getData("http://host.com/my-json-location", callbackHandler);
```

Updated behaviour that must be verified by your unit tests:

* If the if the JSON data returned has a status of "failure" (as in the `failure.json` fixture file):
	* The error argument should be a Javascript `Error`
	* The data argument should have a `null` value
* If the JSON data returned has a status of "success" (as in the `success.json` fixture file):
	* The error argument should have a `null` value
	* The data argument should be an `Object` that contains the contents of the json file, as before

Use the following Chai expect assertions:

* `null`
* Any other assertion you feel is suitable

## Task 4: Update your function to throw an error if it receives invalid arguments

**The tests for this task must not be asynchronous.**

Update `getData()` to throw an Error if:

* The function doesn't receive two arguments
* The first argument is not a string
* The first argument has a length of 0
* The second argument is not a function

Use the following Chai expect() assertions:

* `throw` / `throws`
* Any other assertion you feel is suitable

Hints:

* You'll find `Function.prototype.bind()` useful when using the throw / throws assertion – eg. `getData.bind(null, argument1, argument2)`
* [http://stackoverflow.com/questions/21587122/mocha-chai-expect-to-throw-not-catching-thrown-errors](http://stackoverflow.com/questions/21587122/mocha-chai-expect-to-throw-not-catching-thrown-errors)
* [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)

## Task 5: Incorporate your function into a 'class' called DataHelper with a getData() method

An implementation of `getData()` would now look something like this:

```js
function callbackHandler (error, data) {
    // if needed, handle the error
    // do something with the data
}
 
var myDataHelper = new DataHelper();
myDataHelper.getData("http://host.com/my-json-location", callbackHandler);
```

* Refactor your tests to use the new class-like structure
* Use Mocha's beforeEach() to make a fresh instance of DataHelper available to each of your tests

Hints:

* [http://mochajs.org/#hooks](http://mochajs.org/#hooks)
