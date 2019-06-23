---
title: "An introduction to Swift for Python developers"
description: "An attempt at introducing Swift in a way that Python developers can relate to."
date: "2014-11-08"
categories: 
    - "Swift"
    - "Python"
    - "Programming"
---

While Swift fundamentally isn't very much like Python, it looks as if it has a number of similarities on the surface. For instance both have different REPL environments. These are great ways of learning how to code. I can use the Python REPL as a calculator:

	>>> 2 + 3
	5
	>>> 10 + 2 - 5
	7
	>>> 3.5 * 2
	7.0
	>>>
		
We can do much the same with Swift:

	  1> 2 + 3
	$R0: Int = 5
	  2> 10 + 2 - 5
	$R1: Int = 7
	  3> 3.5 * 2
	$R2: Double = 7
	  4>
		
Although this is quite similar, we can already get a hint here that Swift is different and cares a lot more about type. Along with all results we can see that Swift derives the type of the result. Swift is however a lot stricter language. In Python I can write:

	>>> x = 4
	>>> y = 5
	>>> z = x + y
	>>> z
	9
	>>>
	
Despite the fact that `x`, `y` and `z` does not exist already. By assigning we created them. So there is no way to avoid having the accident of thinking you are assigning to a new variable when in fact you are overwriting the value of an existing one. In Swift if we try this we get:

		4> x = 4
	error: use of unresolved identifier 'x'
	x = 4
	^
	
		4>
		
Instead we have to write `var` to indicate we are creating a new variable:

	 4> var x = 4
	x: Int = 4
	  5> var y = 5
	y: Int = 5
	  6> var z = x + y
	z: Int = 9
	  7>	
	  
We can assign new values to these

		7> x = 30
		8> x
	$R3: Int = 30
		9>
		
However if we made the variable constant by using `let` we could not:

	 12> let x = 4
	x: Int = 4
	 13> x = 5
	error: cannot assign to 'let' value 'x'
	x = 5
	~ ^

Swift is also a lot more strict about not mixing types. While you can do this:

		 13> 2.3 + 3
	$R4: Double = 5.2999999999999998
	
You can't do this:

	 14> let x = 2.3
	x: Double = 2.2999999999999998
	 15> let y = 3
	y: Int = 3
	 16> x + y
	error: cannot invoke '+' with an argument list of type '(Double, Int)'
	x + y
	~~^~~

You will have to explicitly convert the values:

	 17> x + Double(y)
	$R6: Double = 5.2999999999999998
	
## Type annotation

Sometimes Swift will not be able to guess what type you wanted a variable to be. E.g. Swift could not know that `3` was supposed to be a `Double`. But what you can do is either write:

	  1> let y = 3.0
	y: Double = 3 
Or you could annotate the variable y with the type:

	  2> let y : Double = 3
	y: Double = 3 
	
	
## Arrays and Dictionaries

One of the nice things about Python the batteries included approach. We get usefull data types like arrays and dictionaries built into the language. So we can easily create arrays and dictionaries and manipulate them:

	>>> numbers = [2, 4, 6, 8]
	>>> squared = map(lambda x: x*x, numbers)
	>>> squared[0:3]
	[4, 16, 36]	

Swift follows a similar philosophy and allows you to create collections of objects with succint syntax:

	  4> let numbers = [2, 4, 6, 8] 
	numbers: [Int] = 4 values {
	  [0] = 2
	  [1] = 4
	  [2] = 6
	  [3] = 8
	}
	  5> let squared = map(numbers, {x in x*x}) 
	squared: [Int] = 4 values {
	  [0] = 4
	  [1] = 16
	  [2] = 36
	  [3] = 64
	}
	
As you can see this is very similar. But Swift has a number of alternative ways to do this with ruby inspired syntax sugar:

	  8> let squared = map(numbers) {x in x*x}
	squared: [Int] = 4 values {
	  [0] = 4
	  [1] = 16
	  [2] = 36
	  [3] = 64
	}
	  9> let squared = numbers.map {x in x*x} 
	squared: [Int] = 4 values {
	  [0] = 4
	  [1] = 16
	  [2] = 36
	  [3] = 64
	}

