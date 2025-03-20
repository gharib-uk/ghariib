---
title: "The IntelliJ Scala Plugin in 2024"
date: 2025-01-04
categories: 
  - "general"
  - "linux"
tags: 
  - "dev"
  - "developers"
  - "development"
  - "jetbrains"
  - "software"
---

The Year in Review Time flies. Only a year ago, we saw the release of Scala 3.4.0-RC1, and now we’re trying out Scala 3.6.2 with many new experimental features. The last 12 months have brought many new features to the IntelliJ Scala Plugin as well. A year ago we introduced X-Ray mode, which lets you \[…\]

### The Year in Review

Time flies. Only a year ago, we saw the release of Scala 3.4.0-RC1, and now we’re trying out Scala 3.6.2 with many new experimental features. The last 12 months have brought many new features to the IntelliJ Scala Plugin as well. A year ago we introduced X-Ray mode, which lets you easily augment the code you see in the editor with information about inferred types and other details. We’ve improved Scala 3 support, compiler-based highlighting, and the debugger. We’ve improved auto-completion, both the standard and the AI-powered versions. We’ve introduced support for named tuples, which are still an experimental feature, and we’ve implemented a new way to work with sbt projects. On top of all that, this year, JetBrains joined the Scala Advisory Board. You can read about those changes in our release notes and other blog posts:

- _The X-Ray Mode_

- Release Notes 2024.1

- Release Notes 2024.2

- Release Notes 2024.3

- _New Module Layout for sbt Projects_

- _JetBrains Joins the Scala Advisory Board_

### State of Developer Ecosystem Survey

This year, JetBrains conducted its eighth annual State of Developer Ecosystem Survey. We compared your answers about your Scala development preferences to the results from last year, and we learned a few interesting things:

#### Which versions of Scala do you regularly use?

The most significant shift is the jump in Scala 3 usage from 45% in 2023 to 51% in 2024. However, the usage of Scala 2.13 fell a little bit, and even Scala 2.12, though it lost popularity, sits at a modest 23%. What can we make of this? The additional breakdown of responses regarding which versions are used at work and which are used for hobby projects gave us a pretty clear picture. The community has embraced Scala 3 in open-source projects, libraries, and new projects written from scratch, but Scala 2.13 still plays a prominent role in the professional world.

![](https://blog.jetbrains.com/wp-content/uploads/2024/12/WhichScala.png)

#### Which Scala 3 features do you use?

When it comes to adopting Scala 3 features, the features with the most growth (enums, `given` and `using` clauses, top-level definitions) are those that simplify common patterns or reduce boilerplate. This suggests developers prioritize tools that enhance productivity and readability. Other, more advanced features have also become more popular, which corresponds to the rise in popularity of Scala 3 overall. Niche features – like dependent function types and kind polymorphism – will likely remain specialized tools for advanced library authors or specific applications.

Next year, we will move this question from the main Developer Ecosystem Survey to a separate small opinion poll.

![](https://blog.jetbrains.com/wp-content/uploads/2024/12/WhichFeatures.png)

#### Which unit testing frameworks do you regularly use, if any?

With 71% of users, ScalaTest is the clear leader. This reflects its maturity, ease of use, and extensive feature set – all reasons why many teams see it as a go-to solution. MUnit and ScalaCheck have retained stable fan bases, even though they both lost a few points. On the other hand, the decline of ZIO Test’s popularity might be surprising, given that ZIO remains a popular framework.

![](https://blog.jetbrains.com/wp-content/uploads/2024/12/WhichTesting.png)

#### Which frameworks and libraries do you regularly use?

Cats has experienced a small rise, solidifying its position as one of the most widely used libraries in the Scala ecosystem. This trend might be connected with a shift toward more rigorous functional programming in the community since F2 and ZIO, both mature projects built on functional programming principles, also experienced small bumps.

Akka lost 5 points but maintained a solid user base. It looks like Pekko, a fully open-source fork of Akka, has attracted some Akka users seeking a familiar framework without licensing constraints. This is the first year we have asked about Pekko in the Developer Ecosystem Survey, and it already has a 15% share of the responses.

The other libraries either maintained their user bases or experienced slight losses. This may mean that Scala development is moving towards and consolidating around tried and trustworthy solutions. Scala developers still like to experiment but we leave it more for hobby projects, sticking instead to what we know in our full-time jobs. The results for frameworks and libraries used in web development seem to confirm this conclusion: not much has changed; we still see the dominance of a few mature solutions without any new experimental projects significantly rising in popularity.

![](https://blog.jetbrains.com/wp-content/uploads/2024/12/WhichLibraries.png)

#### Which editors or IDEs do you use for Scala development?

Finally, when it comes to the popularity of IDEs and editors used to write Scala code, IntelliJ IDEA with the Scala Plugin is still on top at 80%. VS Code is next with 25%, and Emacs and Vim are in third and fourth place. Many people don’t use just one IDE for all their work, which is why the results don’t add up to 100%. As we already learned from the questions about Scala 3 and its features, Scala developers like to experiment in their hobby projects, but they stick to mature solutions in their professional work. Maybe this explains these results as well.

![](https://blog.jetbrains.com/wp-content/uploads/2024/12/WhichEditors.png)

### Share of top-paid employees

And one more thing before we finish: This year’s Developer Ecosystem Survey shows that, just as in 2023, Scala is the programming language with the largest percentage of top-paid developers, i.e. those whose salaries are in the top quartile of their country or region.

![](https://blog.jetbrains.com/wp-content/uploads/2024/12/Salaries.png)

Check out the State of Developer Ecosystem Report to learn more about the results.

To conclude, we wish you a peaceful end of the year, a lovely holiday season, and… happy development!

The IntelliJ Scala Plugin team

Go to Source
