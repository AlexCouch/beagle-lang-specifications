### Basic Functions
Functions with empty parameters can omit their parameters
```
def fun myFunction{
	//Do stuff
}
```
Functions can be type annotated and return a value
```
def fun myFunction: Int{
	return 5
}
```
Functions can take parameters, but have to be type annotated
```
def fun myFunction(five: Int, string: String){
	//Do stuff
}
```