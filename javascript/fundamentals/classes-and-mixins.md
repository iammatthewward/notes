# Classes and mixins
*Notes from [You Don't Know JavaScript book series](https://github.com/getify/You-Dont-Know-JS/)*

### New Class syntax
* JavaScript contains some class-like syntax (`new`, `instance`), as well as the syntactical sugar of `class` in ES6, however it does not actually have classes.
* While it is possible to use JS in a way that is sometimes similar to class based programming, what is happening under the hood is very different to class based languages.

### Class mechanics
* To create and interact with an object, or *instance* from a class, we have to *instantiate* the class, which provides us with a constructed copy based on the class.
* Classes have *constructors* - methods that aid in the creation of class instances. It's job is to create the instances state. Constructors belong to their class, usually called with `new`.

### Class inheritance
* Classes can be *child classes* that inherit functionality from a parent class.
* Once defined, childhood classes can create their own behaviour and override any inherited behaviour. They are separate from their parent class once instantiated.

#### Polymorphism
