# JavaScript Scope
*Notes from [You Don't Know JavaScript book series](https://github.com/getify/You-Dont-Know-JS/)*

### What is scope?
* A set of rules for looking up variables by their identifier name.
* Defines how and where a variable can be looked up.
* There are two types of lookup, depending on where the variable appears in the assignment:
	* LHS reference - target of an assignment (retrieve the variable rather than the assigned value)
	* RHS reference - source of an assignment (retrieve a value rather than assign a value)
* Code is compiled before being executed, so statements like `var a = 2` are split up into two steps:
	* `var a`: variable declaration in the scope.
	* `a = 2`: lookup the variable declaration and assign the value to it.

### Nested scope
* If a variable cannot be found in the immediate scope, the next outer containing scope is checked, and so on until it reaches the global scope.


### Lexical scope
* The first process of standard language compiling is lexing/tokenizing - breaking up a string of characters into meaningful chunks of code.
* Lexical scope is scope defined at lexing time (and so essentially author time) - based on where variables and blocks of scope are defined when written. This is generally set in stone by the time the code is processed.
* Shadowing - when the same variable name is used at multiple levels of nested scopes. Variable lookups stop when the first instance of a variable is found.

### Cheating lexical scope
* Lexical scope can be cheated, but is poor practice and poor performance.
* `eval(..)` takes a string argument and treats it as if it had been authored with the rest of the code, meaning a string such as `var b = 2` passed to a function and where it is passed as an argument to `eval()` will be treated as if it was authored in that function, modifying it's lexical scope.


### Functions as scope
* Each function declares it's own scope.
* Function scope encourages the idea that all variables belong to the function and can be used throughout the function (and nested scopes).
* By simply wrapping some code in a function, you can hide the code and access to it's declarations. This conforms to the 'Principle of Least Privilege' - that an api should only show what is necessary as a minimum and hide the rest.
* It's not always ideal to use a function to create scope - it creates a named function (polluting the scope the function is defined in), and the function must be called for the code to run.
* IIFE (immediately invoked function expression) fixes the above two problems - by wrapping a function in `(function(){})()`, the scope is contained without creating a named variable, and the function is immediately called.
* While anonymous functions can be useful, they have some drawbacks:
	* no name to display in stark traces
	* recursion is not possible without an identifying name to call itself
	* readability of code is diminished

### Blocks as scopes
* Block scoping used to only exist in a few cases:
	* `with` although frowned upon, when used creates a scope that only exists within the `with` statement.
	* `try/catch` creates a block scope within the `catch` clause.
* With the introduction of `let`, block scoping is now more accessible. `let` attaches the variable to the scope of whatever block it is contained in.
* Let statements are not hoisted in their scope (see below).


### Hoisting
* As all declarations are processed first when being compiled, all declarations can be considered 'hoisted' (moved) to the top of the code.
* For example `var a = 2` will be split into `var a` and `a = 2`, with `var a` being executed during the first stage of compilation, and the value assigned during the second stage.
*  While function declarations (`var a = function()[]`) have only their declaration (`var a`) hoisted, function expressions (`function a(){}`) are hoisted as a complete function.

### Closures
* Dictionary definition: closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.
* As an example:
	* We define a function `foo` that contains a nested function `bar`. When invoked, `foo` returns `bar`.
	* We create a variable `baz` which is equal to `foo()` (note that foo is invoked). `baz` becomes a reference to `bar`, which is originally declared in the lexical scope of `foo`.
	* Since `foo` has been executed, we expect its inner scope to go away since garbage collection is used to free up memory. However this does not happen - the inner scope is still in use by `bar`.
	* `bar` has a lexical scope closure over `foo`, since that's where it was declared, which keeps the scope alive for `bar` to reference any time in the future.
	* The reference that `bar` has to the lexical scope it was defined in is called **closure**

### Loops and closure
* When creating a loop, such as `for (var i=1; 1<=5; i++) {}`, any references to the `i` variable in each loop iteration will actually be references to the *same* variable each time. A new variable `i` is not created each time the loop runs.
* To get over the above problem, `var` can be replaced with `let`. This is because `let` utilises not only block scoping, but iteration block scoping, meaning a new variable is declared for each iteration.

### Modules
* When creating a function with nested functions and scoped variables, we can return an object with key value references to the functions to create a module using the *Revealing Module* pattern.
* By invoking the module, and thus returning the module object (eg. `var foo = bar()`), we create an instance of the module. We now have access to the modules functions that feature in the returned object (`foo.functionA()`) but access to the scoped variables remains private. The object return value is the public API for the module.
* To create a singleton module, we simply wrap the module function in an IIFE and assign its return value to the variable holding our singleton instance.
* Modules require two key characteristics: 1) an outer wrapping function being invoked, to create the enclosing scope 2) the return value of the wrapping function must include reference to at least one inner function that then has closure over the private inner scope of the wrapper.
