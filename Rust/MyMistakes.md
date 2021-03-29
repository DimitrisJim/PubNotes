---
layout: post
title: My Rust Hiccups.
tags: rust, mistakes, learning
---

What I hope this will be is a list of mistakes I've done with Rust along with my explanations of why they are mistakes after I have tried understanding them.  Hopefully, this list will continuously grow as I learn more things.

If you, (meaning me in the future or anyone else, really), see a mistake in my explanations, please do open an issue and call me out on it.

The "what works" section isn't meant to showcase the "right" and "best" way of solving a specific error. It's what I used in order to fix what the compiler was shouting at me for. Without further ado, here's my humble list:

1. [Returning reference to local variable, happened during string interleaving.](#Returning-reference-to-local variable-during-string-interleaving)

### Returning reference to local variable during string interleaving

The issue cropped up when trying to solve [this][https://leetcode.com/problems/merge-strings-alternately/] leetcode problem. Essentially, as I would've done in other languages, I first tried to `zip` iterators over the characters of both strings in order to create the interleaving until the smallest string. Using a `for` based approach, this can easily be done by continuously `push`ing on a `mut String`.  

Since I always want to try things out by applying iterator transformations, after some attempts I ended up `zip`ing and then trying to `map` my `(char, char)` tuples into `&[char, char]` arrays and then flattening it as so:

```rust
s1.chars()
    .zip(s2.chars())
	.flat_map(|pair| &[pair.0, pair.1])
    .collect::<String>(); 
```

Of course, the compiler was having nothing of this and shouted at me like an emotionally abusive father. In short, the following was thrown right in my face:

```rust
error[E0515]: cannot return reference to temporary value
  --> src/main.rs:12:26
   |
12 |         .flat_map(|pair| &[pair.0, pair.1])
   |                          ^----------------
   |                          ||
   |                          |temporary value created here
   |                          returns a reference to data owned by the current function

error: aborting due to previous error
```

for some reason this seemed so weird to me though I can now see how obvious it is (funny how that is so often the case). What the compiler is telling me (in its on special way) is that I'm basically trying to do the equivalent of this:

```rust
fn silly<'a>(a: char, b: char) -> &'a [char; 2] {
    &[a, b]
}
```

which I can very easily see is bad. The solution is to return (as usually suggested) a copy of the thing you're creating, not a reference. With my, admittedly limited, knowledge, this essentially meant I'll return a `Vec` out of those two characters, in short, `vec![pair.0, pair.1]` (ok, we could unpack it in the closure definition).