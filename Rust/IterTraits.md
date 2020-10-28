---
layout: post
title: Iterator TLDR 
tags: iterator, trait
---

The main traits at play here are [`Iterator`][iterTrait], 
[`IntoIterator`][intoiterTrait] and [`FromIterator`][fromiterTrait]. 

### Iterator

This is the main trait we can implement that then gives us access 
to all the functionality defined for `Iterators` (`sum`, `filter` 
and friends). Only requires an implementation for `next` (as is
common in other languages) to define the logic of continuously 
retrieving items. A struct holding the iterator state is defined 
and `Iterator` is implemented on it. This iterator struct is 
usually returned via an `iter` method the collection/object 
defining the iteration. 

Though, as the documentation on the `iter` module [states][note1], 
implementing `Iterator` does automatically mean you've got 
`IntoIterator`, this *only* applies for the iterator struct you 
return after calling `iter`. That is:

```rust
// Want to make FooSequence iterable.
struct FooSequence {
    // fields
}
// So we create a struct to hold necessary state.
struct IterFoo {
    // Hold important state.
}

impl FooSequence {
    // We've got our fancy FooSequence methods first.
    // People can grab our iterator by calling iter
    fn iter(&self) -> IterFoo {}
}

let mut myFoo = FooSequence::new(...); // doesn't have IntoIterator
let itFoo = myFoo.iter();              // has IntoIterator
```

So, in this case, `itFoo` can be used in a `for` loop as is. `myFoo`
cannot. This brings us to `IntoIterator`.

### IntoIterator

This trait, implemented by implementing `into_iter` for your type, 
allows you to magically use the type in a `for` loop without first 
needing to explicitly grab the iterator. Based on the previous
snippet, if we implement `into_iter` for `FooSequence` we won't 
need to first create an `itFoo` to iterate through via a `for` loop.

### FromIterator

Similarly to `IntoIterator`, this is also more straight-forward 
trait. It basically allows us to create our type based on an 
iterator we get. Mainly invoked by [`Iterator.collect`][iterCollect].

### Additionally..

Collections usually also have an `iter_mut` method present, used 
for iterating through a collection via mutable references to its 
elements. In short, [this little section][formsofit] of the docs is handy:

 - `iter()` iterates over `&T`s.
 - `iter_mut()` iterates over `&mut T`s.
 - `into_iter()` iterates over `T`s.

See [`iter` module Traits][itermodTraits] for more coolness.

[iterTrait]: https://doc.rust-lang.org/std/iter/trait.Iterator.html
[intoiterTrait]: https://doc.rust-lang.org/std/iter/trait.IntoIterator.html
[fromiterTrait]: https://doc.rust-lang.org/std/iter/trait.FromIterator.html
[iterCollect]: https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.collect
[note1]: https://doc.rust-lang.org/std/iter/index.html#for-loops-and-intoiterator
[formsofit]: https://doc.rust-lang.org/std/iter/index.html#the-three-forms-of-iteration
[itermodTraits]: https://doc.rust-lang.org/std/iter/index.html#traits
