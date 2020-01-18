# Possible Features
Any possible features that I am not sure about will go here.

### Defer Statements
Defer statements are a way of deferring a statement until the end of the current scope. Considering that Beagle uses RAII, it might not really be necessary.

### Blanket Implementations
Blanket implementations are a way of implementing abstractions for any type T polymorphically.

```ruby
def trait SomeTrait{
	//Some trait stuff
}

def impl<T> SomeTrait for T{
	//Some trait stuff
}

def interface SomeInterface{
	//Some interface stuff
}

def impl<T> SomeInterface for T{
	//Some interface stuff
}
```
