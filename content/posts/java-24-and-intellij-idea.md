---
title: "Java 24 and IntelliJ IDEA"
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

IntelliJ IDEA has supported Java 24 since an earlier release, with more enhancements being added in the later releases! I’m often asked, “What’s the best feature of Java 24?” My answer? Why pick just one? 🙂 Java 24 continues to enhance the language with improvements like Simple source files and instance main methods, Primitive Types \[…\]

IntelliJ IDEA has supported Java 24 since an earlier release, with more enhancements being added in the later releases!

I’m often asked, “What’s the best feature of Java 24?” My answer? Why pick just one? 🙂

Java 24 continues to enhance the language with improvements like Simple source files and instance main methods, Primitive Types in Patterns, instanceof, and switch, Module Import Declarations, Flexible constructor bodies, and API enhancements such as Stream Gatherers – plus many more.

One of Java’s greatest strengths is its predictable six-month release cadence, allowing developers to explore both production and preview features regularly. However, keeping up with new features every six months can feel overwhelming. In this blog post, I’ll walk you through a few key Java 24 features – what they are, how you can use them, and how IntelliJ IDEA supports them. But first, let’s make sure you have the right setup in place.

## Simple source files and instance main methods (JEP 495)

Implicit classes and import statements, shorter `main()` method, ‘`println()`’, to output values – make it easier for beginners to get started in Java. If you are an experienced developer, these features can help you create scripts, games, and utilities using fewer lines of code.

If you are new to this feature, I’d recommend you to check out my introductory blog post – Java 24: ‘HelloWorld’ and ‘main()’ meet minimalistic and detailed blog post – Java 24: Build Games, Prototypes, Utilities, and More –With Less Boilerplate, that talks about its practical use cases. This feature can not only help new developers get started with writing programs like simple calculations, printing patterns, enjoy joy of programming by creating simple console and gui based games, but also help create utilities for experienced developers such as processing files, or access web resources.

In this section, I’ll focus on IntelliJ IDEA’s support for this Java feature.

### Create a simple source file with instance main method

When you use IntelliJ IDEA to create and run your simple files, you could run it like any other executable class (saves you from specifying the compilation or run time command line parameters that you must use otherwise). If you miss setting the language level to 24, IntelliJ IDEA can detect that and prompt you to do so (as shown below):

