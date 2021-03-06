---
layout: post
title: Golang Learning Notes 
date: 2020-04-07 09:24:39 +0800
categories: ['Go']
tags: ['Go']
---

- TOC
{:toc}

---

## Foramtting

With Go we take an unusual approach and let the machine take care of most formatting issues. The `gofmt` program (also available as `go fmt`, which operates at the package level rather than source file level) reads a Go program and emits the source in a standard style of indentation and vertical alignment, retaining and if necessary reformatting comments.

```sh
$ cat formatting.go && gofmt formatting.go 
```

```go
package formatting
type T struct {

        name string // name of the object
            value int // its value
        }
```

```go
package formatting

type T struct {
    name  string // name of the object
    value int    // its value
}
```

## Commentary

 Go provides C-style `/* */` block comments and C++-style `//` line comments. Line comments are the norm; block comments appear mostly as package comments, but are useful within an expression or to disable large swaths of code.

The program—and web server—`godoc` processes Go source files to extract documentation about the contents of the package. Comments that appear before top-level declarations, with no intervening newlines, are extracted along with the declaration to serve as explanatory text for the item. The nature and style of these comments determines the quality of the documentation godoc produces.

```go
/*
Package regexp implements a simple library for regular expressions.

The syntax of the regular expressions accepted is:

    regexp:
        concatenation { '|' concatenation }
    concatenation:
        { closure }
    closure:
        term [ '*' | '+' | '?' ]
    term:
        '^'
        '$'
        '.'
        character
        '[' [ '^' ] character-ranges ']'
        '(' regexp ')'
*/
package regexp
```

```sh
$ go doc builtin new
```
```go
package builtin // import "builtin"

func new(Type) *Type
    The new built-in function allocates memory. The first argument is a type,
    not a value, and the value returned is a pointer to a newly allocated zero
    value of that type.

```

```sh
$ go doc sync Mutex
```
```go
package sync // import "sync"

type Mutex struct {
    // Has unexported fields.
}
    A Mutex is a mutual exclusion lock. The zero value for a Mutex is an
    unlocked mutex.

    A Mutex must not be copied after first use.

func (m *Mutex) Lock()
func (m *Mutex) Unlock()
```

## Names

The visibility of a name outside a package is determined by whether its first character is upper case. 

### Package names

- By convention, packages are given lower case, single-word names; there should be no need for underscores or mixedCaps.
- Another convention is that the package name is the base name of its source directory; the package in `src/encoding/base64` is imported as "`encoding/base64`" but has name `base64`, not `encoding_base64` and not `encodingBase64`. 
- Use the package structure to help you choose good names.
    - The importer of a package will use the name to refer to its contents, so exported names in the package can use that fact to avoid stutter.
    - For instance, the buffered reader type in the `bufio` package is called `Reader`, not `BufReader`, because users see it as `bufio.Reader`, which is a clear, concise name.
    - Moreover, because imported entities are always addressed with their package name, `bufio.Reader` does not conflict with `io.Reader`.
    - Similarly, the function to make new instances of `ring.Ring`—which is the definition of a constructor in Go—would normally be called `NewRing`, but since `Ring` is the only type exported by the package, and since the package is called `ring`, it's called just `New`, which clients of the package see as `ring.New`. 

### Getters

- Go doesn't provide automatic support for getters and setters.
- There's nothing wrong with providing getters and setters yourself, and it's often appropriate to do so, but **it's neither idiomatic nor necessary to put `Get` into the getter's name.**
- If you have a field called `owner` (lower case, unexported), the getter method should be called `Owner` (upper case, exported), not `GetOwner`.
- A setter function, if needed, will likely be called `SetOwner`.
- Both names read well in practice: 

    ```go
    owner := obj.Owner()
    if owner != user {
        obj.SetOwner(user)
    }
    ```

### Interface names

- By convention, one-method interfaces are named by the method name plus an `-er` suffix or similar modification to construct an agent noun: `Reader`, `Writer`, `Formatter`, `CloseNotifier` etc. 
- There are a number of such names and it's productive to honor them and the function names they capture.
- `Read`, `Write`, `Close`, `Flush`, `String` and so on have canonical signatures and meanings.
- To avoid confusion, don't give your method one of those names unless it has the same signature and meaning.
- Conversely, if your type implements a method with the same meaning as a method on a well-known type, give it the same name and signature; call your string-converter method `String` not `ToString`. 

### MixedCaps

Finally, the convention in Go is to use `MixedCaps` or `mixedCaps` rather than underscores to write multiword names. 

## Semicolons

- Like C, Go's formal grammar uses semicolons to terminate statements, but unlike in C, those semicolons do not appear in the source. 

- **If the newline comes after a token that could end a statement, insert a semicolon.**

- Idiomatic Go programs have semicolons only in places such as for loop clauses, to separate the initializer, condition, and continuation elements.

- They are also necessary to separate multiple statements on a line, should you write code that way. 

## Control structures

- There is no do or while loop, only a slightly generalized `for`; `switch` is more flexible;
- `if` and `switch` accept an optional initialization statement like that of `for`;
- `break` and `continue` statements take an optional label to identify what to break or continue;
- and there are new control structures including a type switch and a multiway communications multiplexer, `select`.
- There are no parentheses and the bodies must always be brace-delimited. 

### If

```go
if x > 0 {
    return y
}
```

```go
if f, err: = os.Open(name); err != nil {
   return err
} 
```

### For

```go
// Like a C for
for init; condition; post { }

// Like a C while
for condition { }

// Like a C for(;;)
for { }

// Like a C do-while
for {
    // do something
    if condition; {
        break
    }
}
```

