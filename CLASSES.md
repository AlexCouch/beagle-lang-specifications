# Classes
A class can be defined with the following syntax:
```
def class A
```
A class is used to define a custom data type that follows the *class inheritance* type system. This allows polymorphic programming, generic programming, boxing/autoboxing, and type checking/casting.

#### Constructors
A class can define a primary constructor, which by default is preceeded by the class identifier.
```
def class A()
```
A constructor can define *[constructor properties](PROPERTIES.md#Constructor-Properties)* or just constructor parameters, which are only accessible from within the constructor body.
```
def class A(def number: Int, string: String){
    init{
        println(number) //Valid
        println(string) //Valid
    }

    def fun myClassFun{
        println(this.number) //Valid
        println(this.string) //Invalid; Undefined reference
    }
}
```

A constructor body is declared differently based on the type of constructor: primary or non-primary.
```
def class A(string: String //Primary constructor){
    init{
        //Primary constructor body
        println(string) //Valid
    }

    constructor(number: Int) : this(str(number)){
        println(string) //Invalid; Undefined reference
        println(number) //Valid
    }
}
```
A non-primary constructor can be defined like above. There is no limit on these non-primary constructors, as long as a primary constructor exists, and that primary construct is called by any non-primary constructor.

#### Class Deconstructors
A class can have a deconstructor, which is invoked when the [smart memory manager](MEMORY_MANAGEMENT.md) tries to deconstruct and deallocate the class instance. This is good for calling cleanup functions that your class may need to do at the end of its life.
```
def class A{
    init{
        //Startup
    }

    deinit{
        //cleanup
    }
}
```

### Class Members
Classes can have members that are either uniquely defined in the classes, overrides of a parent class, or a concrete implementation of an abstract member.

#### Class Member Properties
A class can have member [properties](PROPERTIES.md) just the same way that you declare any other property. This property only exists within the scope of this class instance.
```
def class A{
    def string = "Hello world"
}
```

A class can override a parent class's member property as long as it has the appropriate [access](#Encapsulation).
```
def open class A{
    def open string = "Hello world!"
}

def class B : A(){
    override def string = "Goodbye cruel world!"
}
```

#### Class Member Functions
A class can have member [functions](FUNCTIONS.md) just the same way that you declare any other function. This function only exists within the scope of this class instance.
```
def class A{
    def fun aFun{
        //Do stuff
    }
}
```

Class member functions are final by default, so allowing child classes to override a member function, the ``open`` keyword is required.

```
def open class A{
    def open fun aFun{
        //Do stuff
    }
}

def class B : A(){
    override def aFun{
        //Do stuff
    }
}
```

### Encapsulation
Classes can have encapsulated members, in that, they can be defined to be only accessible within certain scopes. All members are *public* by default, however, defining a member with *protected*, *internal*, or *private* allows different levels of encapsulation on that member.

#### Private Members
Private members are only ever accessible from within the containing class instance. This means that nothing can call this member from outside the instance.
```
def class A{
    def number = 5
    private def string = "Hello world"

    def fun aFun{
        println(this.number) //Valid
        println(this.string) //Valid
    }
}

def a = A()
println(a.number) //Valid
println(a.string) //Invalid; private class member access attempted
```

#### Internal Members
Internal members are only ever accessible from within the containing [module](MODULES.md), and all submodules. This means that anything outside this class, albeit, in the same module, has access to this member.
```
def mod A{
    def class B{
        def number = 5
        internal string = "Hello world"
    }

    def fun aFun(b: B){
        println(b.number) //Valid
        println(b.string) //Valid
    }
}

//Somewhere else

import A
def b = B()
aFun(b)
println(b.number) //Valid
println(b.string) //Invalid; module private class member access attempted
```

#### Protected Members
Protected members are only ever accessible from within its containing [type](ABSTRACT_TYPES.md) ([class](CLASSES.md), or [data structure](DATA_STRUCTURES.md)) and all subtypes (for classes, and types). Traits are not considered types but this also applies to trait members.
```
//Abstract type protected example
def type SomeType{
    protected def number: Int
    def fun doSomething
}

//Class protected implementation
def interface SomeInterface: SomeType{
    protected def flag: Boolean
}

def class A: SomeInterface{
    override def number: Int = 5
    override def flag = true
    def string = "This is a public class member"

    override def fun doSomething{
        println(this.number) //Valid
        println(this.flag) //Valid
        println(this.string) //Valid
    }
}

//Data structure protected example
def trait DataTrait{
    protected def flag: Boolean
}

def data B: SomeType, DataTrait{
    override def number: Int = 10
    override def flag = false
    def string = "This is a public data structure member"

    override def fun doSomething{
        println(this.number) //Valid
        println(this.flag) //Valid
    }
}

def a = A()
a.doSomething() //Valid
println(a.string) //Valid
println(a.flag) //Invalid; Protected member
println(a.number) //Invalid; Protected member
def b = B()
b.doSomething() //Valid
println(b.string) //Valid
println(b.flag) //Invalid; Protected member
println(b.number) //Invalid; Protected member
```

### Inheritance
Inheritance is a way of specifying a type hierarchy. This type hierarchy allows you to build types out of already existing types. These types can range from [abstract types](ABSTRACT_TYPES.md) to [class interfaces](INTERFACES.md). Classes use a Class Inheritance System (CIS) as opposed to [data structures](DATA_STRUCTURES.md) which use an Entity Component System (ECS).

Classes can inherit from [abstract types](ABSTRACT_TYPES.md) and [interfaces](INTERFACES.md). However, they can not inherit from [traits](TRAITS.md). Traits are not types, so therefore, non-inheritable, unlike with [data structures](DATA_STRUCTURES.md).
```
def type SomeType{
    def string = "Hello world"
}

def interface SomeInterface{
    def number: Int
}

def class SomeClass: SomeType, SomeInterface{
    override def number = 5
}

def sc = SomeClass()
println(sc.string) //Prints "Hello world"
println(sc.number) //Prints 5
```

Classes and all their members are final by default when in concrete implementations. [Interfaces](INTERFACES.md) and [abstract types](ABSTRACT_TYPES.md) are open by default, due to the logistics and technicalities of a non-concrete abstraction.

#### Calling Parent Specific Members
When calling a parent class specific member, use of the ``super`` keyword is required.
```
def open class A{
    def fun anotherFun{
        //Do stuff
    }
}

def class B : A(){
    def fun myFun{
        super.anotherFun()
        //Do stuff
    }
}
```

This can also be achieved using ``this`` but ``super`` denotes the parent class's implementation rather than an override in the containing class.

```
def open class A{
    def fun anotherFun{
        //Do stuff
    }

    def open fun myFun{
        this.anotherFun()
        //Do stuff
    }
}

def class B : A(){
    override def fun myFun{
        super.myFun()
        //Do stuff
    }
}
```

#### Override Properties
This also goes for properties.
```
def open class A{
    def open string = "Hello world!"
}

def class B : A(){
    override def string = "Goodbye cruel world!"
}
```
Override a property allows you to call a parent specific property getter or setter just like functions.
```
def open class A{
    def open string = "Hello world!"
}

def class B
```

### Companion Objects
Currently my thoughts on companion objects are split between explicit companion objects like in Kotlin (with or without the *object* keyword)
```
def class A{
    def companion object{
        def string = "Statically accessed member property"
        def doSomething{
            println("Statically accessed member function")
        }
    }
}

def a = A()
a.doSomething() //Invalid
println(a.string) //Invalid
A.doSomething() //Valid
println(A.string) //Valid
```
Or by using a [singleton](SINGLETON.md) pattern and using the intrinsic property member *companion* to set the companion object, which will be resolved by the compiler.
```
def class A{
    override def companion = def class{
        def string = "Statically accessed members property"
        def doSomething{
            println("Statically accessed member function")
        }
    }
}

def a = A()
a.doSomething() //Invalid
A.doSomething() //Valid
println(a.string) //Invalid
println(A.string) //Invalid
```
In Kotlin, data classes can be destructured by intrinsically being given *component* functions, which are mapped to each data class member property.
```Kotlin
data class A(val x: Int, val y: Int){
    //fun component1() = this.x
    //fun component2() = this.y
}

fun main(){
    val a = A(5, 10)
    val (x, y) = a
    println("x: $x")
    println(y: $y)
}
```
If we decide to do something similar with companion objects, it may be rather confusing even if we require a member property override. However, an explicit companion object definition may be more readable due to the explicit definition using the 
`object` keyword. However, due to the general semantics of `object` in Kotlin, it may not really make sense to have this as a keyword just for companion objects. Since [singletons](SINGLETONS.md) are defined using a 
*class reference*, it would make more sense to make companion objects an intrinsic class member property.

### Open Classes
An open class is a class that is open to overrides. This means you are not restricted to overriding abstract types and interfaces. By allowing concrete classes to be overridable, makes inheritance so much more flexible and powerful.
```
def open class A{
    def string = "Hello beautiful world"
}

def class B : A(){
    override def string = "Goodbye cruel world!"
}

def a = A()
def b = B()
println(a.string) //Prints "Hello beautiful world"
println(b.string) //Prints "Goodbye cruel world!"
```

### Abstract Classes
Sometimes you need a class to be abstract. Without an abstract class you're limited to having your abstractions be either [abstract types](ABSTRACT_TYPES.md) or [interfaces](INTERFACES.md).
```
def abstract class A{
    abstract def string: String
    def number = 5

    abstract def fun someAbstractFunction(number: Int)
}

def class B: A(){
    override def string: String = "Hello world!"

    override def fun someAbstractFunction(number: Int){
        this.string += number
        println(this.string)
    }
}
```
Using the `abstract` keyword allows you to set a class and any of its members to be abstract. Any inheriting classes must provide a concrete implementation of these members. The `abstract` keyword only applies to CIS types, like interfaces. This cannot be applied to [data structures](DATA_STRUCTURES.md), [traits](TRAITS.md), or [abstract types](ABSTRACT_TYPES.md). An abstract type is not the same as an abstract class.

Abstract classes, like [interfaces](INTERFACES.md), cannot be instantiated directly. They must be instantiated through an implementing class.

```
abstract def class A{
    abstract def string: String
}

def class B : A(){
    override def string = "Hello world!"
}

def a = A() //Invalid; attempt to instantiate abstract class
def b = B() //Valid
println(a.string) //Invalid; no concrete implementation of string
println(b.string) //Valid; string has a concrete implementation; prints "Hello world!"
```