![](https://blog.jetbrains.com/wp-content/uploads/2025/02/new-proj.gif)

### Changing an implicit class to a regular class

When you are ready to level up and work with other concepts like user defined classes, you might want to convert your implicit classes to a regular class. You can use context action ‘_Convert implicitly declared class into regular class_‘, as shown below (this action will add relevant import statements):

![](https://blog.jetbrains.com/wp-content/uploads/2025/02/convert-to-reg-class.gif)

### Converting a regular class to an implicit class

Sometimes a packaged class might be better off as an implicit class because it might not be using the concepts meant for a regular class. If so, you could do that by using the action ‘_Convert into implicitly declared class_’ (as shown below). During the conversion, IntelliJ IDEA will remove the import statements that are no longer required:

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/explicit-to-implicit.gif)

### Behind the scenes – implicit class with instance method main()

Behind the scenes, the Java compiler creates an implicit top level class, with a no-argument constructor, so that these classes don’t need to be treated in a way that is different to the regular classes.

Here’s a gif that shows a decompiled class for you (via Decompiler feature in IntelliJ IDEA) for the source code file _AnimateText.java_:

![](https://blog.jetbrains.com/wp-content/uploads/2024/02/decompile.gif)

### Interacting with console – println() vs. System.out.println() calls

To make it simpler for new developers to interact with the console, that is, output messages to console and to read input from it, a new class – `java.io.IO` was created in Java 23. It contains just a handful of overloaded `readln()` and `println()` methods (as shown below):

![](https://blog.jetbrains.com/wp-content/uploads/2025/02/IO-class-struc.png)

Class `java.io.IO` is automatically imported in implicit classes. So, instead of using `System.out.println()`, you can now use `println()` to output messages to a console (and readln() to read from console). Interestingly, `println()` was added to this class in Java 24.

### Overloaded main method in the implicit class

When you overload `main()` method in an implicit class, there would be an order of preference to be considered ‘the’ `main()` method. All of these following methods are valid signatures of `main()` methods in an implicit class:

```
public static void main(String args[]) {}
public void main(String args[]) {}
public static void main() {}
static void main() {}
public void main() {}
void main() {}

```

When you have overloaded `main()` method in your implicit class, IntelliJ IDEA would show the run icon next to the correct or preferred ‘main’ method:

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/which-main.gif)

### Missing main method in an implicit class

If there is no valid main method detected in an implicit class, IntelliJ IDEA could add one for you, as shown in the following gif:

![](https://blog.jetbrains.com/wp-content/uploads/2024/02/create-a-main-method.gif)

## Primitive Types in Patterns, instanceof, and switch (Preview Feature)

Currently in its second preview, the feature Primitive Types in Patterns, instanceof, and switch enhances Java’s pattern matching capabilities by incorporating primitive types across all pattern. This allows you to use primitive type patterns directly with instanceof and switch expressions (which was earlier restricted to objects), streamlining code and reducing the need for manual type conversions.

### A quick example

This feature enables you to use primitive types in switch expressions with guard patterns, as follows:

```
public String getHTTPCodeDesc(int code) {
   return switch(code) {
       case 100 -> "Continue";
       case 200 -> "OK";
       case 301 -> "Moved Permanently";
       case 302 -> "Found";
       case 400 -> "Bad Request";
       case 500 -> "Internal Server Error";
       case 502 -> "Bad Gateway";
       case int i when i > 100 && i < 200 -> "Informational";
       case int i when i > 200 && i < 300 -> "Successful";
       case int i when i > 302 && i < 400 -> "Redirection";
       case int i when i > 400 && i < 500 -> "Client Error";
       case int i when i > 502 && i < 600 -> "Server Error";
       default                            -> "Unknown error";
   };
}

```

Similarly, you could use primitives with `instanceof` operator. 

This feature is being previewed again without any change. I covered this feature together with how IntelliJ IDEA supports it in my previous blog post – Java 23 and IntelliJ IDEA. I’d suggest you check it out for details. This blog post answers questions, such as what does it mean to add primitive types to Pattern Matching, to multiple examples and robust data flow analysis in IntelliJ IDEA.

### Interview with creators of this feature

We also interviewed the owner of this feature, Aggelos Biboudis, Principal Member Technical Staff, Oracle, Brian Goetz, Java language Architect with Oracle, Tagir Valeev, Java Team Technical Lead, JetBrains.

<iframe loading="lazy" title="JEP Explained. JEP 455: Primitive Types in Patterns, instanceof, and switch" width="500" height="281" src="https://www.youtube.com/embed/tqBV4MZ-qSM?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Check it out to find out the bigger picture of why primitive data types are being added to the Java language and the finer details of the changes they propose. 

## Module Import Declarations

In its second preview, Module Import Declarations enables you to import all packages exported by a module in a single declaration. It simplifies the reuse of modular libraries without requiring the importing code to be modularized itself. For example, the declaration `import module java.base;` imports all public top-level classes and interfaces from packages exported by the java.base module, eliminating the need for multiple individual import statements. This improves readability of code, especially when working with extensive APIs.

### A quick example

Imagine your code includes multiple import statements, such as the following:

```
import java.io.*;
import java.util.HashMap;
import java.util.Map;
import java.lang.reflect.*;
import java.nio.*;
```

These could be replaced by an import module statement, as follows:

```
import java.base.*;
```

### Which packages are exported by the module java.base (or other modules)?

It is simple to answer this question when you are using IntelliJ IDEA. Click on the module name in the editor or use the relevant shortcut (Go to Declaration or Usages) and you could view the definition of this module to find out all the modules exported by this module. This is shown in the following gif:

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/java-base-mod.gif)

### Interview with creators of this feature

We also interviewed the owner of this feature, GavinBierman, Programming Language Designer at Oracle.

<iframe loading="lazy" title="JEP Explained. JEP 476: Module Import Declarations" width="500" height="281" src="https://www.youtube.com/embed/mSYA3cZ5o6c?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Gavin covered the differences between single-type import and type-import-on-demand declarations, explaining what they are and why individuals and organizations prefer one style over the other. He also talked about how the “Module Import Declarations” feature automatically imports on demand from transitive dependencies of modules. He covered ambiguous imports and how to deal with them, name ambiguity, and how to submit relevant feedback on this feature to the teams at OpenJDK. 

## Flexible Constructor bodies

In its third preview, this feature is useful when a superclass calls a method from its constructor and you want to override this method in a subclass and want to access a field from the subclass inside this method. Previously, the subclass field would not be initialized when the method was called from the superclass constructor. Now it is possible to initialize the field and prevent surprises. Here’s an example code to show this feature:

```
abstract class Action {
   public Action() {
       System.out.println("performing " + getText());
   }
   public abstract String getText();
}
class DoubleAction extends Action {
   private final String text;
   private DoubleAction(String text) {
       this.text = text; // this did not compile before Java 23 with preview features enabled.
       super();
   }
   @Override public String getText() {
       return text + text;
   }
}

```

If you are new to this feature, don’t miss checking out my detailed blog post on this feature, Constructor Makeover in Java 22 | The IntelliJ IDEA Blog, which talks about the why and how of this feature.

## Preview Features

The features covered in this blog post are preview and not production features. With Java’s new release cadence of six months, new language features are released as preview features. They may be reintroduced in later Java versions in the second or more preview, with or without changes. Once they are stable enough, they may be added to Java as a standard language feature.

Preview language features are complete but not permanent, which essentially means that these features are ready to be used by developers, although their finer details could change in future Java releases depending on developer feedback. Unlike an API, language features can’t be deprecated in the future. So, if you have feedback about any of the preview language features, feel free to share it on the JDK mailing list (free registration required).

Because of how these features work, IntelliJ IDEA is committed to only supporting preview features for the current JDK. Preview language features can change across Java versions, until they are dropped or added as a standard language feature. Code that uses a preview language feature from an older release of the Java SE Platform might not compile or run on a newer release.

## Summary

Java 24 introduces key enhancements like Simple Source Files, Primitive Type Patterns, Module Import Declarations, and Flexible Constructor Bodies. IntelliJ IDEA has supported Java 24 since an earlier release, with more enhancements being added in the later releases!

With intelligent assistance, IntelliJ IDEA simplifies adopting Java 24’s features by offering smart code suggestions, seamless conversions, and robust analysis. Its support ensures a smooth development experience as Java evolves.

Go to Source
