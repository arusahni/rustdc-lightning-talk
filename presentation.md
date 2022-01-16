%title: Rewriting git-req in Rust
%author: Aru Sahni (@IAmAru), for Rust DC 🦀
%date: 2021-03-16

-> Rewriting git-req in Rust <-
==========

-> An adventure in drinking from the firehose <-

---

-> git-req <-
=============

* A git extension
* Check out GitHub PRs and GitLab MRs by their number (instead of branch name)
* `git checkout feat/PROJECT-13/Bikeshed_yak_shaving_protocol_42` becomes `git req 22`
* Written in Bash
    * Inline Python for regex and JSON

----

-> It worked! <-

Users started ~demanding~requesting new features. I wanted to provide them, but, I refer you to:

> Written in Bash

Ideally, this is something users could download as a binary and execute.

I needed to rewrite it. But in what language?

---

-> Choices <-
=============

1. Python
    * I'm a Python dev, but this ecocsystem still doesn't have a good distribution story.
2. Go
    * Binary distribution
    * I have Opinions on the language's decisions
    * Straddles the boundary between systems and application programming
    * Controlled by a single corporate entity
3. Rust
    * Binary distribution
    * Awesome community
    * Strong value-adds over other systems languages

---

-> ✅ Rust <-

---

-> Don't use all of Rust initially <-
=====================================

-> Simple is key <-

* Start with something that works, and then refactor.
* Do you _need_ async?
* Combinators are great, but can make it harder to reason about the state in the middle of a pipe.
* Don't be afraid to unwrap
* **Poorly written Rust is still pretty fast**

---

-> Start with the important things <-
=====================================

* Logging (`log`, `env_logger`, `DEBUG`)
* CLI parsing
* TESTS

---

-> The compiler is your friend... until it isn't <-
===================================================

-> Glimpses behind the curtain <-

* Lifetimes
* Borrow-checking
* Movement
* Strings

-> Tips <-

* Create scope-local assignments to make ownership easy to follow.
* Understand and implement `Copy` and `Clone` where needed.
* Avoid OOP patterns if you can. Try inverting things with Enums and match expressions.

---

-> Use rust-analyzer instead of RLS <-
======================================

-> 'nuff said <-

---

-> Use Clippy <-
================

-> Automated code review 🤖 <-

---

-> Portability can be tricky <-
===============================

-> If shipping your binary to other machines, you need to worry about linking <-

* As a Python dev, this was largely foreign to me.
* Switch from OpenSSL to rustls to prevent hair pulling.
* Cross + a good CI platform help.

---

-> Watch your binary size <-
============================

-> It's easy to have a beefy binary <-

* Set an `opt-level`.
* Configure `lto` and `codegen-units`.
* Use `cargo-bloat` to understand your dependencies.
    * Swap out crates as needed.
* Use `strip` to remove unnecessary symbols from the binary.
* Abort on panic (if writing an app).

-> Shameless plug: https://arusahni.net/blog/2020/03/optimizing-rust-binary-size.html <-

---

-> Deployment is hard <-
========================

* PPAs are challenging, `deb` files are easier.
* Arch and Homebrew are wonderful targets.

---

-> Fin. <-
==========

* Learning Rust through rewriting a simple project was maddening and fun.
* Rust's meteoric rise means you find a lot of outdated information.
* Oxidized git-req is in the wild, and is a joy to maintain and hack on.

-> https://arusahni.github.io/git-req/ <-

-> *Questions?* <-
