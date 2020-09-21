---
layout: post
title: Dynamically Sized Types
tags: rust, dst, types
---

This actually requires a bit of historical digging considering the Dynamically Sized Types were add in a much earlier version 
of rust. For this historical dig, I'll be using [A core devs blog, babysteps][babysteps]

Initially, DSTs are basically types for which the size is know during execution (hence the dynamic). At runtime, the compiler 
can find out the size of any dynamically sized types and store that information as needed. 

My initial question, what's the point of having them? DSTs, or, unsized types as they were initially called, were introduced due to the 
need to support [vectors, strings and slices][babystepsVecStrSlice] (initially, that is). There, we have the following quote:

> There are two types here that cannot be expressed in the original system: `vec<T>` and `str`. There is a good reason that these types are inexpressible: they do not have a fixed size. 

What troubles me here is the fact that `Vec` was afterwards [moved][babystepsVecSTD] into the `stdlib` where the struct that defines it *does* have a fixed size (a `(pointer, capacity, length)` triplet). This means that I'm missing something, for some reason the representation of structures as they now are in the stdlib cannot be used outside of it. I'm not entirely sure why this is.

So, for now, I'm taking as granted that *certain* types *cannot* be defined in a way that allows the compiler to calculate their size, at least during compilation. (Note to self: examine this at a later point).  

So far Trait Objects, slices and strings are my known DSTs. Though `str` [appears to be][strstdlib] a slice of `[u8]`'s, the documentation 
of [`from_utf8`][fromutf8] doesn't seem to back that up, so I'm guessing there's no relation between them other than beeing able to easily
convert between each other. [In the past][babystepsDSTs], it appears a certain function type was unsized but this doesn't seem to be the 
case now; closures all [implement Sized][sizedClosureRef] and function types, if my reading of its [reference][funcitemRef] is correct, is a zero-sized type (ZST, see [nomicon for more][nomiconZST]). 

As described in [another blog post][babystepsDSTs5], pointers that are used to handle values of DSTs aren't similar to pointers to known
sized types. The pointer to a DST is, charmingly, known as a "fat" pointer due to the fact that they are two words in size. The first 
word points to the object in question while the second pointer holds the missing information that can't be known during compilation. 
For slices and strings, the missing information is the length of the sequence and the length of the string respectively. For Trait objects 
the extra missing information filled in is the virtual method table. These are, I assume, filled in at *some point* (when?) during runtime. 

Relevant trait to be concerned about is [`Sized`][stdSized]. Except for `Self`, all type parameters are by-default bound by `Sized`. 
Opting-out (allowing DSTs) can be done by using `?Sized` as can also be seen in the next snippet.

```rust
// : Sized is superfluous
fn only_sized<T: Sized>() -> () {}
fn all<T: ?Sized>() -> () {}

// Dumbest DST. Last field of struct is DST -> struct is DST
struct MyDST {
 a: str
}

struct SizedStruct<'a> {
    a: &'a str
}

fn main(){
    all::<MyDST>();               // ok, ?Sized means no default Sized bound.
    // only_sized::<MyDST>();     // error, dst.
    all::<SizedStruct>();         // ok.
    only_sized::<SizedStruct>();  // ok.
}
```

[babysteps]: https://smallcultfollowing.com/babysteps/
[babystepsUnsized]: https://smallcultfollowing.com/babysteps/blog/2012/04/27/in-favor-of-types-of-unknown-size/
[babystepsVecStrSlice]: https://smallcultfollowing.com/babysteps/blog/2012/04/23/vectors-strings-and-slices/
[babystepsVecSTD]: https://smallcultfollowing.com/babysteps/blog/2013/11/14/treating-vectors-like-any-other-container/
[babystepsDSTs]: https://smallcultfollowing.com/babysteps/blog/2013/04/30/dynamically-sized-types/
[babystepsDSTs5]: https://smallcultfollowing.com/babysteps/blog/2014/01/05/dst-take-5/#dynamically-sized-types

[strstdlib]: https://doc.rust-lang.org/std/primitive.str.html#representation
[fromutf8]: https://doc.rust-lang.org/std/str/fn.from_utf8.html
[sizedClosureRef]: https://doc.rust-lang.org/reference/types/closure.html#other-traits
[funcitemRef]: https://doc.rust-lang.org/reference/types/function-item.html
[nomiconZST]: https://doc.rust-lang.org/nomicon/exotic-sizes.html#zero-sized-types-zsts
[stdSized]: https://doc.rust-lang.org/std/marker/trait.Sized.html

[dstNom]: https://doc.rust-lang.org/nomicon/exotic-sizes.html
[dstRef]: https://doc.rust-lang.org/reference/dynamically-sized-types.html
[dstBook]: https://doc.rust-lang.org/stable/book/ch19-04-advanced-types.html?highlight=DST#dynamically-sized-types-and-the-sized-trait

[babystepsDSTs]: https://smallcultfollowing.com/babysteps/blog/2013/11/26/thoughts-on-dst-1/
[existentialTypes]: https://stackoverflow.com/questions/292274/what-is-an-existential-type
[externTypes]: https://github.com/rust-lang/rfcs/blob/master/text/1861-extern-types.md
[customDSTs]: https://github.com/rust-lang/rfcs/pull/2594
