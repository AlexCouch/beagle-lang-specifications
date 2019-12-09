# Rules
Rules can be defined on certain types to allow type annotations to comes with data validation rules. Any type can have any rule applied onto it as long as the type definition allows it.

The definition of a rule can be declared with input parameters for which to base validation on. In the case of checking a size/length based object (such as a Collection, Array, String, etc) against some bound can be done this way.
```ruby
def rule LenGreaterThan<Sized>(size: Int){
    if(it.size < size){
        raise "A minimum size of $size is required, but ${it.size} was found."
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
//Line 151 in user_data.bg
def fun createUsername(username: String<LenGreaterThan(5)>){
    //Create username
}
//This would raise an exception detailing the string object "alex" violating the string validation rule LenGreaterThan
//Data Validation Failure:
//  Line 32 in File `create_user.bg`
//  `username` violated data validation rules:
//      Line 151 in user_data.bg
//      LenGreaterThan on type String
//      Rule violation message:
//          "A minimum length of 5 is required, but 4 was found."
//Line 32 in file create_user.bg
createUsername("alex")
```

#### Rule Dependencies
Rules can be depend on each other so that one rule requires another rule to be validated against in order to succeed.

```ruby
def rule RuleA(someInt: Int){
    //Do some stuff
}

//This rule is done before RuleA
def rule RuleB(someString: String) before RuleA(someString.length){
    //Do some stuff
}

//RuleB is done before RuleC
def rule RuleC(someStringIn: String) after RuleB(someStringIn){
    //Do stuff
}

def rule RuleD(something: Float){
    //Do stuff
}

def rule RuleE(anotherThing: Float) before RuleD(anotherThing) after RuleC(anotherThing.toString()){
    //Do stuff
}
```
Specifying a rule dependency with `before` tells the compiler that this rule must execute before the next rule, with `after` being after the next rule.