---
title: "An Exhaustively-Analyzed IDB for ComRAT v4"
date: Tue, 01 Sep 2020 16:45:00 +0000
draft: false
type: posts
---
# An Exhaustively-Analyzed IDB for ComRAT v4

<br/>

<br/>
This blog entry announces the release of an exhaustive analysis of ComRAT v4. [You can find the IDBs here](https://github.com/RolfRolles/IDBs/tree/master/ComRAT%20v4).

More specifically, an IDB for the sample with hash 0139818441431C72A1935E7F740A1CC458A63452, which was mentioned in the [ESET report](https://www.welivesecurity.com/2020/05/26/agentbtz-comratv4-ten-year-journey/) (see especially its [attached PDF](https://www.welivesecurity.com/wp-content/uploads/2020/05/ESET_Turla_ComRAT.pdf)), and which is available online on [Hybrid Analysis](https://www.hybrid-analysis.com/). All of the analysis has been performed in Hex-Rays 64-bit, so the results will be less interesting to IDA users who do not own Hex-Rays 64-bit. That is to say, if you open the IDB, you should definitely use Hex-Rays to view the function decompilations, as that is where all of the naming and commenting has taken place. It is rich with detail, in comparison to the disassembly listing's barrenness.

This analysis took roughly six weeks of full-time work. I have spent the pandemic working on a new training class on C++ reverse engineering; part of the preparation includes large-scale analysis of C++ programs. As such, ESET's report of ComRAT's use of C++ caught my eye. ComRAT has a beautiful architecture, and many sophisticated components, all of which I believe deserve a detailed report unto themselves. I had begun writing such a report, but decided that it was side-tracking me from my ultimate goals with my new training class. Hence, I had decided to wait until the class was ready, and release a collection of reports on the software architectures of C++ malware families (perhaps as a book) after I was done. Thus, my write-up on ComRAT's architecture will have to wait. You can consider this release, instead, as a supplement to the ESET report.

(Note that if you are interested in the forthcoming C++ training class, it probably will not be available for roughly another year. More generally, remote public classes (where individual students can sign up) are temporarily suspended; remote private classes (multiple students on behalf of the same organization) are currently available. If you would like to be notified when public classes become available, or when the C++ course is ready, please sign up on our [no-spam, very low-volume, course notification mailing list](https://www.msreverseengineering.com/training-classes). (Click the button that says "Provide your email to be notified of public course availability".) )

(Note also that I have more analyses like this waiting to be released. FlawedGrace and XAgent are ready; Kelihos is in progress. If you can provide me with a bundle of Careto SGH samples, preferably Windows 64-bit, please get in touch.)

About the Analysis
==================

This analysis was conducted purely statically, without access to RTTI, or any other form of debug information. The only external information I had was the ESET report. I have reverse engineered every function in the binary that is not part of the C++ standard library, and some of those that are. To get an idea of what the sample looks like before and after analysis, here's a screenshot of the binary freshly loaded into IDA on the left, versus the analyzed one on the right. See if you can spot the difference:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1598940218026-FY0ADHHKQCZR79BCS9MS/Analysis-Side-By-Side.png?format=1000w)

Although I believe that the IDB could probably be loaded in versions of IDA prior to 7.5, I nevertheless recommend using IDA 7.5 to view it. The reason for that is because I have made extensive use of 7.5's new "folders" feature to organize the functions and local types windows, which I found massively useful for large-scale reverse engineering. Those two windows have a nearly identical organization; if you were to dock the windows side-by-side, you would see something like this:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1598940250618-HE8A2YBLUAUTL7GENZWP/Folders-Side-By-Side.png?format=1000w)

As a result of this analysis, I wrote many Hex-Rays plugins, and devised a number of techniques in C++ reverse engineering that were new to me. Eventually, I will publish on topics such as the following:

-   A Hex-Rays plugin for navigating virtual function cross-references
    
-   Reverse engineering STL containers, the easy way
    
-   A Hex-Rays plugin for virtual inheritance
    
-   Tips for reverse engineering multiple inheritance
    
-   Automated creation of VTable structure types
    
-   Automation for detecting inlined functions, and the addition of stock comments
    

ComRAT uses a lot of C++ features; a mostly complete list follows. If you're interested in learning how to reverse engineer C++ programs, you might do well to study how I analyzed the parts of the binary that interact with them.

-   Inheritance
    
-   Polymorphism (virtual functions)
    
-   Custom templates
    
-   Multiple and virtual inheritance (due to iostreams)
    
-   STL, listed in descending order of usage frequency:
    
    -   shared\_ptr<T>
        
    -   vector<T>
        
    -   string
        
    -   wstring
        
    -   locale
        
    -   unique\_ptr<T>
        
    -   wstringstream
        
    -   stringstream
        
    -   fstream
        
    -   list<T>
        
    -   map<K,V>
        
    -   regex
        
    -   wstring\_convert
        
    -   random
        

Notes on the Sample
===================

1.  Although the use of Gmail as a covert channel was a major aspect of the ESET report, I could not get my hands on any samples that had that feature. However, this sample does contain some of the Gmail communication code -- the Gumbo library is compiled into it, and the configuration in the virtual file system contains a "mail" subdirectory, with similar entries to those in the ESET report. Perhaps that feature was still in development, or was deliberately not compiled into my sample for whatever reason.
    
2.  One striking feature of the ESET report was that their sample had RTTI information compiled into it, which provided the names of many of the classes used within ComRAT. I.e., section 4.3 of the ESET report mentions specific class names, as created by the ComRAT programmers. However, my sample had no such RTTI information. Therefore, all of my analysis had to be done from scratch. I used the few names provided in the report as a guide when creating my own.
    
3.  To the extent I was able to verify their claims, everything in the ESET report is accurate. There are a few minor technical details in my sample that were different, but are barely worth mentioning, and might have legitimately changed between the creation of my sample and the non-public one they analyzed.

#### [Source](https://www.msreverseengineering.com/blog/2020/8/31/an-exhaustively-analyzed-idb-for-comrat-v4)

<br/>
---
