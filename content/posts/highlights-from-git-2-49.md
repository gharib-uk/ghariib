---
title: "Highlights from Git 2.49"
date: 2025-03-19
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

The open source Git project just released Git 2.49. Here is GitHub’s look at some of the most interesting features and changes introduced since last time.

The post Highlights from Git 2.49 appeared first on The GitHub Blog.

The open source Git project just released Git 2.49 with features and bug fixes from over 89 contributors, 24 of them new. We last caught up with you on the latest in Git back when 2.48 was released.

To celebrate this most recent release, here is GitHub’s look at some of the most interesting features and changes introduced since last time.

## Faster packing with name-hash v2

Many times over this series of blog posts, we have talked about Git’s object storage model, where objects can be written individually (known as “loose” objects), or grouped together in packfiles. Git uses packfiles in a wide variety of functions, including local storage (when you repack or GC your repository), as well as when sending data to or from another Git repository (like fetching, cloning, or pushing).

Storing objects together in packfiles has a couple of benefits over storing them individually as loose. One obvious benefit is that object lookups can be performed much more quickly in pack storage. When looking up a loose object, Git has to make multiple system calls to find the object you’re looking for, open it, read it, and close it. These system calls can be made faster using the operating system’s block cache, but because objects are looked up by a SHA-1 (or SHA-256) of their contents, this pseudo-random access isn’t very cache-efficient.

But most interesting to our discussion is that since loose objects are stored individually, we can only compress their contents in isolation, and can’t store objects as deltas of other similar objects that already exist in your repository. For example, say you’re making a series of small changes to a large blob in your repository. When those objects are initially written, they are each stored individually and zlib compressed. But if the majority of the file’s content remains unchanged among edit pairs, Git can further compress these objects by storing successive versions as deltas of earlier ones. Roughly speaking, this allows Git to store the changes made to an object (relative to some other object) instead of multiple copies of nearly identical blobs.

But how does Git figure out which pairs of objects are good candidates to store as delta-base pairs? One useful proxy is to compare objects that appear at similar paths. Git does this today by computing what it calls a “name hash”, which is effectively a sortable numeric hash that weights more heavily towards the final 16 non-whitespace characters in a filepath (source). This function comes from Linus all the way back in 2006, and excels at grouping functions with similar extensions (all ending in `.c`, `.h`, etc.), or files that were moved from one directory to another (`a/foo.txt` to `b/foo.txt`).

But the existing name-hash implementation can lead to poor compression when there are many files that have the same basename but very different contents, like having many `CHANGELOG.md` files for different subsystems stored together in your repository. Git 2.49 introduces a new variant of the hash function that takes more of the directory structure into account when computing its hash. Among other changes, each layer of the directory hierarchy gets its own hash, which is downshifted and then XORed into the overall hash. This creates a hash function which is more sensitive to the whole path, not just the final 16 characters.

This can lead to significant improvements both in packing performance, but also in the resulting pack’s overall size. For instance, using the new hash function was able to improve the time it took to repack `microsoft/fluentui` from ~96 seconds to ~34 seconds, and slimming down the resulting pack’s size from 439 MiB to just 160 MiB (source).

While this feature isn’t (yet) compatible with Git’s reachability bitmaps feature, you can try it out for yourself using either `git repack`’s or `git pack-objects`’s new `--name-hash-version` flag via the latest release.

\[source\]

## Backfill historical blobs in partial clones

Have you ever been working in a partial clone and gotten this unfriendly output?

```
$ git blame README.md
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (1/1), 1.64 KiB | 8.10 MiB/s, done.
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (1/1), 1.64 KiB | 7.30 MiB/s, done.
[...]
```

What happened here? To understand the answer to that question, let’s work through an example scenario:

Suppose that you are working in a partial clone that you cloned with `--filter=blob:none`. In this case, your repository is going to have all of its trees, commit, and annotated tag objects, but only the set of blobs which are immediately reachable from `HEAD`. Put otherwise, your local clone only has the set of blobs it needs to populate a full checkout at the latest revision, and loading any historical blobs will fault in any missing objects from wherever you cloned your repository.

In the above example, we asked for a `blame` of the file at path `README.md`. In order to construct that blame, however, we need to see every historical version of the file in order to compute the diff at each layer to figure out whether or not a revision modified a given line. But here we see Git loading in each historical version of the object one by one, leading to bloated storage and poor performance.

