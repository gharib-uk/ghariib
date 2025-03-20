---
title: "<div>What's new in Git 2.49.0?</div>"
date: 2025-03-19
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

The Git project recently released Git 2.49.0. Let's look at a few notable highlights from this release, which includes contributions from GitLab's Git team and the wider Git community.

What's covered:

- git-backfill(1) and the new path-walk API
- Introduction of zlib-ng
- Continued iteration on Meson
- Deprecation of .git/branches/ and .git/remotes/
- Rust bindings for libgit
- New name-hashing algorithm
- Promisor remote capability
- Thin clone using `--revision`

## git-backfill(1) and the new path-walk API

When you `git-clone(1)` a Git repository, you can pass it the `--filter` option. Using this option allows you to create a _partial clone_. In a partial clone the server only sends a subset of reachable objects according to the given object filter. For example, creating a clone with `--filter=blob:none` will not fetch any blobs (file contents) from the server and create a _blobless clone_.

Blobless clones have all the reachable commits and trees, but no blobs. When you perform an operation like `git-checkout(1)`, Git will download the missing blobs to complete that operation. For some operations, like `git-blame(1)`, this might result in downloading objects one by one, which will slow down the command drastically. This performance degradation occurs because `git-blame(1)` must traverse the commit history to identify which specific blobs it needs, then request each missing blob from the server separately.

In Git 2.49, a new subcommand `git-backfill(1)` is introduced, which can be used to download missing blobs in a blobless partial clone.

Under the hood, the `git-backfill(1)` command leverages the new path-walk API, which is different from how Git generally iterates over commits. Rather than iterating over the commits one at a time and recursively visiting the trees and blobs associated with each commit, the path-walk API does traversal by path. For each path, it adds a list of associated tree objects to a stack. This stack is then processed in a depth-first order. So, instead of processing every object in commit `1` before moving to commit `2`, it will process all versions of file `A` across all commits before moving to file `B`. This approach greatly improves performance in scenarios where grouping by path is essential.

Let me demonstrate its use by making a blobless clone of `gitlab-org/git`:

```shell
$ git clone --filter=blob:none --bare --no-tags git@gitlab.com:gitlab-org/git.git
Cloning into bare repository 'git.git'...
remote: Enumerating objects: 245904, done.
remote: Counting objects: 100% (1736/1736), done.
remote: Compressing objects: 100% (276/276), done.
remote: Total 245904 (delta 1591), reused 1547 (delta 1459), pack-reused 244168 (from 1)
Receiving objects: 100% (245904/245904), 59.35 MiB | 15.96 MiB/s, done.
Resolving deltas: 100% (161482/161482), done.
```

Above, we use `--bare` to ensure Git doesn't need to download any blobs to check out an initial branch. We can verify this clone does not contain any blobs:

```sh
$ git cat-file --batch-all-objects --batch-check='%(objecttype)' | sort | uniq -c
  83977 commit
 161927 tree
```

If you want to see the contents of a file in the repository, Git has to download it:

```sh
$ git cat-file -p HEAD:README.md
remote: Enumerating objects: 1, done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 1 (from 1)
Receiving objects: 100% (1/1), 1.64 KiB | 1.64 MiB/s, done.

[![Build status](https://github.com/git/git/workflows/CI/badge.svg)](https://github.com/git/git/actions?query=branch%3Amaster+event%3Apush)

Git - fast, scalable, distributed revision control system
=========================================================

Git is a fast, scalable, distributed revision control system with an
unusually rich command set that provides both high-level operations
and full access to internals.

[snip]
```

As you can see above, Git first talks to the remote repository to download the blob before it can display it.

When you would like to `git-blame(1)` that file, it needs to download a lot more:

