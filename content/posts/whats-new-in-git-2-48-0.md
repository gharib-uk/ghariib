---
title: "<div>What’s new in Git 2.48.0?</div>"
date: 2025-01-12
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

The Git project recently released Git 2.48.0. Let's look at a few notable highlights from this release, which includes contributions from GitLab's Git team and the wider Git community.

## Meson build system

For a long time, Git could be built using either a Makefile\-based build system or an Autoconf\-based build system. Git developers have been using mostly the Makefile-based build system, so the Autoconf-based build system has lagged behind in features and maintenance. Another issue was that a lot of Windows developers use integrated development environments (IDEs) that don’t have good support for Makefile- and Autoconf-based build systems.

In 2020, support for building Git using CMake was added. CMake added better Windows support and IDE integration, especially for Visual Studio. Some modern build system features like out-of-source builds were also included.

Recently, it appeared the CMake support was also lagging behind and that it might never be a good option to replace the two other build systems. So Patrick Steinhardt, GitLab Git Engineering Manager, implemented support for the Meson build system with the goal of eventually replacing the Autoconf-, CMake-, and maybe the Makefile-based build systems.

The new Meson-based build system has the following advantages:

- Allows users to easily find the available build options, something which is difficult with Makefiles and CMake
- Has a simple syntax compared to Autoconf and CMake
- Supports many different operating systems, compilers, and IDEs
- Supports modern build system features like out-of-source builds

Here is an example of how it can actually be used to build Git:

```shell
$ cd git             	# go into the root of Git's source code
$ meson setup build/ 	# setup "build" as a build directory
$ cd build           	# go into the "build" directory
$ meson compile      	# actually build Git
$ meson test         	# test the new build
$ meson install      	# install the new build

```

Multiple build directories can be set up using `meson setup <build_dir>`, and the configuration of the build inside a build directory can be viewed or changed by running `meson configure` inside the build directory.

More information on how to build Git using Meson can be found at the top of the `meson.build` file in the Git code repository. A comparison of the different build systems for Git is available as part of Git's technical documentation.

This project was led by Patrick Steinhardt.

## Git is now memory-leak-free (as exercised by the test suite)

In our Git release blog post about the previous Git 2.47.0 release, we talked about our ongoing effort to fix all memory leaks surfaced by existing tests in the project. We said that prior to the Git 2.47.0 release, the project had 223 test files containing memory leaks, and that this had been whittled down to just 60.

We are pleased to report that the memory leaks in all 60 remaining test files have been resolved. As a result, Git, as exercised by the test suite, is now free of memory leaks. This is an important step towards the longstanding goal of “libifying” Git internal components (which means converting those components into internal libraries). It will also help with optimizing Git for memory usage.

Now, any newly added test must be leak-free by default. It's still possible to have leaking tests, but the authors will have to use an escape hatch for that and provide good arguments why their test cannot be made leak free.

This project was led by Patrick Steinhardt.

## Improved bundle URI checks

In our Git release blog post about the Git 2.46.0 release, we talked about some bundle URI fixes by Xing Xin. After those fixes, Xing Xin worked on making it possible for fetches using bundles to be fully checked using the fsck mechanism like regular fetches.

When validating regular fetches, it's possible to specify different severities for different fsck issues to have fine-grained handling of what is accepted and what is rejected in a specific repository. This wasn't possible for fetches using bundles previously.

To further increase the usefulness and safety of bundle-uri, we addressed this problem so that the different severities specified for different fsck issues are now used when checking fetches using bundles, too.

This project was led by Justin Tobler.

## Add reference consistency checks

In our Git release blog post about the Git 2.47.0 release, we mentioned Jialuo She's work on adding a new 'verify' subcommand to git-refs(1) which was part of the Google Summer of Code 2024 (GSoC 2024).

In that blog post, we said that eventually the goal was to integrate this new subcommand as part of git-fsck(1) to provide a unified way to execute repository consistency checks. Jialuo She has decided to work on that after his GSoC was over.

The result from this effort is that git-fsck(1) can now detect and handle a number of reference-related issues, like when the content of a reference is bad, when a symbolic link is used as a symbolic reference, or when the target of a symbolic reference doesn't point to a valid reference. We still need to call `git refs verify` as part of git-fsck(1), and have the former perform all non-backend-specific checks that the latter currently does, but we are closer to our end goal of a unified way to execute all refs consistency checks.

This project was led by Jialuo She.

## Iterator reuse in reftables

In the Git 2.45.0 release, the 'reftables' format was introduced as a new backend for storing references (mostly branches and tags). If you are not yet familiar with the reftables backend, check out our previous Git release blog post where the feature was introduced and our beginner’s guide to learn more about how reftables work.

