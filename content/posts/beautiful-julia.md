---
title: "Beautiful Julia: Cool language constructs and Tricks for Beginners"
description: "Examples of Julia language constructs based of similarly named Python blog."
date: "2014-12-14"
categories: 
    - "Julia"
    - "Python"
    - "Programming"
---

I just read [this blog][pyblog] about cool language constructs in Python. Before clicking it I thought to myself: are there cool things only Python can do or could I easily replicate this in my favorite script language [Julia][julia]?

So without further ado, her are some examples.

### Reverse an iterable

There are two ways of doing this

     julia> a = [1, 2, 4]
     3-element Array{Int64,1}:
      1
      2
      4

The most obvious way is the `reverse` function

     julia> reverse(a)
     3-element Array{Int64,1}:
      4
      2
      1

But we can also do something similar to the Python example, exept Julia requires you to be a bit more explicit about the start and end of the slice

     julia> a[end:-1:1]
     3-element Array{Int64,1}:
      4
      2
      1

This of course works for any types:

    julia> reverse(b)
    (4,3,2)

    julia> b[end:-1:1]
    (4,3,2)

    julia> c = "This is a string"
    "This is a string"

    julia> reverse(c)
    "gnirts a si sihT"

Julia follows the Ruby convention of suffixing with `!` if the function is mutating, so we can be confident these functions created copies.

### Swapping the values of two variables

    julia> a, b = 1, 2
    (1,2)

    julia> a, b = b, a
    (2,1)

    julia> a
    2

    julia> b
    1

In the python example, the author Praveen Kumar disassembled the Python code to demonstrate how it worked by showing the bytecode. That will be quite different for [Julia][julia] as Julia JITs different implementations based on the types of arguments. That is why Julia is so fast. E.g. when we define a function that swaps in Julia we get a generic function which will be specialized when called by the Just in Time compiler:

    julia> foo(a, b) = a, b = b, a
    foo (generic function with 1 method)

So when dumping the assembly with `code_native` we have to specify what the types of arguments  will be as second argument:

    julia> code_native(foo, (Int, Int))
    	.section	__TEXT,__text,regular,pure_instructions
    Filename: none
    Source line: 1
    	push	RBP
    	mov	RBP, RSP
    Source line: 1
    	movq	XMM1, RDI
    	movq	XMM0, RSI
    	punpcklqdq	XMM0, XMM1      ## xmm0 = xmm0[0],xmm1[0]
    	pop	RBP
    	ret

If we want the bytecode like Python we get something that I consider less readable as it is LLVM based which contains a lot of type information to be able to generate efficient machien code output:

    julia> code_llvm(foo, (Int, Int))

    define <2 x i64> @"julia_foo;20220"(i64, i64) {
    top:
      %2 = insertelement <2 x i64> undef, i64 %1, i32 0, !dbg !1422, !julia_type !1423
      %3 = insertelement <2 x i64> %2, i64 %0, i32 1, !dbg !1422, !julia_type !1423
      ret <2 x i64> %3, !dbg !1422
    }

This isn't obviously readable so we need to consult the [LLVM reference][llvmref] manual. [insertelement][insertelem] is instruction which insert an element into a vector at given index. 

First part `<2 x i64>` is a type annotation saying we are operating on a 2 element 64 bit integer vector.
  
First argument is the vector register to operate on. In the first case that is `undef` while in the next line it is `%2`, which is where the result of the first line was put. 

Second argument `i64 %1` says we are inserting a 64 bit integer element. Where is given in third argument: `i32 0`. Insert at index 0 (given as a 32 bit integer).

I'll leave it as a readers excercise to figure out exactly how this performs a swap as going into the nitty bitty details of this was not the intention of this blog post.

### Enumerate

Interestingly the [Julia][julia] approach for iterating over both index and value is almost identical to that of Python:

    julia> for (index, value) in enumerate(["foo", "bar", "zoo"])
             println(index, " " , value)
           end
    1 foo
    2 bar
    3 zoo

### Splitting a string into a list of words and joining them back

Splitting and joining is similar to Python except Julia isn't really object oriented so function name comes first as Julia uses multiple dispatch to decide which implementation to call. 

    julia> b = split("This is a string")
    4-element Array{SubString{ASCIIString},1}:
     "This"  
     "is"    
     "a"     
     "string"

    julia> join(b, " ")
    "This is a string"

    julia> join(b, ", ")
    "This, is, a, string"

Nesting functions call is sometimes hard to read, so Julia offers the `|>` operator to write the code in similar sequence as with object oriented languages .e.g:

    julia> "This is a string" |> split |> join
    "Thisisastring"

### List Comprehensions

The traditional imperative way transforming on list to a new one is:

    julia> M = String[]
    0-element Array{String,1}

    julia> for word in L
               push!(M, word[1:3])
           end

    julia> M
    4-element Array{String,1}:
     "Jul"
     "mak"
     "peo"
     "lov"

Quite similar to the Python example except Julia lists havea type so you have to specify that it is a String. You can create a python like array but then you need to specify that it takes any element:

    julia> M = Any[]
    0-element Array{Any,1}

With list comperhensions you can do this more elegantly with:

    julia> M = [word[1:3] for word in L]
    4-element Array{Any,1}:
     "Jul"
     "mak"
     "peo"
     "lov"
     
However for [Julia][julia] it would be more idiomatic to express what you are doing. E.g. in this case we are mapping from one array to another which we can express like this:

    julia> M = map(word ->  word[1:3], L)
    4-element Array{ASCIIString,1}:
     "Jul"
     "mak"
     "peo"
     "lov"

If we want to filter a list by producing a new list containg only the elements with length greater than 5 we would do it like this:

    for word in L
      if length(word) > 5
        push!(M, word)
      end
    end
    
With Python we could solve this with a list comperhension however with Julia you have to explicity state that you are filtering:

    M = filter(word -> length(word) > 5, L)
    
The reason for this is that Julia has bultin support for multidimensional arrays, given that it was designed for scientific computing where this is important. There is no natural way to filter a multi dimensional array, as e.g. every row in a matrix needs to have the same number of columns.
  
[llvmref]: http://llvm.org/releases/2.6/docs/LangRef.html
[insertelem]: http://llvm.org/releases/2.6/docs/LangRef.html#i_insertelement
[pyblog]: http://www.hackerearth.com/notes/praveen97uma/beautiful-python-some-cool-language-constructs-and-tricks-for-beginners/
[julia]: http://julialang.org