```sh
$ git blame HEAD README.md
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (1/1), 1.64 KiB | 1.64 MiB/s, done.
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (1/1), 1.64 KiB | 1.64 MiB/s, done.
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (1/1), 1.64 KiB | 1.64 MiB/s, done.
remote: Enumerating objects: 1, done.

[snip]

df7375d772 README.md (Ævar Arnfjörð Bjarmason 2021-11-23 17:29:09 +0100  1) [![Build status](https://github.com/git/git/workflows/CI/badge.svg)](https://github.com/git/git/actions?query=branch%3Amaster+event%3Apush)
5f7864663b README.md (Johannes Schindelin 	2019-01-29 06:19:32 -0800  2)
28513c4f56 README.md (Matthieu Moy        	2016-02-25 09:37:29 +0100  3) Git - fast, scalable, distributed revision control system
28513c4f56 README.md (Matthieu Moy        	2016-02-25 09:37:29 +0100  4) =========================================================
556b6600b2 README	(Nicolas Pitre       	2007-01-17 13:04:39 -0500  5)
556b6600b2 README	(Nicolas Pitre       	2007-01-17 13:04:39 -0500  6) Git is a fast, scalable, distributed revision control system with an
556b6600b2 README	(Nicolas Pitre       	2007-01-17 13:04:39 -0500  7) unusually rich command set that provides both high-level operations
556b6600b2 README	(Nicolas Pitre       	2007-01-17 13:04:39 -0500  8) and full access to internals.
556b6600b2 README	(Nicolas Pitre       	2007-01-17 13:04:39 -0500  9)

[snip]
```

We've truncated the output, but as you can see, Git goes to the server for each revision of that file separately. That's really inefficient. With `git-backfill(1)` we can ask Git to download all blobs:

```shell
$ git backfill
remote: Enumerating objects: 50711, done.
remote: Counting objects: 100% (15438/15438), done.
remote: Compressing objects: 100% (708/708), done.
remote: Total 50711 (delta 15154), reused 14730 (delta 14730), pack-reused 35273 (from 1)
Receiving objects: 100% (50711/50711), 11.62 MiB | 12.28 MiB/s, done.
Resolving deltas: 100% (49154/49154), done.
remote: Enumerating objects: 50017, done.
remote: Counting objects: 100% (10826/10826), done.
remote: Compressing objects: 100% (634/634), done.
remote: Total 50017 (delta 10580), reused 10192 (delta 10192), pack-reused 39191 (from 1)
Receiving objects: 100% (50017/50017), 12.17 MiB | 12.33 MiB/s, done.
Resolving deltas: 100% (48301/48301), done.
remote: Enumerating objects: 47303, done.
remote: Counting objects: 100% (7311/7311), done.
remote: Compressing objects: 100% (618/618), done.
remote: Total 47303 (delta 7021), reused 6693 (delta 6693), pack-reused 39992 (from 1)
Receiving objects: 100% (47303/47303), 40.84 MiB | 15.26 MiB/s, done.
Resolving deltas: 100% (43788/43788), done.
```

This backfills all blobs, turning the blobless clone into a full clone:

```shell
$ git cat-file --batch-all-objects --batch-check='%(objecttype)' | sort | uniq -c
 148031 blob
  83977 commit
 161927 tree
```

This project was led by Derrick Stolee and was merged with e565f37553.

## Introduction of zlib-ng

All objects in the `.git/` folder are compressed by Git using `zlib`. `zlib` is the reference implementation for the RFC 1950: ZLIB Compressed Data Format. Created in 1995, `zlib` has a long history and is incredibly portable, even supporting many systems that predate the Internet. Because of its wide support of architectures and compilers, it has limitations in what it is capable of.

The fork `zlib-ng` was created to accommodate the limitations. `zlib-ng` aims to be optimized for modern systems. This fork drops support for legacy systems and instead brings in patches for Intel optimizations, some Cloudflare optimizations, and a couple other smaller patches.

The `zlib-ng` library itself provides a compatibility layer for `zlib`. The compatibility later allows `zlib-ng` to be a drop-in replacement for `zlib`, but that layer is not available on all Linux distributions. In Git 2.49:

- A compatibility layer was added to the Git project.
- Build options were added to both to the `Makefile` and Meson Build file.

These additions make it easier to benefit from the performance improvements of `zlib-ng`.

In local benchmarks, we've seen a ~25% speedup when using `zlib-ng` instead of `zlib`. And we're in the process of rolling out these changes to GitLab.com, too.

If you want to benefit from the gains of `zlib-ng`, first verify if Git on your machine is already using `zlib-ng` by running `git version --build-options`:

```shell
$ git version --build-options
git version 2.47.1
cpu: x86_64
no commit associated with this build
sizeof-long: 8
sizeof-size_t: 8
shell-path: /bin/sh
libcurl: 8.6.0
OpenSSL: OpenSSL 3.2.2 4 Jun 2024
zlib: 1.3.1.zlib-ng
```

