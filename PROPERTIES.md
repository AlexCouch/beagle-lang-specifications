# Properties
Properties can be defined in several different contexts for several different purposes. Properties are different than [variables](VARIABLES.md), which are stack allocated and are only locally defined. Properties generate a getter and setter, just like C#, Scala, and Kotlin.

### Basic Properties
Properties, by default, are heap-allocated and type inferred. When defined top-level, it becomes a ["program property"](#Program-Properties). Properties can be defined with either `val` for immutable definitions or `var` for mutable definitions.
```ruby
def val immutableProperty = "This is an immutable property!"
def var mutableProperty = "This is a mutable property!"
```
Properties can be defined with type annotations as well, but types are generally inferred by the compiler
```ruby
def val immutableNumber: Int = 5
```
Properties come along with built in getters and setters and can be modified flexibly.
```ruby
def val number = 0
    get() = ...
    set(value){
        //Do stuff
    }
```
It also comes with a *[backing field](#Backing-Fields)*. These backing fields are accessed using the soft keyword *field*. This soft keyword only exists inside a property getter and setter.

#### Property Setters
Properties, as mentioned before, come with built in setters. Setters can be written just like any other function.
```ruby
def val number = 0
    set(value){
    	field = value
        //Append this number to the string property. So if I change number in this order: [0, 1, 2, 3], the string should then become '0123'
        string += field
    }
def val string = ""
```

The parameter of the setter can be named however you want. It doesn't have to be *value*. It can be *new*, *next*, *num*, *anything*. Just not a hard keyword.

#### Property Getters
Property getters sure are fun cause they can be defined as *[function expressions](FUNCTIONS.md#FUNCTION-EXPRESSIONS)*, or like regular functions. This makes defining a getter very easy and simple.

```ruby
def val number = 0
    get() = string.length()
def val string = "Hello world"
```
Or like a regular function:
```ruby
def val number = 0
    get(){
        println(string)
        return string.length()
    }
def val string = "Hello world"
```
And just like functions, it requires a [return statement](FUNCTIONS.md#RETURN-STATEMENTS).

#### Backing Fields
Backing fields are a way of non-recursively calling the current value of a property while in its getter/setter. This is useful because, if you were to call a property the usual way from within its own getter, you are then recursively calling that same getter, then causing a *stack overflow*.

### Program Properties
[Program properties](PROGRAM_PROPERTIES.md) are equivalent to C++'s static variables in the global namespace (not to be confused with [constants](CONSTANTS.md))
Program properties allow you to keep track of the state of the program. Program properties, just like any other properties, have getters and setters. Program properties are mostly used in the context of compiler plugins (a form of introspection) and through the use of an embedded interpreter (loading a beagle script at runtime and getting all of its program properties).
**NOTE: This may not stay as a feature for too long as top-level properties are colliquial with program properties. I originally had ideas for the use of program properties but now I'm beginning to doubt its potential.**

### Object Member Properties
There are two types of [object](OBJECTS.md) member properties: class member properties, and struct member properties. [Class](CLASSES.md) member properties are property members of a class. They only exist within the context of a containing class, similar to a data property. A struct member property is a property member of a [struct](STRUCTS.md). Both of these are essentially the same. The difference is the context. Both have getters, setters, and heap allocation.

#### Class Member Properties
```ruby
def class A{
    def val number = 0
}
```

#### Constructor Properties
Class constructors can declare constructor parameters that define a class member property. These parameters are called *constructor members*.
```ruby
def class A(def val string: String)
def struct B(def val num: Int)
```

#### Data Structure Member Properties
```ruby
def struct B{
    def val string = ""
}
```

#### Extension Properties
Extensions are explained in much more detail in [extensions document](EXTENSIONS.md#Extension-Properties).
```ruby
def class A{
    def val number = 5
}

def val A.string = "Hello world"

def val a = A()
println(a.number) //Valid
println(a.string) //Valid
```