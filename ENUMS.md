# Enums
Enums are way of creating constants that share the same type. This allows for pattern matching. Enums allow you to store like information in a way that allows you to distinguish between the variations of like data.
```ruby
def enum IpAddress{
    IPv4,
    IPv6
}
```
Here, `IPv4` and `IPv6` are two different variants of the same thing. You can't make new instances of these but you can change what values they represent. The data that is represented can vary from general unit representations to primitives such as Int, Float, String, to anonymous classes and structs.
```ruby
def enum IpAddress{
    IPv4(String),
    IPv6(String)
}
```
This example has two IpAddress constants that both represent a string object. This can then be accessed during pattern matching.
```ruby
def fun matchIpAddress(ipAddress: IpAddress){
    match(ipAddress){
        IPv4(ipStr) -> {
            println(ipStr)
        }
        IPv6(ipStr) -> {
            println(ipStr)
        }
    }
}
```

#### Advanced Enums
Enum constants don't have to represent one piece of data; they can have all sorts of data. Let's take messages for example:
```ruby
def enum Message{
    Quit,
    Move struct{
        def val x: Int32
        def val y: Int32
    },
    Write(String),
    ChangeColor(Int32, Int32, Int32)
}
```
* `Quit` is an empty unit, which means it contains no data whatsoever.
* `Move` stores an anonymous struct with members `x` and `y` both of which are Int32
* `Write` stores a String object
* `ChangeColor` stores four Int32 objects

They are all variations of the same type, as you can see. This is also really good for [pattern matching](CONTROL_FLOW.md#Pattern-Matching).

#### Pattern Matching
```ruby
def fun processMessage(message: Message){
    match(message){
        Quit -> quitProgram()
        Move(m) -> {
            movePlayer(m)
        }
        Write(s) -> {
            showMessage(s)
        }
        ChangeColor(r, g, b) -> {
            changeColor(r, g, b)
        }
    }
}
```