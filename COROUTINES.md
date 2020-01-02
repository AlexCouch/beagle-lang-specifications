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
def struct MyTask with Task{
    def fun launch{
        //Do stuff on launch
    }
    def fun complete{
        //Do stuff on complete
    }
    def suspend fun cancel{
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
    def val messageQueue = Queue<Message>()

    def fun queueMessage(message: Message){
        this.messageQueue.push(message)
    }
}
```

#### Creating, Launching, and Canceling Threads
Creating a task object is the same as creating any other object, since tasks are expanded out to structs.
```ruby
def val myTask = MyTask(user)
```
Launching a task is done by calling `launch` on a task object, which returns a TaskResult
```ruby
def val myTask = MyTask(user)
def val launchResult = myTask.launch()
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
```ruby
def val myTask = MyTask(user)
def val cancelResult = myTask.cancel()
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

