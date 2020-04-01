# Tasks (Coroutines)
Tasks (or coroutines) can be defined using the `task` keyword.
```ruby
def task MyTask{
    //Task stuff
}
```

#### Basic Tasks
Tasks are provided three functions that can be implemented similar to `init` and `drop` in classes and structs, called `launch` and `cancel` respectively, along with `complete`.
```ruby
def task MyTask{
    launch{
        //Do stuff on launch
    }
    complete{
        //Do stuff on complete
    }
    cancel{
        //Do stuff on cancel
    }
}
```
Tasks are expanded internally to structs
```ruby
struct MyTask with Task{
    fun launch{
        //Do stuff on launch
    }
    fun complete{
        //Do stuff on complete
    }
    suspend fun cancel{
        //Do stuff on cancel
    }
}
```
#### Task Constructors, Start Parameters, and Join Parameters
Tasks can be given constructors so that, upon being constructed (not to be confused with starting a task), it requires arguments to be passed into it.
```ruby
def task MyTask(private def val user: @UserExistsInDatabase(database) User)
```
This use case is also defining a constructor member property which is also possible with tasks.

Tasks can also be given *launch parameters* and *cancel parameters*. These are parameters required in order to launch or cancel this task.
```ruby
def task MyTask{
    launch(user: @UserExistsInDatabase(database) User){
        //Do stuff upon start
    }
    cancel(message: Message){
        //Send message to server/client or something
    }
}
```

#### Task Members
Since tasks are expanded into structs internally, you can create members inside a task just like a struct
```ruby
def task MyTask{
    val messageQueue = Queue<Message>()

    fun queueMessage(message: Message){
        this.messageQueue.push(message)
    }
}
```

#### Creating, Launching, and Canceling Tasks
Creating a task object is the same as creating any other object, since tasks are expanded out to structs.
```kt
val myTask = MyTask(user)
```
Launching a task is done by calling `launch` on a task object, which returns a TaskResult
```kt
val myTask = MyTask(user)
val launchResult = myTask.launch()
match(startResult){
    Ok() -> {
        //Do stuff
    }
    Error(message) -> {
        critical("An error occurred while launching task myTask: \n\t")
        critical(message)
    }
}
```
Canceling a task is done by calling `cancel` on a task object, which also returns a TaskResult
```kt
val myTask = MyTask(user)
val cancelResult = myTask.cancel()
match(startResult){
    Ok() -> {
        //Do stuff
    }
    Error(message) -> {
        critical("An error occurred while canceling task myTask: \n\t")
        critical(message)
    }
}
```

