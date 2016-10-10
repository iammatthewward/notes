# JavaScript function invocation

## Simple invocation and the `call` method

* When a JavaScript function is invoked, it is typically done by literally calling the function with a set of parentheses (and optionally, some arguments):

```
function sayHi(name) {
    console.log('Hi ' + name);
}

// The above function can be called:
sayHi('Matt'); => // 'Hi Matt'
```

* What is actually *sort of* happening when a method is called this way, is that the function is called using the function's `call` method, with this step being hidden by some syntactical sugar.

* The `call` method takes a number of arguments - the first being the context the function is being called on, and any other arguments are passed to the function in the form of specified arguments:

```
sayHi('Matt');
// ^ this invocation desugars to:

sayHi.call(window, 'Matt');
// ^ the above is true except when using ES5, where the first argument passed is undefined
```

* This means that a function invoked this way allows access to it's context through the `this` keyword.


## Member function invocation

* Functions that are a member of an object are called in a similar way, with the object being passed to the call method as the first argument. The same function can be invoked without being attached to an object - the function's context is always set at call time :

```
let greetings = {};

function sayHi(name) {
    console.log('Hi ' + name);
}

greetings.sayHi = sayHi;

sayHi('Matt'); // desugars to sayHi.call(window, 'Matt');

greetings.sayHi('Matt'); // desugars to greetings.sayHi.call(greetings, 'Matt');
```

## Using `function.prototype.bind`

* It can often be useful to set a persistent notion of `this`, and so since ES5 there has existed a `bind` method that can be called on any function to set it's context:

```
let greetings = {
    name: 'Matt',
    sayHi : function() { console.log('Hi from ' this.name) }
}

// the value of this can be bound to the object when being passed around using bind
let boundSayHi = greetings.sayHi.bind(greetings);
```

* This can be useful when passing a function around as a callback.
