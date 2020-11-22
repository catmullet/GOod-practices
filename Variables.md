# Variables
### Keep them private
Global variables are bad for a few reasons but number one is accidental modifciation of shared variables amongst other packages. Another bad
one is cyclical imports. These are annoying to track down if you don't structure your app correctly from the beginning.
#### bad example
```go
var GlobalVariable int

//~~~Different Package~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
func UseGlobalVariable() {
  var totalAmount int
  totalAmount = global.GlobalVariable
}
//~~~Different Package~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
func SetGlobalVariable() {
  global.GlobalVariable = 12
}
//~~~Different Package~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
func SetGlobalVariable() {
  global.GlobalVariable += 15
}
//~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```

#### good example
```go
type GlobalStruct struct {
  privateVariable int
}

func NewGlobalStruct() *GlobalStruct {
  return &GlobalStruct{privateVariable: 8}
}

func (gs *GlobalStruct) SetPrivateVariable(i int) {
  gs.privateVariable = i
}

func (gs *GlobalStruct) GetPrivateVariable(i int) {
  return gs.privateVariable
}
```
Use factory functions where possible to create objects and pass around.  This way you know who controls that variable and there is one function
that can either get or set that variable. Now you have a better chance of not having code duplication related to that variable. You also help to reduce the chance of someone
creating a logic that loops through your global variable in another part of the code.  This would be bad for concurrency and should be caught when testing for race conditions 
but not always.
