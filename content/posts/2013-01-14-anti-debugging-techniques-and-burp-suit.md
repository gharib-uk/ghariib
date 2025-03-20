---
title: "Anti-debugging techniques and Burp Suite"
date: 2013-01-13T22:15:00.000-08:00
draft: false
type: posts
categories: 
- burp suite,debugging,ikki,java
---
# Anti-debugging techniques and Burp Suite

<br/>

<br/>
### Incipit

No matter how good a Java obfuscator is, the bytecode can still be analyzed and partially decompiled. Also, using a debugger, it is possible to dynamically observe the application behavior at runtime making reverse engineering much easier. For this reason, developers often use routines to programmatically detect the execution under a debugger in order to prevent easy access to application's internals. Unfortunately, these techniques can be also extremely annoying for people with good intents.

### Burp Suite

Over the course of the years, starting from the very first release, I have been an enthusiastic supporter of [Burp Suite](http://portswigger.net/). Not only [@PortSwigger](https://twitter.com/PortSwigger) was able to create an amazing tool, but he also built a strong community that welcome each release as a big event. He has also been friendly and open to receive feedback from us, ready to implement suggested features. Hopefully, he won't change his attitude now.

  

Since a few releases, both **Burp Suite Free** and **Pro** cannot be executed under a debugger. Unfortunately, this is a severe limitation - especially considering the latest [Extensibility API](http://blog.portswigger.net/2012/12/new-burp-suite-extensibility.html).  The new extensibility framework is a game-changer: it is now possible to fully integrate custom extensions in our favorite tool. But, how to properly debug extensions in an IDE? Troubleshooting fairly complex extensions (e.g. [Blazer](http://code.google.com/p/blazer/)) requires lot of debugging. Setting breakpoints, stepping in and out of methods, ... are must-have operations.

  

Inspired by necessity, I spent a few hours to review the anti-debugging mechanism used in Burp Suite Free. According to [Burp's EULA (Free Edition)](http://portswigger.net/burp/eula-free.html), reversing does not seem to be illegal as long as it is "_essential for the purpose of achieving inter-operability_". Not to facilitate any illegal activity, this post will discuss details related to the Free edition only.  

**Disclaimer**: Don't be a fool, be cool. If you use Burp Pro, you **must** have a valid license.

### Automatic detection of a debugger

In Java, it is possible to enable remote debugging with the following options:  
  
**\-Xdebug -agentlib:jdwp=transport=dt\_socket,server=y,address=8000,suspend=n**   
  
and attach a debugger with:  
  
 **jdb --attach \[host\]:8000**  
  

A common technique to programmatically understand if a program is running under a debugger involves checking the input arguments passed to the Java Virtual Machine_._ The following is the pseudo-code of a very common technique:

>  for(ManagementFactory.getRuntimeMXBean().getInputArguments() ...){  
>                 if(Argument.contains("-Xdebug") || Argument.contains("-agentlib") ...){  
>                    // Do something annoying for the user  
>             }

In practice, _ManagementFactory_ returns the managed bean for the runtime system of the current Java Virtual Machine that can be used to retrieve the execution arguments (see [RuntimeMXBean API](http://docs.oracle.com/javase/1.5.0/docs/api/java/lang/management/RuntimeMXBean.html) for further details). In case of Burp Free, the application gets shutdown via a _System.exit(0)_;

### Bypass techniques, an incomplete list

First of all, it is always possible to attach the debugger once the Java process is already up and running. Any check performed during the application startup won't block the execution:   

  
**jdb -connect sun.jvm.hotspot.jdi.SAPIDAttachingConnector:pid=\[Process ID\]**  

  

Unfortunately, this is a _read-only_ mechanism and cannot be used within traditional IDEs. A few better solutions require tweaking the application in order to modify the program execution. This can be achieved via static changes in the _.class_ files or using static/dynamic bytecode instrumentation. The code above is pretty simple and can be bypassed in several ways:

-   Using [ClassEditor](http://classeditor.sourceforge.net/), [reJ](http://rejava.sourceforge.net/) or any other tool that allow _.class_ manipulation, it is just necessary to identify all strings in the constant pool used during the string comparison within the _if-statement_. For instance, you could replace all strings with a bunch of "a" so that the program won't even enter in the _if-statement_ body

[![](http://2.bp.blogspot.com/-9odwozd7xow/UOEo39CttSI/AAAAAAAAC94/ridx3R_yv9E/s1600/Screenshot-3.png)](http://2.bp.blogspot.com/-9odwozd7xow/UOEo39CttSI/AAAAAAAAC94/ridx3R_yv9E/s1600/Screenshot-3.png)

Manually changing the Constant Pool of a .class file

  

  

  

-    An even more portable solution, especially when strings obfuscation is used, consist of editing the bytecode using [JavaAssist](http://www.csg.ci.i.u-tokyo.ac.jp/~chiba/javassist/) or similar libraries. This allows to write a piece of code that search a class and patch it:

-   For instance, we could force the _getInputArguments()_ to return an empt_y List;_  
    
-   Or, we could insert an arbitrary unconditional jump _jsr_ to skip the program shutdown;
-   Or again, it is possible to override the _System.exit()_ method with a local method using an empty body. First, we need to create a fake _static exit(int)_ method. Then, we replace _System.exit()_ with the custom method within our class.

[![](http://3.bp.blogspot.com/-3sqlNqS-3MI/UOEr7nBtEJI/AAAAAAAAC-I/9vDtVYEi39g/s1600/Screenshot-4.png)](http://3.bp.blogspot.com/-3sqlNqS-3MI/UOEr7nBtEJI/AAAAAAAAC-I/9vDtVYEi39g/s1600/Screenshot-4.png)

Using JavaAssist to replace an existent method within a Class

### Patching Burp Free for debugging your custom extensions

With the honest intent to simplify the life of coders writing custom Burp's extensions, I have developed a small utility (**_BurpPatchMe_**) to patch your own copy of Burp Free - which will allow you to debug your code in NetBeans, Eclipse, etc.

[![](http://4.bp.blogspot.com/-kQ8d6eE06xI/UOE4YXjclcI/AAAAAAAAC-o/0OZ0Whb4sSY/s1600/Screenshot-2.png)](http://4.bp.blogspot.com/-kQ8d6eE06xI/UOE4YXjclcI/AAAAAAAAC-o/0OZ0Whb4sSY/s1600/Screenshot-2.png)

BurpPatchMe

A few important details:

-   _BurpPatchMe_ works for Burp Suite Free only. I have included a specific check for it as well as I have used a technique compatible with that release only. Again, you won't be able to remove debugging in Burp Suite Pro using this tool. Go and [buy your own copy](https://pro.portswigger.net/buy/) of this amazing tool!
-   _BurpPatchMe_ is compiled without debugging info and it has been obfuscated too. A quick skiddie prevention mechanism to avoid abuses
-   _BurpPatchMe_ does not contain any Burp's code, library or resource. It is your own responsability to accept the EULA agreement and its conditions, before downloading Burp Free. Also, this tool is provided as it is - please do not send emails/comments asking for "features"
-   Java JDK is required in order to use this tool. All other dependencies are included within the jar

You can download [BurpPatchMe](http://www.ikkisoft.com/stuff/BurpPatchMe.jar) here and launch it with:  

> $ java -jar BurpPatchMe.jar -file burpsuite\_free\_v1.5.jar   

 Long life Burp Suite and happy extensions!

#### [Source](http://blog.nibblesec.org/feeds/2957826270869684484/comments/default)

<br/>
---
