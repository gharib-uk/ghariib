---
title: "Optimizing Lifted Bitcode with Dead Store Elimination"
date: Fri, 06 Jul 2018 07:50:11 +0000
draft: false
type: posts
categories: 
- 
---
# Optimizing Lifted Bitcode with Dead Store Elimination

<br/>

<br/>
Tim Alberdingk Thijm As part of my Springternship at Trail of Bits, I created a series of data-flow-based optimizations that eliminate most “dead” stores that emulate writes to machine code registers in McSema-lifted programs. For example, applying my dead-store-elimination (DSE) passes to Apache httpd eliminated 117,059 stores, or 50% of the store operations to Remill’s \[…\]

#### [Source](https://blog.trailofbits.com/2018/07/06/optimizing-lifted-bitcode-with-dead-store-elimination/)

<br/>
---
