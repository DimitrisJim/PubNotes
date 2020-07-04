---
layout: post
title: ABIs
tags: abi, application binary interface
---

The little known, lower-level counterpart to APIs, an application
binary interface is, as the name entails, an interface but on
the binary level.

## A little side note on interfaces

Interfaces, at least in compsci land, are basically a set of
rules that answer the question *"How can I communicate with you?"*. An interface is a public display of the means of communication between two entities: the user of the application and the application itself.

It's worth the side-note to point out that interfaces don't only specify calling conventions (you need to supply an `int`, a class that has a `spam` method, etc) or class/data/struct layouts; they (usually) also require certain pre-conditions (e.g a pointer shouldn't be `NULL`), offer certain post-conditions (e.g the list is sorted in ascended order) and specify error/panic situations and certain complexity guarantees (e.g error if memory runs out, guarantee worse case `O(N)` complexity).

## ABI and API, are we so different?

The common distinction I've seen made between API and ABI is that API is an interface on the source level (that is, how two applications will communicate
on the source code level) while ABI is on the binary level (how can a binary
talk to another binary).

The distinction makes sense considering the demographics: APIs are for high level languages and are usually hardware independent, most people tend to operate on this level. ABIs, on the other hand, apply for more low level languages (`C`, `C++`, `asm`) that are very dependent on the underlying hardware.

When writing in a language such as Python and Javascript you don't need to trouble yourself with what is going on underneath (at least
you usually dont!): the language runtime takes care of these things for you,
it's the abstraction it offers.

On the other hand, writing in a language such as `C` does require you to be
mindful of both. You don't only export an API here, you also export an ABI that results from your compiled code.  

## Stability



[linuxstandardbase]: https://en.wikipedia.org/wiki/Linux_Standard_Base
[itaniumabi]: https://itanium-cxx-abi.github.io/cxx-abi/abi.html
[_seelater]: https://www.reddit.com/r/cpp/comments/fc2qqv/abi_breaks_not_just_about_rebuilding/
[nodejsabi]: https://nodejs.org/uk/docs/guides/abi-stability/
[systemvabi]: https://wiki.osdev.org/System_V_ABI
[redhatabi]: https://accu.org/content/conf2015/JonathanWakely-What%20Is%20An%20ABI%20And%20Why%20Is%20It%20So%20Complicated.pdf
[pyabi]: https://www.python.org/dev/peps/pep-0384/
[wikiabi]: https://en.wikipedia.org/wiki/Binary-code_compatibility
[abibreak]: https://www.acodersjourney.com/20-abi-breaking-changes/
[bincompkde]: https://community.kde.org/Policies/Binary_Compatibility_Issues_With_C%2B%2B
[apiabiyt]: https://www.youtube.com/watch?v=k9PLRAnnEmE
[apiabitytnotes]: https://github.com/CppCon/CppCon2017/blob/master/Presentations/API%20%26%20ABI%20Versioning/API%20%26%20ABI%20Versioning%20-%20Mathieu%20Ropert%20-%20CppCon%202017.pdf

[_seelater2]: https://news.ycombinator.com/item?id=18827460
[_seelater3]: https://en.wikipedia.org/wiki/X32_ABI
[_seelater4]: https://en.wikipedia.org/wiki/Linux_Standard_Base
[_seelater5]: https://nibblestew.blogspot.com/2019/11/some-intricacies-of-abi-stability.html
