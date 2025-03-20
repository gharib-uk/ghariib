---
title: "JITWatch4i: Analyzing IntelliJ IDEA’s Startup"
date: 2025-02-01
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

Introduction A typical Java or Kotlin programmer spends most of their productive time either creating application code in an editor or searching for bugs in a debugger. Occasionally, they might dive into a profiler when looking for places where the application spends too much time. However, they almost never venture into the Java C1 or \[…\]

## **Introduction**

A typical Java or Kotlin programmer spends most of their productive time either creating application code in an editor or searching for bugs in a debugger. Occasionally, they might dive into a profiler when looking for places where the application spends too much time. However, they almost never venture into the Java C1 or C2 compilers and their resulting products – low-level assembly code. For the most part, the Java compilers are black boxes that typically remain closed under normal circumstances.

Some time ago, I tried to analyze the startup of larger projects, such as IntelliJ IDEA. When analyzing program startup in a JIT-based virtual machine, it’s essential to realize that standard profiling tools don’t provide an accurate picture of CPU load or the performance of individual methods. The final times are distorted because some code runs in a non-optimized form and some CPU capacity is used for compiling methods. This led me to search for a tool that could display compilation processes, and that’s how I found JITWatch.

This article introduces JITWatch4i, an IntelliJ IDEA plugin based on JITWatch, designed for analyzing and visualizing compilation processes directly within IntelliJ IDEA. After laying a theoretical foundation with a general overview of Java’s tiered compilation process, we’ll demonstrate the plugin in action, comparing IntelliJ IDEA’s startup speed under different values of the `-XX:TieredOldPercentage` parameter.

### **JITWatch**

Developed by Chris Newland, JITWatch is a Java program that analyzes JVM (HotSpot) compilation logs and provides a detailed analysis of the behavior of both compilers in the Java Virtual Machine.

### **Why JITWatch4i?**

From my point of view, the original JITWatch is undoubtedly a useful tool for analyzing JIT processes in the JVM. However, while using it, I encountered several issues that made my work more difficult:

- **Configuration requires you to set paths to source files**, that is, to all modules containing code for the analyzed project. For large applications like IntelliJ IDEA, whose complete structure you may not even know, this is quite time-consuming.

- **Configuration requires you to set class locations,** essentially duplicating the _classpath_ of the analyzed program.

- **JITWatch uses JavaFX for its UI.** It takes a long time to visualize a project with tens of thousands of compilations in JavaFX, and some charts are practically unusable.

- **Installing and running** JITWatch isn’t complicated, but it also isn’t a one-click process.

When I used JITWatch, I particularly struggled with its lack of direct integration into a modern development environment – specifically the environment of the project I was working on. Eventually, I concluded that such an integration could bring new life to this great project, which led me to create JITWatch4i.

JITWatch4i is a plugin for IntelliJ IDEA that integrates the JIT analysis visualization features of the original JITWatch directly into the IDE. Integration with IntelliJ IDEA removes the need to manually configure paths to source code or compiled classes, as the IDE already has information about the structure of the currently open project and its dependencies, which the plugin can directly use.

Furthermore, the original JavaFX framework was replaced with the older but simpler Swing library, which is significantly optimized in the JetBrains Runtime. As a result, visualizing large projects is still reasonably fast, even when dealing with a large number of compilations.

### **Typical JITWatch use cases**

According to the documentation of the original JITWatch project, this tool is useful in several key areas. It allows you to:

- Verify whether methods you consider performance-critical were JIT-compiled during program execution.

- Find out exactly when certain methods were compiled and better understand the impact of JVM compilation threshold settings.

- Learn how long the compilation of individual methods took, which ones took the compiler the longest, or which generated the most native code.

- Better understand how Java compilers work.

- Track how your source code is translated into bytecode and ultimately into machine code.

## **Introduction to Java compilation**

To delve further into this topic, it’s helpful to have a basic understanding of Java compilers.

Fundamentally, the JVM contains an **interpreter**, which is used for a limited number of initial method calls, and two main compilers:

- **C1**, which is capable of quickly generating less-optimized native code. By default, C1 generates code that also collects profiling statistics later used by C2. This mode is called **tiered compilation**.

- **C2**, although slower than C1, creates code that is significantly faster. C2 leverages statistics collected by the code compiled with C1 to decide how to optimize the code. Statistics for a given method are gathered while it is running in an interpreter or in code compiled with additional profile-gathering code.

### **Compilation levels**

In this context, you’ll often hear about five compilation levels labeled L0–L4:

- **L0 –** A term indicating that a method is executed in the interpreter, during which basic statistics, such as the number of calls and backward jumps, are collected.

- **L1** – C1 compilation that does not include profiling for C2. It provides the fastest possible output from C1 and is mainly used for trivial methods where deep optimization in C2 wouldn’t provide a significant benefit.

