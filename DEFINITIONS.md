# Definitions
There are several different kinds of definitions. A definition is any statement that declares the existence of some construct.
Constructs include but not limited to:
1. [Properties](PROPERTIES.md)
2. [Functions](FUNCTIONS.md)
3. [Classes](CLASSES.md)
4. [Structs](STRUCTS.md)
5. [Constants](CONSTANTS.md)
6. [Enums](ENUMS.md)
7. [Traits](TRAITS.md)
8. [Interface](INTERFACES.md)
9. [Abstract Types](ABSTRACT_TYPES.md)
10. [Modules](MODULES.md)
11. [Aliases](ALIASES.md)
12. [Macros](MACROS.md)
13. [Def Macros](DEF_MACROS.md)
14. [Rules](RULES.md)
15. [References](REFERENCES.md)

#### Defining a property
```ruby
//Immutable property
def val five = 5
//Mutable property
def var six = 6
```

#### Defining a function
```ruby
def fun myFunction(){
    //Do stuff
}
```
#### Defining a class
```ruby
def class A
```
#### Defining a struct
```ruby
def struct Vector(x: Int, y: Int)
```

#### Defining a constant
```ruby
def const five = 5
```

#### Defining an enum
```ruby
def enum States{
    FIRST_STATE,
    SECOND_STATE,
    //So on
}
```
Like structs, enums require something to be defined. You cannot have an empty enum.

#### Defining a trait
```ruby
def trait SomeTrait{
    def five: Int
}
```
Like structs and enums, traits require something to be implemented. There will never be an empty trait. This prevents counterintuition.

#### Defining an interface
```ruby
def interface B
```
Interfaces are class implementation protocols/templates, and thus act like abstract classes, just without concrete implementations like abstract classes do. This means you can have an empty interface just like you can have an empty class.
#### Defining a module
```ruby
def mod M
```
Similar to rust's modules, this allows you to have module only definitions such as module properties, module functions, etc. This is a form of encapsulation, where members of a module can only be called if declared public. The `internal` keyword can be used to define something that can only be accessed from inside the declaring module. Anything defined in this module, including all submodules, have access to all its definitions.

#### Defining A Reference
Defining a [reference](REFERENCES.md#Basic-References) can be done just like a property except for the use of the `ref` keyword.

```ruby
def class A{
    //Stuff
}
//...
def a = A()
def myRef = ref a
```

#### Defining A Rule
Defining a [data validation rule](RULES.md) is very simple using the `rule` keyword.
```ruby
def class A
def class B
def rule MyRule<A, B>(someRuleParam: Int){
    //Do stuff
}
def someFunction(a: @MyRule A, b: @MyRule B): Result<Unit>{
    //Do stuff
}
```

#### Defining A Macro
```ruby
def macro myMacro{
    //Macros are still a WIP construct
}
```
Macros are currently a WIP construct. For a reference on how macros may work in this language, please reference the [Crystial documentation](https://crystal-lang.org/reference/syntax_and_semantics/macros.html).

#### Defining a Def-Macro
```ruby
def defmacro myDefMacro{
    //DefMacros are still a WIP construct
}
```
Def Macros are currently a WIP construct. The idea behind a Def Macro is to be able to create a commonly defined sub-construct in an API, such as views, entities, items, models, etc. This way, the definition of common sub-constructs can be simplified and customized internally by the API.
```ruby
def defmacro entity{
    //Define entity defmacro here
}

def entity MyEntity{
    //Define MyEntity stuff here
}

def val myEntity = MyEntity()
```

#### Defining an Alias
Aliases can be used to simplify the use of some identifier like a type name, or the use of some function call. It can also be used to shorten a generic use case, or function type.
```ruby
def alias MyFunctionTypeAlias = (String) -> Int
def alias PlayerRegistry = HashMap<String, PlayerEntity>

def fun someHigherOrderFunctionIGuess(block: MyFunctionTypeAlias){
    //Do stuff
}

def class GameServer{
    def val playerRegistry = PlayerRegistry()
}
```