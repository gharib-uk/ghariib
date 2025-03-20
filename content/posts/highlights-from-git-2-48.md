---
title: "Highlights from Git 2.48"
date: 2025-01-12
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

The open source Git project just released Git 2.48. Here is GitHub's look at some of the most interesting features and changes introduced since last time.

The post Highlights from Git 2.48 appeared first on The GitHub Blog.

The open source Git project just released Git 2.48 with features and bug fixes from over 93 contributors, 35 of them new. We last caught up with you on the latest in Git back when 2.47 was released.

To celebrate this most recent release, here is GitHub’s look at some of the most interesting features and changes introduced since last time.

## Faster SHA-1s without compromising security

When we published our coverage of Git’s 2.47 release, we neglected to mention a handful of performance changes that went in toward the very end of the cycle. Because this version contains a bugfix related to those changes, we figured now was as good a time as any1 to discuss those changes.

You likely know that Git famously uses SHA-1 hashes by default to identify objects within your repository. (We have covered Git’s capability to use SHA-256 as its primary hash function instead, but for this tidbit we’ll focus on Git in its SHA-1 mode). What you may not know is that Git uses SHA-1 hashes internally in a couple of spots, too. Most notable for our purposes is that the pack format includes a trailing SHA-1 that stores the checksum of the preceding bytes. Git uses this data to validate that a pack’s contents matches what was advertised, and didn’t get corrupted in transit.

By default, Git uses a collision detecting implementation of SHA-1, hardening it against common SHA-1 attacks like SHAttered and Shambles. (GitHub also uses the collision detecting SHA-1 implementation). While the collision detecting SHA-1 implementation protects Git against collision attacks, it does so at the cost of a few extra CPU cycles to look for the telltale signs of these attacks while checksumming.

In most cases, the performance impact is negligible, and the benefit outweighs the minor performance cost. But when computing the checksum of a large pack (like when cloning a large repository), the cost adds up. For instance, we used Callgrind and measured that Git spends around 78% of its CPU computing a checksum during a simulated clone of `torvalds/linux`.

Luckily, the trailing checksum is a data integrity measure, not a security one. For our purposes, this means that we can safely use a faster, non-collision-detecting implementation of SHA-1 specifically when computing trailing checksums (but nowhere else) without compromising security. Git 2.47 introduced new build-time options to specify a separate hash function implementation used specifically when computing trailing checksums. GitHub has used this option, and as a result measured a 10-13% performance improvement in serving fetches/clones across all repositories.

You can try out Git’s ability to select alternative hash function implementations by building with `make OPENSSL_SHA1_UNSAFE=1`, or other `_UNSAFE` variants.

\[source, source, source\]

## Bringing `--remerge-diff` to range-diff

Regular readers of this series will no doubt recall our coverage of Git’s range-diff command (introduced back in Git 2.19), and the newer –remerge-diff option (released in Git 2.36). In case you’re a first-time reader, or neither of those ring a bell for you, don’t worry; here’s a brief refresher.

Git’s `range-diff` command allows you to compare two _sequences_ of commits, including changes to their order, commit messages, and the actual content changes they introduce. This can be useful when comparing a sequence of commits that were rebased (potentially tweaking the order and changes within the patches along the way), to what that set of commits looked like before the rebase.

Git’s `--remerge-diff` option tells `git show`, `git log`, or various diff-related commands to view the differences between where Git would have stopped with the merge, and what is recorded in the merge. This can be useful when dealing with merge conflicts, since the `--remerge-diff` view will show you the difference between the conflicts and their resolution, showing you how a given merge conflict was handled.

In Git 2.48, these two features meet for the first time, and `range-diff` now accepts a `--remerge-diff` option, so that if someone rebases a sequence of commits with `--rebase-merges` and potentially needs to make some changes, then the changes in merge commits can also be reviewed as part of the range-diff.

As a side effect of this work, a longstanding bug with `--remerge-diff` was also fixed, which in particular will allow `git log –remerge-diff` to be used together with options that change the order of commit traversal (such as `--reverse`).

\[source, source\]

## Memory leak-free tests in Git

Beginning all the way back in Git 2.34, the Git project has been focused on reducing memory leaks with the goal of ultimately making Git leak-free. Since Git is a command line tool, each execution typically only lasts for a brief period of time, after which the kernel will free any memory allocated to Git that Git itself did not free. Because of this, memory leaks in Git have not posed a significant practical issue for everyday use.

But, having memory leaks in Git makes it difficult to convert much of Git’s internals into a callable library, where having memory leaks would be a significant issue. To address this, there has been a concerted effort over many years to reduce the number of memory leaks in Git’s codebase, with the ultimate goal of eliminating them altogether.

