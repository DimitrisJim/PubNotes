---
layout: post
title: Rust Debugging
tags: rust, debugging, gdb
---

Normal `gdb` works in a limited way for a rust program. Many constructs do
not get printed in a very friendly way, though.

`rust-gdb` is a little wrapper around gdb that injects some [python code][PyGDB] that pretty prints some of the objects. Other than that, from what I've understood, the language doesn't seem to have first class support so far.
A little more on this can be seen in the following [post][mwrgdb] (see section What Not To Expect).

In another interesting sidenote, breaking on `main` with `b main` isn't possible
since another `main` symbol already seems to exist. E.g, for a given simple `m.rs` file:

```rust
// in m.rs
fn main() {
  let u = 32;
  let y = 23;
  println!("{}", u + y);
}
```

compiling with `rustc -g m.rs` and calling `nm` we can see a couple of symbols
containing `main` being exported:

```bash
# nm -an m | grep "main"
m:                 U __libc_start_main@@GLIBC_2.2.5
m:00000000000052e0 t _ZN1m4main17he77ef024786bfe20E
m:00000000000053d0 T main
```

I'm pretty sure that's `C++` name mangling, using `--demangle` with `nm` yields the de-mangled name:

```
m:                 U __libc_start_main@@GLIBC_2.2.5
m:00000000000052e0 t m::main
m:00000000000053d0 T main
```

so that's the name you can break on, `m::main` in my case. Applying the same steps to a package made with `cargo new`, it
seems that `main` is prefixed with the package name attribute.

Note: No idea what `main` in address `53d0` is. Looking through
`objdump` it calls `m::main` after some other instructions:

```
00000000000053d0 <main>:
    53d0:	48 83 ec 18             sub    $0x18,%rsp
    53d4:	8a 05 db dd 02 00       mov    0x2dddb(%rip),%al        # 331b5 <__rustc_debug_gdb_scripts_section__>
    53da:	48 63 cf                movslq %edi,%rcx
    53dd:	48 8d 3d fc fe ff ff    lea    -0x104(%rip),%rdi        # 52e0 <_ZN1m4main17he77ef024786bfe20E>
```

though I still am not sure where it comes from.

[PyGDB]: https://github.com/rust-lang/rust/blob/master/src/etc/gdb_providers.py
[mwrgdb]: https://michaelwoerister.github.io/2015/03/27/rust-xxdb.html
