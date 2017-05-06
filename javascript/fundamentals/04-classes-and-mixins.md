# Classes and mixins
*Notes from [You Don't Know JavaScript book series](https://github.com/getify/You-Dont-Know-JS/)*

### New Class syntax
* JavaScript contains some class-like syntax (`new`, `instance`), as well as the syntactical sugar of `class` in ES6, however it does not actually have classes.
* While it is possible to use JS in a way that is sometimes similar to class based programming, what is happening under the hood is very different to class based languages.


### Class mechanics
* To create and interact with an object, or *instance* from a class, we have to *instantiate* the class, which provides us with a constructed copy based on the class.
* Classes have *constructors* - methods that aid in the creation of class instances. It's job is to create the instances state. Constructors belong to their class, usually called with `new`.

### Class inheritance
* Classes can be *child classes* that inherit functionality from a parent class.
* Once defined, childhood classes can create their own behaviour and override any inherited behaviour. They are separate from their parent class once instantiated.

#### Polymorphism
* 'Relative polymorphism' refers to a methods ability to reference another method (which may or not have the same name) higher up in the inheritance hierarchy, and change or override it.
* This is relative because we relatively reference the class by saying 'look one level up'.
* Many languages (including new JS class syntax), use the keyword `super` to reference inherited methods.
* A key idea of polymorphism is that there can be multiple methods with the same name but different behaviour up the inheritance tree, with the behaviour of the method defined by where it exists in the hierarchy.
* In class-oriented languages, `super` is a way for a child class constructor to reference it's parents constructor, as the constructor belongs to the class. This is not the case in JS. It is more appropriate to think of the 'class' belonging to the constructor - constructors are not directly related.
* Child classes are not actually linked to their parent classes - instead, a child class simply copies what it needs from a parent.

#### Multiple inheritance
* Some class-oriented languages allow child classes to inherit from more than one parent class. This copies functionality from both parents to the child.
* This causes complications - if both parents have a methods of the same name, how is it decided which the child class will inherit?
* JavaScript does not allow for multiple inheritance, so these issues do not have to be dealt with.

### Mixins
* There are no classes in JavaScript, and objects get *linked* rather than copied. Therefore the JS object mechanism does not automatically copy methods or behaviour when inheriting or instantiating.
* As this behaviour is missing, some JS developers attempt to fake it in order to work in a more class-like manner using mixins.

#### Explicit mixins
* A mixin (often called `extend` in many libraries) is a utility created to manually implement copying behaviour we would expect to see in classes.
* A mixin function would take a source object and a target object and then copy any methods from the source to the target, skipping over any methods which are defined on the target (these would be polymorphic override methods).
* This is still slightly different from class-oriented inheritance as the methods on the target object aren't technically copies, rather references to the functions in the source branch.

#### Implicit mixins
* A function containing within a "class" which directly references another "class" function with a `this` rebinding is an *implicit mixin*, and will allow us to mixin the behaviour of a methdod without sharing it's state. eg:   

```
Another = {
    someFuncion() {
        // implicit mixin - takes behaviour of Something.cool without sharing the state
        Something.cool.call( this );
    }     
}
```