If you're looping over an array, slice, string, or map, or reading from a channel, a `range` clause can manage the loop. 

```go
for key, value := range map {
}

// If you only need the second item in the range (the value),
// use the blank identifier, an underscore, to discard the first: 
for _, value := range map {
}

for index, value := range array {
}

for value := range channel {
}
```

For strings, the `range` does more work for you, breaking out individual Unicode code points by parsing the UTF-8. Erroneous encodings consume one byte and produce the replacement rune U+FFFD. (The name (with associated builtin type) `rune` is Go terminology for a single Unicode code point.) 

```go
for pos, char := range "日本\x80語" { // \x80 is an illegal UTF-8 encoding
    fmt.Printf("character %#U starts at byte position %d\n", char, pos)
}
```

Go has no comma operator and `++` and `--` are statements not expressions. Thus if you want to run multiple variables in a for you should use parallel assignment (although that precludes ++ and --). 

```go
// Reverse a
for i, j := 0, len(a)-1; i < j; i, j = i+1, j-1 {
    a[i], a[j] = a[j], a[i]
}
```

### Switch

Go's switch is more general than C's.
- The expressions need not be constants or even integers,
- the cases are evaluated top to bottom until a match is found,
- and if the `switch` has no expression it switches on `true`.
- It's therefore possible—and idiomatic—to write an `if-else-if-else` chain as a `switch`. 
- There is no automatic fall through, but cases can be presented in comma-separated lists. 

- Although they are not nearly as common in Go as some other C-like languages, `break` statements can be used to terminate a `switch` early.
- Sometimes, though, it's necessary to break out of a surrounding loop, not the switch, and in Go that can be accomplished by putting a label on the loop and "breaking" to that label. 
- Of course, the `continue` statement also accepts an optional label but it applies only to loops. 

```go
Loop:
    for n := 0; n < len(src); n += size {
        switch {
        case src[n] < sizeOne:
            if validateOnly {
                break
            }
            size = 1
            update(src[n])

        case src[n] < sizeTwo:
            if n+1 >= len(src) {
                err = errShortInput
                break Loop
            }
            if validateOnly {
                break
            }
            size = 2
            update(src[n] + src[n+1]<<shift)
        }
    }
```

#### Type switch

A switch can also be used to discover the dynamic type of an interface variable.
- Such a *type switch* uses the syntax of a type assertion with the keyword `type` inside the parentheses.
- If the switch declares a variable in the expression, the variable will have the corresponding type in each clause.
- It's also idiomatic to reuse the name in such cases, in effect declaring a new variable with the same name but a different type in each case. 

```go
var t interface{}
t = functionOfSomeType()
switch t := t.(type) {
default:
	fmt.Printf("unexpected type %T\n", t) // %T prints whatever type t has
case bool:
	fmt.Printf("boolean %t\n", t) // t has type bool
case int:
	fmt.Printf("integer %d\n", t) // t has type int
case *bool:
	fmt.Printf("pointer to boolean %t\n", *t) // t has type *bool
case *int:
	fmt.Printf("pointer to integer %d\n", *t) // t has type *int
}
```

## Functions

### Multiple return values

```go
func (file *File) Write(b []byte) (n int, err error)
```

### Named result parameters

- The return or result "parameters" of a Go function can be given names and used as regular variables, just like the incoming parameters.
- When named, they are initialized to the zero values for their types when the function begins;
- if the function executes a return statement with no arguments, the current values of the result parameters are used as the returned values. 

### Defer

- Go's defer statement schedules a function call (the *deferred* function) to be run immediately before the function executing the defer returns.

- It's an unusual but effective way to deal with situations such as resources that must be released regardless of which path a function takes to return.

    ```go
    func ReadFile(filename string) ([]byte, error) {
        f, err := os.Open(filename)
        if err != nil {
            return nil, err
        }
        defer f.Close()
        return ReadAll(f)
    }
    ```

- The arguments to the deferred function (which include the receiver if the function is a method) are evaluated when the *defer* executes, not when the *call* executes.

- Deferred functions are executed in LIFO order.

    ```go
    for i := 0; i < 5; i++ {
    	defer fmt.Printf("%d ", i)
    }

    // Output:
    // 4 3 2 1 0
    ```

    ```go
	// All function values created by this loop "capture"
	// and share the same variable—an addressable storage location,
	// not its value at that particular moment.
	for i := 0; i < 5; i++ {
		defer func() {
			fmt.Print(i, " ")
		}()
	}

	// Output:
	// 5 5 5 5 5
    ```

    ```go
	for i := 0; i < 5; i++ {
		// declares inner i, intialized to outer i
		i := i
		defer func() {
			fmt.Print(i, " ")
		}()
	}

	// Output:
	// 4 3 2 1 0
    ```

## Data types

```go
bool

string

int8  int16  int32  int64
uint8 uint16 uint32 uint64 uintptr
int uint // either 32 or 64 bits

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64

complex64 complex128

// more types
pointers structs array slices maps functions interfaces channels
```
#### Pointers

```go
// A pointer holds the memory address of a value. 
// Unlike C, Go has no pointer arithmetic.

// The type `*T` is a pointer to a `T` value. Its zero value is `nil`. 
var p *int
i := 42
// The `&` operator generates a pointer to its operand.
p = &i
// The `*` operator ("dereferencing" or "indirecting") denotes the pointer's underlying value.
*p = 21
```

#### Structs

