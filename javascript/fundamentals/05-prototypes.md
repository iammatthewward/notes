# Prototypes
*Notes from [You Don't Know JavaScript book series](https://github.com/getify/You-Dont-Know-JS/)*

### [[Prototype]]

* Objects have an internal property called `[[Prototype]]`, which is just a reference to another object. This is almost always set to a non-null value when the object is created.
* Consider:

```
const myObject = {
    a: 1
};

myObject.a; // 1
myObject.b; // ?

```
* Since the `b` property does not exist on `myObject`, a value cannot be returned directly from the object. However this does not automatically result in returning `undefined`.
* When an object has a property called on it that is not immediately present, the `[[Prototype]]` link is used to try and find an available property of that name. Consider:

```
const myObject = {
    a: 1
};

const anotherObject = Object.create(myObject);

anotherObject; // {}
anotherObject.a; // 1

```
* Calling `anotherObject` will show us the properties that directly exist on the object, and so returns an empty object. When trying to access property `a` on `anotherObject`, no property is found and the `[[Prototype]]` chain is consulted to see if the property exists, and if so is returned. This works in the above example because `Object.create()` creates a new object that uses the object passed to it as its prototype.
* When performing a `[[Prototype]]` lookup, each level of the chain is consulted until a matching property is found. If no property is found, `undefined` is returned.

#### `Object.prototype`

* When an object has a `[[Prototype]]` chain, it must have an endpoint so that `[[Prototype]]` lookups have a place to end. This endpoint is a built-in object: `Object.prototype`;

#### Setting & shadowing properties
* When a property exists on both an object and somewhere in that object's `[[Prototype]]` chain, this is referred to as **shadowing**. The property that exists in the object is shadowing the property in the `[[Prototype]]` chain as this is lowest in the chain and will always be returned for a lookup on this object.
* When setting a value on a property on an object, a chain of events occurs:
    * If the object has the property directly defined, the value is updated.
    * If the object *does not* have the property directly defined, the objects `[[Prototype]]` is traversed to see if the property exists anywhere on the chain:
        * If the property does not exist on the `[[Prototype]]` chain, the property and value are added directly to the object.
        * If the property does exist in the `[[Prototype]]` chain then some surprising behaviour can happen:
            * If the property is *not* marked as read-only, a new property is added directly to the object.
            * If the property *is* marked as read-only, then both the setting of the existing property value and the creation of a shadowed property are not allowed, and the the setting of the property value is ignored (or if in `strict mode`, an error is thrown).
            * If the property is a setter, then the setter will be called; a shadowed property will not be created and the setter will not be redefined.
* It is possible to implicitly shadow properties - care should be taken to avoid this. For example, if an object has a property `a` on its `[[Prototype]]` chain but not directly on the object, then called `myObject.a++;` will not increment the value of the property on the `[[Prototype]]` chain. Instead it will created a shadowed property on the object and instead increment the value of that property.

#### Pretend Classes in JavaScript
* There is a convention in JavaScript to try and coerce the language into acting like a class-based language: creating a function beginning with a capital letter and calling it with `new` to create *instances* of the function. This is a problematic approach, as instances do not exist in JavaScript.
* This approach has been used to utilise a certain behaviour that exists: all functions get a property set on them by default that points to an object `function.prototype`.
* When an *instance* is created by calling `new MyFunc()`, it gets an internal `[[Prototype]]` link to the object that `MyFunc.prototype` is pointing at. This means that all *instances* have a `[[Prototype]]` link to the same object.
* Unlike class based languages where calling `new` creates a copy from a blueprint (which is independent from other copies), calling `new` in JavaScript creates an object linked to all other objects created by calling `new` on that function.

#### 'Constructors'
* In JavaScript, there is no such thing as a constructor in the sense of a constructor method on a class that is called to create an instance of that class.
* When a function is called with `new`, it appears as if a conventional constructor method has been called as that function call will return an object. In reality, this is just a side effect; when a function is called with `new` it evaluates just like any other function, but it also returns an object.
* This creates confusion because of some other odd behaviour. When a function is defined, it has a `constructor` property on it's `FunctionName.prototype` object, which is a reference back to the function.
* This `constructor` reference back to the function also exists on all objects created by calling `new` on that function (as a `[[Prototype]]` reference).
* Rather than a function being a constructor, or having a constructor method available to it, in reality a 'constructor call' only happens when a function is called with `new` in front of it.
