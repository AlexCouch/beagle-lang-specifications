# Functions
Functions can be defined in any context, including, but not limited to:
1. [Classes](CLASSES.md)
2. [Data structures](DATA_STRUCTURES.md)
3. [Enums](ENUMS.md)
4. [Interfaces](INTERFACES.md)
5. [Local](#Local-Functions)
6. [Traits](TRAITS.md)

Functions can be defined in several different ways, including, but not limited to:
1. [Dynamic functions](DUCK_TYPING.md#Dynamic-Functions)
2. [Generating/yielding functions](COROUTINES.md#Generators)
3. [Scope functions](SCOPE_FUNCTIONS.md)
4. [Extension functions](EXTENSIONS.md#Extension-Functions)

Functions a defined using the soft keyword **fun**.

#### Basic Functions
Functions with empty parameters can omit their parameters
```
def fun myFunction{
    //Do stuff
}
```
Functions can be type annotated and return a value. By default, they return *Unit*, which is the same as C's *void* type.
```
def fun myFunction: Int{
    return 5
}
```
Functions can take parameters, but they have to be type annotated
```
def fun myFunction(five: Int, string: String){
    //Do stuff
}
```

#### Function References
You can create a function reference by [nesting its definition](DEFINITIONS.md#Definition-Nesting) inside a property definition.
```
def myFunctionReference = def fun myFunction(number: Int, string: String){
    //Do stuff
}
```
This is good for creating function callbacks. 

Function references can have their [primary identities](IDENTITIES.md#Identity-Order) omitted if their secondary identity (the identity of the reference) is not needed.
```
def myFunRefrence = def fun(number: Int, string: String){
    //Do stuff
}
```

#### Function Types
A function type is a way of specifying the structure of a function. This is exactly the same as in Kotlin.
```
def myFunType: (Int, String) -> Unit
```

#### Storing A Callback In A Class
Storing a function callback inside a class member property would generally be like this:
```
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
An [extension](EXTENSIONS.md) function is a function that extends a class or data structure.
```
def class A(){
    def string = "Hello world"
}

def fun A.printString(){
    println(this.string)
}
```

#### Local Functions
A local function is a function defined inside a function.
```
def fun outer(){
    def fun inner(){
        println("Hello world")
    }
    inner()
}
```

#### Member Functions
A member function is a function that is a member of either a [class](CLASSES.md), [enum](ENUMS.md), [interface](INTERFACE.md), [trait](TRAITS.md), [data structure](DATA_STRUCTURE.md), or [module](MODULES.md).
```
def class A{
    def aFunc(){
        //Do stuff
    }
}

def interface B{
    def bFunc()
}

def enum C{
    C1, C2, C3, C4;

    def cFunc(){
        //Do stuff
    }
}

def data D{
    def dFunc(){
        //Do stuff
    }
}

def trait E{
    def eFunc()
}

def mod F{
    def fFunc(){
        //Do stuff
    }
}
```

#### Pass By Reference
By default, everything is [pass by value](PASS_BY_REFERENCE_OR_VALUE.md). However, you can change that by specifying a reference to something in the function parameters using `def` and passing in a reference either implicitly or explicitly using the `ref` keyword.
```
def aRef = def a = A()
def fun myFun(def a: A){
    //Do stuff
}
myFun(ref a) //Explicit referencing
myFun(aRef) //Implicit referencing
```