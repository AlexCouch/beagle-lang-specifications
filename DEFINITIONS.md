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

#### Defining a basic property
```
def five = 5
```

#### Defining a basic function
```
def fun myFunction(){
	//Do stuff
}
```
#### Defining a basic class
```
def class A
```
#### Defining basic data structure
```
def data Vector(x: Int, y: Int)
```
Data structures require some kind of data to hold, unlike classes

#### Defining a constant
```
def const five = 5
```

#### Defining a basic enum
```
def enum States{
	FIRST_STATE,
	SECOND_STATE,
	//So on
}
```
Like data structures, enums require something to be defined. You cannot have an empty enum.

### Defining a basic trait
```
def trait SomeTrait{
	def five: Int
}
```
Like data structures and enums, traits require something to be implemented. There will never be an empty trait. This prevents counterintuition.

### Defining an interface
```
def interface B
```
Interfaces are class implementation protocols/templates, and thus act like abstract classes, just without concrete implementations like abstract classes do. This means you can have an empty interface just like you can have an empty class.