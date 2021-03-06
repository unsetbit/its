# its [![Build Status](https://secure.travis-ci.org/ozanturgut/its.png?branch=master)](http://travis-ci.org/ozanturgut/its)

Its a utility to simplify common precondition or state checking. It's useful for signaling
to calling methods when they've made invalid calls to a method.

## Usage
There are four available functions:
* `its(expression [, errorType] [, messageTemplate [, messageArgs...]])` for throwing custom errors
* `its.defined(expression [, messageTemplate [, messageArgs...]])` for throwing reference errors
* `its.range(expression [, messageTemplate [, messageArgs...]])` for throwing range errors
* `its.type(expression [, messageTemplate [, messageArgs...]])` for throwing type errors
    * `its.undefined(obj [, messageTemplate [, messageArgs...]])` for throwing type errors
    * `its.null(obj [, messageTemplate [, messageArgs...]])` for throwing type errors
    * `its.boolean(obj [, messageTemplate [, messageArgs...]])` for throwing type errors
    * `its.array(obj [, messageTemplate [, messageArgs...]])` for throwing type errors
    * `its.object(obj [, messageTemplate [, messageArgs...]])` for throwing type errors
    * `its.func(obj [, messageTemplate [, messageArgs...]])` for throwing type errors
    * `its.args(obj [, messageTemplate [, messageArgs...]])` for throwing type errors
    * `its.string(obj [, messageTemplate [, messageArgs...]])` for throwing type errors
    * `its.number(obj [, messageTemplate [, messageArgs...]])` for throwing type errors
    * `its.date(obj [, messageTemplate [, messageArgs...]])` for throwing type errors
    * `its.regexp(obj [, messageTemplate [, messageArgs...]])` for throwing type errors

`expression` is a boolean value which determines whether the precondition will throw an error or not.

`messageTemplate` is a message with 0 or more '%s' placeholders for message arguments

`messageArgs` is a variable argument (0 or more) to fill the placeholders in the message template

`errorType` is used for throwing custom error objects. These objects should inherit from `Error`.

## Examples
```javascript
// Things that should pass
its.string('hi'); // returns true
its.func(function(){}); // returns true
its.date(new Date); //returns true
its.defined("anything"); // returns "anything"
its.type(typeof "something" === "string"); // returns true
its.range(1 < 2 && 1 > 0); // returns true
its(1 === 1); // returns true
its(1 === 1, ReferenceError); // throws true

// Things that shouldn't pass
its.defined(void 0); // throws ReferenceError
its.type(typeof "something" === "number"); // throws TypeError
its.range(1 < 2 && 1 > 2); // throws RangeError
its(1 !== 1); // throws Error
its(1 === void 0, ReferenceError); // throws ReferenceError

// Messages
its.defined(void 0, "This doesn't look right."); // throws ReferenceError with a message of "This doesn't look right."
its.defined(void 0, "This doesn't look %s.", "right"); // throws ReferenceError with a message of "This doesn't look right."
its.defined(void 0, "%s doesn't look %s.", "This", "right"); // throws ReferenceError with a message of "This doesn't look right."

// What real use may look like
var addOnlyNumbersBelow100 = function(number1, number2){
	its.number(number1);
	its.number(number2);
	its.range(number1 < 100);
	its.range(number2 < 100);
	return number1 + number2;
};

addOnlyNumbersBelow100(10, 20); // returns 30
addOnlyNumbersBelow100(10, 338484); // throws RangeError
addOnlyNumbersBelow100("10", 338484); // throws TypeError
```

## Developing
its uses grunt to build.
* `grunt` - Builds the standard and minified version of precondition in the dist folder
* `grunt test` - Builds precondition and runs unit tests (requires PhantomJS)
