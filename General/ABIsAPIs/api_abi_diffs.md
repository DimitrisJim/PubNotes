---
layout: post
title: ABI and API differences
tags: abi, api
---

# TODO: LINK IN TREE

The common distinction I've seen made between API and ABI is that API is an interface on the source level (that is, how two applications will communicate
on the source code level) while ABI is on the binary level (how can a binary
talk to another binary).

The distinction makes sense considering the demographics: APIs are for high level languages and are usually hardware independent, most people tend to operate on this level. ABIs, on the other hand, apply for more low level languages (`C`, `C++`, `asm`) that are very dependent on the underlying hardware.

When writing in a language such as Python and Javascript you don't need to trouble yourself with what is going on underneath (at least
you usually dont!): the language runtime takes care of these things for you,
it's the abstraction it offers.

On the other hand, writing in a language such as `C` does require you to be
mindful of both. You don't only export an API here, you also export an ABI that results from your compiled code.  
