---
title: "Panicking the right way in Go"
date: Wed, 26 Jun 2019 06:50:58 +0000
draft: false
type: posts
categories: 
- 
---
# Panicking the right way in Go

<br/>

<br/>
A common Go idiom is to (1) panic, (2) recover from the panic in a deferred function, and (3) continue on. In general, this is okay, so long there are no global state changes between the entry point to the function calling defer, and the point at which the panic occurs. Such global state changes \[…\]

#### [Source](https://blog.trailofbits.com/2019/06/26/panicking-the-right-way-in-go/)

<br/>
---
