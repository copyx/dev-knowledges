# Golang
## Packages

Go program = packages
Entry point of Go program = `main` package.
Package name = the last element of the import path
Outside a function, every statement begins with a keyword(`var`, `func`, and so on)
### Import Statements

```go
// general import statement
import "fmt"
import "math"
// factord import statement
import (
	"fmt"
	"math"
)
```
### Exported Name

Exported name begins with a capital letter. It is accessible from outside the package.

```go
package pizza

// unexported name
pepperoni = 1
// exported name
Pepperoni = 2
```

```go
package main

import (
	"fmt"
	"pizza"
)

func main() {
	fmt.Println(pizza.Pepperoni)
}
```

## Functions
### Parameter Types

Parameter type specify after parameter name.

```go
func functionName(param1 type, param2 type) type {}
func functionName(param1, param2 type) type {}
```

### Multiple Results

```go
func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b = swap("hello", "world")
	fmt.Println(a, b) // world hello
}
```

### Named Return Values

Named return values can be defined as variables at the top of the function.

```go
func swap(x, y string) (a, b string) { // named return variables
	a = y
	b = x
	return // naked return
}
```

`return` statements without arguments is called **naked return**. It returns the named return values.
Naked return statements can harm readability.

### Function Values

Functions can be treated like other values. It can be passed as arguments to another function or stored in variables.

```go
func a() {}
var b = a
func c(a func()) func() {
  return a
}
```

### Function Closures

A closure is a function value that references variables from outside its body.

```go
package main

import "fmt"

func privateValue() (func(int), func() int) {
  var a int
  return func(x int) {
    a = x
  },
  func() int {
    return a
  }
}

func main() {
  setter, getter := privateValue()
  fmt.Println(getter())
  setter(5)
  fmt.Println(getter())
}
```

## Variables

The `var` statement declares a list of variables. It can be at package or function level.

```go
var a, b, c bool

func foo() {
	var a, b, c int
}
```

It can be initialized at declaration.

```go
var a, b int = 1, 2
// If the initializer is present, the type can be omitted
var foo, bar, raz = true, 1, "str"
```

Variable declarations can be factored into blocks.

```go
var (
	A int  = -1
	B bool = true
	c uint = 4
)
```

### Short Variable Declarations

The `:=` short assignment statement can be used in place of `var` with **implicit type**.
It can be used only inside a function.

```go
a, b := 1, true
```

### Basic Types

```go
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64

complex64 complex128
```

### Zero Values

Variables declared without an explicit initial value are given their zero value.

```go
0 // for numeric types
false // for the boolean type
"" // (the empty string) for strings
```

### Type Conversions

Unlike in C, in Go assignment between items of different type requires an explicit conversion.

```go
T(v)
var i int = 1
var f float64 = float64(i)
u := uint(f)
```

### Type Inference

The variable's type is inferred from the value on the right hand side.

```go
var i int
j := i // int
```

If the right hand side is an untyped numeric constant, the new variable may be an `int`, `float64`, `complex128` **depending on the precision of the constant**.

```go
a := 1 // int
b := 3.14 // float64
c := 0.1 + 0.5i // complex 128
```

### Constants

Declare with `const` keyword.
Cannot be declared using the `:=` syntax.

```go
const Pi = 3.14
const Str = "string"
```

Numeric constants are high-precision values. An untyped constant takes the type needed by its context.

## `for` Statements

```go
import "fmt"

for i := 0; i < 10; i++ {
	fmt.Println(i)
}

j := 0
for ; j < 10; j++ {
	fmt.Println(j)
}

// for is Go's while statements
k := 0
for k < 10 {
	fmt.Println(k)
	k++
}

// infinite loop
for {
	fmt.Println("print forever")
}
```

## `if` Statements

```go
import "fmt"

func f1() {
	a := true
	if a {}
}

func f2() {
	// for-like style. With a short statement
	// Variables declared by the statement are only in if & else block
	if a := true; a {
		fmt.Println(a)
	} else {
		fmt.Println(!a)
	}

	// Error! undefined a
	fmt.Println(a)
}
```

## `switch` Statements

Go's switch is like the other languages except that **Go only runs the selected case, not all the cases that follow.**
Switch cases evaluate cases from top to bottom, stopping when a case succeeds.

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	fmt.Println("When's Saturday?")
	today := time.Now().Weekday()
	fmt.Println(today)
	switch time.Saturday {
	case today + 0:
		fmt.Println("Today.")
	case today + 1:
		fmt.Println("Tomorrow.")
	case today + 2:
		fmt.Println("In two days.")
	default:
		fmt.Println("Too far away.")
	}

	// Switch without a condition = switch true
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
}
```

## `defer` Statements

A `defer` statement defers the execution of a function until the surrounding function returns.

```go
package main

