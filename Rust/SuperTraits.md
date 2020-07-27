---
layout: post
title: Supertraits, Trait bounds.
tags: rust, supertraits, trait bounds
---

Supertraits allow us to specify that a specific trait depends on another trait being implemented.

A succinct example is given in the [corresponding section][supertraitsBook] of the book:

```rust
trait OutlinePrint: fmt::Display {
  fn outline_print(&self) { ... }  
}
```

not much more is stated here.

Looking through the reference afterwards, [another section][supertraitsRef] on supertraits makes the concept a bit clearer:

> Supertraits are declared by trait bounds on the `Self` type of a trait and transitively the supertraits of the traits declared in those trait bounds.

the examples following make it clear that the syntax:

```rust
trait X: Y {}
```

is shorthand for:

```rust
trait X where Self: Y {}
```

that is, as the quoted text states, a supertrait is a trait bound on [`Self`][capitalself]. So the initial example given in the book can be also written as:

```rust
trait OutlinePrint where Self: fmt::Display {
  fn outline_print(&self) { ... }  
}
```

I prefer the shorthand form.

[supertraitsBook]: https://doc.rust-lang.org/stable/book/ch19-03-advanced-traits.html#using-supertraits-to-require-one-traits-functionality-within-another-trait
[supertraitsRef]: https://doc.rust-lang.org/reference/items/traits.html#supertraits
[capitalself]: https://doc.rust-lang.org/reference/paths.html?highlight=Self#self-1
