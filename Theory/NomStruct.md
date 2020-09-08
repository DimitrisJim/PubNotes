---
layout: post
title: Nominal, Structural Types.
tags: rust, types, nominal, structural
---

*Nominal* and *structural* types can pop out when reading different language references. Since I bumped into them 
while reading the Rust reference manual; I'll use examples from there.

Add quotes from book on types by Pierce (see 19.3 Nominal and Structural Type Systems). 

Struct, enum, union are all nominally typed in Rust. add quotes.
Tuple is structurally types. add quote. 

There's a difference between (Nominal/Structural) *typing* and (Nominal/Structural) *subtyping*. Typing is 
what is mainly at play here in rust, not subtyping (see note, see wiki articles on structural and nominal typing).

Add few examples that show nominal/structural *typing* via a function arg.

Type aliases != nominal typing

Note: Trait objects and subtyping: Trait objects don't form a subtyping relationship between types that 
implement trait; trait objects make things opaque and facilitate dynamic binding. Touch on this, see 
reference on trait objects:

> A trait object is an opaque value of another type that implements a set of traits. 


[RefTypeAlias]: https://doc.rust-lang.org/reference/items/type-aliases.html
[RefEnums]: https://doc.rust-lang.org/reference/items/enumerations.html
[RefTuples]: https://doc.rust-lang.org/reference/types/tuple.html
[RefStructs]: https://doc.rust-lang.org/reference/items/structs.html 
[RefUnions]: https://doc.rust-lang.org/reference/items/unions.html
[RefSubtyping]: https://doc.rust-lang.org/reference/subtyping.html
[NomSubtyping]: https://doc.rust-lang.org/nomicon/subtyping.html

