---
title: "Economics Simulator of Comparative Advantage, Trade and Production"
description: "A simulation of how economies grow"
date: "2014-01-01"
katex: true
---


When you look around the world at different economies you see that a common pattern is that countries richly endowed with natural resources are often the least industrialized. While many countries or areas such as Japan, Singapore, Hong Kong and Switzerland with little resources of importance often have sophisticated industries.

There are of course lots of factors at work here and there are many exceptions to the rule. But what we want to explore is whether we can predict this sort of development in a model using just a few parameters. The hypothesis is that this development can be described by only modeling comparative advantage and simple trade and production.

My hypothesis is that given two producers \\( P_1 \\) and \\( P_2 \\) where \\( P_1 \\) has access to raw materials, while \\( P_2 \\) doesn't have any resources but have to import, then over time \\( P_2 \\) will develop further than \\( P_1 \\) We assume that in the start there will be minor differences but due to comparative advantage each producer will double down on their particular advantage and thus the difference will grow bigger.

# Description of the Model

A producer is a tuple 

$$ P = (c_u, c_f, m, s, p_u) $$

where \\( c_u \\) and \\( c_f \\) is unit cost and fixed cost for producing the output goods. \\( m \\) is the max production capacity of producer \\( P \\) \\( s \\) is the supply of output for sale. \\( p_u \\) is the unit price for the produced goods.

We assume production happens in iterations or turns so that \\( q_1 \\) quantity was produced after \\( q_0 \\) and \\( q_i \\) after \\( q_{i - 1} \\)

We want the following properties for our model. Production cost per unit \\( c_u \\) should fall when the number of units produced increase. So

$$ \forall n, m \textrm{ where } n > m $$

$$ c_u(n) < c_u(m) $$

If supply \\( s \\) keeps increasing

$$ s_{i + 1} > s_i \textrm{ where } i = 1, 2, 3, ... $$

That means we are either producing too much or charging too much so price \\( p \\) or produced quantity \\( q \\) should fall:

$$ p_{i + 1} \leq p_i \lor q_{i + 1} \leq q_i $$


However if supply \\( s \\) keeps falling \\( s_{i+1} < s_i \\) then prices or production should be increasing:

$$ p_{i+1} \geq p_i \lor q_{+1} \geq q_i $$

However if production quantity \\( q \\) can not increase then only prices \\( p \\) need to increase.

$$ q_i = m \implies p_{i+1} \geq p_i$$

## Determining correct output

A challenge with the model is that we are attempting to determine correct values for two variables production quantity \\( q \\) and price \\( p \\) based on just one input, the supply level \\( s \\) If a store is not clearing inventory then it could either be due to prices being too high or that one is making more than the market needs.

Possible ways of solving this problem is either:

1. Try adjusting both \\( q \\) and \\( p \\)
2. Make decision on more input. E.g. compare out sales price \\( p \\) to the other other sellers.


Assuming \\( p \\) is the price set by producer \\( P \\) and \\( p_a \\) is set by one of the other producers \\( a \in A \\) Then if

$$ \forall a \in A, p_a \leq p \land g(q, p) - c(q, p) > 0 $$

where \\( g(q, p) \\) is the gain or profit from producing \\( q\\) units at price \\( p \\) and \\( c(q, p) \\) is the monetary cost of doing so. If the above is true, then prices ought to  be reduced to \\( p' \\) as long as \\(  g(q, p') - c(q, p') \geq 0 \\)