```go
// A struct is a collection of fields. 
type Vertex struct {
    X, Y int
}

var (
    // A struct literal denotes a newly allocated struct value by listing the values of its fields.
    v1 = Vertex{1, 2}  // has type Vertex
    // You can list just a subset of fields by using the Name: syntax.
    // (And the order of named fields is irrelevant.)
    v2 = Vertex{X: 1}  // Y:0 is implicit
    v3 = Vertex{}      // X:0 and Y:0
    // The special prefix & returns a pointer to the struct value
    p  = &Vertex{1, 2} // has type *Vertex
)

func main() {
    // Struct fields are accessed using a dot. 
    p.X = 1e9
    fmt.Println(v1, p, v2, v3)
}
```

#### Arrays

- The type `[n]T` is an array of `n` values of type `T`. 

- Arrays are values. Assigning one array to another copies all the elements.

- In particular, if you pass an array to a function, it will receive a copy of the array, not a pointer to it.

- The size of an array is part of its type. The types `[10]int` and `[20]int` are distinct, so arrays cannot be resized.

```go
var a [2]string
a[0] = "Hello"
a[1] = "World"

// an array literal
primes := [6]int{2, 3, 5, 7, 11, 13}
```

#### Slices

- A slice, on the other hand, is a dynamically-sized, flexible view into the elements of an array.
- The type `[]T` is a slice with elements of type `T`.
- A slice is formed by specifying two indices, a low and high bound, separated by a colon:
    ```go
    // This selects a half-open range which includes the first element, but excludes the last one. 
    a[low : high]
    ```
- The following expression creates a slice which includes elements 1 through 3 of a:
    ```go
    a[1:4]
    ```

**Slices are like references to arrays**

- A slice does not store any data, it just describes a section of an underlying array.
- A slice hold references to an underlying array, and if you assign one slice to another, both refer to the same array. 
- Changing the elements of a slice modifies the corresponding elements of its underlying array.
- Other slices that share the same underlying array will see those changes. 

**Slice literals**

- A slice literal is like an array literal without the length. 
    ```go
    []bool{true, true, false}
    ```

**Slice defaults**

- When slicing, you may omit the high or low bounds to use their defaults instead.
- The default is zero for the low bound and the length of the slice for the high bound. 

```go
// For the array
var a [10]int
// these slice expressions are equivalent:
a[0:10]
a[:10]
a[0:]
a[:]
```

**Slice length and capacity**

- A slice has both a *length* and a *capacity*. 
- The length of a slice is the number of elements it contains.
- The capacity of a slice is the number of elements in the underlying array, counting from the first element in the slice.
- The length and capacity of a slice s can be obtained using the expressions `len(s)` and `cap(s)`.
- You can extend a slice's length by re-slicing it, provided it has sufficient capacity. 

**Nil slices**

- The zero value of a slice is `nil`.
- A `nil` slice has a length and capacity of 0 and has no underlying array. 

**Appending to a slice**

- It is common to append new elements to a slice, and so Go provides a built-in `append` function.

    ```go
    func append(s []T, vs ...T) []T
    ```
- The resulting value of `append` is a slice containing all the elements of the original slice plus the provided values.
- If the backing array of `s` is too small to fit all the given values a bigger array will be allocated. The returned slice will point to the newly allocated array. 

    ```go
    var s []int
    
    // append works on nil slices.
    s = append(s, 0)
    
    // The slice grows as needed.
    s = append(s, 1)
    
    // We can add more than one element at a time.
    s = append(s, 2, 3, 4)
    ```

#### Maps

- Maps are a convenient and powerful built-in data structure that associate values of one type (the key) with values of another type (the element or value).

- The key can be of any type for which the equality operator is defined, such as integers, floating point and complex numbers, strings, pointers, interfaces (as long as the dynamic type supports equality), structs and arrays.

- Slices cannot be used as map keys, because equality is not defined on them.

- Like slices, maps hold references to an underlying data structure. If you pass a map to a function that changes the contents of the map, the changes will be visible in the caller. 

- The zero value of a map is `nil`. A `nil` map has no keys, nor can keys be added.

- Map literals are like struct literals, but the keys are required. 

    ```go
    var m map[string]int // <nil>
    m = map[string]int{
        "hello": 100,
        "world": 200,
    }
    ```

- The `make` function returns a map of the given type, initialized and ready for use. 

    ```go
    m := make(map[string]int)
    
    // insert or update an element
    m["Answer"] = 42
    
    // delete an element:
    delete(m, "Answer")
    
    // retrieve an element
    v := m["Answer"]
    
    // test that a key is present with a two-value assignment
    v, ok := m["Answer"]
    ```

#### Functions

- Functions are values too. They can be passed around just like other values.

- Function values may be used as function arguments and return values. 

- Go functions may be closures.

    - A closure is a function value that references variables from outside its body.

    - The function may access and assign to the referenced variables; in this sense the function is "bound" to the variables. 

    ```go
    func adder() func(int) int {
    	sum := 0
    	return func(x int) int {
    		sum += x
    		return sum
    	}
    }
    
    func main() {
    	pos, neg := adder(), adder()
    	for i := 0; i < 3; i++ {
    		fmt.Println(
    			pos(i),
    			neg(-2*i),
    		)
    	}
    
    	// Output:
    	// 0 0
    	// 1 -2
    	// 3 -6
    }
    ```

#### Methods

- Go does not have classes. However, you can define methods on any named type (except a pointer or an interface). 

- A method is a function with a special receiver argument.

- The receiver appears in its own argument list between the `func` keyword and the method name. 

- You can only declare a method with a receiver whose type is defined in the same package as the method. 

**Choosing a value or pointer receiver**

-  There are two reasons to use a pointer receiver.

    - The first is so that the method can modify the value that its receiver points to.

    - The second is to avoid copying the value on each method call. This can be more efficient if the receiver is a large struct, for example.

