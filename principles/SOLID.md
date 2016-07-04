# The Principles of SOLID

## S - Single Responsibility Principle
**What it is**
* Every class, method/function should have one responsibility only.
* If a method/function does more than one computation, one of these should be extracted to it's own method/function.
* Classes/methods/functions should be named appropriately to their responsibility.
**Why is it important**
* If something has more than one responsibility, any changes to one of the responsibilities may impair any other of the others.

## O - Open-Closed Principle
**What it is**
* Open to extension.
* Closed to modification.
* Code should be designed in a way that allows new functionality to be added with minimum changes to the existing code.
* Existing code is encapsulated.
**Why is it important**
* Code that uses the open-closed principle is well encapsulated, and can be adapted with greater speed and agility.

## L - Liskov Substitution Principle
**What it is**
* If S is a subtype of T, then objects of type T may be replaced with objects of type S.
* If a Tesla is a subtype of a car, then I should be able to drive a Tesla.
* We should be able to replace the base type with a subtype.
* Duck typing - if it walks like a duck and quacks like a duck, it's a duck.
* As long as something has the methods that we expect of a type, then we can consider it of that type.
**Why is it important**
* To eliminate strange behaviour between classes and subclasses.

## I - Interface Segregation Principle
**What it is**
* No client should be forced to depend on methods it does not use.
* Fat interfaces should be broken down into smaller and more specific interfaces.
* Clients only need to know about methods that are of interest to them.
**Why is it important**
* It helps a system be easier to refactor and change.

## D - Dependency Inversion Principle
**What it is**
* High level modules should not depend on low level modules - they should both depend upon abstractions.
* The reduction of coupling between different pieces of code.
* Moving dependencies externally.
**Why is it important**
* To develop reusable components that rely upon external dependencies.
