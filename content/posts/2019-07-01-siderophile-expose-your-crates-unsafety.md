---
title: "Siderophile Expose your Crates Unsafety"
date: Mon, 01 Jul 2019 11:30:06 +0000
draft: false
type: posts
categories: 
- 
---
# Siderophile Expose your Crates Unsafety

<br/>

<br/>
Today we released a tool, siderophile, that helps Rust developers find fuzzing targets in their codebases. Siderophile trawls your crate’s dependencies and attempts to finds every unsafe function, expression, trait method, etc. It then traces these up the callgraph until it finds the function in your crate that uses the unsafety. It ranks the functions \[…\]

#### [Source](https://blog.trailofbits.com/2019/07/01/siderophile-expose-your-crates-unsafety/)

<br/>
---
