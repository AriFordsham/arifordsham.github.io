---
layout: post
title: What Can I Refactor For You Today?
---
As developers, we love building things. Haskell gives us great opportunities to channel our creative drives into producing libraries and tools that are (hopefully) both intellectually elegant and functionally useful. But there's more to building software than just creating. 

Unfortunately, software often needs maintenance, or other changes after the initial write. On older projects, library or GHC changes mean code might no longer compile. A project may have been architected without accounting for ways it may need to change in the future, and extending it is now a struggle. In the rush to get something completed, an engineer may be left with an implementation that he cannot be confident is free of bugs, or is simply not satisfyingly tidy or elegant.

That's why I'd like to offer my services, as a Haskell consultant, to maintain and refactor existing code, either yours or someone else's. If any of the following apply to you, I'd love to know if I can be of service.

- You're interested in using a Haskell tool or library, but it -  or one of its dependencies - has 'bit-rotted' - it won't build on current GHC/`base`.

- You've got a project you'd like to bring up to date with more recent GHC features or library idioms.

- You have a project using library *x*, and you'd like to port it to use library *y* instead.

- You wish the internals of your project were easier to reason with than they are.

- You'd like to untangle pure code from `IO`, or invert dependencies to make your program more extensible.

- You'd like to introduce typeclasses to generalize and extend your code.

- You'd like to lift some of your program's semantics from runtime errors and `Maybe`s into the type system.

- You'd like to move your project, or a library you are using, over to the `stack` build system for more dependable dependency tracking and reproducible builds.

- You'd like to see a library (possibly somebody else's) added to Stackage for the CI quality assurance that provides.
 
I'm especially excited to see how I can leverage Facebook's [`retrie`](https://github.com/facebookincubator/retrie) tool to automate changes over sizable codebases.

Dependable refactoring needs test coverage to make sure you're not breaking anything. Types help with this, but they are not the whole answer - There's still a need for tests. I can also help with writing or expanding test suites.

Of course, I'm also available to assist you along your Haskell journey in other ways - whether through development or training/tutoring.

If you think I might be able to be of assistance with your project, you can [read my CV](https://docs.google.com/document/d/1hSYvXiOq99diyL_6Ulj0ENJ2neXxc4NZ7SrUaMMEw0A/edit?usp=sharing), and be in touch by email to [arifordsham@gmail.com](mailto:arifordsham@gmail.com).

Ari Fordsham
