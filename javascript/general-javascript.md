# General

## Function expression vs declaration

* There are two ways to write functions:

```
// function expression
let hello = function() {
    return 'hello';
};

// function declaration
function hello() {
    return hello;
}
```
* The main differences between the two is:
    * Function expressions are just variables pointing to anonymous functions, and so the variable can be reassigned.
    * Whereas function expressions have their variables hoisted, but not the function they point to, function declarations are hoisted in their entirety. This means that function declarations can be called *before* they are defined, but functions expressions cannot.
