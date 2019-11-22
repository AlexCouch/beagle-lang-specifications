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

#### Class Member Properties
A class can have member [properties](PROPERTIES.md) just the same way that you declare any other property. This property only exists within the scope of this class instance.
```
def class A{
    def string = "Hello world"
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
```

```
import A
def b = B()
aFun(b)
println(b.number) //Valid
println(b.string) //Invalid; module private class member access attempted
```
