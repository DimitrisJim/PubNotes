---
layout: post
title: Landau Notation for algorithm analysis. 
tags: algorithm, bigoh, landau
---

Okay, main gist is that big-Oh (`O`), big omega (`Ω`) and big theta 
(`Θ`) don't relate in any way to worse, best and average cases. 
Usually when one says "this operation is order `O(logn)`" they 
*usually* mean worse case since it's more meaningfull than a 
best-case (many times, `O(1)`) scenario and easier to find than 
an average case (which might require probabilistic analysis) 
scenario.

Each of these can be used to describe best, worse and average cases.
Big-Oh is famously an upper bound for complexity, big-omega a lower 
bound and Big-Theta an upper *and* lower bound. 

Good to find examples where this is highlighted (e.g from wiki take 
Timsort or Heapsort for Big-Oh bounds, try and find some theta and 
or omega bounds.)

[landWiki]: https://en.wikipedia.org/wiki/Big_O_notation
[heapsortWiki]: https://en.wikipedia.org/wiki/Heapsort

[csexch1]: https://cs.stackexchange.com/questions/57/how-does-one-know-which-notation-of-time-complexity-analysis-to-use
[csexch2]: https://cs.stackexchange.com/questions/23068/how-do-o-and-%ce%a9-relate-to-worst-and-best-case

[rutanen]: https://arxiv.org/abs/1309.3210