If the last line includes `zlib-ng` then your Git is already built using the faster `zlib` variant. If not, you can either:

- Ask the maintainer of the Git package you are using to include `zlib-ng` support.
- Build Git yourself from source.

These changes were introduced by Patrick Steinhardt.

## Continued iteration on Meson

In our article about the Git 2.48 release, we touched on the introduction of the Meson build system. Meson is a build automation tool used by the Git project that at some point might replace Autoconf, CMake, and maybe even Make.

During this release cycle, work continued on using Meson, adding various missing features and stabilization fixes:

- Improved test coverage for CI was merged in 72f1ddfbc9.
- Bits and pieces to use Meson in `contrib/` were merged in 2a1530a953.
- Assorted fixes and improvements to the build procedure based on meson were merged in ab09eddf60.
- Making Meson aware of building `git-subtree(1)` was merged in 3ddeb7f337.
- Learn Meson to generate HTML documentation pages was merged in 1b4e9a5f8b.

All these efforts were carried out by Patrick Steinhardt.

## Deprecation of .git/branches/ and .git/remotes/

You are probably aware of the existence of the `.git` directory, and what is inside. But have you ever heard about the sub-directories `.git/branches/` and `.git/remotes/`? As you might know, reference to branches are stored in `.git/refs/heads/`, so that's not what `.git/branches/` is for, and what about `.git/remotes/`?

Way back in 2005, `.git/branches/` was introduced to store a shorthand name for a remote, and a few months later they were moved to `.git/remotes/`. In 2006, `git-config(1)` learned to store remotes. This has become the standard way to configure remotes and, in 2011, the directories `.git/branches/` and `.git/remotes/` were documented as being "legacy" and no longer used in modern repositories.

In 2024, the document BreakingChanges was started to outline breaking changes for the next major version of Git (v3.0). While this release is not planned to happen any time soon, this document keeps track of changes that are expected to be part of that release. In 8ccc75c245, the use of the directories `.git/branches/` and `.git/remotes/` was added to this document and that officially marks as them deprecated and to be removed in Git 3.0.

Thanks to Patrick Steinhardt for formalizing this deprecation.

## Rust bindings for libgit

When compiling Git, an internal library `libgit.a` is made. This library contains some of the core functionality of Git.

While this library (and most of Git) is written in C, in Git 2.49 bindings were added to make some of these functions available in Rust. To achieve this, two new Cargo packages were created: `libgit-sys` and `libgit-rs`. These packages live in the `contrib/` subdirectory in the Git source tree.

It's pretty common to split out a library into two packages when a Foreign Function Interface is used. The `libgit-sys` package provides the pure interface to C functions and links to the native `libgit.a` library. The package `libgit-rs` provides a high-level interface to the functions in `libgit-sys` with a feel that is more idiomatic to Rust.

So far, the functionality in these Rust packages is very limited. It only provides an interface to interact with the `git-config(1)`.

This initiative was led by Josh Steadmon and was merged with a4af0b6288.

## New name-hashing algorithm

The Git object database in `.git/` stores most of its data in packfiles. And packfiles are also used to submit objects between Git server and client over the wire.

You can read all about the format at `gitformat-pack(5)`. One important aspect of the packfiles is delta-compression. With delta-compression not every object is stored as-is, but some objects are saved as a _delta_ of another _base_. So instead of saving the full contents of the objects, changes compared to another object are stored.

Without going into the details how these deltas are calculated or stored, you can imagine that it is important group files together that are very similar. In v2.48 and earlier, Git looked at the last 16 characters of the path name to determine whether blobs might be similar. This algorithm is named version `1`.

In Git 2.49, version `2` is available. This is an iteration on version `1`, but modified so the effect of the parent directory is reduced. You can specify the name-hash algorithm version you want to use with option `--name-hash-version` of `git-repack(1)`.

Derrick Stolee, who drove this project, did some comparison in resulting packfile size after running `git repack -adf --name-hash-version=<n>`:

| Repo | Version 1 size | Version 2 size |
| --- | --- | --- |
| fluentui | 440 MB | 161 MB |
| Repo B | 6,248 MB | 856 MB |
| Repo C | 37,278 MB | 6,921 MB |
| Repo D | 131,204 MB | 7,463 MB |

You can read more of the details in the patch set, which is merged in aae91a86fb.

