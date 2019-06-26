---
title: "Munching Data with Julia"
description: "How to process data from in tables from e.g. surveys using the Julia programming language."
date: "2019-06-24"
categories: 
    - "Julia"
    - "Datascience"
    - "Googleforms"
    - "Datetime.jl"
---

I've recently performed a survey at my work using [Google form][forms], unfortunately the data wasn't usable right out of the box, because my company allowed people to register an answer multiple times. However a person should only be allowed to vote once.

I chose to clean the data with [the Julia programming language][julia], which might not be the best choice for this sort of thing. Not because julia isn't suited but because SQL, sed or awk might do such a specialized and simple task faster. Unfortunately I am crappy at all 3. The data looked like this (anonymized):

    5/8/2014 10:32:14	spam@mycompany.no	Bonus	2,5%	40%	4,42%
    5/8/2014 10:35:03	bar@mycompany.no	Higher salary	2,9%	37,5%	3,9%
    5/8/2014 10:41:10	spam@mycompany.no	Bonus	2,5%	40%	4,42%
    5/8/2014 10:45:10	foo@mycompany.no	Higher salary	2,9%	37,5%	3,9%

We were voting on how to distribute money available to give us higher salary or bonus. The percentages shown in the last column is just a description of what higher salary or bonus means in terms of actual concrete allocation to 3 different areas.

The file was exported with tab as separator since some of the columns used comma in the text. In Europe we often use comma instead of dot for separating decimals.

So the task was to eliminate to double or more votes, keeping only the most recent choice. So e.g. in our example *spam@mycompany.no* voted twice. So the 1st line needs to be elimenated and the 3rd kept (most recent choice by spam).

### Read the data

First we read in our data. Indicating that data is separated with tab and that first line is a header:

```julia
using DelimitedFiles
ourdata, header = readdlm("result.tsv", '\t', has_header=true)
```

This gives us a 2D array (matrix) which sounds neat. Well it is quite neat in a lot of ways. I can ask for e.g. the whole second column containing the email of the respondents with:

```julia
ourdata[:,2]
```
	
If I instead wanted the 3rd row I could do:

```julia
ourdata[3, :]
```

What is **NOT neat** about it compared to the regular arrays within arrays found in languages such a python and ruby is that you can't process this with a `foreach` type of construct. Like this:

	for row in ourdata
       dostuff(row)
    end

`row` will in this case just be a substring and not an actual array object. 

### Find unique rows
Instead you have to iterate by index:

```julia
byname = Dict{String, Any}()
for i in 1:size(ourdata,1)
    byname[ourdata[i, 2]] = ourdata[i, :]
end
```

What we are doing here is to record each line on email. Thus only the last line for a given email get stored. Previous duplicated gets discareded. Just what we want. `size(ourdata, 1)` gives us the number of rows. `size(ourdata, 2)` would have given number of columns. `length(ourdata)` or `size(ourdata)` would not have worked because this is not an array of arrays. So your usual python or ruby reflexes don't apply.

### Count each line matching a predicate
So to check how many of our respondents wanted higher bonus we do:

```julia
count(values(byname)) do row
    row[3] == "Bonus"
end
```


And to get those who want higher salary:

```julia
count(values(byname)) do row
    row[3] == "Higher salary"
end
```

### Working with Dates in Julia
When I first started looking at this problem, I thought I needed to work with dates, which wasn't necessary because the data was already sorted by date and time, but if it wasn't you need to get hold of the julia [datetime package][datetime]. There isn't anything builtin to deal with dates. Here are some practical examples on how to use it. Our data had date and time like this:

	5/8/2014 0:16:03
	
Which isn't the format the package deals with date and time. So here are some tricks for how to convert:

```julia
d, t = split("5/8/2014 0:16:03") # Put date in d, and time t
```

We can then turn this into a formate our date creation function `date` wants:

```julia
using Dates
Date(reverse(parse.(Int, split(d, '/')))...)
```
	
### Iteratively creating code in the Julia REPL
That was a *mouthfull*. Usually when writing lines like this I develop them interactively by starting with:

	julia> split(d, '/')
	3-element Array{String,1}:
	 "5"   
	 "8"   
	 "2014"
	 
This looks like what I wanted, but the data is string and I need integers so I:

* Hit arrow up button to get back `split`
* Ctrl+A to go to start of line and write `parse.(Int`
* Ctrl+E to get to end of line and close it with `)`

This gives me the next line:

    julia> parse.(Int, split(d, '/'))
    3-element Array{Int64,1}:
        5
        8
     2014
 
Looks good, but not in reverse order compared to what `date` wants. So we do:

    julia> reverse(parse.(Int, split(d, '/')))
    3-element Array{Int64,1}:
     2014
        8
        5

Okay so we are good, except `Date` doesn't take an array as argument. So we need to explode our array to function arguments with the `...`.
	

[julia]: http://julialang.org
[forms]: http://www.google.com/google-d-s/createforms.html
[datetime]: https://github.com/karbarcca/Datetime.jl


