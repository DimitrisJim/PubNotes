---
layout: post
title: Dynamic Programming 
tags: algorithms, dynamic programming, tools
---

Wikipedia [has a nice description][wikiDP] of the key attributes needed in order for a problem to
be solvable via dynamic programming: *optimal substructure* and *overlapping sub-problems*.

The main issue I've always had was with optimal substructure, the fact that 
Fibonacci numbers can be quickly found with DP (dynamic programming) also doesn't help.
The problem, specifically with Fibonacci, is that I don't really see them as displaying
optimal substructure since I don't see substructures (read: subproblems) compeeting for
a solution. 

explain optimal substruct with shortest path.

Recognizing them:

 - Can be broken apart into simpler overlapping problems. Simpler, as in, we reduce
   the problem space (optimal) and overlappping as in sub-problems are encountered more than once.
 - Can you/How to/Best way to/ problems. That is, Decision/Combinatoric/Optimization
   problems can all be solved in this way. 

Helpful Approaches:

 - Try and understand how the problem is reduced: Are we decreasing a number/the number
   of elements in an array/nodes in tree/vertices in graphs etc. .
 - Visualizing as a tree. This is helpful for visualizing which problems overlap, where
   our base cases are reached and for analyzing space/time complexity.
 - Brute-force it. This should be a straight-forward translation of the tree visualized,
   add base cases noticed, recurse on each branch. 
 - Memoization/tabulation => re-use the solutions to the overlapping subproblems noticed
   during visualization.

**Links**:

A couple of links for more:

 - [Wikipedia article on Dynamic Programming][wikiDP] gives a good explanation. 
 - [and on Optimal Substructure.][wikiOS]
 - [aand Overlapping Subproblems][wikiOLS]
 - [5 hours of DP problems/solving them][dynprog5hours] Thanks, free code camp.

[wikiDP]: https://en.wikipedia.org/wiki/Dynamic_programming
[wikiOS]: https://en.wikipedia.org/wiki/Optimal_substructure
[wikiOLS]: https://en.wikipedia.org/wiki/Overlapping_subproblems
[dynprog5hours]: https://www.youtube.com/watch?v=oBt53YbR9Kk
[fccDP]: https://www.freecodecamp.org/news/follow-these-steps-to-solve-any-dynamic-programming-interview-problem-cc98e508cd0e/
