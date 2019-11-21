Side note: Comments follow this syntax: `//Comment here`

# Definitions
There are several different kinds of definitions in this hypothetical language. A definition is any statement that declares the statement of some construct.
Constructs include but not limited to:
1. [Properties](PROPERTIES.md)
2. [Functions](FUNCTIONS.md)
3. [Classes](CLASSES.md)
4. [Data structures](DATA_STRUCTURES.md)
5. [Constants](CONSTANTS.md)
6. [Enums](ENUMS.md)
7. [Traits](TRAITS.md)
8. [Interface](INTERFACES.md)
9. [Modules](MODULES.md)

#### Definitions As Expressions
Every definition can be used as an expression to return a reference to the definition. This comes in all sorts of types of references, including but not limited to:

1. [Property Reference](REFERENCES.md#Property-References)
2. Class References, aka, [Singletons](SINGLETONS.md)
3. [Function References](REFERENCES.md#Function-References)
4. [Default enums](ENUMS.md#Default-Enums)

#### Defining a property
```
def five = 5
```

#### Defining a function
```
def fun myFunction(){
    //Do stuff
}
```
#### Defining a class
```
def class A
```
#### Defining a data structure
```
def data Vector(x: Int, y: Int)
```
Data structures require some kind of data to hold, unlike classes

#### Defining a constant
```
def const five = 5
```

#### Defining an enum
```
def enum States{
    FIRST_STATE,
    SECOND_STATE,
    //So on
}
```
Like data structures, enums require something to be defined. You cannot have an empty enum.

#### Defining a trait
```
def trait SomeTrait{
    def five: Int
}
```
Like data structures and enums, traits require something to be implemented. There will never be an empty trait. This prevents counterintuition.

#### Defining an interface
```
def interface B
```
Interfaces are class implementation protocols/templates, and thus act like abstract classes, just without concrete implementations like abstract classes do. This means you can have an empty interface just like you can have an empty class.
#### Defining a module
```
def mod M
```
Similar to rust's modules, this allows you to have module only definitions such as module properties, module functions. This is a form of encapsulation, similar to the `internal` keyword in Kotlin. Anything defined in this module, including all submodules, have access to all its definitions.

#### Defining a reference
Nesting any kind of definition inside of a property definition yields some kind of property value. In the case of functions, it yields a [function reference](FUNCTIONS.md#Function-References). In the case of classes it yields a [singleton](SINGLETONS.md). In the case of properties, it yields a *reference* to that defined object. This is almost endless. You can have a def inside a def inside a def inside a def.

```
def myRef1 = def myRef2 = def myRef3 = def fun someNestedFunctionDef = def class{
    //Stuff
}
```
This is absolutely convoluted, but this *in theory* should yield a reference to a reference to a reference to a function expression that immediately returns a singleton. Not at all practical but it encapsulates the way the compiler should work, and how extensible it should be with definitions.

Specifying a reference to some type can be done
```
def myRef: ref A = def a = A()
```

#### Defining None
You can define something as [None](INITIALIZATION.md#None) using the `None` type, which is not a primitive or keyword like it is in Python.
```
def a: A? = None
```
All classes are immediately subclassed by None, just like `Nothing` in Kotlin. As long as you provide some type annotation, you can make anything `None`, as long as you tell the compiler its okay, using [null safety](NULLABILITY.md).