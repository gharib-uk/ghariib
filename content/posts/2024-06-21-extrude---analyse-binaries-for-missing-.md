---
title: "Extrude - Analyse Binaries For Missing Security Features Information Disclosure And More"
date: Fri, 21 Jun 2024 08:30:00 -0400
draft: false
type: posts
categories: 
- Extrude,Nx,Portable Executable,Relro
---
# Extrude - Analyse Binaries For Missing Security Features Information Disclosure And More

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/a/AVvXsEjMpNK6ke1fMDaTp3RU_dC5NuigZxpNAUS1hYMy1SLi0veJSiMVOts2jqCZK3LFM6iiAq-pYiOzinewBQIvkLmbc25i3UBTZd_sRBoj2YxpOZuGaYdSjRha5ZIi1cM-DlHeqLJWkU6U_rg3T7nHulq2MQTPoOF0UQxmjX8K_4Y4jp7pRFJ-VZiLOEgEnCqO=w640-h438)](https://blogger.googleusercontent.com/img/a/AVvXsEjMpNK6ke1fMDaTp3RU_dC5NuigZxpNAUS1hYMy1SLi0veJSiMVOts2jqCZK3LFM6iiAq-pYiOzinewBQIvkLmbc25i3UBTZd_sRBoj2YxpOZuGaYdSjRha5ZIi1cM-DlHeqLJWkU6U_rg3T7nHulq2MQTPoOF0UQxmjX8K_4Y4jp7pRFJ-VZiLOEgEnCqO)

  

Analyse binaries for missing security features, information disclosure and more.

Extrude is in the early stages of development, and currently only supports [ELF](https://www.kitploit.com/search/label/ELF "ELF") and MachO binaries. PE (Windows) binaries will be supported soon.

  

Usage
-----

```
Usage:  extrude [flags] [file]Flags:  -a, --all               Show details of all tests, not just those which failed.  -w, --fail-on-warning   Exit with a non-zero status even if only warnings are discovered.  -h, --help              help for extrude
```

Docker
------

You can optionally run extrude with docker via:

```
docker run -v `pwd`:/blah -it ghcr.io/liamg/extrude /blah/targetfile
```

Supported Checks
----------------

### ELF

-   PIE
-   RELRO
-   BIND NOW
-   Fortified Source
-   Stack Canary
-   NX Stack

### MachO

-   PIE
-   Stack Canary
-   NX Stack
-   NX Heap
-   ARC

### Windows

_Coming soon..._

TODO
----

-   Add support for PE
-   Add secret scanning
-   Detect packers

  
  

**[Download Extrude](https://github.com/liamg/extrude "Download Extrude")**

[Source](http://www.kitploit.com/2024/06/extrude-analyse-binaries-for-missing.html)
<br/>
---