import "fmt"

func main() {
  for i := 0; i < 10; i++ {
    if i % 2 == 1 {
      fmt.Println(i)
    } else {
      defer fmt.Println(i)
    }
  }
}
```

The arguments of deferred function are evaluated immediately.
Deferred function calls are pushed onto a stack. So the calls are executed in last-in-first-out order.

# Pointers

Go has pointers. The type `*T` is a pointer to a `T` value. Its zero value is `nil`.
Unlike C, Go has no pointer arithmetic.

```go
var p *int
i := 1
p = &i
```

# `struct`

It is a collection of fields.

```go
package main

import "fmt"

type Vertex struct {
  X int
  Y int
}

func main() {
  v := Vertex{1, 2}
  fmt.Println(v.X)

  p := &v
  fmt.Println(p.X) // You don't have to use dereference (*p).X

  // Struct literals
  var (
  	v1 = Vertex{1, 2}  // has type Vertex
  	v2 = Vertex{X: 1}  // Y:0 is implicit
  	v3 = Vertex{}      // X:0 and Y:0
  	p  = &Vertex{1, 2} // has type *Vertex
  )
}
```

# Arrays

The type `[n]T` is an array of `n` values of type `T`.

```go
var a [2]int = {1, 2}
// Array literal
// [length]T{V1, V1, ...}
b = [3]bool{true, false, true}
```

An array's length is part of its type, so arrays cannot be resized.

# Slices

The type `[]T` is an slice with elements of type `T`.

```go
var a [5]int = {1, 2, 3, 4, 5}
var s []int = a[1:3] // low & high bound indices
var zs []int // nil

// Slice literal
// []T{V1, V2, ...}
t := []int{1, 2, 3}
u := []struct {
  i int
  b bool
}{
  {4, true}
  {5, false}
}

// Omit low or high bound
os := []int{1, 2, 3, 4, 5}
os[1:4] // [2, 3, 4]
os[1:] // [2, 3, 4, 5]
os[:4] // [1, 2, 3, 4]
```

`[low:high]` form selects **half-open range** that includes the first element, but excludes the last element.

A slice doesn't store any data, it just describe a section of an underlying array. If any value of slice is changed then original array is also changed.

- length `len(s)`
  - the # of slice's elements
- capacity `cap(s)`
  - the # of the underlying array's elements
  - **counting from the first element in the slice**
    - if make slice with low bounce then the capacity is changed.
  - It can be extended by re-slicing.

### `make()` Function

The `make()` is a built-in function. It allocates a zeroed array and return a slice that refers to the array.

```go
a := make([]int, 5) // len(a) = 5
b := make([]int, 0, 5) // len(b) = 0, cap(b) = 5
```

### `append()` Function

The `append()` is a built-in function. It append new element to a slice.

```go
func append(slice []T, values ...T) []T
```

### `range` Form

The `range` form of the `for` loop iterates over a slice or map. There are two values for each iteration. The first is the index, and the second is a copy of the element at that index.

```go
var s = []int{1, 2, 3, 4, 5}
for index, value := range s {}

// skip index or value
for index, _ := range s {}
for index := range s {}
for _, value := range s {}
```

## Maps

A map maps key to values.
The zero value is `nil`.

```go
var m map[string]int = make(map[string]int)

// map literal
var n map[string]int{
  "key1": 1,
  "key2": 2,
}

// mutating map
m["key"] = 1 // insert or update
value = m["key"] // retrieve
delete(m, "key") // delete
value, ok = m["key"] // test the key is present by second return value
```

## Methods

Go doesn't have classes. But methods of types can be defined.

```go
func (receiver T) functionName() {}
T.functionName()
```

**A method is just a function with special receiver argument.**

It can be declared on non-struct types too. But the method of the types which is defined in another package can't be declared.

### Pointer Receiver

The method must have a pointer receiver to change the value of original value.

With a value receiver, the method operates on a copy of the original value. (This is the same behavior as for any other function argument)

```go
func (receiver *T) functionName()
T.functionName
```

Two reasons to use a pointer receiver

- The method can modify the value that its receiver points to.
- To avoid copying the value on each method call.

## Interfaces

interface = a set of method signatures.

A variable of interface type can hold any value that implements the methods. An error occurs if the methods doesn't implement.

```go
type I interface {
  M() int
}

type Int int

func (i Int) M() int {}

