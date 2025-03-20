---
title: "Debuginfod project update 2024"
date: 2025-01-15
tags: 
  - "comprehensive"
---

It has been five years since we first introduced `debuginfod`, a tool for distributing debugging resources such as executables, debug information, and source files. Since then we have continued to improve `debuginfod` as well as tools that make use of `debuginfod`, such as GDB and Valgrind.

In this article we'll review some of the notable updates and enhancements to `debuginfod` over the past year as well as the scale at which public `debuginfod` servers are operating.

First I will briefly describe what debug info is and how `debuginfod` helps. Debug info is needed for mapping the binary contents of executables, shared libraries, and core files back to source code. Debug info enables tools to provide detailed information such as the function name, source file, and line number corresponding to binary instructions in an executable that trigger a crash. Debug info is very useful but acquiring and managing it can be tedious. Traditional methods require manual installation of debug RPM packages along with root access. Debug info for large executables or shared libraries can get quite large, making storage a concern. For example, `libwebkitgtk.so` debug info is over 3GB. Additionally, matching the exact versions of debug info for older binaries or remote processes can be difficult.

`debuginfod` addresses these challenges by serving as an HTTP client and server for distributing executables, debug info, and source files. It automates access to debugging resources, eliminating the need for manual installations and ensuring that tools fetch only the exact versions needed. By leveraging build IDs—unique identifiers for specific builds—`debuginfod` simplifies the debugging process for developers.

Setting up a `debuginfod` server involves indexing directories for ELF binaries and archives (RPN, DEB, etc.), with options for custom file types. Servers can federate, querying each other for missing files. Developers can interact with `debuginfod` via commands like `debuginfo-find` or APIs provided by `libdebuginfod`. Requested files are downloaded into a local cache, helping speed up repeated queries.

## Metrics and scale of debuginfod servers

`debuginfod`’s web API provides access to server metrics in the Prometheus format, which can be viewed at the `/metrics` endpoint of any server. For example, see this resource. These metrics are monitored by the `debuginfod` developers using a Red Hat internal Grafana instance in order to analyze server activity.

Fedora’s official `debuginfod` server is the leader in terms of data served, handling 4.5 TB of data weekly. Arch Linux follows with 2.75 TB per week. Corpus size, which represents the total size of all indexed debugging files, is similarly impressive. Fedora’s staging server manages 25.7 TB, while its main server closely follows with 25.6 TB. Ubuntu’s server also hosts a substantial 18.9 TB. In terms of database index size, Ubuntu leads with 297 GB, while Fedora’s servers each maintain indices of 157 GB.

To enhance performance, servers maintain a file descriptor cache for recently extracted files. Fedora’s public server has the largest cache among public servers at 98 GB, with Debian close behind at 94.8 GB.

## New tools and features in debuginfod

An addition to the ecosystem of tools supporting `debuginfod` is `eu-srcfiles`, part of the elfutils project. This tool allows developers to list source files and Compilation Unit (CU) names associated with an ELF binary, shared library, or core dump. It can also process a running process’s ID to identify relevant files.

Beyond listing, `eu-srcfiles` can download all source files related to an ELF binary or process, ensuring exact matches using build IDs. The downloaded files are organized into a directory tree and packaged as a zip archive for convenience. This functionality is particularly useful for replicating the exact development environment needed for debugging.

Another enhancement is the introduction of metadata queries. Servers can now provide JSON-formatted data about files matching specific paths or patterns. This makes it easy for tools to map source file names to build IDs. The debugging tool SystemTap uses metadata queries in order to easily acquiring debug info for multiple versions of a library or executable, which facilitates live-patching a range of binaries known to have a security bug, for example.

## IMA verification support

`debuginfod` now optionally supports per-file Integrity Measurement Architecture (IMA) verification. IMA is a Linux kernel feature that detects file tampering. Fedora has recently introduced support for per-file IMA verification, adding an extra layer of security for users. `debuginfod` can now take advantage of Fedora's per-file signature in order to help verify the integrity of downloaded files. This feature can be controlled via the `DEBUGINFOD_URLS` environment variable (see the `debuginfod-find` man page for more information), with options to enforce verification (`ima:enforcing`) or bypass it (`ima:ignore`).

## Addressing kernel VDSO extraction bottlenecks

Performance issues with kernel virtual dynamic shared object (vDSO) extraction were a longstanding challenge. These shared libraries are automatically loaded into user-space processes and are frequently queried by tools like GDB. Previously, extracting `vdso.debug` from kernel debug archives could take over 60 seconds on Fedora’s `debuginfod` server.

The root of the problem lay in the compression format used. Kernel debug RPM and DEB archives use XZ compression, which supports random access with multithreaded compression. By implementing random access functionality in `debuginfod`, the extraction process was significantly optimized. The time required to extract `vdso.debug` was reduced to under a second, a 200-fold improvement. For more information see Omar Sandoval's blog post on this topic. Omar is the author of this vDSO extraction optimization.

## Lazy debug info downloading in Valgrind and GDB

Both Valgrind and GDB have implemented or are in the process of implementing lazy downloading, which postpones fetching unnecessary debug info until it is required. This approach addresses the inefficiency of downloading large volumes of debug info that may not be used during a debugging session.

In Valgrind, lazy downloading reduces the amount of debug info downloaded at startup. Previously, Valgrind downloaded all debug info for shared libraries during the program’s initialization phase, leading to delays. With lazy downloading, debug info is fetched only when needed, such as during backtrace generation. Testing has shown best-case reductions of up to 90% in downloaded debug info, with overall runtime improving by approximately 25%. Valgrind lazy debuginfo support is available in Fedora 37+ and Red Hat Enterprise Linux (RHEL) 8+.

GDB’s approach to lazy downloading is still under development. It focuses on downloading only the `gdb_index` initially. This section provides enough information to set breakpoints and resolve symbols without fetching the full debug info file (see this resource for more information on `gdb_index`). For scenarios where source file information is needed, GDB downloads additional sections like `debug_line` or `debug_line_str`, which are much smaller compared to full debug info files. The combined sizes of `gdb_index`, `debug_line`, and `debug_line_str` average about 8% of the size of the full debug info file. The lazy method ensures efficient downloading while maintaining the flexibility required for comprehensive debugging. GDB lazy debuginfo downloading is currently under development.

## Conclusion

Recent updates to `debuginfod` highlight its growing role in simplifying debugging workflows and addressing performance challenges. From tools like `eu-srcfiles` to features like IMA verification and lazy downloading, `debuginfod` continues to adapt to the needs of developers working on complex projects. As these improvements are adopted across tools and distributions, they promise to make debugging a more streamlined and efficient process.

The post Debuginfod project update 2024 appeared first on Red Hat Developer.  
  

Go to Source
