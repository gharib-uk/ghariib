---
title: "How the IntelliJ Infrastructure Team Cuts Build Execution Time by 30% With TeamCity"
date: 2025-03-19
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

Introduction JetBrains is known for developing some of the world’s most popular IDEs, including IntelliJ IDEA, PyCharm, and WebStorm. Supporting this development is a dedicated infrastructure team that ensures hundreds of developers can efficiently build, test, and release these products. At the core of their workflow is TeamCity, JetBrains’ own CI/CD solution, which powers build \[…\]

## Introduction

JetBrains is known for developing some of the world’s most popular IDEs, including IntelliJ IDEA, PyCharm, and WebStorm. Supporting this development is a dedicated infrastructure team that ensures hundreds of developers can efficiently build, test, and release these products.

At the core of their workflow is TeamCity, JetBrains’ own CI/CD solution, which powers build automation, testing, and scalable infrastructure management.

<figure>

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/Example-of-a-build-chain-in-TeamCity.png)

<figcaption>

_Example of a build chain in TeamCity_

</figcaption>

</figure>

To understand how TeamCity supports this large-scale development process, we spoke with the IntelliJ Infrastructure team about their approach to managing CI/CD pipelines for 700–800 developers, running thousands of daily builds, and optimizing workflows to deliver fast and reliable releases.

## The challenge: scaling CI/CD for hundreds of developers

With a large team continuously pushing changes, maintaining fast, reliable, and scalable CI/CD pipelines is a complex task. The IntelliJ Infrastructure team requires a solution capable of handling thousands of builds per day without overloading resources while leveraging smart automation to reduce manual effort and ensure high-quality code delivery.

Additionally, their CI/CD setup must support a diverse infrastructure, including on-premises servers, AWS, and macOS, Linux, and Windows agents. Managing hundreds of thousands of automated tests while minimizing flaky test failures is another critical challenge.

<figure>

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/teamcity-grafana-dashboard.png)

<figcaption>

_TeamCity Grafana dashboard_

</figcaption>

</figure>

To meet these demands, the team relies on TeamCity, JetBrains’ own powerful CI/CD solution, which provides the scalability, flexibility, and automation needed to streamline their development workflows.

> > “We build IDEs and everything related to them in TeamCity, like internal services, services for statistics, etc. I’m very used to TeamCity and feel like it’s a hammer in my hand: you can do anything with it.”
> 
> ![](https://blog.jetbrains.com/wp-content/uploads/2025/03/dmitry-panov-1.jpeg)
> 
> **Dmitry Panov, IntelliJ Infrastructure Technical Team Lead**

Read the full case study to learn how TeamCity helps the IntelliJ Infrastructure team to manage the CI/CD process for 700+ engineers, while cutting the build time by 30%.

Read the case study

Go to Source
