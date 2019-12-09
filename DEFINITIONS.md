Side note: Comments follow this syntax: `//Comment here`

# Definitions
There are several different kinds of definitions in this hypothetical language. A definition is any statement that declares the statement of some construct.
Constructs include but not limited to:
1. [Properties](PROPERTIES.md)
2. [Functions](FUNCTIONS.md)
3. [Classes](CLASSES.md)
4. [Structs](STRUCTS.md)
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
#### Defining a struct
```
def data Vector(x: Int, y: Int)
```
Structs require some kind of data to hold, unlike classes

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
Like structs, enums require something to be defined. You cannot have an empty enum.

#### Defining a trait
```
def trait SomeTrait{
    def five: Int
}
```
Like structs and enums, traits require something to be implemented. There will never be an empty trait. This prevents counterintuition.

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

#### Defining A Reference
Defining a [reference](REFERENCES.md#Basic-References) can be done just like a property except for the use of the `ref` keyword.

```
def class A{
    //Stuff
}
//...
def a = A()
def myRef = ref a
```
