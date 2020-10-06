---
layout: post
title: Best, worse and average case. 
tags: algorithm, case analysis, landau
---

I'm writting this note mainly because a couple of times I've seen a 
false equivalence between the [Landau Notations][landWiki] and the 
best, worse and average cases in the analysis of algorithms.

What's commonly stated is that, the big-Oh (`Ο`), indicating an upper 
bound, the big-omega (`Ω`) indicating a lower bound and big-theta (`Θ`)
indicating both are related to worse, best and average case respectively.

This isn't the case. The problem stems from the fact that we mostly 
talk about the big-Oh complexity of an operation and silently 
accept we're talking about the worse case. We easily say that 
`list.pop` in Python is `O(1)` without actually pointing out that this is 
specifically about the ([*amortized*][amortAnal]) *worse-case*. 

Best, worse and average are each a collection of problem 
instances in which the proposed algorithm performs worse, best or 
has average performance. 

For example, consider Bubble Sort as the algorithm and lists of 
integers as the input we're applying it on. We can say that the 
*best case* for calling Bubble sort on a given list consists of 
the lists for which the input is sorted. That is, the best case 
is the set of *all* those lists that are sorted, so:

```python
[1, 2, 3, 4], [-1, -20, -30], [1], [-10, 0, 203, 291, ..., 293932, ...]
```

in these instances of the input data we can say that bubble sort is 
`Ο(n)` where `n` is the input size. This applies only to these instances 
which we have identified as the *best case*. So bubble sort is 
*best case* `Ο(n)`.

Similarly, we can say that the *worse case* is when the list is 
sorted in the opposite direction. Once again, *all* list who 
have this property belong in the problem-set for the worse-case:

```python
[4, 3, 2, 1], [-30, -20, 1], [..., 293932, ..., 291, 203, 0, -10]
``` 

in these instances of the input data we can say that bubble sort is 
`Ο(n^2)` where `n` is the input size. This applies only to these instances 
which we have identified as the *worse case*. So bubble sort is 
*worse case* `Ο(n^2)`.

Finding the complexity for the average case is considerably a more 
difficult endeavor involving probabilities and I'm not getting into 
that.

All in all, though we mostly care about the big-Ο/Θ for the worse 
case, the point I'm getting across is that conflating these 
entities is mistaken. Each of these should be analyzed if that 
is required. 

Note: I haven't explicitly defined what big-O/Omega/Theta are, ]
this is because their definition is a slightly tricky (for me) 
subject. This [thesis][rutanen] is, as far as I know, a good 
attempt to formalize it. Any interested souls, follow that.

[landWiki]: https://en.wikipedia.org/wiki/Big_O_notation
[heapsortWiki]: https://en.wikipedia.org/wiki/Heapsort
[amortAnal]: https://en.wikipedia.org/wiki/Amortized_analysis

[csexch1]: https://cs.stackexchange.com/questions/57/how-does-one-know-which-notation-of-time-complexity-analysis-to-use
[csexch2]: https://cs.stackexchange.com/questions/23068/how-do-o-and-%ce%a9-relate-to-worst-and-best-case

[rutanen]: https://arxiv.org/abs/1309.3210
[rutanenStack]: https://cs.stackexchange.com/questions/3149/what-is-the-meaning-of-omn