Since that release, we continued to improve this backend, and we recently focused on improving its performance by reusing some internal iterators when reading random references. Before these changes, reading a single reference required us to create a whole new iterator, seek it to the correct location in the respective tables, and then read the next value from it, which can be quite inefficient when reading many references in quick succession. After the change we now only create a single iterator and reuse it to read multiple references, thus saving some overhead.

The result of this work is increased performance in a number of reftables-related use cases, especially a 7% speedup when creating many references in a transaction that performs many random reads. Furthermore, this creates the possibility for more optimizations as we can continue to reuse more state kept in the iterators.

This project was led by Patrick Steinhardt.

## Support for reflogs in `git-refs migrate`

After the 'reftables' backend was introduced in Git 2.45.0 (see the section above), we worked on tooling to migrate reference backends in Git 2.46.0, which consisted of adding a new `migrate` subcommand to git-refs(1).

Our article about Git 2.46.0 talked about this work and mentioned some limitations that still existed. In particular, the article said:

"The reflogs in a repository are a component of a reference backend and would also require migration between formats. Unfortunately, the tooling is not yet capable of converting reflogs between the files and reftables backends."

We are pleased to report that we have lifted this limitation in Git 2.48.0. Reflogs can now also be migrated with `git refs migrate`. The migration tool is not yet capable of handling a repository with multiple worktrees, but this is the only limitation left. If you don't use worktrees, you can already take advantage of the reftables backend in your existing repositories.

This project was led by Karthik Nayak.

## Ref-filter optimization

The 'ref-filter' subsystem is some formatting code used by commands like `git for-each-ref`, `git branch` and `git tag` to sort, filter, format, and display information related to Git references.

As repositories grow, they can contain a huge number of references. This is why there is work not only on improving backends that store references, like the reftables backend (see above), but also on optimizing formatting code, like the 'ref-filter' subsystem.

We recently found a way to avoid temporarily buffering references and iterating several times on them in the ref-filter code when they should be processed in the same sorting order as the order the backends provide them. This results in memory savings and makes certain commands up to 770 times faster in some cases.

This project was led by Patrick Steinhardt.

## Read more

This blog post highlighted just a few of the contributions made by GitLab and the wider Git community for this latest release. You can learn about these from the official release announcement of the Git project. Also, check out our previous Git release blog posts to see other past highlights of contributions from GitLab team members.

- What’s new in Git 2.47.0?
- What’s new in Git 2.46.0?
- What’s new in Git 2.45.0
- A beginner's guide to the Git reftable format

The Git project recently released Git 2.48.0. Let's look at a few notable highlights from this release, which includes contributions from GitLab's Git team and the wider Git community.

## Meson build system

For a long time, Git could be built using either a Makefile\-based build system or an Autoconf\-based build system. Git developers have been using mostly the Makefile-based build system, so the Autoconf-based build system has lagged behind in features and maintenance. Another issue was that a lot of Windows developers use integrated development environments (IDEs) that don’t have good support for Makefile- and Autoconf-based build systems.

In 2020, support for building Git using CMake was added. CMake added better Windows support and IDE integration, especially for Visual Studio. Some modern build system features like out-of-source builds were also included.

Recently, it appeared the CMake support was also lagging behind and that it might never be a good option to replace the two other build systems. So Patrick Steinhardt, GitLab Git Engineering Manager, implemented support for the Meson build system with the goal of eventually replacing the Autoconf-, CMake-, and maybe the Makefile-based build systems.

The new Meson-based build system has the following advantages:

- Allows users to easily find the available build options, something which is difficult with Makefiles and CMake
- Has a simple syntax compared to Autoconf and CMake
- Supports many different operating systems, compilers, and IDEs
- Supports modern build system features like out-of-source builds

Here is an example of how it can actually be used to build Git:

```shell
$ cd git             	# go into the root of Git's source code
$ meson setup build/ 	# setup "build" as a build directory
$ cd build           	# go into the "build" directory
$ meson compile      	# actually build Git
$ meson test         	# test the new build
$ meson install      	# install the new build

```

Multiple build directories can be set up using `meson setup <build_dir>`, and the configuration of the build inside a build directory can be viewed or changed by running `meson configure` inside the build directory.

More information on how to build Git using Meson can be found at the top of the `meson.build` file in the Git code repository. A comparison of the different build systems for Git is available as part of Git's technical documentation.

This project was led by Patrick Steinhardt.

## Git is now memory-leak-free (as exercised by the test suite)

In our Git release blog post about the previous Git 2.47.0 release, we talked about our ongoing effort to fix all memory leaks surfaced by existing tests in the project. We said that prior to the Git 2.47.0 release, the project had 223 test files containing memory leaks, and that this had been whittled down to just 60.

