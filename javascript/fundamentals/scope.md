# JavaScript Scope

*Notes from [You Don't Know JavaScript book series]('https://github.com/getify/You-Dont-Know-JS/')*

## JavaScript Compilation
* JS is usually compiled immediately before runtime
* Usually when a piece of code is compiled it goes through three broad steps:
    * Tokenizing/Lexing: breaking up a string of characters into meaningful chunks of code
        eg. `var a = 2;` is broken up into `var`,`a`,`=`,`2`,`;` (whitespace may be tokenized if it is meaningful)
    * Parsing: taking the tokenized chunks and forming them into an **A**bstract **S**yntax **T**ree to represent the structure of the program
        eg. `var a = 2;` might form the tree: 
        
        ```
        VariableDeclaration
            -> Identifier (value: a)
            -> AssignmentExpression
                -> NumericLiteral (value: 2)
       ``` 
    * Code-Generation: turning the **AST** and turning it into code that can be executed, for example taking the above tree and creating a memory slot for the variable `a`, then assigning the value `2` so the variable can be called upon and return it's value.
* Much more happens than just these three steps, but they represent a broad summary of the compilation process
* As JS usually compiles immediately before invocation, it isn't able to perform optimisations as well as other traditionally compiled languages are
