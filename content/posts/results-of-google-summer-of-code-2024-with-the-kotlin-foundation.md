---
title: "Results of Google Summer of Code 2024 With the Kotlin Foundation"
date: 2025-01-22
categories: 
  - "development"
tags: 
  - "dev"
  - "developers"
  - "development"
  - "jetbrains"
  - "linux"
  - "software"
---

2024 marked another exciting year with respect to the Kotlin Foundation’s participation in Google Summer of Code (GSoC). GSoC is a global online program that introduces new contributors to open-source development. This year, contributors worked to expand the Kotlin ecosystem under the guidance of mentors from Google, Gradle, Microsoft, and JetBrains. Join our Slack channel \[…\]

2024 marked another exciting year with respect to the Kotlin Foundation’s participation in Google Summer of Code (GSoC). GSoC is a global online program that introduces new contributors to open-source development. This year, contributors worked to expand the Kotlin ecosystem under the guidance of mentors from Google, Gradle, Microsoft, and JetBrains.

Join our Slack channel to learn about the next GSoC and other opportunities to contribute.

Join GSoC Slack

We’re thrilled to share the results of GSoC 2024, with four projects successfully making it into production:

## Storytale – a Compose Multiplatform component gallery generator

This project introduces a much-needed gallery generator for Compose Multiplatform, designed to help developers showcase and test composable functions across platforms. To achieve this, contributor WhiteScent developed a Gradle plugin and runtime library that define and generate a centralized, multiplatform gallery app (for web, desktop, iOS, and Android), offering parameter adjustments and interactions in an isolated visual interface.

A big thanks to WhiteScent, a computer science student and Android enthusiast, and Artem Kobzar, a software engineer working on Kotlin/Wasm at JetBrains, for their great work!

Read the full blog post.

Share ideas and contributions to this project: Storytale on GitHub.

## Incremental compilation for the Kotlin/Wasm compiler

This project enhanced the Kotlin-to-Wasm compiler with incremental compilation capabilities, reducing build times by allowing it to recompile only modified files. The contributor, Osama Ahmad, optimized and reused components from the Kotlin/JS backend while improving documentation and refactoring the codebase.

Thanks to Osama Ahmad and the mentor from JetBrains, Igor Yakovlev, for their great input!

Read the full blog post.

Explore Kotlin/Wasm.

## Android support in the Gradle Build Server

This project brought Android project support to the Gradle Build Server. Despite starting with limited Gradle API knowledge, contributor Tanish Ranjan successfully implemented features like composite build support, Java Home handling, and Android project discovery. These enhancements have been integrated into production, enriching Gradle for Kotlin/Android development.

We thank Tanish Ranjan and the team of mentors, Oleg Nenashev, Donát Csikós, Bálint Hegyi, Sheng Chen, and Reinhold, for their valuable contributions to the Kotlin and Gradle ecosystems.

Read the full blog post.

Check out the project page.

## Support for Android targets in `kotlinx-benchmark`

This project enhanced the `kotlinx-benchmark` library, an open-source tool for benchmarking Kotlin code across platforms such as JVM, JS, wasmJs, and native, by adding support for Android benchmarking through integration with `androidx.benchmark`. To achieve this, the developers detected Android targets, generated template projects, integrated benchmark annotations, validated tests, and captured results.

Thank you to Qizhao Chen and the mentor team, Abduqodiri Qurbonzoda, Dustin Lam, and Rahul Ravikumar, for your great input.

Read the full blog post.

Check out the `kotlinx-benchmark` library.

We are immensely grateful to our contributors, the mentors, and the Kotlin Foundation for making GSoC 2024 a success!

Join our Slack channel to stay tuned for more updates and other opportunities to contribute:

Join GSoC Slack

Let’s continue to expand Kotlin’s reach and usability across platforms. Thank you!

Go to Source
