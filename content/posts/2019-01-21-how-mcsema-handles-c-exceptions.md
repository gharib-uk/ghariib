---
title: "How McSema Handles C Exceptions"
date: Mon, 21 Jan 2019 07:50:27 +0000
draft: false
type: posts
categories: 
- 
---
# How McSema Handles C Exceptions

<br/>

<br/>
C++ programs using exceptions are problematic for binary lifters. The non-local control-flow “throw” and “catch” operations that appear in C++ source code do not map neatly to straightforward binary representations. One could allege that the compiler, runtime, and stack unwinding library collude to make exceptions work. We recently completed our investigation into exceptions and can \[…\]

#### [Source](https://blog.trailofbits.com/2019/01/21/how-mcsema-handles-c-exceptions/)

<br/>
---
