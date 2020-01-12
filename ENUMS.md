# Enums
Enums are a way of creating a fixed set of tags and the type of data those tags can represent. This allows for pattern matching. Enums allow you to store tagged data in a way that allows you to functionally match patterns against them as a form of control flow.
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

When directly calling an enum constant, you must complete it. If it is holding some kind of data, then give it the right data to hold.

```ruby
def fun sendTextMessage(user: @UserExistsInDatabase(database) User, message: String): Result<Unit>{
    let sendResult = sendMessage(user, Message.Write(message)) //We are giving Message.Write the string object `message` to hold
}

def fun sendMessage(user: @UserOnline(database) User, message: Message): Result<Unit>{
    match(message){
        Quit -> {
            //Log user off server and quit the application
        }
        Move(m) -> {
            def val (x, y) = m
            //Do stuff with x and y
        }
        Write(mStr) -> {
            let sendResult = someNetworkingApiInstance.sendEncodedMessage(user.ipAddress, mStr)
            match(sendResult){
                Ok() -> {
                    return Ok()
                }
                Error(err) -> {
                    let errorMessage = StringBuilder()
                    errorMessage.append("An error occurred while trying to send encoded string message:\n\t")
                    errorMessage.append(err)
                    return Error(errorMessage.toString())
                }
            }
        }
        ChangeColor(r, g, b) -> {
            let newColor = Color(r, g, b)
            //Do stuff with color
        }
    }
}
```