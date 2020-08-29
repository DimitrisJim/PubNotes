---
layout: post
title: move keyword
tags: rust, move, captures
---

As far as I can find, `move` is only used in capturing contexts when dealing with closures and `async` blocks.

Initially encountered in the section on [Capture Modes][capmodes]
where it's stated that:

> If the `move` keyword is used, then all captures are by move or, for `Copy` types, by copy, regardless of whether a borrow would work. The `move` keyword is usually used to allow the closure to outlive the captured values, such as if the closure is being returned or used to spawn a new thread.

it's placed before the initial `|` tokens for defining a function and *only* affects the captured variables; the explicit arguments that a closure might accept aren't affected.

The second place where `move` is utilized is with `async` blocks:

 - Implicitly when defining an `async fn` which desugars into [an `async` block with `move`.][asyncfn]
 - Explicitly when using an [`async` block.][asyncblock]

Async blocks, as stated in the reference, behave similarly to executing a closure. Therefore, `move` used after the `async` keyword leads to the [same behaviour][asynccapture] as in closures.

---

After grepping through the rust source code, I couldn't find any other cases where `move` is used. It seems like it's only there to control capturing for closure and closure-like constructs.


[capmodes]: https://doc.rust-lang.org/reference/types/closure.html?highlight=move#capture-modes
[asyncfn]: https://doc.rust-lang.org/reference/items/functions.html#async-functions
[asyncblock]: https://doc.rust-lang.org/reference/expressions/block-expr.html#async-blocks
[asynccapture]: https://doc.rust-lang.org/reference/expressions/block-expr.html#capture-modes