After much effort toward that end, Git can now run its test suite successfully with leak checking enabled. As a satisfying end result, much of the test infrastructure we talked about back in 2.34 was removed, resulting in a simpler test infrastructure. Making Git memory leak-free represents significant progress toward being able to convert parts of Git’s internals into a callable library.

\[source, source, source, source, source, source, source, source, source, source, source, source\]

## Introducing Meson into Git

The Git project uses GNU Make as the primary means to compile Git, meaning that if you can obtain a copy of Git’s source, running `make` should be all you need to get a working Git binary (provided you have the necessary dependencies, etc.). There are a couple of exceptions, namely that Git has some support for Autoconf and CMake, though they are not as up-to-date as Git’s `Makefile`.

But as the Git project approaches its 20th anniversary later this year, its `Makefile` is starting to show its age. There have been over 2,000 commits to the `Makefile`, resulting in a build script that is nearly 4,000 lines long.

In this release, the Git project has support for a new build system, Meson, as an alternative to building with GNU Make. While support for Meson is not yet as robust as building with Make, Meson does offer a handful of advantages over Make. Meson is easier to use than Make, making the project more accessible to newcomers or contributors who don’t have significant experience working with Make. Meson also has extensive IDE support, and supports out-of-tree and cross-platform builds, which are both significant assets to the Git project.

Git will retain support for Make and CMake in addition to Meson for the foreseeable future, and retain support for Autoconf for a little longer. But if you’re curious to experiment with Git’s newest build system, you can run `meson setup build && ninja -C build` on a fresh copy of Git 2.48 or newer.

\[source\]

* * *

- As the Git project has grown over the years, it has accumulated a number of features and modes that, while reasonable when first introduced, have since become outdated or superseded and are now deprecated. In Git 2.48, the Git project began collecting these now-deprecated features in a list stored in `Documentation/BreakingChanges.txt`.
    
    This document enables the Git project to discuss deprecating certain features and collects the project’s anticipated deprecations in a single place. On the other side of the equation, it allows users to see if they might be affected by an upcoming deprecation, and share their use-case of a particular feature with the project. Check out the list to see if there is anything on there that you might miss, and to get an early picture of what an eventual Git 3.0 release might look like!
    
    \[source\]
    
- If you’ve ever scripted around your repository’s references, you are likely familiar with Git’s `for-each-ref` command. In case you’re not, `for-each-ref` is a flexible tool that allows you to list references in your repository, apply custom formatting specifiers to them, and much more.
    
    Back in Git 2.44, we talked about some performance improvements that allowed `git for-each-ref` to run significantly faster by combining reference filtering and formatting into the same codepath, eliminating the need to store and sort the results in certain conditions.
    
    Git 2.48 extends those changes by allowing us to take advantage of the same optimizations even when asked to output the references in sorted order (under certain conditions). As long as those conditions are met, you can quickly output a small number of references even under `--sort=refname` independent of how many references your repository actually has.
    
    \[source\]
    
- While we’re on the topic of references, the reftable subsystem has received some more attention in this release. Git’s reftable implementation was updated to avoid explicit dependencies on some of Git’s convenience APIs, making further progress on being able to compile the reftable code without `libgit.a`. The reftable implementation was also updated to gracefully handle memory allocation failures instead of exiting the process immediately. Last but not least, the reftable code was updated to be able to reuse reference iterators, resulting in faster reference creation and lower memory usage when using reftables.
    
    For more about reftables, check out our previous coverage of reftables.
    
    \[source, source, source, source\]
    
- When you clone from a remote repository, the default branch that the remote repository uses is reflected in `refs/remotes/origin/HEAD` locally2. In prior versions of Git, subsequent fetches and pulls did not update this symbolic reference. With Git 2.48, if the remote has a default branch but `refs/remotes/origin/HEAD` is missing locally, then a fetch will update it.
    
    If you want to take it a step further, you can set `remote.origin.followRemoteHead` configuration to `warn` or `always`; if you do so, when `refs/remotes/origin/HEAD` already exists but does not match the default branch on the remote side, then when you run `git fetch` it will either warn you about the change or just automatically update `refs/remote/origin/HEAD` to the appropriate value depending on what setting you used.
    
    \[source, source\]
    
- Partial clones also received some love this cycle, fixing an infinite loop and avoiding promisor to non-promisor references that could break the repository after a `git gc`.
    
    For those unfamiliar with partial clones or want to learn more about their internals, you can read the guide “Get up to speed with partial clone and shallow clone.”
    
    \[source, source, source, source\]
    

* * *

## The rest of the iceberg

That’s just a sample of changes from the latest release. For more, check out the release notes for 2.48, or any previous version in the Git repository.

#### Notes

* * *

1. A better time would have been in the Highlights from Git 2.47 blog post, but who’s counting? ↩
2. By default, anyway. You can specify a name other than “origin” with the `-o` option ↩

The post Highlights from Git 2.48 appeared first on The GitHub Blog.

Go to Source