We are pleased to report that the memory leaks in all 60 remaining test files have been resolved. As a result, Git, as exercised by the test suite, is now free of memory leaks. This is an important step towards the longstanding goal of “libifying” Git internal components (which means converting those components into internal libraries). It will also help with optimizing Git for memory usage.

Now, any newly added test must be leak-free by default. It's still possible to have leaking tests, but the authors will have to use an escape hatch for that and provide good arguments why their test cannot be made leak free.

This project was led by Patrick Steinhardt.

## Improved bundle URI checks

In our Git release blog post about the Git 2.46.0 release, we talked about some bundle URI fixes by Xing Xin. After those fixes, Xing Xin worked on making it possible for fetches using bundles to be fully checked using the fsck mechanism like regular fetches.

When validating regular fetches, it's possible to specify different severities for different fsck issues to have fine-grained handling of what is accepted and what is rejected in a specific repository. This wasn't possible for fetches using bundles previously.

To further increase the usefulness and safety of bundle-uri, we addressed this problem so that the different severities specified for different fsck issues are now used when checking fetches using bundles, too.

This project was led by Justin Tobler.

## Add reference consistency checks

In our Git release blog post about the Git 2.47.0 release, we mentioned Jialuo She's work on adding a new 'verify' subcommand to git-refs(1) which was part of the Google Summer of Code 2024 (GSoC 2024).

In that blog post, we said that eventually the goal was to integrate this new subcommand as part of git-fsck(1) to provide a unified way to execute repository consistency checks. Jialuo She has decided to work on that after his GSoC was over.

The result from this effort is that git-fsck(1) can now detect and handle a number of reference-related issues, like when the content of a reference is bad, when a symbolic link is used as a symbolic reference, or when the target of a symbolic reference doesn't point to a valid reference. We still need to call `git refs verify` as part of git-fsck(1), and have the former perform all non-backend-specific checks that the latter currently does, but we are closer to our end goal of a unified way to execute all refs consistency checks.

This project was led by Jialuo She.

## Iterator reuse in reftables

In the Git 2.45.0 release, the 'reftables' format was introduced as a new backend for storing references (mostly branches and tags). If you are not yet familiar with the reftables backend, check out our previous Git release blog post where the feature was introduced and our beginner’s guide to learn more about how reftables work.

Since that release, we continued to improve this backend, and we recently focused on improving its performance by reusing some internal iterators when reading random references. Before these changes, reading a single reference required us to create a whole new iterator, seek it to the correct location in the respective tables, and then read the next value from it, which can be quite inefficient when reading many references in quick succession. After the change we now only create a single iterator and reuse it to read multiple references, thus saving some overhead.

The result of this work is increased performance in a number of reftables-related use cases, especially a 7% speedup when creating many references in a transaction that performs many random reads. Furthermore, this creates the possibility for more optimizations as we can continue to reuse more state kept in the iterators.

This project was led by Patrick Steinhardt.

## Support for reflogs in `git-refs migrate`

After the 'reftables' backend was introduced in Git 2.45.0 (see the section above), we worked on tooling to migrate reference backends in Git 2.46.0, which consisted of adding a new `migrate` subcommand to git-refs(1).

Our article about Git 2.46.0 talked about this work and mentioned some limitations that still existed. In particular, the article said:

"The reflogs in a repository are a component of a reference backend and would also require migration between formats. Unfortunately, the tooling is not yet capable of converting reflogs between the files and reftables backends."

We are pleased to report that we have lifted this limitation in Git 2.48.0. Reflogs can now also be migrated with `git refs migrate`. The migration tool is not yet capable of handling a repository with multiple worktrees, but this is the only limitation left. If you don't use worktrees, you can already take advantage of the reftables backend in your existing repositories.

This project was led by Karthik Nayak.

## Ref-filter optimization

The 'ref-filter' subsystem is some formatting code used by commands like `git for-each-ref`, `git branch` and `git tag` to sort, filter, format, and display information related to Git references.

As repositories grow, they can contain a huge number of references. This is why there is work not only on improving backends that store references, like the reftables backend (see above), but also on optimizing formatting code, like the 'ref-filter' subsystem.

We recently found a way to avoid temporarily buffering references and iterating several times on them in the ref-filter code when they should be processed in the same sorting order as the order the backends provide them. This results in memory savings and makes certain commands up to 770 times faster in some cases.

This project was led by Patrick Steinhardt.

## Read more

This blog post highlighted just a few of the contributions made by GitLab and the wider Git community for this latest release. You can learn about these from the official release announcement of the Git project. Also, check out our previous Git release blog posts to see other past highlights of contributions from GitLab team members.

- What’s new in Git 2.47.0?
- What’s new in Git 2.46.0?
- What’s new in Git 2.45.0
- A beginner's guide to the Git reftable format

Go to Source
