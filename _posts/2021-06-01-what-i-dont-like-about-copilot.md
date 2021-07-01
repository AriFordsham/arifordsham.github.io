---
layout: post
title: What I don't like about Github Copilot
---
DISCLAIMER: I haven't had a chance to use Copilot - I'm on the waitlist, like everyone else. I'm writing based on the impressions I've got from the [quite nice website](https://copilot.github.com/), the underlying [OpenAI Codex](https://beta.openai.com/) technology, and my own rudimentary ideas of how machine learning works.

Microsoft, the owners of Github, annouced that a new project -- Github Copilot -- entered Technical Preview on the 29th June 2021. It's an extension to the Visual Studio Code editor that analyzes your code and suggests completions -- even multi-line functions -- based on a machine learning algorithm, that was trained on all of Github. From the online examples and testimonials, this sounds like a a Big Deal.

You are now invited to [join the waitlist](https://github.com/features/copilot/signup) and who knows? you may be one of the lucky ones who gets to give it a spin. Spoiler: Github have [said](https://copilot.github.com/#faqs) "we are offering GitHub Copilot to a limited number of testers for free" and "if the technical preview is successful, our plan is to build a commercial version of GitHub Copilot in the future." The production version almost certainly won't be available free, and probably won't be cheap. If it's as good as it promises, it could become a staple of commercial programming shops, but indie developers like me who don't *need* this might not be able to justify it. Oh well, just saying.

I think Copilot does have genuine promise, and the possibility of making meaningful improvements to the process of coding. But from what I've seen -- and this makes sense -- Copilot does not *create*. All it does is attempt to piece together an ad-hoc description of the programmer's meaning from information embedded in code, comments and names into hopefully correct executable code. Your informal description probably needs to contain barely less detail than the code itself. In this way, Copilot transforms and sharpens, rather than innovates[^2].

[^2]: This observation, by the way, can be generalized to every machine learning (ML) project I have seen to date -- very powerful but blunt generalization tools that don't really have more than 'one level' of insight (whatever that means.) Can AI do more? Very possibly, but I haven't yet seen a single proof-of-concept.

There's a joke going around: after Copilot comes Pilot, and then I'll have to get a new job (if there are any). Jokes aside, getting from Copilot to Pilot is WAY bigger than getting from nothing to Copilot - it's not even the same kind of thing.

But here's the problem. The most fundamental principle of software development is DRY -- Don't Repeat Yourself. Repeating the same code in two places is a code smell - you should find some way of writing it only once, such as putting it in a function.
The formal name for avoiding repetition is *abstraction*.

Now, in order for a model like Copilot to learn, it needs a lot of redundancy in it's dataset. The success of Copilot hinges on the fact that in the code we write today, there is still loads of duplication -- we're not nearly there yet. And the real solution to redundancy is getting better at abstraction -- finding and using new abstraction mechanisms, and learning how to apply abstraction more widely and effectively[^1].

[^1]: At this point I'll give a plug for my favorite technologies: pure functional programming languages, especially [Haskell](https://www.haskell.org/). Functional languages take abstraction very seriously -- they have a bunch of powerful abstraction mechanisms, and their communities try very hard to discover new ways of encoding common patterns. Haskell programs use fewer lines of code to accomplish the same tasks. It would be very interesting to me to see if Copilot is less effective on Haskell because it has less redundancy.

Copilot's breakthrough party trick is the ability to analyze natural language in comments and function and variable names to come up with suggestions. However, many of the examples on the website show that Copilot can -- and does -- suggesti multi-line code snippets, the kind of thing I look at and think: that shouldn't be pasted in, it should be abstracted away. By doing this, Copilot is locking in a duplicative coding style. At the same time as Copilot is doing your work for you, it's also taking away the pain of being verbose and repetitive and thereby encouraging copy-paste-style coding. I worry how this could hold back progress on searching for abstractions. Abstraction is more than saving developer time -- it can even slow you down up front. Rather, it helps make code more readable and maintainable (by moving irrelevant detail away), it can even make code more correct and performant (by spending the time getting it right once), but above all -- *it adds to our understanding of software development* -- it lets us recognise these pattens, even give them a name, and thereby guides us to think higher level. Blindly pasting code from Copilot doesn't do any of these things.

Of course, on the flip side, Copilot might encourage developers to write comments, which is a good thing. Then again, they probably won't delete or trim the comments after they write(?) the code, and therefore the comments will repeat what the code says, which is bad[^3].

[^3]: See [Chapter 1 of The Practice of Programming](https://learning.oreilly.com/library/view/the-practice-of/9780133133448/ch01.html#ch01lev1sec6) for why.

Can we use AI to find abstractions? Very possibly, and that would be amazing. but it will be a lot harder than merely analyzing and parroting code, and no one the size of Microsoft/Github is even trying.

I'm not knocking Co-pilot -- it definitely seems to be, rather than a gimmick, an amazing product that has the potential to transform the way we code, and especially help developers bridge the gap from junior to proficient. I just hope the activity of programming will not take a step back as a result.
