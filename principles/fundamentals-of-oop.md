# The Four Fundamentals of OOP
There are four major principles that make a programming language object-oriented:
* [Encapsulation](#encapsulation)
* [Abstraction](#abstraction)
* [Polymorphism](#polymorphism)
* [Inheritance](#inheritance)

## 1. Encapsulation
Encapsulation is hiding the internal workings of an object so that the properties of the object can only be inspected or manipulated through methods defined for that purpose. This is done through two  types of methods: accessors and mutators.  

**Accessors**  
Accessors are methods which are used to ask an object about itself, and these are generally used to reveal the values of an objects properties. These can be referred to as *getter* methods.

**Mutators**  
Mutators are methods which are used to change the value of an objects properties. These can be referred to as *setter* methods.

Well encapsulated objects only have accessors and mutators where they are explicitly required, and hide all other object internals from outside inspection or manipulation.

## 2. Abstraction
Abstraction is tied closely to encapsulation, and denotes the characteristics of an object, without going into detail about the internal workings of the object. Abstraction is used to deconstruct complex systems into their smaller and more manageable components, and can be described as the process of defining the interfaces and functionality of objects rather than their implementation details.

## 3. Polymorphism
Polymorphism is the ability to process different objects in different ways, using the same methods, depending on their type or class. Simply put, polymorphism is being able to use the same interface for different types of data, with the functionality performed by the interface being defined by the data type.

## 4. Inheritance
Inheritance is the process of establishing subtypes of an existing object or class, and allowing those subtypes to inherit methods or properties from their parent. Inheritance allows common characteristics to only be defined once in a base class and then used by objects which have a relationship to that base class. 
