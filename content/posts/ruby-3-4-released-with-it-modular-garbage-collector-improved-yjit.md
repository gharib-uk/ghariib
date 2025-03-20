---
title: "Ruby 3.4 Released with ‘it’, Modular Garbage Collector, Improved YJIT"
date: 2025-01-02
categories: 
  - "general"
---

![](https://ubuntuhandbook.org/wp-content/uploads/2024/01/ruby-logo-250x250.webp)

Ruby, the free open-source high-level general-purpose programming language, released new 3.4.0 (then 3.4.1 with quick fix) today at Christmas!

The release introduced `it` block parameter reference, which behaves very much the same as `_1`. As the feature request descripted:

> _`it` is implemented as a special case of `getlocal` insn, not a method. `it` without an argument is considered `_1` or a normal local variable if defined. `it` is considered a method call only when it has any positional/keyword/block arguments._

![](https://ubuntuhandbook.org/wp-content/uploads/2024/12/ruby-it-700x378.webp)

In Ruby 3.4, **the socket library now features Happy Eyeballs Version 2 (RFC 8305)**. Which, ensures minimized connection delays, even if a specific protocol or IP address is delayed or unavailable.

This feature is enabled by default. Though, can be disabled either on a per-method basis by using the keyword argument `fast_fallback: false`, or globally by setting environment variable `RUBY_TCP_NO_FAST_FALLBACK=1` or calling `Socket.tcp_fast_fallback=false`

The release also **introduced modular garbage collector (GC)**. It can be enabled at build time by `--with-modular-gc` configuration option. To load at runtime, use the environment variable `RUBY_GC_LIBRARY`.

The built-in garbage collector has been splitted. It can be built as a library and enabled by environment variable `RUBY_GC_LIBRARY=default`. While, there’s also an experimental GC library based on MMTk, which can be built then enabled by `RUBY_GC_LIBRARY=mmtk`.

The new release also **switched the default parser from parse.y to Prism**. While the previous parser is available by using `--parser=parse.y` command-line argument.

The YJIT compiler has been improved with better performance on x86-64 and arm64 platforms, and less memory usage through compressed metadata and a unified memory limit.

While, there are new `--yjit-mem-size` command option to unify memory limit (default 128MiB) to track total YJIT memory usage, and `--yjit-log` that enables a compilation log to track what gets compiled.

Other changes in Ruby 3.4.0 include:

- Keyword splatting `nil` when calling methods is now supported.
- Block passing and keyword arguments are no longer allowed in index.
- Add `GC.config` to allow setting configuration variables on the Garbage Collector.
- Introduce `rgengc_allow_full_mark` GC configuration parameter.
- Add `Ractor.main?`, `Ractor.[]`, and `Ractor.[]=`.

There are as well other core classes, standard library, API updates, and bug-fixes. See the official release note for details.

### How to Install Ruby 3.4.1

Ruby is available in App Center (or Ubuntu Software) as official Snap package, though NOT updated to the lastest at the moment of writing.

If you can’t wait Snap or third-party package manager, then, you may download the source tarball from the link button below:

Download Ruby (tarball)

And, here’s a step by step guide (Method 4) talking about how to build it from source.

Go to Source
