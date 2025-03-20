---
title: "How to check if a mutex is locked in Go"
date: Tue, 09 Jun 2020 07:50:53 +0000
draft: false
type: posts
categories: 
- 
---
# How to check if a mutex is locked in Go

<br/>

<br/>
TL;DR: Can we check if a mutex is locked in Go? Yes, but not with a mutex API. Here’s a solution for use in debug builds. Although you can Lock() or Unlock() a mutex, you can’t check whether it’s locked. While it is a reasonable omission (e.g., due to possible race conditions; see also Why \[…\]

#### [Source](https://blog.trailofbits.com/2020/06/09/how-to-check-if-a-mutex-is-locked-in-go/)

<br/>
---
