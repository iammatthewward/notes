# Scala notes
Notes from [Modern Web Development With Scala](https://leanpub.com/modern-web-development-with-scala/read_sample)

## Values
* `val` defines a constant
* `var` defines a variable (rarely used)

* Constants and variables infer types if none is given
* Types can be explicitly stated after the const or var name:
		* `val myNum: Short = 4`

## Methods
* `def` defines a method
* Methods are defined with their arguments in parentheses, and types must be supplied for arguments. Supplying the type of the return value is optional. If supplied, it appears after the parentheses:

	```
	// No return type
	def greet(name: String) = "Hello " + name
	
	// Return type
	def greet(name: String): String = "Hello " + name
	```
	
* The body of a method can either be on the same line as the signature, or you can use a line break. Curly braces are not required:

	```
	def greet(name: String) =
			"Hello " + name
	```
* The last expression in a methods body will be it's return value - there is no need to use a return keyword.
* If a method is used in a procedural way and will not have a return value, you can specify its return type as `Unit`
* If methods do not have parameters, the parentheses can be omitted when defining the method. However this method will need to be called without parentheses too, so when calling this method it will look like you are accessing a value:

```
def id = Math.random

// This works
id => Double = 0.34234

// This does not
id() => Error to let you know this method does not accept parameters
```

* Functions can be defined and assigned to a variable or constant

```
// To specify the return type (a function from String to String)
val greetVar: String => String = (name) => "Hello " + name

// To infer the return type
val greetVar = (name: String) => "Hello " + name
```

* Methods can also be assigned to consts and vars by adding an underscore after the reference:

```
def greet(name: String): String = "Hello " + name
val sayHello = greet _

sayHello("Oreo") => "Hello Oreo"
```


## Type hierarchy
![Scala types](http://docs.scala-lang.org/tutorials/tour/unified-types-diagram.svg)
* Everything in Scala has a type, and inherits from `Any`: `Any => [AnyRef, AnyVal]`
* Primitives inherit from `AnyVal` and reference types inherit from `AnyRef`: `AnyRef => [List, Option, YourClass] AnyVal => [Double, Float, Boolean]`
* Types can be checked using `isInstanceOf`: `"Hello".isInstanceOf[String] => Boolean: true`
* Scala also has _bottom types_ which implicitly inherit from other types (more later)

### Collections
* Scala lists are created and accessed in the same way, using parentheses: 

```
val myList = List(1, 2, 3)
myList(0) => 1

val myArr = Array(1, 2, 3)
myArr(0) => 1
```

* Lists in Scala always have a type, and one will be inferred from the items in a list if no type is specified (if mixed types exist, the type will be `Any`
* A lists type can be supplied in-between the keyword `List` and the parentheses containing the list items: `val myList = List[Short](1, 2, 3)`
* By default, Scala uses immutable collections, which come with attached methods that create new collections rather than altering the existing collection. For example the `:+` method:

```
val listA = List(1, 2 ,3)
val listB = listA :+ 4

listA => List(1, 2, 3)
listB => List(1, 2, 3, 4)
```
* 
