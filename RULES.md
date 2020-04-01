# Rules
Rules can be defined on certain types to allow type annotations to comes with data validation rules. Any type can have any rule applied onto it as long as the type definition allows it.

The definition of a rule can be declared with input parameters for which to base validation on. In the case of checking a size/length based object (such as a Collection, Array, String, etc) against some bound can be done this way.
```ruby
def rule LenGreaterThan<Sized>(size: Int){
    if(it.size < size){
        return Error("A minimum size of $size is required, but ${it.size} was found.")
    }
}

fun someOtherFunction(name: @LenGreaterThan(5) String): Result<Unit>{
    //Do something
}
```
This internally expands out to a regular generic function whose call is injected to the top of the calling function's body
```kt
fun <T> T.rule_LenGreaterThan(size: Int): RuleResult where T : Sized{
    if(this.size < size){
        return RuleResult.Error("A minimum size of $size is required, but ${it.size} was found.")
    }
    return RuleResult.Ok()
}

fun someOtherFunction(name: String): Result<Unit>{
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
To tell the Beagle compiler to allow a type to be validated against defined rules, you use a generic-like syntax, and any types that inherit/implement that type can have the rule checked against an object of it.
```kt
class A: Sized{
    val size: UInt32 get(){
        //Some kind of size calculation algorithm here
        return field
    }
}
```
On-site usage of a data validation rule is done in the type annotation.
```kt
fun createUser(username: @LenGreaterThan(5) String): Result<User>{
    //Create username
}
val createUsernameResult = createUsername("alex")
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