# Functions
Functions can be defined in any context, including, but not limited to:
1. [Classes](CLASSES.md)
2. [Structs](STRUCTS.md)
3. [Enums](ENUMS.md)
4. [Interfaces](INTERFACES.md)
5. [Local](#Local-Functions)
6. [Traits](TRAITS.md)
7. [Abstract Types](ABSTRACT_TYPES.md)
8. [Modules](MODULES.md)
9. [Top-Level](FUNCTIONS.md)

Functions a defined using the soft keyword **fun**.

#### Basic Functions
Functions with empty parameters can omit their parameters
```kt
fun myFunction{
    //Do stuff
}
```
Functions can be type annotated and return a value. By default, they return *Unit*, which is the same as C's *void* type.
```kt
fun myFunction: Int{
    return 5
}
```
Functions can take parameters, but they have to be type annotated
```kt
fun myFunction(five: Int, string: String){
    //Do stuff
}
```
Functions can have default arguments.
```kt
fun myFunction(five: Int = 5, string: String){
    //Do stuff
}

myFunction("Hello world!") //No need to pass in an argument for 'five' parameter
```

#### Function Types
A function type is a way of specifying the structure of a function. This is exactly the same as in Kotlin.
```kt
val myFunType: (Int, String) -> Unit
```

#### Higher-Order Functions
A function can be *higher order* if at least one of its parameters is a *function type*.
```kt
myHigherOrderFun(callback: (String) -> Int){
    let lengthOfString = callback("Hello world") //Will return length of "Hello world!"
    println(lengthOfString) //Print the length of "Hello world!"
}

myHigherOrderFun{ string ->
    string.length() //Return string.length
}
```
A higher order function can receive a lambda function in one of three ways.
```kt
myHigherOrderFun(string: String = "Default string", callback: (String) -> Int){
    let lengthOfString = callback(string) //Will return length of string parameter
    println(lengthOfString) //Print the length of string parameter
}

//Enclosed by function call parentheses
myHigherOrderFun("Hello world!", {string -> 
    string.length()
})

//Outside function parentheses
myhigherOrderFun("Hello world!"){
    string.length()
}

//Without function parentheses if no other arguments passed
myHigherOrderFun{
    string.length()
}
```

#### Extension Functions
An [extension](EXTENSIONS.md) function is a function that extends a class or struct.
```kt
class A(){
    val string = "Hello world"
}

fun A.printString(){
    println(this.string)
}
```

#### Local Functions
A local function is a function defined inside a function.
```kt
fun outer(){
    fun inner(){
        println("Hello world")
    }
    inner()
}
```

#### Member Functions
A member function is a function that is a member of either a [class](CLASSES.md), [enum](ENUMS.md), [interface](INTERFACE.md), [trait](TRAITS.md), [structs](STRUCTS.md), [abstract type](ABSTRACT_TYPES.md), or [module](MODULES.md).
```kt
class A{
    fun aFunc{
        //Do stuff
    }
}

interface B{
    fun bFunc
}

enum C{
    C1, C2, C3, C4;

    fun cFunc{
        //Do stuff
    }
}

struct D{
    fun dFunc(){
        //Do stuff
    }
}

trait E{
    fun eFunc
}

type F{
    fun fFunc
}

mod F{
    fun fFunc(){
        //Do stuff
    }
}
```

#### Pass By Reference
By default, everything is [pass by value](MEMORY_MANAGEMENT.md#Pass-By-Reference). However, you can change that by specifying a reference to something in the function parameters using `ref` and passing in a reference either implicitly or explicitly using the `ref` keyword.
```rust
val a = A()
val aRef = ref a
fun myFun(ref a: A){
    //Do stuff
}
myFun(ref a) //Explicit referencing
myFun(aRef) //Implicit referencing
```