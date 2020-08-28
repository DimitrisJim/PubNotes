---
layout: post
title: Auto Deref coercion
tags: rust, autoderef, coercions
---

An implicit coercion done at some point *during compilation*. Is not described in [RFC 0401][RFC0401] but is in the coercion sections of [the nomicon][nomcoerce] and [the reference][refcoerce]. The reference mentions both cases:

 > `&T` or `&mut T` to `&U` if `T` implements `Deref<Target = U>`

 > `&mut T` to `&mut U` if `T` implements `DerefMut<Target = U>`.

I'm pretty sure these are defined for most, if not all, of the smart pointers in the std. ([`Box`][BoxDeref], for example) Implementing smart pointers is where it usually makes more sense to define these (anything we might commonly work with that's behind a reference, really.)

There's a number of places this automatic dereference takes place and there's also an [adjustable limit][rec_limit] to how many times it can be performed. Coercions in the [reference][refcoerce] do a bit of a better job here but they don't mention two additional cases:

 - Auto-deref during lookups [of methods][methcoerce].
 - Auto-deref during [field accesses][facoerce].

Not sure if others might exist, haven't found any thus far.

In the following snippet, most cases are covered:

```rust
use std::ops::Deref;

#[derive(Debug)]
// For method call, field access
struct Dummy{a: i32}
// For struct instantiation
struct Basket<'a>(&'a str);

// Dummy box
struct MyBox<T>(T);

impl<T> MyBox<T>{
    fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}

impl<T> Deref for MyBox<T>{
    type Target = T;

    fn deref(&self) -> &T {
        println!("Invoked");
        &self.0
    }
}

impl Dummy{
    fn does(&self) {
        println!("Did.");
    }
}

fn arg_coerce(_: &str) {
    let _ = 0;
}

fn main() {
    // `let` statment coercion, similarly for `static` and `global`
    let _s: &str = &MyBox::new(String::from(""));
    // Struct instantiation coercion.
    let _b = Basket(&MyBox::new(String::from("")));
    // Function call coercion.
    arg_coerce(&MyBox::new(String::from("")));

    // Method and field access coercions.
    let _u = &MyBox::new(Dummy{a: 30});
    println!("Field: {:?}", u.a);
    u.does();

    // array literal.
    let _a: [&i32; 1] = [&MyBox(20)];
    // tuple literal.    
    let _t: (&i32, &i32) = (&MyBox(20), &MyBox(20));
    // parenthesized expression
    let _pe: (&f64) = (&MyBox(20.1));
    // block expression.
    let _b: &i32 = {
        println!("Don't");
        &MyBox(2)
    };
}
```

Note how the type always needs to be explicitly specified for in places were they are allowed to be omitted. If not, no coercion will not be performed since the types will be inferred as `MyBox`es.

[RFC0401]: https://github.com/rust-lang/rfcs/blob/master/text/0401-coercions.md
[refcoerce]: https://doc.rust-lang.org/reference/type-coercions.html
[nomcoerce]: https://doc.rust-lang.org/nomicon/coercions.html
[BoxDeref]: https://doc.rust-lang.org/std/boxed/struct.Box.html#impl-Deref
[rec_limit]: https://doc.rust-lang.org/reference/attributes/limits.html#the-recursion_limit-attribute
[methcoerce]: https://doc.rust-lang.org/reference/expressions/method-call-expr.html
[facoerce]: https://doc.rust-lang.org/reference/expressions/field-expr.html?search=
