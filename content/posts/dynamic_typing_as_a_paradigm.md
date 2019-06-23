---
title: "Dynamic Typing as a Paradigm"
description: "Dynamic languages could be better understood by considering dynamic typing as a way of programming."
date: "2014-12-08"
categories: 
    - "Dynamic Typing"
    - "Static Typing"
    - "Programming"
---

The Dynamic vs Static Typing debate has been going on for decades and never seems to end. While I like both static and dynamic programming languages I probably have a slight preference for dynamic typing. However I feel of the two types of languages I feel the dynamic ones are the ones that are most profoundly misunderstood. The popular blog post [Dynamic languages are static languages][dynisstatic] illustrates the misunderstanding very well.

I think part of the problem is that when trying to articulate what the advantages of dynamic languages are it is easy to get very vague. So I am trying another approach. 

## Dynamic typing as a paradigm

In this post I will start by claiming that it is not so much about static vs dynamic languages but rather about two different paradigmes or approaches to programming. Much the same way as it can be argued that Object Oriented programming is a paradigme rather than a feature of a programming language. Many of you might already be familier with Object Oriented Programming in C. That is quite popular on Linux in the [Gtk toolkit][gtk] used on the Gnome desktop. Of course C as a language was not designed for this sort of programming the way say Java or Smalltalk was designed for it.

The same argument could be made about static vs dynamic typing. You could write in a dynamic style in statically typed language. I've done that plenty of times. GUI based programming is often a good example. Static typing isn't very well suited for this task and thus we got things like [Qt][qt] a C++ GUI toolkit which added a preprocessor which gave dynamic featues to C++. Objective-C did much the same earlier by adding dynamic features to C. 

## Objects as Hashtables

So how would you program in a more dynamic fashion in a statically typed language? One way which I have used a lot myself is to use hash tables more to define your objects rather than classes. This is essentially what an object is in Python or Ruby. In C++ or Java you could store your date in a hash table and create a number of functions or methods to manipulate this hash table much the same way as you create classes with corresponding methods. But why would you do that? You lose type safety. Of course but you also gain something:

1. You can easily iterate over the member of your object. Handy when applying a transformation to all members such as serializing them to disk, or duplicating them.
2. You can add members at runtime.
3. You can give rich meta data descriptions available at runtime for dynamic creation of e.g. GUIs.
4. It can solve complicated problems with defining a proper taxonomy. Usually in Object Oriented programming we try to reuse code by creating an inheritance hierarchy so that we can share the implementation of methods which work in similar ways across types which are similar to each other. However a such a taxonomy might not always be easy to define. With hashtables it is easy to write a function which works on hash tables with different number of members as long as they share a common minimum of members that the function operates on.

The latter point was realized in [Go][go] when they allowed satisfying interfaces implicitly. Point 3 allows creating such things as the *Predicate Editor* found in Cocoa. One can imagine creating descriptions of each property saying what unit the property is in e.g. meters, kilo grams etc and what are legal value ranges. This can then be used to derive a GUI that allows viewing or editing corresponding objects. Visualization pipelines such as the [Visualization Toolkit][vtk] works along simular lines. You can at runtime connect algorithms sort of like Unix pipelines. To support an editor which lets humans connect different algorithms there needs to be a way to query each potential component in the pipeline and ask what sort of inputs and outputs they require. A compiler can't check this sort of type information. First because it varies with the application domain what a type specifies and it needs to be determined at runtime for interactive usage.

So a hash table with a bunch of functions is a way to simulate dynamic typing in a staticly typed languge, but thinking dynamic is more fundamental than that. 

## Pascal vs C

To illustrate the problem with strict typing I will go all the way back to Pascal. Pascal failed and C succeeded much because of Pascal's embrace of the static typing over practicality mentality. Brian W. Kernighan wrote an article in 1981 called [Why Pascal is Not My Favorite Programming Language][pascalnotfav] which illustrates many of these problems. In one of his examples he says:

> If one declares

     var     arr10 : array [1..10] of integer;
             arr20 : array [1..20] of integer;