- In general, all methods on a given type should have either value or pointer receivers, but not a mixture of both.

- The rule about pointers vs. values for receivers is that value methods can be invoked on pointers and values, but pointer methods can only be invoked on pointers. 

- This rule arises because pointer methods can modify the receiver; invoking them on a value would cause the method to receive a copy of the value, so any modifications would be discarded. The language therefore disallows this mistake. There is a handy exception, though. When the value is addressable, the language takes care of the common case of invoking a pointer method on a value by inserting the address operator automatically. 

    ```go
    package bufio // import "bufio"
    
    func (b *Reader) Read(p []byte) (n int, err error)
    
    func (b *Writer) Write(p []byte) (nn int, err error)
    ```

**Nil is a valid receiver value**

- Just as some functions allow nil pointers as arguments, so do some methods for their receiver, especially if nil is a meaningful zero value of the type, as with maps and slices.

- When you define a type whose methods allow nil as a receiver value, it’s worth pointing this out explicitly in its documentation comment.

#### Interfaces

Interfaces in Go provide a way to specify the behavior of an object: if something can do *this*, then it can be used *here*.

**Interfaces are implemented implicitly**

- A type implements an interface by implementing its methods. There is no explicit declaration of intent, no "implements" keyword.

- Implicit interfaces decouple the definition of an interface from its implementation, which could then appear in any package without prearrangement. 

**Interface values**

- Under the hood, interface values can be thought of as a tuple of a value and a concrete type:

   ```go
   (value, type)
   ```

- An interface value holds a value of a specific underlying concrete type.

- Calling a method on an interface value executes the method of the same name on its underlying type. 

**Interface values with nil underlying values**

- If the concrete value inside the interface itself is nil, the method will be called with a nil receiver.

- In some languages this would trigger a null pointer exception, but in Go it is common to write methods that gracefully handle being called with a nil receiver.

- Note that an interface value that holds a nil concrete value is itself non-nil. 

    ```go
    type I interface {
    	M()
    }
    
    type T struct{}
    
    func (t *T) M() {
    	if t == nil {
    		fmt.Println("<nil>")
    		return
    	}
    }
    
    func main() {
    	var i I
    	var t *T
    	i = t
    	i.M()
    	fmt.Printf("(%v, %T)\n", i, i)
    
    	i = &T{}
    	i.M()
    	fmt.Printf("(%v, %T)\n", i, i)
    
    	// Output:
    	// <nil>
    	// (<nil>, *main.T)
    	// (&{}, *main.T)
    }
    ```

**Nil interface values**

- A nil interface value holds neither value nor concrete type.

- Calling a method on a nil interface is a run-time error because there is no type inside the interface tuple to indicate which concrete method to call. 

    ```go
    var i I
    fmt.Printf("(%v, %T)\n", i, i)
    i.M()
    // (<nil>, <nil>)
    // panic: runtime error: invalid memory address or nil pointer dereference
    ```

**The empty interface**

- The interface type that specifies zero methods is known as the empty interface:

    ```go
    interface{}
    ```

- An empty interface may hold values of any type. (Every type implements at least zero methods.)

- Empty interfaces are used by code that handles values of unknown type. 

**Generality**

- If a type exists only to implement an interface and will never have exported methods beyond that interface, there is no need to export the type itself.

- Exporting just the interface makes it clear the value has no interesting behavior beyond what is described in the interface.

- It also avoids the need to repeat the documentation on every instance of a common method.

- In such cases, the constructor should return an interface value rather than the implementing type. 

**Interface conversions and type assertions**

- A type assertion provides access to an interface value's underlying concrete value.

    ```go
    t := i.(T)
    ```

    - This statement asserts that the interface value `i` holds the concrete type `T` and assigns the underlying `T` value to the variable `t`.

    - If `i` does not hold a `T`, the statement will trigger a panic.

- To *test* whether an interface value holds a specific type, a type assertion can return two values: the underlying value and a boolean value that reports whether the assertion succeeded.

    ```go
    t, ok := i.(T)
    ```

    - If `i` holds a `T`, then `t` will be the underlying value and `ok` will be `true`.

    - If not, `ok` will be `false` and `t` will be the zero value of type `T`, and no panic occurs. 

**Type switches**

- The declaration in a type switch has the same syntax as a type assertion `i.(T)`, but the specific type `T` is replaced with the keyword `type`. 

    ```go
    switch v := i.(type) {
    case T:
        // here v has type T
    case S:
        // here v has type S
    default:
        // no match; here v has the same type as i
    }
    ```

#### Embedding: interfaces and structs

- Go does not provide the typical, type-driven notion of subclassing, but it does have the ability to “borrow” pieces of an implementation by embedding types within a struct or interface. 

    ```go
    package io // import "io"
    
    type Reader interface {
        Read(p []byte) (n int, err error)
    }
    
    type Writer interface {
        Write(p []byte) (n int, err error)
    }
    
    // ReadWriter is the interface that combines the Reader and Writer interfaces.
    type ReadWriter interface {
        Reader
        Writer
    }
    ```
    
    ```go
    package bufio // import "bufio"
    
    type Reader struct {
        // Has unexported fields.
    }
    
    func (b *Reader) Read(p []byte) (n int, err error)
    
    type Writer struct {
        // Has unexported fields.
    }
    
    func (b *Writer) Write(p []byte) (nn int, err error)
    
    // ReadWriter stores pointers to a Reader and a Writer.
    // It implements io.ReadWriter.
    type ReadWriter struct {
        *Reader
        *Writer
    }
    ```

