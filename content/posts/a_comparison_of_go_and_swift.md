---
title: "Comparison of the Go tour with Swift"
description: "Go has a nice tour of the language showing of the featuers. This is comparison of Swift to Go using examples from the tour."
date: "2014-07-15"
categories: 
    - "Swift"
    - "Go"
    - "Programming"
---

One of my favorite programming languages is Go, so I thought it might be fun to do a bit of a superficial comparison of it with Swift which I am hoping will be a new favorite given that I work professionally as an iOS developer. Go takes such a bare bones and simple approach to programming that one can quite quickly get a sense of what it is like using it. It will take longer time to make up an opinion about Swift.


### Hello world
The Go tour starts with the infamous "Hello world" program:

	package main
	
	import "fmt"
	
	func main() {
	    fmt.Println("Hello, 世界")
	}
	
As you can see below Swift looks very similar at first glance. Although `main` doesn't have any special meaning. It is just a function.
	
	import Foundation
	
	func main() {
    	println("Hello, 世界")
	}

So we should rather replace the code with:

	import Foundation
	
    println("Hello, 世界")
	
### Import

In Go one divides the code into lots of small packages, which you can import as shown in the tour example:

	package main
	
	import (
	    "fmt"
	    "math/rand"
	)
	
	func main() {
	    fmt.Println("My favorite number is", rand.Intn(10))
	}
	
With Swift we typically import large frameworks containg a number of libraries. For just simple math libraries `Foundation` or `Cocoa` isn't needed. You can import e.g. the `Darwin` framework.
	
	import Darwin
	
	let favorite = rand() % 10
	println("My favorite number is", favorite)
	
### Functions
	
	func add(x int, y int) int {
	    return x + y
	}
	
	func main() {
	    fmt.Println(add(42, 13))
	}
	
Swift is similar placing type info last instead of traditional C/C++ way of putting type first.

	func add(x: Int, y: Int) -> Int {
	    return x + y
	}
	
	
	println(add(42, 13))
	

### Multiple results
	
	func swap(x, y string) (string, string) {
	    return y, x
	}
	
	func main() {
	    a, b := swap("hello", "world")
	    fmt.Println(a, b)
	}

Swift doesn't really return multiple values like Go functions but instead returns tuples which lets you accomplish basically the same thing.

	func swap(x: String, y: String) -> (String, String) {
	    return (y, x)
	}
	
	var (a, b) = swap("hello", "world")
	println("\(a) \(b)")
	
Another minor difference is that Swift can do `println(a)` but not `println(a, b)`

### Named results

You can name the results in both Go and Swift but the meaning is very different, because Go has multiple return variables and Swift has tuples instead. In the Go example `x` and `y` can be assigned to directly:

	func split(sum int) (x, y int) {
	    x = sum * 4 / 9
	    y = sum - x
	    return
	}
	
	func main() {
	    fmt.Println(split(17))
	}

You can't do that in Swift, because naming here has a different meaning. What you are doing is naming the entries in the tupple so, that you can refer to the entries later. It is sort of like making a struct on the fly.

	func split(sum: Int) -> (x: Int, y: Int) {
	    var x = sum * 4 / 9
	    var y = sum - x
	    return (x, y)
	}
	
	var result = split(17)
	
	println(result.x)
	println(result.y)
	
### Variables

In Go `var` start the definition of one or more variables with the type last.
	
	var i int
	var c, python, java bool
	
	func main() {
	    fmt.Println(i, c, python, java)
	}
	
Swift is similar, except in Swift variables are never automatically initialized to 0 or false like in Go. If a variable is not initialized it is undefined and you are not allowed to use it. So you  must assign to it before you can read from it. Also multiple assignment relies on the tuple concept.

	var i: Int
	var c, python, java: Bool
	var (c, python, java) = (false, false, false)
	
	println("\(c) \(python) \(java)")

So when doing initialization Go and Swift look more similar:

	var i, j int = 1, 2
	var c, python, java = true, false, "no!"
	
	func main() {
	    fmt.Println(i, j, c, python, java)
	}
	

With the difference that type annotations work a bit different. If you want every variable in the tupple to be of type `Int` you have to specify that for each individual variable.

	var (i: Int, j: Int)  = (1, 2)
	var (c, python, java) = (true, false, "no!")
	
	println("\(i) \(j) \(c) \(python) \(java)")
	
