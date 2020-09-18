---
layout: post
title: Dynamically Sized Types
tags: rust, dst, types
---

This actually requires a bit of historical digging considering the Dynamically Sized Types were add in a much earlier version 
of rust. For this historical dig, I'll be using [A core devs blog, babysteps][babysteps]

Initially, DSTs are basically types for which the size is know during execution (hence the dynamic). At runtime, the compiler 
can find out the size of any dynamically sized types and store that information as needed. 

My initial question was, why have them in the first place? What's the point of having them? DSTs, or, unsized types as they were 
initially called, exist due to the need to support [vectors, strings and slices][babystepsVecStrSlice] (initially, that is). There, we have the following quote:

> There are two types here that cannot be expressed in the original system: `vec<T>` and `str`. There is a good reason that these types are inexpressible: they do not have a fixed size. 

What troubles me here is the fact that `Vec` was afterwards [moved][babystepsVecSTD] into the `stdlib` where the struct that defines it *does* have a fixed size (a `(pointer, capacity, length)` triplet). This means that I'm missing something, for some reason the representation of structures as they now are in the stdlib cannot be used outside of it. I'm not sure why this is.

So, for now, I'm taking as granted that *certain* types *cannot* be defined in a way that allows the compiler to calculate their size, at least during compilation. This is something I'd want to examine at a later point.  

So far Trait Objects, slices and strings are my known DSTs (and strings might just be a slice into a `vec<u8>`), any more?
(Closures appear to have been Unsized in the past but not any more [see note in Closure Types of ref that says they all 
implement sized).

---

This actually flirts with heavy type theory things (existential types, see link on this) but I'll try to keep it a simple and introductory as possible.

Main question that bugs me is the why, why were these added? (See 'babystepsUnsized' link).

The thoughts on DSTs link leads to a number of posts by core-dev on this, also see 'Treating Vectors like any other container' post. Main 'known' DSTS: slices (that back strings?) and trait objects, existential. Great concern the coercions from sized types to unsized types (and vice-versa?). 
Implemented as fat pointers (implementation detail? -- yea?) whose size is known because its essentially a (size, \*data) tuple; size is filled in by the compiler at a later stage.

Nomicon, Ref and Book don't *really* go into detail but I should re-check those. Update: only nomicon is of help, really.

To wrap up:

 - Rationale, why do they exist? What do they solve?
 - Core DSTs (you can make custom dsts but support is not really great for these -- I remember seeing a link for an issue that's blocking it regarding how to calculate its size? -- no, this is about extern types see Extern Types link.)
 - Coercions between sized-unsized (read DST, take 5 for more on this.)

Another note: `Vec` used to be a DST but was eventually moved to std and represented as `(pointer, capacity, length)` triplet. 
This raises the question of *why* isn't the same thing done for strings, for example. Can't see why only vectors were moved. 
Note that this doesn't mean that the need for DST's wouldn't exist: trait objects are still a thing.

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

[babysteps]: https://smallcultfollowing.com/babysteps/
[babystepsUnsized]: https://smallcultfollowing.com/babysteps/blog/2012/04/27/in-favor-of-types-of-unknown-size/
[babystepsVecStrSlice]: https://smallcultfollowing.com/babysteps/blog/2012/04/23/vectors-strings-and-slices/
[babystepsVecSTD]: https://smallcultfollowing.com/babysteps/blog/2013/11/14/treating-vectors-like-any-other-container/


[dstNom]: https://doc.rust-lang.org/nomicon/exotic-sizes.html
[dstRef]: https://doc.rust-lang.org/reference/dynamically-sized-types.html
[dstBook]: https://doc.rust-lang.org/stable/book/ch19-04-advanced-types.html?highlight=DST#dynamically-sized-types-and-the-sized-trait

[babystepsDSTs]: https://smallcultfollowing.com/babysteps/blog/2013/11/26/thoughts-on-dst-1/
[existentialTypes]: https://stackoverflow.com/questions/292274/what-is-an-existential-type
[externTypes]: https://github.com/rust-lang/rfcs/blob/master/text/1861-extern-types.md
[customDSTs]: https://github.com/rust-lang/rfcs/pull/2594
