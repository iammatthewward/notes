# JavaScript values, types, and operators
*Notes taken from studying [Eloquent JavaScript](http://eloquentjavascript.net/) chapter 1.*

## Values
* In a JavaScript environment, all **values** are made up of chunks of bits.
* There are 6 types of value in JavaScript:
    * [numbers](#numbers)
    * [strings](#strings)
    * [booleans](#booleans)
    * objects
    * functions
    * [undefined values](#undefined-values)

### Numbers
* Numbers, obviously, refer to number values.
* JavaScript uses 64 bits to store a number value. Some of these bits are used to store whether the number is positive or negative, and some bits are used to store the location of the decimal point, meaning the total amount of numbers that can be stored sits at around 9 quadrillion.
* Whole numbers (integers) are written as a number `10`.
* Fractional numbers (floats) are written with a dot `10.5`.
* Very large or small numbers can be written with scientific notation, using an exponent: `2.998e8` which is equal to `2.998 x 10^8  = 299,800,000,000`

#### Arithmetic
* Calculations with whole numbers smaller than 9 quadrillion are guaranteed to be accurate.
* Calculations with fractional numbers generally aren't as accurate, as 64 bits is not enough to always have the correct precision. This is only really a problem in very specific situations.
* There are three special numbers in JavaScript:
    * `Infinity` - represents infinity.
    * `-Infinity` - represents minus infinity.
    * `NaN` - stands for Not a Number.

##### Arithmetic operators
* When performing arithmetic on numbers, symbols such as `+`, `-`, and `*` are called **operators**:
    * `+` - add
    * `-` - subtract
    * `*` - multiply
    * `/` - divide
    * `%` - remainder
* When multiple operators are used, the order of operations is determined by the precedence of the operators. This isn't a concern, and parentheses can be used to ensure certain calculations are performed first.

### Strings
* Strings are used to represent text, and are written surrounded by either single or double quotes: `'I am a string'` `"I am also a string"`.
* Most characters can be in a string, however some must be escaped to be properly displayed in the string:
    * `\n` - creates a new line.
    * `\t` - creates a tab.
    * `\'` - escapes a single quote.
    * `\"` - escapes a double quote.
    * `\\` - escapes a backslash.
* Arithmetic operators cannot be used on strings, with the exception of the `+` operator, which concatenates strings together.

### Booleans
* Booleans have just two values - `true` and `false`.
* Booleans can be created by using comparison operators to compare two values against each other and return true or false depending on if the comparison is true or not.
* The comparison operators are:
    * `>` - greater than.
    * `<` - less than.
    * `>=` - greater than or equal to.
    * `<=` - less than or equal to.
    * `==` - equal to.
    * `!=` - not equal to.
    * `===` - specific equal to, also comparing the type of the value.
* There is only one value in JavaScript that is not equal to itself, and that is NaN , which stands for “not a number”.

#### Logical operators
* Logical operators can be applied to booleans themselves, and three exist:
    * `&&` - the and operator, which will only return true if *both* values are true. Any other situations will return false.
    * `||` - the or operator, which will return true if *either* value is true.
    * `!` - the not operator, which will flip the value of a boolean, making true false and false true.
* The `||` operator will return the value to its left when that can be converted to true and will return the value on its right otherwise:
    * `null || "user" => "user"`
    * `"Karl" || "user" => "Karl"`
* This allows the `||` to be used as a fallback operator, specifying a default value to return if the first argument does not evaluate to true.

#### Ternary operator
* The final type of operator to be used with booleans is the ternary operator, which is a conditional operator. This takes two a value that evaluates to a boolean, and returns the value to the left of the colon if true, and the value to the right of the colon if false:
```
true ? 1 : 2 => 1

false ? 1 : 2 => 2
```

### Undefined values
* `null` and `undefined` are used to denote the absence of a meaningful value.

### Automatic type conversion
* When an operator is applied to the wrong type of value, JavaScript will try to coerce it to the correct type - this is called **type coercion**.



### Unary operators
* Some operators are written as words, rather than symbols, such as `typeof` which produces a string value naming the type of value it is given: `typeof 4.5 => number`.
* `typeof` is a unary operator, taking only one value as an argument.
* The minus operator `-` can be used as a binary or unary operator, and when used on a single number will convert the number to a negative value.
