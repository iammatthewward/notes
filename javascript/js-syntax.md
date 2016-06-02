# General https://learnxinyminutes.com/docs/javascript/
* in JS a function is just a function. A constructor function is so, due to the fact you call it with 'new'. By convention it begins with a capital letter (telling others not to call it without new).
* a constructor function is designed to help make new objects that have the same properties.
* the function keyword always creates a new object.
* the 'apply' function allows an array to be taken as an argument list:
  eg: Math.min.apply([1, 3, 6]); => 1


* console.log() > Anything between parentheses printed to terminal
* var > variables are declared after the var keyword

# Strings
  * .replace() > replaces specified parts of strings
    - eg: var string = 'this myString'; myString = var.replace('this', 'that');  

# Loops
  * while loop runs while a condition is true:
    while (condition) {
      code to run;
    }
  * do loop runs exactly like a while loop, except it always runs at least once first:
    do {
      code to run;
    } while (condition);
  * for loop run similar to while, but is more concise:
    for (i=0; i===4; i++){
      code to run;
    }
  * for in loops iterates over every property they are called on. They should not be used on arrays where the index
    order is important as there is no guarantee it will be return the indexes in any particular order:
    for (var x in variable){
      code to run involving x;
    }


# Math
  * .math used for math more complex than standard operators.
    - eg: math.round(0.5)

# 'prototype' vs 'this' http://stackoverflow.com/questions/310870/use-of-prototype-vs-this-in-javascript
  * a constructor's 'prototype' gives a way to share methods and values amongst instances via the private 'prototype' property
  * a function's 'this' is set by how the function is called. Where a function is called on an object, 'this' within the method references the object.
  * 'prototype' updates for _all_ instances of an object, whereas 'this' just updates it's own instance of an object.

# this http://javascriptissexy.com/understand-javascripts-this-with-clarity-and-master-it/
  * this is _nothing_ like self in ruby. this is not clearly defined, and the value of it is dependent on the way you call the function.
  * if 'this' is called with an explicit object (eg. dog.bark()) then 'this' becomes what it was called on (dog). If it is called without an object (Dog.bark()) then 'this' becomes the global object (window).
  * new creates a brand new empty object, with _this_ being the new object.
  * this should be defined within the constructor because they are properties of the object.

# prototype
  * prototypes are shared between all instances of an object. The instances of objects do not themselves have functions, their functions are stored in the prototype of the object they are instances of.
  * this means it is possible to change a prototype function on an object and _all_ instances of that object will have their functions changed.
  * prototype inheritance - when a function is called on an object, it looks for a function with that name. If it cannot find one it looks to the prototype of that object for a function with that name.
  * prototypes should be defined outside the constructor otherwise they will be invoked each time the constructor is invoked.
  * useful method - object.hasOwnProperty('property-name');
