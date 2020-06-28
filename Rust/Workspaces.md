---
layout: post
title: Rust Workspaces
tags: rust, workspaces
---

When trying to test a small `lib` that manipulates [attribute macros](attr_macros)
I needed to create some sample macros to test against. In the beginning, I simply
created a little macro inside my `lib.rs` and changed `Cargo.toml` to include
`proc-macro = True` as one would do. Problem is that, in the end, I don't really
want my `lib` to contain macros since that's not the functionality it's supposed
to offer. On the other hand, I need to have macros somewhere because I need to
test the library.

What this boils down to is this: I need macros but I want them in some
little dark basement where nobody can see them. This is where workspaces help,
they let you create this little dark basement.[^1] Apart from also keeping your
dependencies in sync, workspaces also help you test everything in one go with
`cargo --test --all` which is of great value when running actions on GitHub.[^3]

The *public-facing* documentation for workspaces can be found in both [the book](workspacesBook)
and in [cargo's docs](workspacesCargo). Both of these do a pretty good job of
giving an overview of the situation. In addition to these, it's always good to
search around for an RFC[^2] for a given feature, it tends contain some additional
information not suited for more concise tutorials. RFC for workspaces is [RFC-1525](rfc1525)
Note that workspaces are a 2018 edition feature.

Using workspace jargon, my goal now was to have a *root* crate containing my
main library and a side-crate to contain macros that use my library.
Following the documentation for these, the steps are simple. Let's say
my library and root crate is called `rustyroot` and my sublibrary and second
crate is called `rustyfoo`.

 - Create a `[workspace]` entry in my top-level `rustyroot/Cargo.toml`.
 - Create a new crate for my macros inside `rustyroot`.
 - Add this new crate as a member in the `[workspace]` entry for my top-level
   `Cargo.toml` with `members = ["rustyfoo"]`
 - Since I want to test `rustyroot`, add it as a dependency in
   `rustyfoo/Cargo.toml` with `rustyroot = { path = ".." }`.

Though weird, I also needed to include the dependencies in `rustyroot/Cargo.toml`
*again* in `rustyfoo/Cargo.toml`. I thought these would be available (since we
share the `Cargo.lock`) but this is probably a starters misunderstanding.

All in all, after doing these I was able to use `rustyroot`'s public interface
in the sub crate and test things out with `test --all`.

The next thing I'll probably need to see is how `[workspace]` behaves when you
publish things; I think each crate can be published independently but this
raises the question of what happens when a member of workspace is defined but
not found. I'd guess that you either publish without the sub crate or you
somehow mark it as private. I hope I'll soon see.

### Footnotes:
[^1]: I can't simply not export the macro since `proc-macro` being `True` [*requires only*](linkage_) macros be exported.
[^2]: Request For Comments documents are the standard way new features are introduced.
[^3]: I was mainly concerned with the testing aspect, I'd guess there's ways to do this without workspaces.

[attr_macros]: https://doc.rust-lang.org/reference/procedural-macros.html#attribute-macros
[linkage_]: https://doc.rust-lang.org/reference/linkage.html#linkage
[workspacesBook]: https://doc.rust-lang.org/book/ch14-03-cargo-workspaces.html
[workspacesCargo]: https://doc.rust-lang.org/cargo/reference/workspaces.html
[rfc1525]: https://rust-lang.github.io/rfcs/1525-cargo-workspace.html
