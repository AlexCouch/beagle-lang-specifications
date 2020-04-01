# Possible Features
Any possible features that I am not sure about will go here.

### Defer Statements
Defer statements are a way of deferring a statement until the end of the current scope. Considering that Beagle uses RAII, it might not really be necessary.

### Blanket Implementations
Blanket implementations are a way of implementing abstractions for any type T polymorphically.

```rust
trait SomeTrait{
	//Some trait stuff
}

impl<T> SomeTrait for T{
	//Some trait stuff
}

interface SomeInterface{
	//Some interface stuff
}

impl<T> SomeInterface for T{
	//Some interface stuff
}
```

### Sealed Classes
Sealed classes are a more RAII form of enumerable types, where you have fixed variations in a type hierarchy that behave similar to enums but unlike enums, they are instantiable. This is a feature in Kotlin but due to the planned mechanisms of enums, this may not be feasible.

```kt
sealed class ASTNode(open val name: String, open val position: Position){
    class RootNode: ASTNode("root-node", Position.ORIGIN)
    class VariableNode(override val name: String, override val position: Position, val assignedExpression: Expression<*>)
    sealed class Expression<T>(override val name: String, override val position: Position, open val data: T): ASTNode(name, position){
        data class IntegerLiteral(overide val data: Int, override val position: Position): Expression<Int>("intliteral", position, data)
        data class FloatLiteral(overide val data: Float, override val position: Position): Expression<Float>("floatliteral", position, data)
        data class StringLiteral(overide val data: String, override val position: Position): Expression<String>("strliteral", position, data)
    }
}