## Promisor remote capability

It's known that Git isn't great in dealing with large files. There are some solutions to this problem, like Git LFS, but there are still some shortcomings. To give a few:

- With Git LFS the user has to configure which files to put in LFS. The server has no control about that and has to serve all files.
- Whenever a file is committed to the repository, there is no way to get it out again without rewriting history. This is annoying, especially for large files, because they are stuck for eternity.
- Users cannot change their mind on which files to put into Git LFS.
- A tool like Git LFS requires significant effort to set up, learn, and use correctly.

For some time, Git has had the concept of promisor remotes. This feature can be used to deal with large files, and in Git 2.49 this feature took a step forward.

The idea for the new “promisor-remote” capability is relatively simple: Instead of sending all objects itself, a Git server can tell to the Git client "Hey, go download these objects from _XYZ_". _XYZ_ would be a promisor remote.

Git 2.49 enables the server to advertise the information of the promisor remote to the client. This change is an extension to `gitprotocol-v2`. While the server and the client are transmitting data to each other, the server can send names and URLs of the promisor remotes it knows about.

So far, the client is not using the promisor remote info it gets from the server during clone, so all objects are still transmitted from the remote the clone initiated from. We are planning to continue work on this feature, making it use promisor remote info from the server, and making it easier to use.

This patch set was submitted by Christian Couder and merged with 2c6fd30198.

## Thin clone using `--revision`

A new `--revision` option was added to `git-clone(1)`. This enables you to create a thin clone of a repository that only contains the history of the given revision. The option is similar to `--branch`, but accepts a ref name (like `refs/heads/main`, `refs/tags/v1.0`, and `refs/merge-requests/123`) or a hexadecimal commit object ID. The difference to `--branch` is that it does not create a tracking branch and detaches `HEAD`. This means it's not suited if you want to contribute back to that branch.

You can use `--revision` in combination with `--depth` to create a very minimal clone. A suggested use-case is for automated testing. When you have a CI system that needs to check out a branch (or any reference) to perform autonomous testing on the source code, having a minimal clone is all you need.

This change was driven by Toon Claes.

# Read more

- What’s new in Git 2.48.0?
- What’s new in Git 2.47.0?
- What’s new in Git 2.46.0?

The Git project recently released Git 2.49.0. Let's look at a few notable highlights from this release, which includes contributions from GitLab's Git team and the wider Git community.

What's covered:

- git-backfill(1) and the new path-walk API
- Introduction of zlib-ng
- Continued iteration on Meson
- Deprecation of .git/branches/ and .git/remotes/
- Rust bindings for libgit
- New name-hashing algorithm
- Promisor remote capability
- Thin clone using `--revision`

## git-backfill(1) and the new path-walk API

When you `git-clone(1)` a Git repository, you can pass it the `--filter` option. Using this option allows you to create a _partial clone_. In a partial clone the server only sends a subset of reachable objects according to the given object filter. For example, creating a clone with `--filter=blob:none` will not fetch any blobs (file contents) from the server and create a _blobless clone_.

Blobless clones have all the reachable commits and trees, but no blobs. When you perform an operation like `git-checkout(1)`, Git will download the missing blobs to complete that operation. For some operations, like `git-blame(1)`, this might result in downloading objects one by one, which will slow down the command drastically. This performance degradation occurs because `git-blame(1)` must traverse the commit history to identify which specific blobs it needs, then request each missing blob from the server separately.

In Git 2.49, a new subcommand `git-backfill(1)` is introduced, which can be used to download missing blobs in a blobless partial clone.

Under the hood, the `git-backfill(1)` command leverages the new path-walk API, which is different from how Git generally iterates over commits. Rather than iterating over the commits one at a time and recursively visiting the trees and blobs associated with each commit, the path-walk API does traversal by path. For each path, it adds a list of associated tree objects to a stack. This stack is then processed in a depth-first order. So, instead of processing every object in commit `1` before moving to commit `2`, it will process all versions of file `A` across all commits before moving to file `B`. This approach greatly improves performance in scenarios where grouping by path is essential.

Let me demonstrate its use by making a blobless clone of `gitlab-org/git`:

```shell
$ git clone --filter=blob:none --bare --no-tags git@gitlab.com:gitlab-org/git.git
Cloning into bare repository 'git.git'...
remote: Enumerating objects: 245904, done.
remote: Counting objects: 100% (1736/1736), done.
remote: Compressing objects: 100% (276/276), done.
remote: Total 245904 (delta 1591), reused 1547 (delta 1459), pack-reused 244168 (from 1)
Receiving objects: 100% (245904/245904), 59.35 MiB | 15.96 MiB/s, done.
Resolving deltas: 100% (161482/161482), done.
```

Above, we use `--bare` to ensure Git doesn't need to download any blobs to check out an initial branch. We can verify this clone does not contain any blobs:

```sh
$ git cat-file --batch-all-objects --batch-check='%(objecttype)' | sort | uniq -c
  83977 commit
 161927 tree
```

If you want to see the contents of a file in the repository, Git has to download it:

```sh
$ git cat-file -p HEAD:README.md
remote: Enumerating objects: 1, done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 1 (from 1)
Receiving objects: 100% (1/1), 1.64 KiB | 1.64 MiB/s, done.

[![Build status](https://github.com/git/git/workflows/CI/badge.svg)](https://github.com/git/git/actions?query=branch%3Amaster+event%3Apush)

Git - fast, scalable, distributed revision control system
=========================================================

Git is a fast, scalable, distributed revision control system with an
unusually rich command set that provides both high-level operations
and full access to internals.

[snip]
```

As you can see above, Git first talks to the remote repository to download the blob before it can display it.

When you would like to `git-blame(1)` that file, it needs to download a lot more:

```sh
$ git blame HEAD README.md
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (1/1), 1.64 KiB | 1.64 MiB/s, done.
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (1/1), 1.64 KiB | 1.64 MiB/s, done.
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (1/1), 1.64 KiB | 1.64 MiB/s, done.
remote: Enumerating objects: 1, done.

[snip]

df7375d772 README.md (Ævar Arnfjörð Bjarmason 2021-11-23 17:29:09 +0100  1) [![Build status](https://github.com/git/git/workflows/CI/badge.svg)](https://github.com/git/git/actions?query=branch%3Amaster+event%3Apush)
5f7864663b README.md (Johannes Schindelin 	2019-01-29 06:19:32 -0800  2)
28513c4f56 README.md (Matthieu Moy        	2016-02-25 09:37:29 +0100  3) Git - fast, scalable, distributed revision control system
28513c4f56 README.md (Matthieu Moy        	2016-02-25 09:37:29 +0100  4) =========================================================
556b6600b2 README	(Nicolas Pitre       	2007-01-17 13:04:39 -0500  5)
556b6600b2 README	(Nicolas Pitre       	2007-01-17 13:04:39 -0500  6) Git is a fast, scalable, distributed revision control system with an
556b6600b2 README	(Nicolas Pitre       	2007-01-17 13:04:39 -0500  7) unusually rich command set that provides both high-level operations
556b6600b2 README	(Nicolas Pitre       	2007-01-17 13:04:39 -0500  8) and full access to internals.
556b6600b2 README	(Nicolas Pitre       	2007-01-17 13:04:39 -0500  9)

[snip]
```

We've truncated the output, but as you can see, Git goes to the server for each revision of that file separately. That's really inefficient. With `git-backfill(1)` we can ask Git to download all blobs:

```shell
$ git backfill
remote: Enumerating objects: 50711, done.
remote: Counting objects: 100% (15438/15438), done.
remote: Compressing objects: 100% (708/708), done.
remote: Total 50711 (delta 15154), reused 14730 (delta 14730), pack-reused 35273 (from 1)
Receiving objects: 100% (50711/50711), 11.62 MiB | 12.28 MiB/s, done.
Resolving deltas: 100% (49154/49154), done.
remote: Enumerating objects: 50017, done.
remote: Counting objects: 100% (10826/10826), done.
remote: Compressing objects: 100% (634/634), done.
remote: Total 50017 (delta 10580), reused 10192 (delta 10192), pack-reused 39191 (from 1)
Receiving objects: 100% (50017/50017), 12.17 MiB | 12.33 MiB/s, done.
Resolving deltas: 100% (48301/48301), done.
remote: Enumerating objects: 47303, done.
remote: Counting objects: 100% (7311/7311), done.
remote: Compressing objects: 100% (618/618), done.
remote: Total 47303 (delta 7021), reused 6693 (delta 6693), pack-reused 39992 (from 1)
Receiving objects: 100% (47303/47303), 40.84 MiB | 15.26 MiB/s, done.
Resolving deltas: 100% (43788/43788), done.
```