- There's an important way in which embedding differs from subclassing.

    - When we embed a type, the methods of that type become methods of the outer type, but when they are invoked the receiver of the method is the inner type, not the outer one.
    - For example, when the Read method of a `bufio.ReadWriter` is invoked, the receiver is the `reader` field of the `ReadWriter`, not the `ReadWriter` itself. 

        ```go
        type Reader struct {
        }
        
        func (r *Reader) Read() {
        	fmt.Println("Read")
        }
        
        type Writer struct {
        }
        
        func (r *Writer) Write() {
        	fmt.Println("Write")
        }
        
        type ReadWriter struct {
        	*Reader
        	*Writer
        }
        
        func main() {
        	rw := ReadWriter{}
        	rw.Read() // same as rw.Reader.Read()
        	rw.Reader.Read()
        	// Output:
        	// Read
        	// Read
        }
        ```

- Embedding types introduces the problem of name conflicts but the rules to resolve them are simple. 

    - First, a field or method X hides any other item X in a more deeply nested part of the type.
    - Second, if the same name appears at the same nesting level, it is usually an error. 
        - However, if the duplicate name is never mentioned in the program outside the type definition, it is OK.
        - This qualification provides some protection against changes made to types embedded from outside; there is no problem if a field is added that conflicts with another field in another subtype if neither field is ever used. 

#### Channels

- Channels are a typed conduit through which you can send and receive values with the channel operator, `<-`. 

    ```go
    ch <- v    // Send v to channel ch.
    v := <-ch  // Receive from ch, and assign value to v.

    // (The data flows in the direction of the arrow.) 
    ```

- Like maps and slices, channels must be created before use:

    ```go
    // By default, sends and receives block until the other side is ready.
    // This allows goroutines to synchronize without explicit locks or condition variables. 
    blockChan := make(chan int)
    
    // Sends to a buffered channel block only when the buffer is full.
    // Receives block when the buffer is empty. 
    bufChan := make(chan int, 100)
    ```
- A sender can `close` a channel to indicate that no more values will be sent. 

    - The multi-valued assignment form of the receive operator reports whether a received value was sent before the channel was closed. 

        ```go
        // ok is false if there are no more values to receive and the channel is closed. 
        v, ok := <-ch
        ```
    - The loop for `v := range c` receives values from the channel repeatedly until it is closed. 

    - Attempting to close an already-closed channel causes a panic, as does closing a nil channel.

    -  **Note**: Only the sender should close a channel, never the receiver. Sending on a closed channel will cause a panic.

    - **Another note**: Channels aren't like files; you don't usually need to close them. Closing is only necessary when the receiver must be told there are no more values coming, such as to terminate a `range` loop. 

- A channel may be constrained only to send or only to receive by assignment or explicit conversion. 

    ```go
    func main() {
    	var (
    		_ = make(chan int)   // bidirectional
    		_ = make(<-chan int) // receive-only
    		_ = make(chan<- int) // send-only
    	)
    
    	ch := make(chan int)
    
    	// send-only
    	go func(ch chan<- int) {
    		for i := 0; i < 3; i++ {
    			ch <- i
    		}
    		close(ch)
    	}(ch)
    
    	// receive-only
    	go func(ch <-chan int) {
    		for v := range ch {
    			fmt.Println(v)
    		}
    	}(ch)
    
    	time.Sleep(time.Millisecond)
    	// Output:
    	// 0
    	// 1
    	// 2
    }
    ```

    ```go
    func main() {
    	ch := make(chan int)
    	quit := make(chan int)
    
    	go func(ch chan<- int) {
    		for i := 0; i < 3; i++ {
    			ch <- i
    		}
    		quit <- 0
    	}(ch)
    
    	//  The select statement lets a goroutine wait on multiple communication operations.
    	//  A select blocks until one of its cases can run, then it executes that case.
    	//  It chooses one at random if multiple are ready.
    	for {
    		select {
    		case x := <-ch:
    			fmt.Println(x)
    		case <-quit:
    			return
    		}
    	}
    }
    ```

### Type conversions

The expression `T(v)` converts the value `v` to the type `T`. 

```go
// Some numeric conversions:

var i int = 42
var f float64 = float64(i)
var u uint = uint(f)

// Or, put more simply:

i := 42
f := float64(i)
u := uint(f)
```

## Initialization 

### Constants

- Constants are declared like variables, but with the `const` keyword.

- Constants cannot be declared using the `:=` syntax. 

- Constants are created at compile time, even when defined as locals in functions, and can only be numbers, characters (runes), strings or booleans.

- Because of the compile-time restriction, the expressions that define them must be constant expressions, evaluatable by the compiler. 

- In Go, enumerated constants are created using the `iota` enumerator.

```go
type Weekday int

const (
    Sunday Weekday = iota + 1 // iota: 0 ~ Sunday    : 1
    _                         // iota: 1 ~ iota increased
    // comments               // iota: 1 ~ skip: comment
                              // iota: 1 ~ skip: empty line
    Monday                    // iota: 2 ~ Monday    : 3
    Tuesday                   // iota: 3 ~ Monday    : 4
    Wednesday                 // iota: 4 ~ Monday    : 5
    Thursday                  // iota: 5 ~ Monday    : 6
    Friday                    // iota: 6 ~ Monday    : 7
    Saturday                  // iota: 7 ~ Monday    : 8
)
```

### Allocation with `new` and `make`

- Go has two allocation primitives, the built-in functions `new` and `make`. They do different things and apply to different types, which can be confusing, but the rules are simple.

