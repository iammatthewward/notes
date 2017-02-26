# JavaScript `this`
*Notes from [You Don't Know JavaScript book series](https://github.com/getify/You-Dont-Know-JS/)*

### What is `this`
* The `this` keyword is automatically defined in the scope of every function.
* There are a few common misconceptions about `this`:
	* It does not refer to the function itself, rather to the context the function is invoked from. To self reference a function from within it's lexical scope, an identifier can be bound to the function name itself (e.g. `foo.something` inside a `foo` function).
	* It does not refer to the function's lexical scope - the scope object is not accessible to JavaScript code. It is not possible to create a bridge between the lexical scopes of two functions (for example, calling function `this.bar` from within function `foo` will not give `bar` access to `foo`'s lexical scope).
* `this` is a runtime binding, rather than a lexical binding, and is all about *where* a function is invoked, rather than where it is declared.
* Remember - when a function is called (e.g. `foo()`) it is a shorthand for using the `call` method, which passes context as the first argument to a function (e.g. `foo.call( this )`).

### Call-site (how a function is called)
* The only thing that matters for `this` bindings is the call-site: where the function was called from.
* When diagnosing a `this` binding, referring to the call stack will help identify the flow of function calls, and the call-site of the function containing the binding.

### Call-site rules
* When a function is called, there are four rules that determine where `this` points to during execution (see below).
* When a call-site has multiple rules which can be applied, there is an order of precedence:

new binding > explicit binding > implicit binding > default binding

#### Default binding
* This is the default 'catch-all' rule, and is the most common.
* This binding occurs when the function is called with a plain undecorated function reference (e.g. `foo()`).
* n.b if using `strict mode` then the global object cannot be used for default binding, so any references to `this` within functions called from the global scope will result in a `this is undefined` error. This only applies to `strict mode` being used *within* the lexical scope of the function being called.

#### Implicit binding
* When the call-site has a context object, this object is implicitly used as the reference for `this` (e.g. when calling `obj.foo()`, any references to `this` will refer to the context of `obj`).
* When calling functions in nested objects, the last object reference will be used for `this` context (e.g. calling `obj1.obj2.foo()` will bind `this` to `obj2`).
* As the call-site of a function is what defines `this` context, implicit bindings can be lost if a reference to an implicitly bound function call is called from another call-site (e.g. `var bar = obj.foo` is simply an reference to the `foo` function, and so calling `bar()` will bind `this` to whatever call-site the function is invoked in).

#### Explicit binding
* If we want to force a function to use a particular object for it's `this` binding, we can use explicit binding with the `call()` and `apply()` functions that exist on every function.
*  Both functions take `this` as their first parameter, and invoke the function using the `this` parameter as their call-site binding.
*  When a primate value (string, boolean, number) is passed as the context, it is wrapped in its object form by having `new String/Boolean/Object()` called on it. This is called *boxing*.
*  Since ES5, `Function.prototype.bind` has existed for explicit hard binding, and takes a parameter to hard code the context for the function call.

#### `new` binding
* When a function is called using the `new` keyword it creates a brand new object, and this object is set as the `this` context for the function `new` is called on.

#### Objects
* JavaScript has object sub-types, referred to as built in objects, that include `String`, `Number`, `Boolean`, and `Function`.
* These built in objects may look similar to classes in other languages - they can be called with `new` - however these are just built in functions. Calling `new` on any of them will return a newly constructed object of that type.
* There are key differences between a primitive value, and an object of the same subtype as the primitive. For example, `var myString = 'string';` creates a primitive literal, whereas `var myString = new String('string);` creates a string object. While they may appear the same, a primitive literal is an immutable value and cannot have operations such as length checking and accessing individual characters, unlike a string object.
* While a primitive literal has limitations compared to it's object counterpart, JavaScript automatically coerces the primitive to the correct object type where necessary, so it is usually not necessary to explicitly create the object.

#### Duplicating objects
* Duplicating objects is quite complicated, in that there are so many different questions to be asked about *how* a particular object should be duplicated.
* ES6 added the ability to create shallow copies of objects (primitive values are copied, but non-primitives such as functions or arrays have only their references copied). This is achieved with `Object.assign(...)`, which takes a target object as it's first parameter and one or more source objects as other parameters.

#### Property descriptors
* Since ES5, properties are described via `Object.getOwnPropertyDescriptor(object, property);`, which returns a data object containing the properties value, writability status, enumerable status, and configurable status.
* We can also manually define these values with `Object.defineProperty(object, property, descriptorObject)`.

#### Immutability
* Immutability can be achieved in several ways:
	* Setting `writable` and `configurable` to false creates a constant.
	* `Object.preventExtensions(object)` on an object or array will prevent the values being added.
	* `Object.seal(object)` performs the same as `preventExtensions` but also sets all existing properties to not be configurable (note you can still modify property values).
	* `Object.freeze(object)` performs the same as `seal` but also sets all properties to not be writable, and makes their values immutable.

#### Getters and setters
* ES5 introduced getters and setters for objects, allowing you to create values that point to hidden functions that return or create the values of object properties.
* For example, below creates a getter and a setter for value `bar` on object `foo`:


```
var foo = {
	get bar() {
		return this._bar_ || 2;
	},

	set bar(value) {
		this._bar_ = value * 2;
	}

};
```

#### Existence
* The existence of a property in an object can be checked with `myObj.hasOwnProperty("property");`, which returns a boolean.
* This circumvents the issue of a purposefully set `undefined` value looking identical to a non-existent value that returns `undefined`.