This backfills all blobs, turning the blobless clone into a full clone:

```shell
$ git cat-file --batch-all-objects --batch-check='%(objecttype)' | sort | uniq -c
 148031 blob
  83977 commit
 161927 tree
```

This project was led by Derrick Stolee and was merged with e565f37553.

## Introduction of zlib-ng

All objects in the `.git/` folder are compressed by Git using `zlib`. `zlib` is the reference implementation for the RFC 1950: ZLIB Compressed Data Format. Created in 1995, `zlib` has a long history and is incredibly portable, even supporting many systems that predate the Internet. Because of its wide support of architectures and compilers, it has limitations in what it is capable of.

The fork `zlib-ng` was created to accommodate the limitations. `zlib-ng` aims to be optimized for modern systems. This fork drops support for legacy systems and instead brings in patches for Intel optimizations, some Cloudflare optimizations, and a couple other smaller patches.

The `zlib-ng` library itself provides a compatibility layer for `zlib`. The compatibility later allows `zlib-ng` to be a drop-in replacement for `zlib`, but that layer is not available on all Linux distributions. In Git 2.49:

- A compatibility layer was added to the Git project.
- Build options were added to both to the `Makefile` and Meson Build file.

These additions make it easier to benefit from the performance improvements of `zlib-ng`.

In local benchmarks, we've seen a ~25% speedup when using `zlib-ng` instead of `zlib`. And we're in the process of rolling out these changes to GitLab.com, too.

If you want to benefit from the gains of `zlib-ng`, first verify if Git on your machine is already using `zlib-ng` by running `git version --build-options`:

```shell
$ git version --build-options
git version 2.47.1
cpu: x86_64
no commit associated with this build
sizeof-long: 8
sizeof-size_t: 8
shell-path: /bin/sh
libcurl: 8.6.0
OpenSSL: OpenSSL 3.2.2 4 Jun 2024
zlib: 1.3.1.zlib-ng
```

If the last line includes `zlib-ng` then your Git is already built using the faster `zlib` variant. If not, you can either:

- Ask the maintainer of the Git package you are using to include `zlib-ng` support.
- Build Git yourself from source.

These changes were introduced by Patrick Steinhardt.

## Continued iteration on Meson

In our article about the Git 2.48 release, we touched on the introduction of the Meson build system. Meson is a build automation tool used by the Git project that at some point might replace Autoconf, CMake, and maybe even Make.

During this release cycle, work continued on using Meson, adding various missing features and stabilization fixes:

- Improved test coverage for CI was merged in 72f1ddfbc9.
- Bits and pieces to use Meson in `contrib/` were merged in 2a1530a953.
- Assorted fixes and improvements to the build procedure based on meson were merged in ab09eddf60.
- Making Meson aware of building `git-subtree(1)` was merged in 3ddeb7f337.
- Learn Meson to generate HTML documentation pages was merged in 1b4e9a5f8b.

All these efforts were carried out by Patrick Steinhardt.

## Deprecation of .git/branches/ and .git/remotes/

You are probably aware of the existence of the `.git` directory, and what is inside. But have you ever heard about the sub-directories `.git/branches/` and `.git/remotes/`? As you might know, reference to branches are stored in `.git/refs/heads/`, so that's not what `.git/branches/` is for, and what about `.git/remotes/`?

Way back in 2005, `.git/branches/` was introduced to store a shorthand name for a remote, and a few months later they were moved to `.git/remotes/`. In 2006, `git-config(1)` learned to store remotes. This has become the standard way to configure remotes and, in 2011, the directories `.git/branches/` and `.git/remotes/` were documented as being "legacy" and no longer used in modern repositories.

In 2024, the document BreakingChanges was started to outline breaking changes for the next major version of Git (v3.0). While this release is not planned to happen any time soon, this document keeps track of changes that are expected to be part of that release. In 8ccc75c245, the use of the directories `.git/branches/` and `.git/remotes/` was added to this document and that officially marks as them deprecated and to be removed in Git 3.0.

Thanks to Patrick Steinhardt for formalizing this deprecation.

## Rust bindings for libgit

When compiling Git, an internal library `libgit.a` is made. This library contains some of the core functionality of Git.

