---
title: "Tricks in Julia"
description: "Usefull tricks in Julia"
date: "2014-12-21"
categories: 
    - "Julia"
    - "Programming"
---

After doing various small Julia projects I've had to learn a number of tricks or solutions to small problems, that I think would be usefull to collect:

### Parsing Strings

Extract 3 numbers from a string into 3 variables:

    a, b, c = map(int, split("11 12 13"))

### Initialization

There are some gotchas with Julia initialization if you come from C/C++ background although this should not be so odd to Python or Ruby developers.

    > type Point
        x::Int
        y::Int
      end
    
    > points = fill(Point(0, 0), 4)
    4-element Array{Point,1}:
     Point(0,0)
     Point(0,0)
     Point(0,0)
     Point(0,0)
     
That looks all fine, until you make a change and get this:
    
    > points[2].x = 20
    > points
    4-element Array{Point,1}:
     Point(20,0)
     Point(20,0)
     Point(20,0)
     Point(20,0)

That was probably not as expected. The reason for this is that references to the created `Point` object in fill is copied not the values themselves. So what we actually got to do is:

    points = [Point(0, 0) for _ in 1:4]

## Integers

There are a lot of issues with just dealing with something simple as integers. First of all how do yo get the min and max values of the integers you are using:

    > typemax(Int8)
    127
    > typemax(Uint8)
    0xff # which is 255 decimal
    > typemin(Int8)
    -128

One also has to remember that Julia integers do not get promoted to larger values when overflowing. So e.g. taking the factorial of a really big number like 100 requires using `BigInt`:

     > factorial(BigInt(25))
     15511210043330985984000000


### Division

One thing I got confused by being more used to C/C++ is how division works in Julia. There are several ways of doing it depending on what you want. Using integers in division does not produce integers automatically. The default is to give floating point answer:

    > 10 / 6
    1.6666666666666667
    
Which has of course the minor inaccuracies of floating point arithmetic. If you want accurate results you can compute fractions with the `//` division operator:
    
    > a = 10 // 6
    5//3
    > a.num
    5
    > a.div
    3

If you want to work with C/C++ style division then we use the `div`, `rem` and `mod` functions:

    > div(10, 6)
    1
    > rem(10, 6)
    4
    
C/C++ style remainder

    > 10 % 6
    4
    
Modulus is not quite the same as remaineder. E.g. 

    > rem(-21, 4)
    -1
    > mod(-21, 4)
    3
  
With modulus we can imagine doing calculations on a clock.

### Reading files

Julia has some nice ways you can compose functions to read files.

    lines = open(readlines, "myfile.txt")
    
This corresponds to writing:

    f = open("myfile.txt")
    lines = readlines(f)
    close(f)
    
With the julia `do end` syntax sugar you can use this to conveniently read a file and not forget to close it:

    open("myfile.txt") do f
       println(readline(f))     # print out first line in file
    end                         # file stream automatically closes here
    
### Working with dates

This is just to show how to deal with a common case:

    > using Dates
    > (date, time) = split("5/8/2014 0:16:03")
    2-element Array{SubString{ASCIIString},1}:
     "5/8/2014"
     "0:16:03" 
     
Then as shown before we can unpack an array into separate variables like this:

    > year, month, day = reverse(map(int, split(date, '/')))
    3-element Array{Int64,1}:
     2014
        8
        5
    > date = Date(year, month, day)
    2014-08-05
    
You could also do this in one go by unpacking an array into function arguments using `...`:

    > Date(reverse(map(int, split(date, '/')))...)
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

    type FooBar
    	foo::Int64
    	bar::String
    end

    > a = FooBar(42, "The answer to everything")
    > a.bar
    "The answer to everything"
    > a.foo
    42

However accessing fields not known at compile time is different. Imagine the user inputs the name of a field to access or the names of fields are read of a file. Unlike Ruby and Python, Julia's objects are not implemented as hashtables. So accessing fields given a name is a bit different:

    > a.(:bar)
    "The answer to everything"

If you only got the name of a field as a string you can do:

    > a.(symbol("bar"))
    "The answer to everything"
    
To find out what fields actually exist you can use `names`:

    > names(FooBar)
    2-element Array{Symbol,1}:
     :foo
     :bar
    
Alternatively if you got an instance you can see both the name of the fields and their values with `dump`:

    > dump(a)
    FooBar 
      foo: Int64 42
      bar: ASCIIString "The answer to everything"
      
Most of these code snippets or approaches I picked up from doing small things like implement an algorithm in [project Euler](https://projecteuler.net). I hope to write some bigger piece of code and see how Julia stacks up and how well suited Julia is for more software engineering type of challenges.
