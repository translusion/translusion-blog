---
title: "Julia One Liners compared to Scala"
description: "A comparison of one liners in Scala and how they would look in Julia."
date: "2016-05-04"
categories: 
    - "Julia"
    - "Programming"
---

This was inspired by the blog post [10 Scala One Liners to Impress Your Friends](https://mkaz.github.io/2011/05/31/10-scala-one-liners-to-impress-your-friends/).

### 1. Multiple Each Item in a List by 2
Original Scala version

	(1 to 10) map { _ * 2 }

I think the Julia version is clearer. The `x->2x` notation for anonymous function looks very similar to regular mathematical notation.

	map(x->2x, 1:10)

### 2. Sum a List of Numbers
Sums numbers from 1 to 1000 in Scala either by using `reduceLeft` or `sum`.

	(1 to 1000).reduceLeft( _ + _ )
	(1 to 1000).sum

Very similar in Julia, but the `reduce` seems more elegant in that you don’t have to explicitly give the arguments to `+`. `reduce` takes a binary operator. So you just pass that. You don’t have to tell Julia how to add numbers with it.

	reduce(+, 1:1000)
	sum(1:1000)

### 3. Verify if Exists in a String
Check if a word in a word list is present in a string.

	val wordList = List("scala", "akka", "play framework", "sbt", "typesafe")
	val tweet = "This is an example tweet talking about scala and sbt."

	(wordList.foldLeft(false)( _ || tweet.contains(_) ))
	wordList.exists(tweet.contains)

Here I really think the Scala version is rather cryptic. I can not really guess how this works without learning more about Scala. In Julia:

	wordlist = ["scala", "akka", "play framework", "sbt", "typesafe"]
	tweet = "This is an example tweet talking about scala and sbt."
	any(word->contains(tweet, word), wordlist)

### 4. Read in a File
This is a line for reading a file as a string in Scala. While that is apparently impressive by Java standards it is rather verbose for other languages.

	val fileText = io.Source.fromFile("data.txt").mkString

In Julia this would just be 

	filetext = readall(“data.txt”)`

There is another Scala example of reading the file and turing every line into an element in list.

	val fileLines = io.Source.fromFile("data.txt").getLines.toList

This is also the sort of thing that is trivial in Julia. Julia’s file open command can take a function as an argument and provide that function with a stream object and then close the file after the function is finished. There are many functions like `readall`, `readlines` etc which takes streams as arguments. So you can write:

	filelines = open(readlines, "data.txt")

### 5. Happy Birthday to You!
Here the Scala one ends up looking a bit better as Julia doesn’t have anything like `foreach`:

	(1 to 4).map { i => "Happy Birthday " + (if (i == 3) "dear NAME" else "to You") }.foreach { println }

In Julia it is clearer to do it in two steps:

	lines = map(1:4) do i 
			"Happy Birthday " * 
			if i == 3 "dear NAME" else "to You" end 
	end
	print(join(lines, "\n"))

One thing to notice though is that you can write this stuff in Julia with a lot less clutter of parenthesis and curly braces. In Scala need 4 parenthesis for the `if` statement, while Julia avoids this and use a single `end` keyword instead.

### 6. Filter list of numbers
Partition a list of students into two categories.

	val (passed, failed) = List(49, 58, 76, 82, 88, 90) partition ( _ > 60 )

There is no function like partition in Julia so you would have to write this using multiple lines:

	grades = [49, 58, 76, 82, 88, 90]
	passed = filter(grade->grade > 60, grades)
	failed = filter(grade->grade <= 60, grades)

But of course nothing prevents us from defining a `partition` function in Julia:

	function partition{T}(p, xs::Vector{T})
	    as = Vector{T}()
	    bs = Vector{T}()
	    for x in xs
	    	if p(x)
	    	    push!(as, x)
	    	else
	    	    push!(bs, x)
	    	end
	    end
	    (as, bs)
	end

Which then would allow us to do like in Scala:

	passed, failed = partition(grade->grade > 60, grades)

### 7. Fetch and Parse an XML web service
Here’s an example fetching the Twitter search feed in Scala.

	val results = XML.load("http://search.twitter.com/search.atom?&q=scala")

I don’t know anything about XML stuff in Julia, so I will not attempt and alternative.

### 8. Find minimum (or maximum) in a List
Another couple of examples using reduceLeft to iterate through a list and apply a function. Added simpler examples of the method min/max on the list.

	List(14, 35, -7, 46, 98).reduceLeft ( _ min _ )
	List(14, 35, -7, 46, 98).min

	List(14, 35, -7, 46, 98).reduceLeft ( _ max _ )
	List(14, 35, -7, 46, 98).max

Again this is the sort of thing were Julia is very succinct. 

	reduce(min, [14, 35, -7, 46, 98])
	min(14, 35, -7, 46, 98)

	reduce(max, [14, 35, -7, 46, 98])
	max(14, 35, -7, 46, 98)

### 9. Parallel Processing
The following one-liner would give you parallel processing over the list in Scala.

	val result = dataList.par.map( line => processItem(line) )

### 10. Sieve of Eratosthenes
We got this crazy line from Scala, but no I am not going to figure out what this really does and rewrite it in Julia.

	(n: Int) => (2 to n) |> (r => r.foldLeft(r.toSet)((ps, x) => if (ps(x)) ps -- (x * x to n by x) else ps))

Perhaps one day when I am *very* bored I’ll revisit this and update the blog.
