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
```ruby
def fun myFunction{
    //Do stuff
}
```
Functions can be type annotated and return a value. By default, they return *Unit*, which is the same as C's *void* type.
```ruby
def fun myFunction: Int{
    return 5
}
```
Functions can take parameters, but they have to be type annotated
```ruby
def fun myFunction(five: Int, string: String){
    //Do stuff
}
```
Functions can have default arguments.
```ruby
def fun myFunction(five: Int = 5, string: String){
    //Do stuff
}

myFunction("Hello world!") //No need to pass in an argument for 'five' parameter
```

#### Function Types
A function type is a way of specifying the structure of a function. This is exactly the same as in Kotlin.
```ruby
def val myFunType: (Int, String) -> Unit
```

#### Higher-Order Functions
A function can be *higher order* if at least one of its parameters is a *function type*.
```ruby
def myHigherOrderFun(callback: (String) -> Int){
    def lengthOfString = callback("Hello world") //Will return length of "Hello world!"
    println(lengthOfString) //Print the length of "Hello world!"
}

myHigherOrderFun{ string ->
    string.length() //Return string.length
}
```
A higher order function can receive a lambda function in one of three ways.
```ruby
def myHigherOrderFun(string: String = "Default string", callback: (String) -> Int){
    def lengthOfString = callback(string) //Will return length of string parameter
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

#### Storing A Callback In A Class
Storing a function callback inside a class member property would generally be like this:
```ruby
def class A{
    def funReference: ((Int, String) -> Unit)? = None

    def fun doSomething(){
        this.funReference(5, "Hello world")
    }
}

def myFunRef = def fun(number: Int, string: String){
    println(number)
    println(string)
}

def a = A()
a.funReference = myFunRef
//This should print 5 followed by "Hello world" on the next line
a.doSomething()
```

#### Extension Functions
An [extension](EXTENSIONS.md) function is a function that extends a class or struct.
```ruby
def class A(){
    def string = "Hello world"
}

def fun A.printString(){
    println(this.string)
}
```

#### Local Functions
A local function is a function defined inside a function.
```ruby
def fun outer(){
    def fun inner(){
        println("Hello world")
    }
    inner()
}
```

#### Member Functions
A member function is a function that is a member of either a [class](CLASSES.md), [enum](ENUMS.md), [interface](INTERFACE.md), [trait](TRAITS.md), [structs](STRUCTS.md), [abstract type](ABSTRACT_TYPES.md), or [module](MODULES.md).
```ruby
def class A{
    def fun aFunc{
        //Do stuff
    }
}

def interface B{
    def fun bFunc
}

def enum C{
    C1, C2, C3, C4;

    def fun cFunc{
        //Do stuff
    }
}

def struct D{
    def fun dFunc(){
        //Do stuff
    }
}

def trait E{
    def fun eFunc
}

def type F{
    def fun fFunc
}

def mod F{
    def fun fFunc(){
        //Do stuff
    }
}
```

#### Pass By Reference
By default, everything is [pass by value](MEMORY_MANAGEMENT.md#Pass-By-Reference). However, you can change that by specifying a reference to something in the function parameters using `ref` and passing in a reference either implicitly or explicitly using the `ref` keyword.
```ruby
def val a = A()
def val aRef = ref a
def fun myFun(ref a: A){
    //Do stuff
}
myFun(ref a) //Explicit referencing
myFun(aRef) //Implicit referencing
```