The tradeoff here is that that allows you to chose which variables you want infered and which ones you want to specify. So with Swift you could write:

	var (i: Float, j)  = (1, 2)

which would turn `i` into a floating point number while `j` would be infered to be an integer. Writing: 

	var i Float64, j  = 1, 2
	
in Go would be a compiler error. Type has to come last and apply to all variables.

### Shorthand

In Go there is a shorthand notation using `:=` when infering variables so you could write:

    k := 3
    c, python, java := true, false, "no!"
    
There isn't anything similar in Swift.

### Type conversions

Both Swift and Go are much more strict on type conversions than what is traditional for C/C++. Conversion from `int` to `float` won't happen automatically.

    var x, y int = 3, 4
    var f float64 = math.Sqrt(float64(x*x + y*y))
    var z int = int(f)
    fmt.Println(x, y, z)
    
Swift works similar:

    var (x : Int, y : Int) = (3, 4)
    var f : Float = sqrt(Float(x*x + y*y))
    var z : Int = Int(f)
    println("\(x) \(y) \(z)")
    
Both Swift and Go can treat primitive types as first class citizens. You can create a type from a e.g. an `int` in Go and add method to it. Swift is slightly different in this regard in that the most primitive types are not exposed directly. `Int`, `Float` etc are really structs which may contain methods. Conversion from say `Float` to `Int` isn't built into the language but rather exploits one of the initializer methods on the `Int` type that takes a `Float` as an argument.

The example above could also be simplified. We don't need to write types as often. Here is an example from a Swift REPL session, demonstrating this:

    $ swift
    Welcome to Swift!  Type :help for assistance.
      1> var (x, y) = (3, 4)
    x: Int = 3
    y: Int = 4
    
As you can see the REPL will inform us that it infered the `x` and `y` to be of type `Int`. Regular math functions such as `sqrt` is in the *Darwin* package.

      2> import Darwin
      3> var f = sqrt(Float(x*x + y*y)) 
    f: (Float) = 5
      4> var z = Int(f)
    z: Int = 5
    
Swift infers that the result of `sqrt` has to be of type `Float` if the input is of type `Float`. You can't quite do the same in Go since Go does not overload functions on types.

### Structs

Both Go and Swift support structs. There meaning is quite different however, since Swift is more like C# in that it has the notion of both structs and classes to deal with the lack of pointers. In Go it is idiomatic to access variables directly without accessors.

    type Vertex struct {
        X int
        Y int
    }

    func main() {
        v := Vertex{1, 2}
        v.X = 4
        fmt.Println(v.X)
    }

In Swift we can't access member variables directly. Although the code looks almost identical Swift always creates accessors conceptually. In machine code it might of course perform a direct access. 

    struct Vertex {
      var x : Int
      var y : Int
    }
    
    var v = Vertex(x: 1, y: 2)
    v.x = 4
    println(v.x)
    
In Swift initializers we also have to give the names of the arguments. With Go this is optional. The Go code could have been written like this:

    v := Vertex{X: 1, Y: 2}
    
A significant conceptual difference is that in Swift creating any object requires running an initializer function. In the Go example the data structure is just initialized directly. No function call is done. If you want an initialiser function for `Vertex` objects in Go you have to write a free function explicity:

    func NewVertex(x, y int) Vertex {
      return Vertex{x, y}
    }
    
### Swift inout and Go pointers

Go has real pointers, but Swift allows you to things which makes it look as if it has pointers. Take this example of pointer usage in Go:

    func swapXY(v *Vertex) {
        tmp := v.X
        v.X = v.Y
        v.Y = tmp
    }
    
    v := Vertex{1, 2}
    swapXY(&v) // v becomes Vertex{2, 1}
    
You can do almost exactly the same in Swift:

    func swapXY(inout v: Vertex) {
       var tmp = v.x 
       v.x = v.y 
       v.y = tmp 
    } 
    
    var v = Vertex(x: 1, y: 2)
    swapXY(&v)
    
However except for when using unsafe features you can't represent an inout function argument as a separate variable in Swift code the ampersand, only makes sense in the context of calling a function. So this will NOT work in Swift:

    var p = &v // error: type 'inout Vertex' of variable is not materializable
    swapXY(p)

However Go has real pointers which allows you to write:

    p := &v
    swapXY(p)
    