- `new` is a built-in function that allocates memory, but unlike its namesakes in some other languages it does not initialize the memory, it only zeros it.

    -  That is, `new(T)` allocates zeroed storage for a new item of type `T` and returns its address, a value of type `*T`.

    -  In Go terminology, it returns a pointer to a newly allocated zero value of type `T`.

    - Since the memory returned by new is zeroed, it's helpful to arrange when designing your data structures that the zero value of each type can be used without further initialization.

    - This means a user of the data structure can create one with new and get right to work.

    - For example, the documentation for `bytes.Buffer` states that "the zero value for Buffer is an empty buffer ready to use."

- The built-in function `make(T, args)` serves a purpose different from `new(T)`.

    - It creates slices, maps, and channels only, and it returns an initialized (not zeroed) value of type `T` (not `*T`).

    - The reason for the distinction is that these three types represent, under the covers, references to data structures that must be initialized before use. 

    ```go
    var p *[]int = new([]int)       // allocates slice structure; *p == nil; rarely useful
    var v  []int = make([]int, 100) // the slice v now refers to a new array of 100 ints
    
    // Unnecessarily complex:
    var p *[]int = new([]int)
    *p = make([]int, 100, 100)
    
    // Idiomatic:
    v := make([]int, 100)
    ```

### The init function

- Each source file can define its own niladic `init` function to set up whatever state is required.

- Actually each file can have multiple init functions.

- `init` is called after all the variable declarations in the package have evaluated their initializers, and those are evaluated only after all the imported packages have been initialized.

```go
package hello

import (
	"fmt"
)

func init() {
	fmt.Print("hello ")
}
```

```go
package world

import (
	"fmt"
	_ "hello"
)

func init() {
	fmt.Print("world")
}
```

```go
package main

import (
	"fmt"
	_ "world"
)

const mark = "!"

func init() {
	fmt.Print(mark)
}

func main() {
    // Output:
    // hello world!
}
```

### Zero values

Variables declared without an explicit initial value are given their zero value.

The zero value is:

- `0` for numeric types,
- `false` for the boolean type,
- `""` (the empty string) for strings,
- `nil` for the pointers, slices, maps, functions, interfaces, channels,

## Concurrency

### Race Conditions

- A **race condition** is a situation in which the program does not give the correct result for some interleaving of the operations of multiple goroutines.

- A **data race**, that is, a particular kind of race condition, occurs whenever two goroutines access the same variable concurrently and at least one of the accesses is a write. It follows from this definition that there are three ways to avoid a data race.

    - The first way is not to write the variable.

    - The second way (*channels: share memory by communication*) to avoid a data race is to avoid accessing the variable from multiple goroutines.

    - The third way (*mutual exclusion: `sync.Mutex`, `sync.RWMutex`*) to avoid a data race is to allow many goroutines to access the variable, but only one at a time.

- Synchronization is about more than just the order of execution of multiple goroutines; synchronization also affets memory.

### Race Detector

- The race detector (just add the `-race` flag to your `go build`, `go run`, or `go test` command) studies this steam of events, looking for cases in which one goroutine reads or writes a shared variables that was most recently written by a different goroutine without an intervening synchronization operation. 

- The race detector reports all data races that wre actually executed. However, it can only detect race conditions that occur during a run; it cannot prove that none will ever occur.

    ```go
    func main() {
    	var wg sync.WaitGroup
    
    	var x, y int
    
    	wg.Add(1)
    	go func() {
    		defer wg.Done()
    		x = 1
    		fmt.Printf("y = %d\n", y)
    	}()
    
    	wg.Add(1)
    	go func() {
    		defer wg.Done()
    		y = 1
    		fmt.Printf("x = %d\n", x)
    	}()
    
    	wg.Wait()
    }
    ```
    
    ```sh
    $ go run -race race.go 
    ```
    
    ```console
    x = 0
    ==================
    WARNING: DATA RACE
    Write at 0x00c0000a6020 by goroutine 7:
      main.main.func1()
          /tmp/race.go:16 +0x8a
    
    Previous read at 0x00c0000a6020 by goroutine 8:
      main.main.func2()
          /tmp/race.go:24 +0xaa
    
    Goroutine 7 (running) created at:
      main.main()
          /tmp/race.go:14 +0x119
    
    Goroutine 8 (finished) created at:
      main.main()
          /tmp/race.go:21 +0x166
    ==================
    ==================
    WARNING: DATA RACE
    Read at 0x00c0000a6028 by goroutine 7:
      main.main.func1()
          /tmp/race.go:17 +0xaa
    
    Previous write at 0x00c0000a6028 by goroutine 8:
      main.main.func2()
          /tmp/race.go:23 +0x8a
    
    Goroutine 7 (running) created at:
      main.main()
          /tmp/race.go:14 +0x119
    
    Goroutine 8 (finished) created at:
      main.main()
          /tmp/race.go:21 +0x166
    ==================
    y = 1
    Found 2 data race(s)
    exit status 66
    ```

### Happen before

- Within a single goroutine, reads and writes must behave as if they executed in the order specified by the program.

- That is, compilers and processors may reorder the reads and writes executed within a single goroutine only when the reordering does not change the behavior within that goroutine as defined by the language specification.

- Because of this reordering, the execution order observed by one goroutine may differ from the order perceived by another.

    - For example, if one goroutine executes `a = 1; b = 2`;, another might observe the updated value of `b` before the updated value of `a`. 

- To specify the requirements of reads and writes, we define **happens before**, a partial order on the execution of memory operations in a Go program.

    - If event *e1* happens before event *e2*, then we say that *e2* happens after *e1*.

    - Also, if *e1* does not happen before *e2* and does not happen after *e2*, then we say that *e1* and *e2* **happen concurrently**.

- Within a single goroutine, the happens-before order is the order expressed by the program. 

- Programs that modify data being simultaneously accessed by multiple goroutines must serialize such access.