Git 2.49 introduces a new tool, `git backfill`, which can fault in any missing historical blobs from a `--filter=blob:none` clone in a small number of batches. These requests use the new path-walk API (also introduced in Git 2.49) to group together objects that appear at the same path, resulting in much better delta compression in the packfile(s) sent back from the server. Since these requests are sent in batches instead of one-by-one, we can easily backfill all missing blobs in only a few packs instead of one pack per blob.

After running `git backfill` in the above example, our experience looks more like:

```plaintext
$ git clone --sparse --filter=blob:none git@github.com:git/git.git[...] # downloads historical commits/trees/tags
$ cd git
$ git sparse-checkout add builtin
[...] # downloads current contents of builtin/
$ git backfill --sparse
[...] # backfills historical contents of builtin/
$ git blame -- builtin/backfill.c
85127bcdeab (Derrick Stolee 2025-02-03 17:11:07 +0000   1) /* We need this macro to access core_apply_sparse_checkout */
85127bcdeab (Derrick Stolee 2025-02-03 17:11:07 +0000   2) #define USE_THE_REPOSITORY_VARIABLE
85127bcdeab (Derrick Stolee 2025-02-03 17:11:07 +0000   3)
[...]
```

But running `git backfill` immediately after cloning a repository with `--filter=blob:none` doesn’t bring much benefit, since it would have been more convenient to simply clone the repository without an object filter enabled in the first place. When using the backfill command’s `--sparse` option (the default whenever the sparse checkout feature is enabled in your repository), Git will only download blobs that appear within your sparse checkout, avoiding objects that you wouldn’t checkout anyway.

To try it out, run `git backfill` in any `--filter=blob:none` clone of a repository using Git 2.49 today!

\[source, source\]

* * *

- We discussed above that Git uses compression powered by zlib when writing loose objects, or individual objects within packs and so forth. zlib is an incredibly popular compression library, and has an emphasis on portability. Over the years, there have been a couple of popular forks (like intel/zlib and cloudflare/zlib) that contain optimizations not present in upstream zlib.
    
    The zlib-ng fork merges many of the optimizations made above, as well as removes dead code and workarounds for historical compilers from upstream zlib, placing a further emphasis on performance. For instance, zlib-ng has support for SIMD instruction sets (like SSE2, and AVX2) built-in to its core algorithms. Though zlib-ng is a drop-in replacement for zlib, the Git project needed to update its compatibility layer to accommodate zlib-ng.
    
    In Git 2.49, you can now build Git with zlib-ng by passing `ZLIB_NG` when building with the GNU Make, or the `zlib_backend` option when building with Meson. Early experimental results show a ~25% speed-up when printing the contents of all objects in the Git repository (from ~52.1 seconds down to ~40.3 seconds).
    
    \[source\]
    
- This release marks a major milestone in the Git project with the first pieces of Rust code being checked in. Specifically, this release introduces two Rust crates: libgit-sys, and libgit which are low- and high-level wrappers around a small portion of Git’s library code, respectively.
    
    The Git project has long been evolving its code to be more library-oriented, doing things like replacing functions that exit the program with ones that return an integer and let the caller decide to exit or, cleaning up memory leaks, etc. This release takes advantage of that work to provide a proof-of-concept Rust crate that wraps part of Git’s `config.h` API.
    
    This isn’t a fully-featured wrapper around Git’s entire library interface, and there is still much more work to be done throughout the project before that can become a reality, but this is a very exciting step along the way.
    
    \[source\]
    
- Speaking of the “libification” effort, there were a handful of other related changes that went into this release. The ongoing effort to move away from global variables like `the_repository` continues, and many more commands in this release use the provided `repository` instead of using the global one.
    
    This release also saw a lot of effort being put into squelching `-Wsign-compare` warnings, which occur when a signed value is compared against an unsigned one. This can lead to surprising behavior when comparing, say, negative signed values against unsigned ones, where a comparison like `-1 < 2` (which should return true) ends up returning false instead.
    
    Hopefully you won’t notice these changes in your day-to-day use of Git, but they are important steps along the way to bringing the project closer to being able to be used as a standalone library.
    
    \[source, source, source, source, source\]
    
