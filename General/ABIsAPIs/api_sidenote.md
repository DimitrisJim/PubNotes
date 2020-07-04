---
layout: post
title: API Considerations
tags: api
---

# TODO: LINK IN TREE

Interfaces, at least in compsci land, are basically a set of
rules that answer the question *"How can I communicate with you?"*. An interface is a public display of the means of communication between two entities: the user of the application and the application itself.

It's worth the side-note to point out that interfaces don't only specify calling conventions (you need to supply an `int`, a class that has a `spam` method, etc) or class/data/struct layouts; they (usually) also require certain pre-conditions (e.g a pointer shouldn't be `NULL`), offer certain post-conditions (e.g the list is sorted in ascended order) and specify error/panic situations and certain complexity guarantees (e.g error if memory runs out, guarantee worse case `O(N)` complexity).