- To serialize access, protect the data with **channel operations** or other **synchronization primitives** such as those in the `sync` and `sync/atomic` packages. 

### Share by communicating

- *Do not communicate by sharing memory; instead, share memory by communicating.*

    - Go encourages a different approach in which shared values are passed around on channels and, in fact, never actively shared by separate threads of execution.

    - Only one goroutine has access to the value at any given time. Data races cannot occur, by design.

- One way to think about this model is to consider a typical single-threaded program running on one CPU.

    - It has no need for synchronization primitives.

    - Now run another such instance; it too needs no synchronization.

    - Now let those two communicate; if the communication is the synchronizer, there's still no need for other synchronization.

    - Unix pipelines, for example, fit this model perfectly.

    - Although Go's approach to concurrency originates in Hoare's *Communicating Sequential Processes* (CSP), it can also be seen as a type-safe generalization of Unix pipes. 

### Goroutines

- A goroutine has a simple model: it is a function executing concurrently with other goroutines in the same address space.

    - It is lightweight, costing little more than the allocation of stack space.

    - And the stacks start small, so they are cheap, and grow by allocating (and freeing) heap storage as required. 

- Goroutines are multiplexed onto multiple OS threads so if one should block, such as while waiting for I/O, others continue to run.

    - Their design hides many of the complexities of thread creation and management.

    - Prefix a function or method call with the `go` keyword to run the call in a new goroutine. When the call completes, the goroutine exits, silently.

    - The evaluation of `f`, `x`, `y`, and `z` of `go f(x, y, z)` happens in the current goroutine and the execution of `f` happens in the new goroutine. 

        ```go
        package main
        
        import (
        	"fmt"
        	"time"
        )
        
        func main() {
        	// All function values created by this loop “capture” 
        	// and share the same variable—an addressable storage location,
        	// not its value at that particular moment. 
        	for i := 0; i < 5; i++ {
        		go func() {
        			fmt.Print(i, " ")
        		}()
        	}
        
        	time.Sleep(time.Millisecond)
        
        	fmt.Println()
        
        	for i := 0; i < 5; i++ {
        		i := i
        		go func() {
        			fmt.Print(i, " ")
        		}()
        	}
        
        	time.Sleep(time.Millisecond)
        
        	// Output:
        	// 5 5 5 5 5
        	// 4 0 1 2 3	// ignore the order
        }
        ```

### Channels

- Like maps, channels are allocated with `make`, and the resulting value acts as a reference to an underlying data structure.

    - If an optional integer parameter is provided, it sets the buffer size for the channel.

    - The default is zero, for an unbuffered or synchronous channel. 

        ```go
        ci := make(chan int)            // unbuffered channel of integers
        cj := make(chan int, 0)         // unbuffered channel of integers
        cs := make(chan *os.File, 100)  // buffered channel of pointers to Files
        ```
- Receivers always block until there is data to receive.

- The sender blocks only until the value has been copied to the buffer;

- A buffered channel can be used like a semaphore, for instance to limit throughput.

- The assembly line metaphor (pipeline) is useful one for channels and goroutines.

### Parallelization

- Be sure not to confuse the ideas of concurrency—structuring a program as independently executing components—and parallelism—executing calculations in parallel for efficiency on multiple CPUs.

- Although the concurrency features of Go can make some problems easy to structure as parallel computations, Go is a concurrent language, not a parallel one, and not all parallelization problems fit Go's model. 

    ```go
    package runtime // import "runtime"
    
    func NumCPU() int
        NumCPU returns the number of logical CPUs usable by the current process.
    
        The set of available CPUs is checked by querying the operating system at
        process startup. Changes to operating system CPU allocation after process
        startup are not reflected.
    ```
    
    ```go
    package runtime // import "runtime"
    
    func GOMAXPROCS(n int) int
        GOMAXPROCS sets the maximum number of CPUs that can be executing
        simultaneously and returns the previous setting. If n < 1, it does not
        change the current setting. The number of logical CPUs on the local machine
        can be queried with NumCPU. This call will go away when the scheduler
        improves.
    ```

## Errors

- Library routines must often return some sort of error indication to the caller.

- Go's multivalue return makes it easy to return a detailed error description alongside the normal return value.

- It is good style to use this feature to provide detailed error information.

-  By convention, errors have type `error`, a simple built-in interface.


    ```go
    type error interface {
        Error() string
    }
    ```

- The simplest way to create an `error` is by calling `errors.New`, which return a new `error` for a given error message.

- Calls to `errors.New` are relatively infrequent because there's a conveninent wrapper function, `fmt.Errorf`, that does string formatting too.

- When feasible, error strings should identify their origin, such as by having a prefix naming the operation or package that generated the error.

    - For example, in package image, the string representation for a decoding error due to an unknown format is "image: unknown format". 

- Callers that care about the precise error details can use a type switch or a type assertion to look for specific errors and extract details. 

### Panic

- There is a built-in function panic that in effect creates a run-time unrecoverable error that will stop the program.

- The function takes a single argument of arbitrary type—often a string—to be printed as the program dies. 

    ```go
    package builtin // import "builtin"
    
    func panic(v interface{})
        The panic built-in function stops normal execution of the current goroutine.
        When a function F calls panic, normal execution of F stops immediately. Any
        functions whose execution was deferred by F are run in the usual way, and
        then F returns to its caller. To the caller G, the invocation of F then
        behaves like a call to panic, terminating G's execution and running any
        deferred functions. This continues until all functions in the executing
        goroutine have stopped, in reverse order. At that point, the program is
        terminated with a non-zero exit code. This termination sequence is called
        panicking and can be controlled by the built-in function recover.
    ```

