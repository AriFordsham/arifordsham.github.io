---
layout: post
title: Rust is more permissive than C, but C has better concurrency support
---

```
f() + g();
```

What happens if `f` and `g` touch the same bit of state?

- In **Rust**, this is perfectly valid, and `f` runs strictly before `g`.

- In **C**, this construct is not legal - it is 'undefined behaviour.' Your compiler
  won't (can't) disallow it, but it is allowed to assume that it never happens.

This means that _theoretically_, a **C** compiler is allowed to automatically parallelize
this code, but in **Rust**, we must wait for `f` to complete before `g` is able to run.

Enjoy the rest of your day.