- **L2** – C1 compilation with limited profiling, with statistics collected on the number of method calls and backward jumps. This allows us to determine which methods are actively used so that their subsequent compilations can be planned. L2-compiled code is on average about 30% faster than L3-compiled code. At application startup, when the C2 compiler is overloaded, it’s more time-efficient to compile code using L2. If the method remains active, the scheduler’s decision mechanism will later choose to compile it at L4.

- **L3** – C1 compilation with full profiling. Unlike L2, this level also gathers statistics on conditional branches and information about which classes are used in the method. L3 code is the slowest compiled code produced by the C1 compiler. The compiler scheduler aims to minimize the time a method spends running in L3 code.

- **L4** – Compilation with C2, which leverages the statistics collected previously. This makes it possible to generate faster, more efficient native code.

### **Compilation queue**

The JVM uses a compilation queue to manage and prioritize tasks across compiler threads. Methods are queued based on a compilation policy that prioritizes those likely to benefit most from optimization, ensuring the efficient use of resources and delivering performance gains.

### **Compilation parameters**

During its lifetime, a single method can run under 5 different compilation levels (L0–L4). Transitions between the per-method compilation levels (L1–L4) in the JVM are controlled by a set of key parameters. These parameters dictate when a particular method is promoted to a higher level of optimization, which in turn influences both startup speed and long-term performance. Below are the most important ones:

- `-XX:Tier3InvocationThreshold` – The number of calls required to transition to L3. Default value: `2,000`.

- `-XX:Tier4InvocationThreshold` – The number of calls required to transition to L4 (C2). Default value: `15,000`.

- `-XX:TieredOldPercentage` – A somewhat mysterious parameter that significantly impacts startup speed. It specifies the percentage threshold after which a method is considered old and ceases to be prioritized, based on the length of the compilation queue. Default value: `1000`.

These parameters influence how quickly methods transition between compilation levels. Compilation levels are normally upgraded, with the exceptions being cases of deoptimization or when the C2 compiler is overloaded. Lowering these parameters accelerates the progression of code through its compilation levels, effectively speeding up its “maturity.” However, this comes at the cost of increased overhead during application startup, as methods are compiled more frequently and at earlier stages.

## **Analyzing the startup of IntelliJ IDEA**

One use case for the JITWatch4i plugin is analyzing an application’s startup. Let’s demonstrate this with an example of IntelliJ IDEA’s startup under different values of the `-XX:TieredOldPercentage` parameter. For simplicity, we’ll compare two tests: one with `-XX:TieredOldPercentage=100000`, which is the default value in IntelliJ IDEA, and another with `-XX:TieredOldPercentage=1000`, which is the default value in the JVM.

To analyze the startup, we need to run IntelliJ IDEA with parameters that generate compilation logs, which we will then load into JITWatch4.

For the first test, we set the following in `idea64.vmoptions` (the configuration of `TieredOldPercentage` is already in use):

```
-XX:TieredOldPercentage=100000
-XX:+UnlockDiagnosticVMOptions
-XX:+LogCompilation
-XX:LogFile=compilation_100k.log
```

For the second test, we set this in `idea64.vmoptions`:

```
-XX:TieredOldPercentage=1000
-XX:+UnlockDiagnosticVMOptions
-XX:+LogCompilation
-XX:LogFile=compilation_1k.log
```

For easier comparison, we use the following command to make IntelliJ IDEA run for the same amount of time in both cases:

```
timeout --kill-after=5 20 ./idea.sh
```

We load the compilation logs into JITWatch4i and compare them using the _Timeline_ and _Comp. Activity_ tabs.

### **Timeline**

The graph on the _Timeline_ tab illustrates the L1–L4 compilations over time, with each line color representing a specific compilation level:

- **Black** – Total compilations

- **Blue** – L1

- **Red** – L2

- **Magenta** – L3

- **Green** – L4

<figure>

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe_ZUvdY2OdoMKBlFaobQPX0h3fgRWOWi0SuzsIdhXH_qPeCumDklSiAjLEwtZ-OE3m4ttKBfofgIIQCPbGoJtWYYlrPalHXt9XQxShwPjhW51_Chgij8ca12vBpFup63XsRykK8Q?key=-TGROJfV7_SUuvTbEWKQJxdO)

<figcaption>

`-XX:TieredOldPercentage=100000`

</figcaption>

</figure>

<figure>

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcYANQnnsZjiShiHe9cHq33uglqKxm6JObGi_FLo5rPElr_BlWt1BMyVlDAtcsh4k08y1wrUG8OUWYjU1UGTg9Suws0UtO6ZM9bd87muBTC8pkLvHMsleReQOFq8f8ec3FAEe85?key=-TGROJfV7_SUuvTbEWKQJxdO)

<figcaption>

`-XX:TieredOldPercentage=1000`

</figcaption>

</figure>

When comparing the charts, it’s clear that:

- With `-XX:TieredOldPercentage=100000`, there are far more L2 (red line) compilations than with `-XX:TieredOldPercentage=1000`. Typically, a method follows the path **L0 → L3 → L4** unless the C2 compiler is overloaded. When C2 is overloaded, a method may be compiled from L0 to L2 instead of to L3. Whether a method takes the L2 path or skips straight to L3 or L4 depends on how busy the C2 compiler is and whether the method is considered “old”.

- If the compiler is overloaded, the method may go to L2.

- If the method is old, L2 is skipped entirely and the method goes from L0 to L3 or L3 to L4.

The `TieredOldPercentage` parameter determines when a method is considered old by adjusting the JVM `*Threshold` parameters at which a method changes levels. Once a method is classified as old, the JVM stops routing it through L2. An issue arises, however, when the C2 compiler is overloaded and unable to accept new tasks, which means methods cannot graduate from L3 to L4, leaving them stuck at L3 for an extended period. This slows performance because L3 compilation involves collecting extensive statistics.

- The charts show that for `-XX:TieredOldPercentage=100000`, the numbers of L3 and L4 compilations are almost the same. In this case, methods that do not progress to L4 remain in L2 instead of being promoted to or staying in the slower L3 code. L2 code is approximately 30% faster than L3 code, so this configuration avoids generating an excess of slower L3 methods. As a result, IntelliJ IDEA starts faster with this parameter.

### **Compilation queue**

<figure>

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfbbPFHz_OzRNhOzScy8cQXxtY7109wBBjGDRQn5xeOvOZdGJhXeaiaf45IIk0um2HKNixGy8avHP4gAlUzS8eNKy6wAxJdnfXku9qMMuF1-ou-Wky2jEe65ZUPjqtusvwG9WC2hQ?key=-TGROJfV7_SUuvTbEWKQJxdO)

<figcaption>

`-XX:TieredOldPercentage=100000`

</figcaption>

</figure>

<figure>

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfew5_tMrXJFiMJCyE2z35SnH7t23IBzzFsuGlJ6VmtDZTpRhH-VKDg7l-pFz9mot1csAiXlxlp2v9EeDV0Q9NRZ46wYkxI-qolzVxomkLZNcMGsTBrOSpbHwhH1YHQtSgEocONyA?key=-TGROJfV7_SUuvTbEWKQJxdO)

<figcaption>

`-XX:TieredOldPercentage=1000`

</figcaption>

</figure>

In the _Compiler Queues_ chart on the _Comp.Activity_ tab, you can see the length of the compiler queue over time. Comparing them reveals that the `-XX:TieredOldPercentage=1000` queue is initially more overloaded than the `-XX:TieredOldPercentage=100000` queue.

### **Compilation activity**

<figure>

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcROY-3pYU3UVsKLWFZJ3PrxfqjNNKDbKT3kuUhrMc5d9VD3YeIXl6qZtEHH4qdtAy1yeLJpS37StyY6BOM5CJIYINPWzq-2U6pennExYvreItoGTzzcCZ7R3RPPNgwLd-NIf_1?key=-TGROJfV7_SUuvTbEWKQJxdO)

<figcaption>

`-XX:TieredOldPercentage=100000`

</figcaption>

</figure>

<figure>

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcQ2a9RIX9OBv6CdmVls51j0nJ8f4t8IEdVZgoWCCvDQnTr8RxfNt1z-e4Zi0gEAfhPpdMHtQADdhiHnyxgARTdxME00sJfJYh3qSY4x9-dkY4Lq9-3xEawokbM9NT2mbGGLTEc?key=-TGROJfV7_SUuvTbEWKQJxdO)

<figcaption>

`-XX:TieredOldPercentage=1000`

</figcaption>

</figure>

Let’s compare the lengths of individual method compilations in the _Native Size_ chart on the _Comp.Activity_ tab. The X-axis represents time, and the rectangles correspond to the compilation of individual methods. The height of a rectangle is proportional to the length of the resulting native method. It is apparent that the compilation of some methods in C2 is truly long. 

Further gains may be achievable by postponing the compilation of methods in C2 that take a long time. Initial experiments suggest this does yield results. Though the improvements have only been marginal so far, further tweaking the set of postponed methods could boost them.

## **Conclusion**

JITWatch4i expands the capabilities of the original JITWatch through a plugin-based integration with IntelliJ IDEA, eliminating source-path setups and speeding up visualization for large projects. The example of IntelliJ IDEA’s startup under different `-XX:TieredOldPercentage` values shows how the JVM balances quick-to-compile L2 code versus slower but highly optimized L4 code. Using the analysis in JITWatch4i, these optimization steps become transparent, allowing you to understand the impact of your settings and ultimately leading to more informed performance tuning and faster startup times.

### **References:**

- JITWatch4i

- JITWatch

Go to Source