- Long-time readers might remember our coverage of Git 2.39 where we discussed `git repack`’s new `--expire-to` option. In case you’re new around here or could use a refresher, we’ve got you covered. The `--expire-to` option in `git repack` controls the behavior of unreachable objects which were pruned out of the repository. By default, pruned objects are simply deleted, but `--expire-to` allows you to move them off to the side in case you want to hold onto them for backup purposes, etc.
    
    `git repack` is a fairly low-level command though, and most users will likely interact with Git’s garbage collection feature through `git gc`. In large part, `git gc` is a wrapper around functionality that is implemented in `git repack`, but up until this release, `git gc` didn’t expose its own command-line option to use `--expire-to`. That changed in Git 2.49, where you can now experiment with this behavior via `git gc --expire-to`!
    
    \[source\]
    
- You may have read that Git’s `help.autocorrect` feature is too fast for Formula One drivers. In case you haven’t, here are the details. If you’ve ever seen output like:
    
    ```plaintext
    $ git psuh
    git: 'psuh' is not a git command. See 'git --help'.
    
    The most similar command is
    push
    ```
    
    …then you have used Git’s autocorrect feature. But its configuration options don’t quite match the convention of other, similar options. For instance, in other parts of Git, specifying values like “true”, “yes”, “on”, or “1” for boolean-valued settings all meant the same thing. But `help.autocorrect` deviates from that trend slightly: it has special meanings for “never”, “immediate”, and “prompt”, but interprets a numeric value to mean that Git should automatically run whatever command it suggests after waiting that many deciseconds.
    
    So while you might have thought that setting `help.autocorrect` to “1” would enable the autocorrect behavior, you’d be wrong: it will instead run the corrected command before you can even blink your eyes1. Git 2.49 changes the convention of `help.autocorrect` to interpret “1” like other boolean-valued commands, and positive numbers greater than 1 as it would have before. While you can’t specify that you want the autocorrect behavior in exactly 1 decisecond anymore, you probably never meant to anyway.
    
    \[source, source\]
    
- You might be aware of `git clone`’s `--branch` option, which allows you to clone a repository’s history leading up to a specific branch or tag instead of the whole thing. This option is often used in CI farms when they want to clone a specific branch or tag for testing.
    
    But what if you want to clone a specific revision that isn’t at any branch or tag in your repository, what do you do? Prior to Git 2.49, the only thing you could do is initialize an empty repository and fetch a specific revision after adding the repository you’re fetching from as a remote.
    
    Git 2.49 introduces a convenient alternative to round out `--branch`‘s functionality with a new `--revision` option, which fetches history leading up to the specified revision, regardless of whether or not there is a branch or tag pointing at it.
    
    \[source\]
    
- Speaking of remotes, you might know that the `git remote` command uses your repository’s configuration to store the list of remotes that it knows about. You might not know that there were actually two different mechanisms which preceded storing remotes in configuration files. In the very early days, remotes were configured via separate files in `$GIT_DIR/branches` (source). A couple of weeks later, the convention changed to use `$GIT_DIR/remote` instead of the `/branches` directory (source).
    
    Both conventions have long since been deprecated and replaced with the configuration-based mechanism we’re familiar with today (source, source). But Git has maintained support for them over the years as part of its backwards compatibility. When Git 3.0 is eventually released, these features will be removed entirely.
    
    If you want to learn more about Git’s upcoming breaking changes, you can read all about them in `Documentation/BreakingChanges.adoc`. If you really want to live on the bleeding edge, you can build Git with the `WITH_BREAKING_CHANGES` compile time switch, which compiles out features that will be removed in Git 3.0.
    
    \[source, source\]
    
- Last but not least, the Git project had two wonderful Outreachy interns that recently completed their projects! Usman Akinyemi worked on adding support to include uname information in Git’s user agent when making HTTP requests, and Seyi Kuforiji worked on converting more unit tests to use the Clar testing framework.
    
    You can learn more about their projects here and here. Congratulations, Usman and Seyi!
    
    \[source, source, source, source\]
    

## The rest of the iceberg

That’s just a sample of changes from the latest release. For more, check out the release notes for 2.49, or any previous version in the Git repository.

* * *

1. It’s true. It takes humans about 100-150 milliseconds to blink their eyes, and setting `help.autocorrect` to “1” will run the suggested command after waiting only 100 milliseconds (1 decisecond). ↩

The post Highlights from Git 2.49 appeared first on The GitHub Blog.

Go to Source