Go pointers can be passed around and stored like any other variable. This also displays so key design differences between Swift and Go. Swift is generally designed to make code safe and prevent you from accidentally doing something dangerous or wrong. Because you have to prefix with ampersand at the call site it will always be clear to the code reader when you are potentially modifying a struct. Go instead opts for simplicity by having one simple feature which has many use cases.

### Classes

This naturally brings us to classes. Classes is the way Swift allows us to simulate some of the properties of pointers in Go. If we defined `Vertex` as a class instead of a struct we could perform the `swapXY` without using `inout` because variables of class types are always pointers.

    class Vertex {
      var x : Int
      var y : Int
      
      // classes don't have initializers automatically made
      init(x: Int, y: Int) {
        self.x = x
        self.y = y
      }
    }
    
    // this works because v is now essentially a pointer
    func swapXY(v: Vertex) {
       var tmp = v.x 
       v.x = v.y 
       v.y = tmp 
    }
    
    var v = Vertex(x: 1, y: 2)
    swapXY(v) // Becomes Vertex(2, 1)
    
    
So Swift structs allow us to use variables as values. Meaning they get copied with every assignment (except when using `inout` arguments) while classes let use variables as pointers (assignment does not cause a copy). With Go we can manage with just `struct` because we can make pointers to values using the ampersand operator.

Other than that Swift classes works much the same as classes in other languages. It is what allows us to support implementation inheritance. Go does not support implementation inheritance. Go is not alone in this regard new languages like Clojure and Rust also tries to avoid inheritance. For Swift that was not an option as one of the important design goals was to be able to smoothly interface with Objective-C and existing Cocoa libraries which use inheritance hierarchies heavily.

Both Swift and Go allows you to specify methods on structs:

    type Vertex struct {
        X, Y float64
    }

    func (v Vertex) Abs() float64 {
        return math.Sqrt(v.X*v.X + v.Y*v.Y)
    }

    func main() {
        v := Vertex{3, 4}
        fmt.Println(v.Abs())
    }
    
In Go the methods are placed outside the struct, while in Swift they must be inside. To Go approach makes it easy to split method definitions over several files. You can achieve the same in Swift using class extensions.

    struct Vertex {
      var x : Float
      var y : Float
      
      func abs() -> Float {
        return sqrt(x*x + y*y)
      }
    }
    
    var v = Vertex(x:3, y:4)
    println(v.abs())
    
    
However in Swift it would be more idiomatic to treat the length of a vertex as a calculated property. To do that the code becomes:

    struct Vertex {
      var x : Float
      var y : Float
    
      var abs : Float {
        return sqrt(x*x + y*y)
      }
    }
  
    var v = Vertex(x:3, y:4)
    println(v.abs) // Looks like member variable access

### Swift Protocols and Go Interfaces

Lack of classes does of course not mean that Go doesn't support polymorphism. It does that using *interfaces* which are quite similar to Swift *protocols*. We don't have to explicitly define that a struct implements an interface in Go, so if we define a interface like this:

    type Abser interface {
        Abs() float64
    }
    
We can automatically use `Vertex` with it since it has a `Abs()` method even if `Vertex` came from a third party library we didn't own and couldn't modify.

    func main() {
        var a Abser
        v := Vertex{3, 4}
        a = v

        fmt.Println(a.Abs()) // displays 5
    }
    
Even though Swift does not allow classes or struct to implicitly implement a protocol like Go, it does still allow us to make third party classes and struct implement protocols they were never designed for. If we write:

    protocol Abser { 
      var abs : Float { get }
    }

    var v : Abser = Vertex(x:3, y:4)
    
We will get the error 

  > type 'Vertex' does not conform to protocol 'Abser'

So we can't do like Go, but we can tell Swift that `Vertex` implements `Abser` using a class extension (or perhaps we should call it struct extension in this case). Here is an excerpt from a Swift REPL session demonstrating that:

     13> extension Vertex : Abser { 
     14. }    
     15> var v : Abser = Vertex(x:3, y:4) 
    v: Vertex = {
      x = 3
      y = 4
    }
     16> v.abs
    $R0: Float = 5
     17>  
    
If our `Vertex` class had not already implemented `abs` we could have added it with the class extension:

    extension Vertex : Abser {
        var abs : Float {
          return sqrt(x*x + y*y)
        }    
    }    
    
    
# Conclusion

Go and Swift have a number of superficial similarities. So you can write code that looks almost identical and does the same thing but the way it works under the hood might be radically different. E.g. using pointers in Go and inout variables in Swift often looks almost identical but works very different.