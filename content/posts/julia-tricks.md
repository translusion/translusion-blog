---
title: "Tricks in Julia"
description: "Usefull tricks in Julia for dealing with numbers, strings, dates and more."
date: "2019-06-25"
categories: 
    - "Julia"
    - "Programming"
---

After doing various small Julia projects I've had to learn a number of tricks or solutions to small problems, that I think would be usefull to collect:

### Parsing Strings

Extract 3 numbers from a string into 3 variables:

```julia
a, b, c = parse.(Int, split("11 12 13"))
```

This requires some explanation. `parse(Int, s)` will parse a single string `s` as a number. However when you call `parse.(Int, ["11", "12", "13"])`, the dot causes `parse` to be called for every element of the array. 

### Initialization

There are some gotchas with Julia initialization if you come from C/C++ background although this should not be so odd to Python or Ruby developers.


    julia> mutable struct Point
             x::Int
             y::Int
           end
    
    julia> points = fill(Point(0, 0), 4)
        4-element Array{Point,1}:
         Point(0,0)
         Point(0,0)
         Point(0,0)
         Point(0,0)
     
That looks all fine, until you make a change and get this:
    
    julia> points[2].x = 20
    julia> points
    4-element Array{Point,1}:
     Point(20,0)
     Point(20,0)
     Point(20,0)
     Point(20,0)

That was probably not as expected. The reason for this is that references to the created `Point` object in fill is copied not the values themselves. So what we actually got to do is:

    points = [Point(0, 0) for _ in 1:4]

## Integers

There are a lot of issues with just dealing with something simple as integers. First of all how do yo get the min and max values of the integers you are using:

    julia> typemax(Int8)
    127
    
    julia> typemax(UInt8)
    0xff # which is 255 decimal
    
    julia> typemin(Int8)
    -128

One also has to remember that Julia integers do not get promoted to larger values when overflowing. So e.g. taking the factorial of a really big number like 100 requires using `BigInt`:

     julia> factorial(BigInt(25))
     15511210043330985984000000


### Division

One thing I got confused by being more used to C/C++ is how division works in Julia. There are several ways of doing it depending on what you want. Using integers in division does not produce integers automatically. The default is to give floating point answer:

    julia> 10 / 6
    1.6666666666666667
    
Which has of course the minor inaccuracies of floating point arithmetic. If you want accurate results you can compute fractions with the `//` division operator:
    
    julia> a = 10 // 6
    5//3
    
    julia> numerator(a)
    5
    
    julia> denominator(a)
    3

If you want to work with C/C++ style division then we use the `div`, `rem` and `mod` functions:

    julia> div(10, 6)
    1
    
    > rem(10, 6)
    4
    
C/C++ style remainder

    julia> 10 % 6
    4
    
Modulus is not quite the same as remaineder. E.g. 

    julia> rem(-21, 4)
    -1
    
    julia> mod(-21, 4)
    3
  
With modulus we can imagine doing calculations on a clock.

### Reading files

Julia has some nice ways you can compose functions to read files.

```julia
lines = open(readlines, "myfile.txt")
```

This corresponds to writing:

```julia
f = open("myfile.txt")
lines = readlines(f)
close(f)
```

With the julia `do end` syntax sugar you can use this to conveniently read a file and not forget to close it:

```julia
open("myfile.txt") do f
   println(readline(f))     # print out first line in file
end                         # file stream automatically closes here
```
    
### Working with dates

This is just to show how to deal with a common case:

    julia> using Dates
    
    julia> (date, time) = split("5/8/2014 0:16:03")
    2-element Array{SubString{ASCIIString},1}:
     "5/8/2014"
     "0:16:03" 
     
Then as shown before we can unpack an array into separate variables like this:

    julia> y, m, d = reverse(parse.(Int, split(date, '/')))
    3-element Array{Int64,1}:
     2014
        8
        5
        
    julia> date = Date(y, m, d)
    2014-08-05
    
You could also do this in one go by unpacking an array into function arguments using `...`:

    julia> Date(reverse(parse.(Int, split(date, '/')))...)
    2014-08-05

### Enumeration

Regular enumeration in Julia is straightforward but it might not be obvious how you enumerate over both values and keys or indicies in an array or dictionary:

    > for (index, value) in enumerate(["one", "two", "three"])
               println(index, ": ", value)
           end
    1: one
    2: two
    3: three
    
### Accessing fields of a type

The syntax for accessing fields known at compile time is similar in Julia as in most other languages with Algol syntax:

```julia
struct FooBar
	foo::Int64
	bar::String
end
```

    julia> a = FooBar(42, "The answer to everything")
    
    julia> a.bar
    "The answer to everything"
    
    julia> a.foo
    42

However accessing fields not known at compile time is different. Imagine the user inputs the name of a field to access or the names of fields are read of a file. Unlike Ruby and Python, Julia's objects are not implemented as hashtables. So accessing fields given a name is a bit different:

    julia> getfield(a, :bar)
    "The answer to everything"

If you only got the name of a field as a string you can do:

    julia> getfield(a, Symbol("bar"))
    "The answer to everything"
    
To find out what fields actually exist you can use `names`:

    julia> fieldnames(FooBar)
    (:foo, :bar)
    
Alternatively if you got an instance you can see both the name of the fields and their values with `dump`:

    julia> dump(a)
    FooBar
      foo: Int64 42
      bar: String "The answer to everything"
      
Most of these code snippets or approaches I picked up from doing small things like implement an algorithm in [project Euler](https://projecteuler.net). I hope to write some bigger piece of code and see how Julia stacks up and how well suited Julia is for more software engineering type of challenges.
