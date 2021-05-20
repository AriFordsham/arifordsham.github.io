---
layout: post
title: How I reduced my Haskell CI time by 84%
---

I know how confounded I was by CI before I got into it, and how straightforward it seems now, so I thought I'd write down my experiences for anyone following in my footsteps.

In this post, I will speak about how I implemented Gitlab Continuous integration (CI), and then sped up execution time dramatically by setting the right options. My project is Haskell-centric, but many of the takeaways can be applied  to any language.

I use Michael Snoyman's excellent [stack](https://docs.haskellstack.org/en/stable/README/) build tool for my Haskell projects. Amongst many features that generally improve my quality of life as a developer, stack tries to guarantee *reproducible builds* - controlling as many variables as possible, to ensure build behaviour is the same - reproducible - between builds, even if I change the configuration of my machine, or even move the project to a different machine.

To achieve this, stack downloads all project dependencies innto the project directory. But it goes further - it downloads a fixed version of GHC, the Haskell compiler, and stashes it away in a special location. This ensures every version uses a specific version of the build toolchain, and updating my system GHC install  will never break a project due to a subtle change in compiler behaviour, for example.

I have a shell script I use that defines what I consider a full working build for my project, stored at `ci/build.sh`:
```haskell
stack build --test --coverage --haddock $@
```
- Run the stack build command to build the project (based on the poject's `package.yaml` file.)
- Run all test suites
- Generate a [test coverage](https://wiki.haskell.org/Haskell_program_coverage) report
- Generate [haddock](https://www.haskell.org/haddock/) documentation
- The `$@` passes command-line options from the script through to `stack build` - useful for one-off build scenarios.

My project is hosted on Gitlab, so I wanted to get GitLab's excellent CI to run and validate this script on every push.

Initial setup was dead straightforward (once I knew how!): I created a `.gitlab-ci.yml` file in the root of my project as follows:
```yaml
image: migamake/stack-build-image:17.0

stack:
  script:
    - ci/build.sh
```
- Define which Docker image to use. I'm using [stack-build-image](https://hub.docker.com/r/migamake/stack-build-image) from [Migamake](https://migamake.com/), which provides stack preinstalled, for [LTS Haskell](https://github.com/commercialhaskell/lts-haskell#lts-haskell-version-your-ecosystem) version 17.
- Define the `stack` job, which runs the script `ci/build.sh`.

Job done! On every push, Gitlab runs the build script in a Docker container. Since `stack` returns an error code if any test suite fails, I get a big red cross next to my commit on GitLab if my project fails to build or run correctly..

Here comes the issue: after writing that script, ever CI run took in excess of 33 minutes. Not that this is really a problem in any way: My CI script also runs in a pre-commit hook[^1] (which runs in under a minute on my development machine), so I never push without knowing CI passes. It's just nice to not have to wait long for that satisfying green tick, and with all those bitcoin miners around, It feels better not to be unneccesarily squandering the planet's resources.

[^1]: Here's how you do a precommit hook:

    Your git repo has a hidden directory, `.git`, where git stores it's bookkeeping. Under that directory is a directory called `hooks` (By default, it contains a set of `.sample` files showing what hooks are available). I created a file there called `pre-commit`:
    
    ```sh
    sh ci/build.sh
    ```
    
    On Linux, you'll generally need to set this as executable by running `chmod a+x .git/hooks/pre-commit`.
    
    This will now run before every commit. As with CI, an error code from the script will cause the commit to fail. You can override this with `git commit --force`.
    
A quick look at the build log shows the problem. As mentioned, stack downloads the GHC compiler and all dependencies. Since Docker does not store any state between runs, the build system has loads of downloading and compiling to do before it can even start building and testing the project itself.

That's just unnecessary duplication of work - the dependencies don't change between runs. On my local machine, stack keeps a cache of already-built libraries, so rebuilds are near-instant. However, to ensure reproducibility, all Docker runs start with a clean slate (besides whatever is in the container itself,) so this cache is not kept.

Gitlab's caching feature comes to the rescue. You can tell Gitlab that changes to certain directories won't affect the build correctness of your project, so Gitlab will go ahead and preserve those between runs. I added the following to my `.gitlab-ci.yml`:
```yaml
cache:
  key: "ALL"
  paths:
    - .stack-work/
```
This sets up Gitlab's cache for the `.stack-work/` directory, where stack keeps library dependencies[^2]. Now, at the end of every run, Gitlab zips up the `.stack-work` directory, and uploads it to a cloud bucket. Then, next time, before it begins running your CI scripts, it downloads and unzips it into your project directory.

This should allow stack to now find its cache from last time, and should enable zippy-fast builds, like I get on my local machine.

However, pushing the build script (twice; once to create the initial cache, and then again to see how it's used) gives only a negligible improvement in CI run time. A glance at the build log seems to show that the cache is working correctly. So what is going on?

stack caches project-specific artifacts in `.stack-work`, under the project directory. However, the bulk of its local cache, including the GHC build chain and many libraries, is stored by default in `.stack` under the user home directory. This enables the sharing of the cache between projects.

Now there's an additional complication: While Docker containers provide a home directory for projects to use, Gitlab cache only works for directories under the project directory. So I also need to set the `$STACK-ROOT` environment variable, to tell stack to store its build chain where the cache can see it.

My final `.gitlab-ci.yml` looks like this:
```yaml
image: migamake/stack-build-image:17.0

stack:
  script:
    - ci/build.sh

variables:
  STACK_ROOT: "$CI_PROJECT_DIR/.stack"

cache:
  key: "ALL"
  paths:
    - .stack-work/
    - .stack/
```

Success! After generating the cache, run time has dropped from 32 minutes to 7 minutes.

A look at the build log shows that this time is dominated by downloading and uploading the cache zip, so there's no more simple optimization opportunities.

Since I initially set up CI, my project has grown and added dependencies. So I ran the latest version, with and without the cache enabled.

A regular run now takes just under eight minutes, but without the cache, it takes almost 50 minutes, or an 84% reduction in run time.

CI looks daunting, but it simple enough once you invest the time to learn how to do what you want, andif you do it right (and don't overcomplicate!) you can significantly improve and smooth your workflow.

[^2]: The key field is required: it allows you to use different caches for different scenarios, such as branches, by setting the key to an environment variable. I trust stack to always use the right library versions from the `package.yaml`, and so the more sharing the better. Therefore I just use an arbitrary contant string so all scenarios share a single cache.