func main() {
  var a I
  a = Int(1)
  a = "test" // error!!!
  // cannot use "test" (constant of type string) as I value in assignment: string does not implement I (missing method M)
}
```

There is no "implements" keyword. Interfaces are implemented implicitly. **Implicit interfaces decouple the definition of an interface from its implementation.**
The implementation could appear in any package without prearrangement.

### Interface values

Interface values can be thought of as a tuple of a value and a concrete type.

```go
package main
import "fmt"

type I interface {
	M()
}

type T struct {
	S string
}

func (t *T) M() {
	fmt.Println(t.S)
}

func describe(i I) {
  fmt.Printf("(%v, %T)\n", i, i) // (value, type)
}

func main() {
  var i I
  describe(i) // (<nil>, <nil>)
  // i.M() // panic: runtime error: invalide memory address or nil pointer dereference
  
  i = &T{"Hello"}
  describe(i) // (&{Hello}, *main.T)

  var t T
  i = t
  describe(i) // (<nil>, *main.T)
  
}
```

The value can be `nil`. But this wouldn't trigger a null pointer exception. It is common to write methods that gracefully handle the nil receiver.

### The Empty Interface

The interface without methods. It can hold values of any type. It is used to handle unknown type values.

```go
interface{}
```

#### Type Assertions

It provides access to an underlying concrete value of interface value.

```go
t := i.(T)
t, ok := i.(T)
```

#### Type Switches

```go
switch v := i.(type) {
case int:
  // do something
case float64:
  // do something
default:
  // no match
}
```

A type switch can assert several types in series.

### Stringers

```go
type Stringer interface {
  String() string
}
```

A Stringer is built-in interface that can describe itself as a string. It is defined by the `fmt` package.

```go
package main
import "fmt"
type Int int

func (i Int) String() string {
  return fmt.Sprintf("%d %d", i, i)
}

func main() {
  var a Int = 12
  fmt.Println(a) // 12 12
}
```

### Errors

The `error` type is built in interface.

```go
type error interface {
  Error() string
}
```

We can make custom error by `error` interface.

```go
package main

import (
	"fmt"
)

type ErrNegativeSqrt float64

func (e ErrNegativeSqrt) Error() string {
	return fmt.Sprintf("cannot Sqrt negative number: %f", e)
}

func Sqrt(x float64) (float64, error) {
	if x < 0 {
		return 0, ErrNegativeSqrt(x)
	}
	
	return x, nil
}

func main() {
	fmt.Println(Sqrt(2))
	fmt.Println(Sqrt(-2))
}
```

## Generics

```go
func Index[T comparable](s []T, x T) int
```

The `T` is a type parameter.
The `comparable` is the built-in constraint.

### Generic Types

A type can be parameterized with a type parameter.

```go
type List[T any] struct {
  next *List[T]
  val T
}
```

## Goroutines

A `goroutine` is a lightweight thread managed by the Go runtime.

```go
go f(x)
```

### Channels

Channels are typed conduit. We can send and receive values with the channel operator(`<-`).

```go
ch := make(chan int)
ch <- 1
v := <- ch
```

By default, sends and receives block until the other side is ready.

#### Buffered Channels

Channels can be buffered.

```go
ch := make(chan int, 100)
```

Sends block when the buffer is full.
Receives block when the buffer is empty.

#### Close and Range

A sender can close a channel to indicate that no more value will be sent. Receivers can test whether a channel has been closed.

```go
v, after := <- ch // false: channel is closed.
```

The loop `for i := range c` receives values from the channel repeatedly until it is closed.

#### Select

The `select` statement lets a goroutine wait on multiple communication operations.

A `select` blocks until one of its cases can run, then it executes that case. It chooses one at random if multiple cases are ready.

```go
package main

import "fmt"

func main() {
	a := make(chan int)
	b := make(chan int)
	go func() {
		a <- 1
		a <- 2
		a <- 3
		b <- 0
	}()
	
	selectExample(a, b)
}

func selectExample(a, b chan int) {
	for {
		select {
		case c := <- a:
			fmt.Println(c)
		case d := <- b:
			fmt.Println(d)
			return
		}
	}
}
```

The `default` case in a `select` is run if no other case is ready.

## Modules, packages, and versions

https://go.dev/ref/mod#

A module = a collection of packages.
Modules may be downloaded directly from version control repositories or module proxy servers.
A module is identified by a module path which is declared in a `go.mod` file. The file has information about the module's dependencies.
The module root directory = the directory that contains the `go.mod` file.
The main module = the module containing the directory where the go command is invoked.
A module directive = the main module's path.
A go.mod file must contain exactly one.

A package = a collection of source files in the same directory.
They are compiled together.
A package path = the module path + subdirectory containing the package
e.g.) "html" package of "golang.org/x/net" module = "golang.org/x/net/html"
