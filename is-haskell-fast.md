# Is Haskell fast?

If you try to research this question, you might come up with confusing and contradictory answers, strongly-stated opinions, and loads of technical jargon.

The answer depends which perspective the questioner is coming from, and what assumtions, expectations and preconceived notions they bring with them.

## Correctly written Haskell is fast for a high-level language

Haskell is a *high-level language* - a tool used to write programs with a goal of avoiding the programmer having to specify implementation details, as far as practical. This is typically the most productive way of writing most software, where performance must be 'adequate' rather than optimum.

In this space, developers typically reach for *dynamically typed, interpreted* languages such as JavaScript and Python. This kind of language design has an inherent overhead, because there is no opportunity to eliminate runtime work ahead-of-time, and in particular the program has to shuffle around and bookkeep type information.

Haskell, on the other hand, is *type-erased* and *compiled*. The GHC compiler does an excellent job of optimizing Haskell code to make it run much faster than it otherwise might. So as long as the developer is aware of the necessary caveats and pitfalls (more on those later), Haskell programs will run dramatically faster than their HLL counterparts.

## Straightforward Haskell will be slower than C

It remains a fact that code written in clear, idiomatic Haskell will (at present) be slower than code written in a low-level language such as C, C++ or Rust, which would typically be used for performance-critical code. GHC cannot yet optimize all the overhead from the Haskell abstractions, and will lose out to code that runs close to the metal, typically with a slowdown of around 50% to 4x.

This leaves Haskell in a bit of a tight spot when it comes to performance: it competes well against other high level languages in a domain where performance isn't that important, but can't quite keep up with the incumbents in the race for ultimate speed.

However, if someone is using Haskell for other reasons, to write more concise, maintainable code that will be easier to be confident in its correctness, they will get decent performance and responsiveness 'for free!'

## You can write performance-competitive Haskell

So what to do with performance-critical code? One option is to write it in a low-level language, possibly calling in from Haskell through the Foreign Function Interface. In doing so, the programmer accepts the sacrifice of expressivity and safety, and must factor for the overhead of FFI and marshalling.

But there is another way. With a strong understanding of the GHC execution model, there is a set of techniques that can be used to write Haskell code that compiles to run at least as fast as equivalent C programs. This takes quite a bit of expertise to do, and it won't be quite as tidy as idiomatic Haskell, but it should still be more readable (to the practiced eye) and maintainable than a highly-tuned C implementation.

A full rundown is beyond the scope of this post, but I'll link to some resources:

- Writing Haskell as fast as C: ... (Written in 2008)

- All about stricness analysis (How to take advantage of the latest compiler optimizations)

## Watch out for the Haskell performance traps

There are, however, some things that every production Haskell developer needs to be aware of. There is Haskell code that looks correct, but will run *way* slower, and use far more memory, than the worst you will come across in an interpreted language. The slowdown can run to several factors, with programs occasionally running out of stack space before they can get anything done!

This is due to a controversial design decision called *laziness*, compounded by, in hindsight, subptimal library design and idiom choice, and needs to be understood in order to be able to gaurantee consistent reasonable performance from your Haskell code.

I won't go into all the details here, but in short, never use `foldl` (use `foldl'` instead), know how to use bang patterns, and read this article by Michael Snoyman:

- All about strictness

## Abstaction doesn't have to cost; it can even pay

People seem to think there is inevitably a price to pay for writing high-level code. In fact the opposite is true. Theoretically speaking, high-level code is easier to optimize than low-level code. It simply provides more information to the compiler about the programmer's intent.

As an illustration, let's take the case of Rust. Rust is widely regarded as faster than C and C++, despite being somewhat higher level. Mozilla is in the process of rewriting the engine of Firefox from C++ to Rust, and is already seeing performance gains. In spite of this, if you look through the Rust issues on Github, you'll see many instances of the 'slow' tag, suggesting unexploited optimization opportunities. This suggests Rust is set to get faster yet.

How can this be? Surely everyone knows nothing beats hand-written C code?

I believe this to be a widely held myth. Optimization is more-or-less an exact science, and machines must eventually outperform humans. And ultimately, a perfect Haskell compiler will outperform a perfect optimizing C compiler.

Why is this?

- A C program prescribes precisely how a problem is to be solved. There is a limit how far the compiler can go in rearranging the code without breaking the language semantics. A high-level program describes the *problem*, and gives the compiler more freedom to choose the best implementation.

- Few programmers have a comprehensive knowledge of how best to optimize their programs. Compiler optimizations can be written once by a community of domain experts, and then applied widely.

- Even the most experienced C programmer must keep objectives other than sheer speed in mind while writing his code. The program must be able to be evaluated for correctness without losing his train of thought, and it must be extendable and maintainable, often by other people. In the source code, these are unavoidable trade-offs.

- Because of this, even programmers steeped in C performance technique must program defensively to ensure program correctness. A compiler can perform aggressive optimization that will make a programmer's jaw drop, simply because it can keep track of far more program state at a time in order to gaurantee correctness. The compiler can then often build optimizations on other optimizations.Imagine trying to aggrssively inline a program by hand.

## Summary

As long as it your code is written with an eye to a few common pitfalls, Haskell is a fast language compared to it's closest competitors, although not quite fast enough to compete in the speed stakes. Heavily hand-optimized Haskell *can* be competitive, and we can expect compiler advances to further narrow the gap for more idiomatic code in coming years.
