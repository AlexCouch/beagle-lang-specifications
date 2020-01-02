# Threads
Threads can be defined using the `thread` keyword.
```ruby
def thread MyThread{
    //Do thread stuff
}
```
#### Basic Threads
Threads are provided two functions that can be implemented similar to `init` and `drop` in classes and structs, called `start` and `join` respectively.
```ruby
def thread MyThread{
    start{
        //Do stuff on start
    }
    join{
        //Do stuff on join
    }
}
```
Threads are expanded internally to classes
```ruby
def class MyThread: Thread{
    def fun start{
        //Do stuff on start
    }
    def suspend fun join{
        //Do stuff on start
    }
}
```
#### Thread Constructors, Start Parameters, and Join Parameters
Threads can be given constructors so that, upon being constructed (not to be confused with starting a thread), it requires arguments to be passed into it.
```ruby
def thread MyThread(private def val user: @UserExistsInDatabase(database) User)
```
This use case is also defining a constructor member property which is also possible with threads.

Threads can also be given *start parameters* and *join parameters*. These are parameters required in order to start or join this thread.
```ruby
def thread MyThread{
    start(user: @UserExistsInDatabase(database) User){
        //Do stuff upon start
    }
    join(message: Message){
        //Send message to server/client or something
    }
}
```

#### Thread Members
Since threads are expanded into classes internally, you can create members inside a thread just like a class
```ruby
def thread MyThread{
    def val messageQueue = Queue<Message>()

    def fun queueMessage(message: Message){
        this.messageQueue.push(message)
    }
}
```

#### Creating, Starting, and Joining Threads
Creating a thread object is the same as creating any other object, since threads are expanded out to classes.
```ruby
def val myThread = MyThread(user)
```
Starting a thread is done by calling `start` on a thread object, which returns a ThreadResult
```ruby
def val myThread = MyThread(user)
def val startResult = myThread.start()
match(startResult){
    Ok() -> {
        //Do stuff
    }
    Error(message) -> {
        critical("An error occurred while starting thread MyThread: \n\t")
        critical(message)
    }
}
```
Joining a thread is done by calling `join` on a thread object, which also returns a ThreadResult
```ruby
def val myThread = MyThread(user)
//Start thread
//Later on
def val joinResult = myThread.join()
match(joinResult){
    Ok() -> {
        //Do stuff after join
    }
    Error(message) -> {
        critical("An error occurred while joining thread MyThread: \n\t")
        critical(message)
    }
}
```