To slice arrays we can either do that with a closed `...` or open interval `..<`:

	  6> squared[0...3]
	$R0: Slice<Int> = 4 values {
	  [0] = 4
	  [1] = 16
	  [2] = 36
	  [3] = 64
	}
	  7> squared[0..<3]
	$R1: Slice<Int> = 3 values {
	  [0] = 4
	  [1] = 16
	  [2] = 36
	}


## Functions

With python you can easily create a function like this:

	def add(a, b):
		return a + b
		
However that is not going to work in Swift:

	 21> func add(a, b) { 
	 22.     return a + b 
	 23. }    
	error: use of undeclared type 'a'
	func add(a, b) {
	         ^
	error: use of undeclared type 'b'
	func add(a, b) {
	            ^
Because Swift needs to know what the type of your variables are. Unlike like our previous example where we assigned a value right away, there is no way for Swift to know what the type of `a` and `b` is going to be. It can't infer it, so we have annotate the variables:

	func add(a: Int, b: Int) -> Int {
		return a + b
	}
	
But this has some limitations compared to the Python example:

	 4> add(2, 3)
	$R0: Int = 5
	  5> add(2.0, 3.3)
	error: cannot convert the expression's type '(FloatLiteralConvertible, FloatLiteralConvertible)' to type 'Int'
	add(2.0, 3.3)
	^~~~~~~~~~~~~

## Generics
Python allowed us to use any type of number, but now we have limited ourselves to integers. We can't e.g. use float values. The Swift solution for this is Generics as found in C#, Java and many other languages. If this had been C++ I could have written something like this:

	func add<T>(a: T, b: T) -> T {
		return a + b
	}

To indicate that `a` and `b` can be of any type but they have to be of the same type `T`. Likewise with the result. However this would give us the error:

	error: cannot invoke '+' with an argument list of type '(T, T)'
    return a + b
           ~~^~~
           
The reason being that an arbitrary type `T` doesn't support adding. C++ solves this problem by checking every use of `add()` in the program and making sure only types which support `+` is provided as arguments. But that is actually a terrible solution as it produces errors inside the function at the offending line. Thus users of a third party library has to understand how a third party function is implemented. So the Swift way of doing this is:

	func add<T: Addable>(a: T, b: T) -> T {
		return a + b
	}

To indicate that that the type `T` has to implement the protocol `Addable`. A protocol is like a C# or Java interface. It lists functions which any class which wants to adhere to the protocol has to implement. In this case we say that `T` has to be of a type that support the `+` operator. Unfortunatly no `Addable` protocol exists in Swift. In fact Swift is at the moment quite lacking in protocols. The whole Swift standard library is quite tiny at the momment. So the above code will give the error:

		error: use of undeclared type 'Addable'
	func add<T: Addable>(a: T, b: T) -> T {
	            ^~~~~~~ 

Thus we have to specify the `Addable` protocol ourselves:

	protocol Addable {
	    func +(a: Self, b: Self) -> Self
	}
	
The use of `Self` looks strange. Self is a sort of placeholder for the type of the object the method is being invoked on. So e.g. when `+` is being invoked on an `Int` object then `Self` is the same as `Int`.

Now we have the protocol but that only takes us half way. No concrete type is actually implementing this protocol. If this was Java or C++ that would be the end of this, because we would have had to implement our own number types somehow. Existing types can't implement a new interface. Except in Swift you can. We can add methods to existing types or have existing types implement new protocols through extensions:

	extension Int: Addable {}
	extension Double: Addable {}
	
This specifies that `Int` and `Double` implements `Addable`. Notice we don't have to actually implement any methods ourselves because `Int` and `Double` already have the `+` method/operator. Now we can write:

	  1> add(1, 2)
	$R0: (Int) = 4
	  2> add(2.0, 3.0)
	$R1: (Double) = 5
	
However if we use a `Float` we get:

	  3> let x : Float = 2
	x: Float = 2
	  4> let y : Float = 3
	y: Float = 3 
	  5> add(x, y)
	error: Type 'Float' does not conform to protocol 'Addable'
	
We get an error because we have not specified that `Float` implements our `Addable` protocol.