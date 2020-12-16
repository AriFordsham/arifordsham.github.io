---
layout: post
title: Haskell - Doomed to Succeed?
---
The unofficial motto of Haskell, the predominant lazy functional language, has long been:

> *"Avoid success at all costs."*

This is attributed to Simon L. Peyton-Jones (SPJ), the main architect of the Haskell language and the GHC compiler, and mentioned in the [slides](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/07/HaskellRetrospective.ppt) of a [talk](https://www.microsoft.com/en-us/research/publication/wearing-hair-shirt-retrospective-haskell-2003/) he gave at the POPL conference in 2003.

This seems an odd statement. What's wrong with a bit of success?

The exact meaning of this statement is controversial. Simon Marlow, another major Haskell figure, [quotes SPJ](https://twitter.com/simonmar/status/246335257677271040) that this statement (expression?) should not be bracketed `(avoid success) at all costs`, i.e. do all you can to ensure success doesn't happen, but rather `avoid (success at all costs)` - success may be a nice idea, but avoid falling in to the trap of focusing all the community's efforts to attaining it.

But isn't success the goal of any project?

The answer can be found in an [article](https://www.aosabook.org/en/ghc.html) written by the aformentioned Marlow and Peyton-Jones:

> [T]he ultimate goal for us, the main developers of GHC, is to produce research rather than code. We consider developing GHC to be an essential prerequisite: the artifacts of research are fed back into GHC, so that GHC can then be used as the basis for further research that builds on these previous ideas. 

In other words, the success referred to is *commercial* success, or wide user adoption. The stated goal of GHC (and the Haskell language) is to better understand the principles of functional programming, and the mathematical and logical principles that underly them.

A wide user base, using Haskell in important projects, poses a responsibility to the Haskell designers. Users have needs, and can push the project in directions irrelevant to the core research goals, as the authors go on to state:

>  [A] great deal of effort is put into ensuring that [GHC] can be relied on for production use. There has often been some tension between these two seemingly contradictory goals, but by and large we have found a path that is satisfactory both from the research and the production-use angles.

They certainly seem to have been successful: GHC is widely considered a robust and reliable compiler, and the GHC developers take the quality of their product very seriously, despite their very limited resources.

So while SPJ claims to have been saying "don't allow efforts to make the platform appealing to production users take away from Haskell's key principles," It definitely has been understood in practice to mean "Let's try not to draw attention to ourselves, because too many users will prevent us doing what we want to do."

Until now.

In his POPL talk, SPJ quotes Anthony Hoare as saying:

> *"I fear that Haskell is doomed to succeed.”*

Haskell's advantages - a strong mathematical basis, facilities for abstraction and de-duplication, [excellent performance compared to other high-level languages](https://arifordsham.com/is-haskell-fast/), good correctness guarantees, ease of concurrency, but most of all, *enabling a better way to think about code*, have increasingly lead to its [widespread adoption in production code](https://wiki.haskell.org/Haskell_in_industry). Haskell is contending with success despite itself.

This shift in the makeup of the Haskell community certainly has led to a shift in focus. At the Haskell eXchange conference in November 2020, SPJ [announced](https://youtu.be/MEmRarBL9kw) the [Haskell Foundation](https://haskell.foundation/), explicitly "dedicated to broadening the adoption of Haskell" as well as "supporting its ecosystem of tools, libraries, education, and research."

So while the original architects of Haskell may find this unintended success unwanted, it is certainly a vindication of their vision: remaining faithful to strong mathematical principles and resisting calls for premature pragmatism can transform the things we do.

### UPDATE

Simon Peyton-Jones has graciously commented on this post - you can read his remarks [here](https://discourse.haskell.org/t/new-blog-post-haskell-doomed-to-succeed/1662/2).

In short, he reiterates that wide production adoption was never an anti-goal of Haskell, but rather not "making fundamental compromises of core principles [of Haskell] in pursuit of short-term production goals."
