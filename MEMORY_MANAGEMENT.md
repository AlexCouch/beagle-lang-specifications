# Memory Management
Beagle will have a form of manual memory management system, where the way you write code will tell the compiler how to generate memory and object management code by following a few rules, with no exceptions.

### Rules to Beagle Memory Management
Beagle will have 3 very simple rules to memory management.

1. Properties are always heap allocated. 
2. Local variables are always stack allocated
3. Properties can be referenced but local variables cannot.

### Reference Counting and Memory Cleanup
Beagle will have a memory management code generator which will use reference counting for its automatic memory manager, however, its completely dependent upon the code written. Reference counting will ensure that when a heap allocated object's declaring scope ends, and there are no more references to this object, it gets dropped from memory. This will mean that classes and structs will have dropper overloads, by declaring a `drop` block much like an `init` block for the initializer. Local variables will be treated as *pass-by-value*, and when they go out of scope, it gets dropped from memory.

#### Local Variables: Move and Copy
Local variables, as mentioned briefly will only be allowed to be moved or copied. When a local variable is passed into a function, it will be either moved or copied depending upon its reference counting. If it is 'referenced' more than once, it will be copied.

```js
//This will result in a move operation only
let something = SomeObject()
funCall1(something) //Moved

//This will result in a copy operation
let something = SomeObject()
funCall1(something) //First copy
...
funCall2(something) //Second copy
```

This will also apply to return statements.
```js
let something = SomeObject()
funCall1(something) //First copy
return something //Second copy, all remaining copies are dropped
```

#### Local Properties: References, Move, and Copy
Local properties can be created the same way that non-local properties are created: *val/var*. When done locally, the same rules apply. They can be referenced like non-local properties, and as opposed to local variables, they will never be moved or copied unless you tell the compiler to do so, using the following keywords:

* `ref`
    - Creates a reference to the following object
    - `let someRef = ref someProperty`
* `move`
    - Moves the object into the ownership of the specified receiver. See [ownership](#OWNERSHIP).
    - `funCall(move someProperty)`
* `copy`
    - Creates a copy of the object which is then given to the specified receiver. See [ownership](#OWNERSHIP)

Local properties, just like non-local properties, will have generated getters and setters, which can be modified at its definition site.
```kt
fun funCall: SomeObject{
    val someLocalProperty = SomeObject() //Immutable object, with an overloaded getter
        get(){
            //Stuff
            return field
        }
    //Do stuff
    return someLocalProperty //Return a reference to 'someLocalProperty'; this is not moved!
}
```

Properties can be explicitly referenced using the keyword `ref`. This can also be used in an object's type annotation to explicitly specify a reference, which will prevent local variables from being given.
```kt
class A{
    fun saySomething{
        println("Hello")
    }
}

fun funCall(ref a: A){
    println(a.saySomething)
}

val a = A()

fun main{
    let b = B()
    funCall(ref a)
    funCall(b) //Error: Local variables cannot be reference.
}
```

### Ownership
Beagle's ownership mechanism is much kinder and simpler than other ownership mechnisms (such as Rust's borrow checker). It will ensure that objects are not moved when they shouldn't be. Local variables will be moved and copied automatically, however, the `move` and `copy` keywords on properties will make sure that the object has a valid receiver and that the object will be moved into the ownership of the receiver.

#### Ownership Receiver
An *ownership receiver* is a scope that is obtaining ownership of an object. This could be a function scope, an object scope (classes, structs, etc), a thread scope, a coroutine scope, etc. When using `move` or `copy` on an object while being passed into a function call argument, the copied object will be moved into that function. So when that called function goes out of scope, that copied object will then be dropped. This applies to all other ownership receivers.
```kt
class A(val name: String){
    fun saySomething{
        println("[$name] Hello")
    }
}

def thread MyThread(val a: A){
    start{
        a.saySomething
    }
}

val a = A("alex")
fun main(){
    let myThread = MyThread(copy a)
    let result = myThread.start()
    match(result){
        OkResult -> ...
        ErrorResult -> ...
    }
    a.saySomething
}
```