> then arr10 and arr20 are arrays of 10 and 20 integers respectively.  Suppose we want to write a procedure 'sort' to sort an integer array.  Because arr10 and arr20 have different types, it is not possible to write a single procedure that will sort them both.

This might sound laughable by todays standards, but although it is perhaps and extreme example it is really just a variation of the same problem that one will often encounter with the static typing approach. Static typing allows the compiler to catch many mistakes you do before the code runs. Making the array lenght part of its type meant you would be less likely to get out of bounds errors or pass arrays of the wrong length around. But it also made it hard to reuse code and write generic functions.

## 3D Graphics and OpenGL

When writing graphics code it would be tempting to make everything into types in much the same manner as Pascal. One might argue that a point in 2D space should be different from a point in 3D space or even 4D space. We could write types like this (julia syntax):

    type Point2D
      x :: Float64
      y :: Float64
    end
    
    type Point3D
      x :: Float64
      y :: Float64
      z :: Float64
    end
    
We could then create array of 10 point objects like this:

    points2d = Array(Poin2D, 10)
    points3d = Array(Point3D, 10)
    
Perhaps we want to perform rotation or translation of all these points with a matrix. The problem that now arises is much the same as with Pascal. If we define a function which transforms an array of points using a matrix:

    function transform(matrix :: Matrix, points :: Vector{Point2D})
    
Then this can not be reused for an array of `Point3D` points. Thus we get pascal style code duplication. Mathematically speaking matrix muliplication is a generic operation which we can write an algorithm for which perform the operation a matrix of any size. If we do not treat our points as types like `Point2D` and `Point3D` but just as arrays of numbers then we can transform an array of 10 2D points by just treating it as a matrix with 2 rows and 10 columns. `Point3D` would be a matrix of 3 rows and 10 columns. This allows us to reuse standard fast matrix multiplication functions.

OpenGL is a good example of this practice. There are no types for matricies or vectors. They are just arrays of numbers. This allows us to reuse OpenGL function for many cases.

## Large Scale Programs

I hope that these simple examples in the small with Pascal arrays and matricies and vectors in 3D programming will allow you to get an intuition about how these problems also exist at a larger scale in most statically typed programs. When a large piece of software gets developed over many years by many different teams and pieces get bought and integrated there will be a lot of types which are similar but not quite the same, but which non the less needs to work together. We can imagine them like our 2D and 3D points. When they are separate types we will need to write a lot of conversion code to be able to reuse algorithms created for another type of object, or to be able to write some shared algorithm.

With the dynamic approach this sort of problem usually disappear. That is the reason why glue languages are usually dynamic languages. Connecting different pieces of software that was not designed for each other gets quite tricky with a static language which insists on knowing everything about types at compile time. A concrete example of this is why a REPL for a statically typed language never can work as well as one for a dynamically typed one.

In a dynamic language you can make structural changes at runtime. You can add or remove member variables of methods at runtime. That is fine because that does not change the type. For a statically typed language this poses a problem, since by adding or removing members you are changing the static type which all previous code was dependent on. The assumptions the compiler previously had about how to handle your types, would no longer be true and can thus not be allowed.

## Conclusion

Not having done any research on this I can only speculate. But my speculation is that statically typed languages mainly work for medium sized programs while dynamic typing works for the small and high level. As your program grows it can get easier to mix up your types when you only use dynamic typing. So for larger size static typing has some advantages. However as a program gets even larger one gets into the problem of having to deal with many parts that were not designed for each other. This is where the flexibility of dynamically typed languages gains ground again.

So perhaps a usefull design is to have chunks of statically typed code fittet together with a dynamically typed language which orchastrates everything at a high level.


[vtk]: http://www.vtk.org  
[go]: https://golang.org
[pascalnotfav]: http://www.lysator.liu.se/c/bwk-on-pascal.html
[qt]: http://qt-project.org
[gtk]: http://www.gtk.org
[dynisstatic]: https://existentialtype.wordpress.com/2011/03/19/dynamic-languages-are-static-languages/