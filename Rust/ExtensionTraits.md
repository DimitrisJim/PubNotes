---
layout: post
title: Extension Traits
tags: rust, extension traits
---

Basically the term we use for local traits we implement for types not local to our crate.

For example, say we wanted to add a `reversed` method on `Vec`s. Since we can't define an `impl` block for types outside our crate, we can define a trait that has the needed functionality (this is one of the things [`E0116`][error0116] suggests we do). Then, we implement that trait on `Vec<T>`:

```rust
trait ReverseEx {
    fn reverse(&mut self);
}
```

(note, a suffix of 'Ex' seems to be common) Then, we implement it on `Vec<T>`:

```rust
impl<T> ReverseEx for Vec<T>
    where T: Ord + Copy
{
    fn reverse(&mut self) {
        self.sort_by_key(|&k| Reverse(k))
    }
}
```

and now our extension trait `ReverseEx` allows us to use `reverse` on vectors.

[error0116]: https://doc.rust-lang.org/stable/error-index.html#E0116
