---
title: "Fail Fast: when failure is a win"
date: 2025-01-07
categories: 
  - "cloud"
  - "cloudnative"
  - "devops"
  - "linux"
---

![fail fast approach image](https://www.codemotion.com/magazine/wp-content/uploads/2024/10/FAIL-FAST-APPROACH.webp)

In a world driven by rapid development and continuous innovation, failure isn’t always a setback—in fact, it can be a **winning strategy**.

Let’s talk about _Fail Fast_, a fundamental methodology in software development that aims to quickly identify the limitations and critical issues of a solution.

#### **What is Fail Fast?**

_Fail Fast_ is an approach with a single goal: to find a potential stopping point. If Descartes said _Cogito ergo sum_ (I think, therefore I am), here we could say _Deficio ergo sum_ (I fail, therefore I exist).

This approach is especially crucial in contexts like the IoT (Internet of Things), where the variables are numerous and often beyond our direct control. But trust me, this methodology can prove useful in any field.

Imagine working with a “black box,” an element over which we have little visibility or influence. The documentation is brief, with a list of expected behaviors, but few certainties. On the other hand, we have detailed user stories, reviewed together with UX and QA teams, ready to be tested against rigorous test plans.

#### **So, why aim to fail?**

Because discovering as early as possible that something isn’t working allows us to make timely decisions, adjust the course, and, if necessary, escalate the issue.

_Fail Fast_ is the opposite of “sandbagging,” which is the habit of postponing a problem, hoping it will be forgotten or classified as “non-reproducible.”

By acting immediately, we can prevent problems from accumulating and becoming unmanageable.

#### **How to Put it into Practice?**

_Fail Fast_ often finds its application through _Proof of Concept_ (PoC).

A team, or even a single developer, tests a solution to quickly validate it. But it’s not just about confirming whether something works; the PoC should challenge the solution, testing it under conditions that could lead to failure.

Senior or Expert profiles are typically involved in these activities due to their experience in identifying critical points and “hitting” where problems are most likely to arise. However, it’s not an absolute requirement: anyone with the right mindset and approach can contribute to the _Fail Fast_ process.

#### **A Practical Example: BLE Synchronization in Background on iOS**

A context where it’s easy to apply this strategy is hardware, often characterized by “black boxes” over which we have limited control.

Many of you may have a smart band or a smart ring. These devices continuously collect data, and for you users, it’s convenient to open the application and find updated data without waiting for a long synchronization process, right?

Great, but if we consider BLE (Bluetooth Low Energy) and iOS background operations as potential pain points, we’re dealing with a powder keg ready to explode at any moment.

From experience, we know there are critical patterns involving BLE and iOS in different scenarios (we could talk about it for days…). A classic example of applying _Fail Fast_ would be to immediately verify if the peripheral simply requires physical interaction to maintain or re-establish the connection periodically.

If we don’t run this test right away, we risk discovering too late that background synchronizations don’t work as expected. This could compromise the seamless user experience they anticipate, forcing us to review the entire software or hardware project.

Or, worse, see negative reviews on app stores increase steadily.

#### **Fail Fast: From PxD (Physical x Digital) to D² (Digital x Digital)**

In the _Physical x Digital_ (PxD) context, as is often the case in IoT projects, _Fail Fast_ is almost mandatory. But it’s also worth applying it in the _Digital x Digital_ (D²) realm—purely digital projects. If we receive documentation for a REST or GraphQL API, for example, why not test it immediately in a real-world scenario where it might fail? Better for us to find out than the client.

#### **“But aren’t there already automated tests?”**

It’s true, many believe that a solution validated by unit tests and internal QA is safe. However, recent failures, even in fields like aerospace, show that even an apparently stable system can present surprises when a third party starts interacting with it. Testing with a _Fail Fast_ approach, perhaps with a precise time-box, could make all the difference.

I’m curious to hear your thoughts and whether you’ve adopted this practice as well. Do you have any concrete examples? Let me know!

The post Fail Fast: when failure is a win appeared first on Codemotion Magazine.

Go to Source