While this library (and most of Git) is written in C, in Git 2.49 bindings were added to make some of these functions available in Rust. To achieve this, two new Cargo packages were created: `libgit-sys` and `libgit-rs`. These packages live in the `contrib/` subdirectory in the Git source tree.

It's pretty common to split out a library into two packages when a Foreign Function Interface is used. The `libgit-sys` package provides the pure interface to C functions and links to the native `libgit.a` library. The package `libgit-rs` provides a high-level interface to the functions in `libgit-sys` with a feel that is more idiomatic to Rust.

So far, the functionality in these Rust packages is very limited. It only provides an interface to interact with the `git-config(1)`.

This initiative was led by Josh Steadmon and was merged with a4af0b6288.

## New name-hashing algorithm

The Git object database in `.git/` stores most of its data in packfiles. And packfiles are also used to submit objects between Git server and client over the wire.

You can read all about the format at `gitformat-pack(5)`. One important aspect of the packfiles is delta-compression. With delta-compression not every object is stored as-is, but some objects are saved as a _delta_ of another _base_. So instead of saving the full contents of the objects, changes compared to another object are stored.

Without going into the details how these deltas are calculated or stored, you can imagine that it is important group files together that are very similar. In v2.48 and earlier, Git looked at the last 16 characters of the path name to determine whether blobs might be similar. This algorithm is named version `1`.

In Git 2.49, version `2` is available. This is an iteration on version `1`, but modified so the effect of the parent directory is reduced. You can specify the name-hash algorithm version you want to use with option `--name-hash-version` of `git-repack(1)`.

Derrick Stolee, who drove this project, did some comparison in resulting packfile size after running `git repack -adf --name-hash-version=<n>`:

| Repo | Version 1 size | Version 2 size |
| --- | --- | --- |
| fluentui | 440 MB | 161 MB |
| Repo B | 6,248 MB | 856 MB |
| Repo C | 37,278 MB | 6,921 MB |
| Repo D | 131,204 MB | 7,463 MB |

You can read more of the details in the patch set, which is merged in aae91a86fb.

## Promisor remote capability

It's known that Git isn't great in dealing with large files. There are some solutions to this problem, like Git LFS, but there are still some shortcomings. To give a few:

- With Git LFS the user has to configure which files to put in LFS. The server has no control about that and has to serve all files.
- Whenever a file is committed to the repository, there is no way to get it out again without rewriting history. This is annoying, especially for large files, because they are stuck for eternity.
- Users cannot change their mind on which files to put into Git LFS.
- A tool like Git LFS requires significant effort to set up, learn, and use correctly.

For some time, Git has had the concept of promisor remotes. This feature can be used to deal with large files, and in Git 2.49 this feature took a step forward.

The idea for the new “promisor-remote” capability is relatively simple: Instead of sending all objects itself, a Git server can tell to the Git client "Hey, go download these objects from _XYZ_". _XYZ_ would be a promisor remote.

Git 2.49 enables the server to advertise the information of the promisor remote to the client. This change is an extension to `gitprotocol-v2`. While the server and the client are transmitting data to each other, the server can send names and URLs of the promisor remotes it knows about.

So far, the client is not using the promisor remote info it gets from the server during clone, so all objects are still transmitted from the remote the clone initiated from. We are planning to continue work on this feature, making it use promisor remote info from the server, and making it easier to use.

This patch set was submitted by Christian Couder and merged with 2c6fd30198.

## Thin clone using `--revision`

A new `--revision` option was added to `git-clone(1)`. This enables you to create a thin clone of a repository that only contains the history of the given revision. The option is similar to `--branch`, but accepts a ref name (like `refs/heads/main`, `refs/tags/v1.0`, and `refs/merge-requests/123`) or a hexadecimal commit object ID. The difference to `--branch` is that it does not create a tracking branch and detaches `HEAD`. This means it's not suited if you want to contribute back to that branch.

You can use `--revision` in combination with `--depth` to create a very minimal clone. A suggested use-case is for automated testing. When you have a CI system that needs to check out a branch (or any reference) to perform autonomous testing on the source code, having a minimal clone is all you need.

This change was driven by Toon Claes.

# Read more

- What’s new in Git 2.48.0?
- What’s new in Git 2.47.0?
- What’s new in Git 2.46.0?

Go to Source
