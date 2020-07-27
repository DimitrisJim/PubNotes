---
layout: post
title: Auto, Blanket, Trait Implementations
tags: rust, traits, documentation
---

As seen in the documentation for structs, three different sections on
trait Implementations are listed. As an example, looking at [`Vec`][vecDocs].

### Trait Implementations

These are traits that are implemented in the same source file as the struct, cases where the implementation is dependent on the actual type we're implementing it on. This case doesn't really need more explaining.

### Blanket Trait Implementations

The book has a small paragraph explaining these when talking about traits:

> We can also conditionally implement a trait for any type that implements another trait. Implementations of a trait on any type that satisfies the trait bounds are called *blanket implementations* and are extensively used in the Rust standard library. For example, the standard library implements the `ToString` trait on any type that implements the `Display` trait.

Blanket trait implementations are implemented inside the source file that
defines the trait. They don't all require trait bounds (i.e `From` for vectors, apparently.)

### Auto Trait Implementations

These are implemented automatically by the compiler when certain conditions hold. Most of them seem to be present in the `std::marker` module. [Definitions][sendDef] use `pub auto` which does not seem to be documented in the reference for [traits][traitRef], maybe an internal construct?

For example: `Vec`s documentation states that it implements `Send` only if the
type inside the `Vec` also implements it (trait bound on `Send`). Using a vector type that does not satisfy that trait bound will, in turn, lead to that
specific vector not supporting `Send`.

[vecDocs]: https://doc.rust-lang.org/std/vec/struct.Vec.html
[sendDef]: https://doc.rust-lang.org/src/core/marker.rs.html#38-40
[traitRef]: https://doc.rust-lang.org/reference/items/traits.html