### Recover

- When `panic` is called, including implicitly for run-time errors such as indexing a slice out of bounds or failing a type assertion, 

    - it immediately stops execution of the current function

    - and begins unwinding the stack of the goroutine,

    - running any deferred functions along the way.

    - If that unwinding reaches the top of the goroutine's stack, the program dies.

- However, it is possible to use the built-in function `recover` to regain control of the goroutine and resume normal execution.

- A call to `recover` stops the unwinding and returns the argument passed to panic.

    - Because the only code that runs while unwinding is inside deferred functions, recover is only useful inside deferred functions. 

    ```go
    func F() {
    	panic("F: panic.")
    }
    
    func G() {
    	defer func() {
    		e := recover()
    		if e != nil {
    			fmt.Println("G: recover:", e)
    		}
    	}()
    
    	F()
    }
    
    func main() {
    	G()
    	// Output:
    	// G: recover: F: panic.
    }
    ```

## Testing

- The `go test` subcommand is a test driver for Go packages that are orgnized according to certain conventions.

- In a package directory, files whose names end with `_test.go` are not part of the package ordinarily built by `go build` but are a part of it when built by `go test`.

- Within `*_test.go` files, three kinds of functions are treated specially: **tests**, **benchmarks**, and **examples**.

    - A **test function**, which is a function whose name begins with **Test**, exercises some program logic for correct behavior; `go test` calls the test function and report the result, which is either **PASS** or **FAIL**.

    - A **benchmark function** has a name beginning with **Benchmark** and measures the performance of some operation; `go test` reports the mean execution time of the operation.

    - And an **example function**, whose name starts with **Example**, provides machine-checked documentation.

## Modules

```go
// In Go, if an old package and a new package have the same import path,
// the new package must be backwards compatible with the old package.
```

```go
// There is certainly a cost to needing to introduce a new name for each backwards-incompatible API change,
// but as the semver FAQ says, that cost should encourage authors to more clearly consider
// the impact of such changes and whether they are truly necessary.
```
- A *module* is a collection of related Go packages that are versioned together as a single unit.

- Modules record precise dependency requirements and create reproducible builds.

- Most often, a version control repository contains exactly one module defined in the repository root.

- Summarizing the relationship between repositories, modules, and packages:

    - A repository contains one or more Go modules.

    - Each module contains one or more Go packages.

    - Each package consists of one or more Go source files in a single directory.

- Modules must be semantically versioned according to [semver](https://semver.org/), usually in the form `v(major).(minor).(patch)`, such as `v0.1.0`, `v1.2.3`, or `v1.5.0-rc.1`.

    - The leading `v` is required.

    - If using Git, tag released commits with their versions. 

- A module is defined by a tree of Go source files with a `go.mod` file in the tree's root directory.

- A module declares its identity in its `go.mod` via the `module` directive, which provides the *module path*.

    - The import paths for all packages in a module share the module path as a common prefix.

    - The module path and the relative path from the `go.mod` to a package's directory together determine a package's import path.

- In Go source code, packages are imported using the full path including the module path.

```sh
$ go help modules
```

```console
$ go help go.mod
```

```console
$ go help module-private
```

```sh
$ go help goproxy
```

```sh
$ go env GOPROXY # https://proxy.golang.org,direct
```

```sh
$ go env -w GOPROXY=https://goproxy.cn,direct
```

```sh
$ go env GOPROXY # https://goproxy.cn,direct
```

```sh
$ go help gopath
```

## References

1. [https://github.com/golang/go/wiki/Iota](https://github.com/golang/go/wiki/Iota)
1. [https://golang.google.cn/ref/spec#Iota](https://golang.google.cn/ref/spec#Iota)
1. [https://stackoverflow.com/questions/24790175/when-is-the-init-function-run](https://stackoverflow.com/questions/24790175/when-is-the-init-function-run)
1. [https://golang.google.cn/doc/effective\_go.html](https://golang.google.cn/doc/effective_go.html)
1. [https://golang.google.cn/ref/mem](https://golang.google.cn/ref/mem)
1. [Capturing Iteration Variables in Go Language](/2017/05/15/capturing-iteration-variables-in-go-language/)
1. [Errors in Go language](/2017/05/15/errors-in-go-language/)
1. [Object-oriented Programming in Go Language](/2017/05/21/object-oriented-programming-in-go-language/)
1. [Goroutines and Channels in Go Lanugage](/2017/06/11/goroutines-and-channels-in-go-lanugage/)
1. [Concurrency with Shared Variables in Go Language](/2017/06/17/concurrency-with-shared-variables-in-go-language/)
1. [Testing in Go Language](/2017/07/01/testing-in-go-language/#benchmark-functions)
1. [https://research.swtch.com/vgo-import](https://research.swtch.com/vgo-import)
1. [https://semver.org/](https://semver.org/)
1. [https://research.swtch.com/vgo-import](https://research.swtch.com/vgo-import)
1. [https://research.swtch.com/vgo-module](https://research.swtch.com/vgo-module)
1. [https://research.swtch.com/vgo-mvs](https://research.swtch.com/vgo-mvs)
1. [https://github.com/golang/go/wiki/Modules](https://github.com/golang/go/wiki/Modules)
1. [https://medium.com/@adiach3nko/package-management-with-go-modules-the-pragmatic-guide-c831b4eaaf31](https://medium.com/@adiach3nko/package-management-with-go-modules-the-pragmatic-guide-c831b4eaaf31)
1. [Practical Go: Real world advice for writing maintainable Go programs](https://dave.cheney.net/practical-go/presentations/qcon-china.html)
