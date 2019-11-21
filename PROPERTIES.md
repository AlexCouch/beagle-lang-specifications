# Properties
Properties can be defined in several different contexts for several different purposes. Properties are different than variables, which are stack allocated and are only locally defined. Properties generate a getter and setter, just like C#, Scala, and Kotlin.

### Basic Properties
Properties, by default, are heap-allocated and type inferred. When defined top-level, it becomes a ["program property"](#Program-Properties).
Properties can be defined with type annotations as well, but types are generally inferred by the compiler
```
def five: Int = 5
```
Properties can be top-level, class-level, or local-level
```
//Top level property
def five = 5

def class A{
	//Class level property
	def string = "Hello world"
}

def fun myFunction{
	//Local level property
	def someFlag = true
}
```

## Program Properties
Program properties are equivalent to C++'s static variables in the global namespace (not to be confused with [constants](CONSTANTS.md))