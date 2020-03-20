# Def Macros
Def macros give you the ability to create your own *definable construct*. This means that you can create syntactic sugar for a common type or range of types in an API. It also makes API maintanance easier since you're less likely to break dependants' code if API undergoes architecture changes.
```c#
def defmacro entity{
    //Users can create constructors
    canHaveConstructor = true
    //A definable construct that means this entity has a ticking mechanism
    val tickableAttr = createAttribute("tickable")
    //Intrinsic properties methods that are treated as first class, and can be used as syntactic sugar
    intrinsics{
        //Intrinsic methods
        methods{
            //An intrinsic called "onSpawn" which will translate to the Entity::onSpawn method
            +"onSpawn" to Entity::onSpawn
            //Just like "onSpawn" but for "onDespawn"
            +"onDespawn" to Entity::onDespawn
            //With the tickable attribute provided by the use site, allow "onTick" to be used for Tickable::onTick
            with(provides(tickableAttr)){
                +"onTick" to Tickable::onTick
            }
        }
    }
}
```
As long as the relavent types are supplied in this hypothetical API, a user can theoretically use this defmacro like this:
```ruby
def tickable entity Pig(val pigName: String){
    onSpawn{
        //Do stuff on spawn
    }
    onDespawn{
        //Do stuff on despawn
    }
    onTick{
        //Do stuff on every tick
        //NOTE: This can only be done if the "tickable" attribute exists, like it does in this example
    }
}
```