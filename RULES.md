# Rules
Rules can be defined on certain types to allow type annotations to comes with data validation rules. Any type can have any rule applied onto it as long as the type definition allows it.

The definition of a rule can be declared with input parameters for which to base validation on. In the case of checking a size/length based object (such as a Collection, Array, String, etc) against some bound can be done this way.
```ruby
def rule LenGreaterThan<Sized>(size: Int){
    if(it.size < size){
        return Error("A minimum size of $size is required, but ${it.size} was found.")
    }
}

def fun someOtherFunction(name: @LenGreaterThan(5) String): Result<Unit>{
    //Do something
}
```
This internally expands out to a regular generic function whose call is injected to the top of the calling function's body
```ruby
def fun <T> T.rule_LenGreaterThan(size: Int): RuleResult where T : Sized{
    if(this.size < size){
        return RuleResult.Error("A minimum size of $size is required, but ${it.size} was found.")
    }
    return RuleResult.Ok()
}

def fun someOtherFunction(name: String): Result<Unit>{
    let ruleCheck_LenGreaterThanResult = name.rule_LenGreaterThan(5)
    match(ruleCheck_LenGreaterThanResult){
        Ok() -> {
            //Rest of function body here
        }
        Error(message) -> {
            let errorMessage = StringBuilder()
            errorMessage.append("Data validation failed while checking LenGreaterThan on String object `name`:\n\t")
            errorMessage.append(message)
            return Result.Error(errorMessage.toString())
        }
    }
}
```
To tell the Beagle compiler to allow a type to be validated against defined rules, you use a [`using` clause](USING_CLAUSES.md#Using-Rules).
```ruby
def class A: Sized{
    def size: UInt32 get(){
        //Some kind of size calculation algorithm here
        return field
    }
}
```
On-site usage of a data validation rule is done in the type annotation.
```ruby
def fun createUser(username: @LenGreaterThan(5) String): Result<User>{
    //Create username
}
def val createUsernameResult = createUsername("alex")
match(createUsernameResult){
    Ok(user) -> {
        //Do something with the new user object
    }
    Error(message) -> {
        critical("An error occurred while creating username:\n\t")
        critical(message)
    }
}
```

#### Rule Chains
Specifying a rulechain allows you to create an alias for a chain of rules to be executed in a certain order.

```ruby
def rule RuleA(someInt: Int){
    //Do some stuff
}

//This rule is done before RuleA
def rule RuleB(someString: String){
    //Do some stuff
}

//RuleB is done before RuleC
def rule RuleC(someStringIn: String){
    //Do stuff
}

def rule RuleD(something: Float){
    //Do stuff
}

def rule RuleE(anotherThing: Float){
    //Do stuff
}

def rulechain AThroughC(startingData: Int) = RuleA(startingData) -> RuleB(startingData.toString()) -> RuleC(startingData.toString())
```
