---
layout: post
title: Dynamically Sized Types
tags: rust, dst, types
---

This actually flirts with heavy type theory things (existential types, see link on this) but I'll try to keep it a simple and introductory as possible.

Main question that bugs me is the why, why were these added? Maybe its about lifting types w unknown size to 'first class citizens' but that doesn't seem like it covers all the story for me. Also see extern_types rfc link.

The thoughts on DSTs link leads to a number of posts by core-dev on this, also see 'Treating Vectors like any other container' post. Main 'known' DSTS: slices (that back strings?) and trait objects, existential. Great concern the coercions from sized types to unsized types (and vice-versa?). Implemented as fat pointers (implementation detail?)

Nomicon, Ref and Book don't *really* go into detail but I should re-check those.

To wrap up:

 - Rationale, why do they exist? What do they solve?
 - Core DSTs (you can make custom dsts but support is not really great for these -- I remember seeing a link for an issue that's blocking it regarding how to calculate its size?)
 - Coercions between sized-unsized.

```rust
// Basic check for sized, can add equivalent ones for
// !Sized (I believe it is, don't know if we can use it, though)
// and ?Sized.
fn check<T: Sized>(t: T) -> T {t}

// Dumbest DST. Last field of struct is DST -> struct is DST
struct MyDST {
    a: str
}
```


[dstNom]: https://doc.rust-lang.org/nomicon/exotic-sizes.html
[dstRef]: https://doc.rust-lang.org/reference/dynamically-sized-types.html
[dstBook]: https://doc.rust-lang.org/stable/book/ch19-04-advanced-types.html?highlight=DST#dynamically-sized-types-and-the-sized-trait

[babystepsDSTs]: https://smallcultfollowing.com/babysteps/blog/2013/11/26/thoughts-on-dst-1/
[existentialTypes]: https://stackoverflow.com/questions/292274/what-is-an-existential-type
[externTypes]: https://github.com/rust-lang/rfcs/blob/master/text/1861-extern-types.md
