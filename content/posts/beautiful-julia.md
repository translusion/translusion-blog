---
title: "Beautiful Julia: Cool Language Constructs and Tricks for Beginners"
description: "Examples of Julia language constructs based of similarly named Python blog."
date: "2019-06-25"
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

    julia> b = (2, 3, 4)
    (2, 3, 4)
    
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
    ; ┌ @ REPL[61]:1 within `foo'
    	decl	%eax
    	movl	%edx, (%edi)
    	decl	%eax
    	movl	%esi, 8(%edi)
    	decl	%eax
    	movl	%edi, %eax
    	retl
    	nopl	(%eax,%eax)
    ; └


If we want the bytecode like Python we get something that I consider less readable as it is LLVM based which contains a lot of type information to be able to generate efficient machien code output:

    julia> code_llvm(foo, (Int, Int))

    ;  @ REPL[61]:1 within `foo'
    define void @julia_foo_12498([2 x i64]* noalias nocapture sret, i64, i64) {
    top:
      %.sroa.0.0..sroa_idx = getelementptr inbounds [2 x i64], [2 x i64]* %0, i64 0, i64 0
      store i64 %2, i64* %.sroa.0.0..sroa_idx, align 8
      %.sroa.2.0..sroa_idx1 = getelementptr inbounds [2 x i64], [2 x i64]* %0, i64 0, i64 1
      store i64 %1, i64* %.sroa.2.0..sroa_idx1, align 8
      ret void
    }

This isn't obviously readable so we need to consult the [LLVM reference][llvmref] manual.

But a few tips to get you going. The `i64` you see dotted around is 64-bit integer type annotation. The `%1` and `%2` refers to the function arguments. They are numbered rather than using name aliases. 

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
    4-element Array{SubString{String},1}:
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

Quite similar to the Python example except Julia lists have a type so you have to specify that it is a String. You can create a python like array but then you need to specify that it takes any element:

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
    4-element Array{String,1}:
     "Jul"
     "mak"
     "peo"
     "lov"

If we want to filter a list by producing a new list containg only the elements with length greater than 5 we would do it like this:

    [word for word in L if length(word) > 5]
    
This is using a conditional array comperhension. However you can also use the `filter` function.

    M = filter(word -> length(word) > 5, L)
    
When dealing with multidemensional arrays there is not obvious way of doing this. You would likely take an imperative approach.
  
[llvmref]: http://llvm.org/releases/2.6/docs/LangRef.html
[insertelem]: http://llvm.org/releases/2.6/docs/LangRef.html#i_insertelement
[pyblog]: http://www.hackerearth.com/notes/praveen97uma/beautiful-python-some-cool-language-constructs-and-tricks-for-beginners/
[julia]: http://julialang.org



