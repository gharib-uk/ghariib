---
title: "An Exhaustively Analyzed IDB for ComLook"
date: Tue, 25 Jan 2022 20:37:53 +0000
draft: false
type: posts
---
# An Exhaustively Analyzed IDB for ComLook

<br/>

<br/>
This blog entry announces the release of an exhaustive analysis of ComLook, a newly-discovered malware family about which little information has been published. It was recently discovered by [ClearSky Cyber Security, and announced in a thread on Twitter](https://twitter.com/ClearskySec/status/1484211242474561540). You can find the [IDB for the DLL here](https://github.com/RolfRolles/IDBs/tree/master/ComLook), in which every function has been analyzed, and every data structure has been recovered.

Like the previous two entries in this series on [ComRAT v4](https://www.msreverseengineering.com/blog/2020/8/31/an-exhaustively-analyzed-idb-for-comrat-v4) and [FlawedGrace](https://www.msreverseengineering.com/blog/2021/3/2/an-exhaustively-analyzed-idb-for-flawedgrace), I did this analysis as part of my preparation for an upcoming class on C++ reverse engineering. The analysis took about a one and a half days (done on Friday and Saturday). ComLook is an Outlook plugin that masquerades as Antispam Marisuite v1.7.4 for The Bat!. It is fairly standard as far as remote-access trojans go; it spawns a thread to retrieve messages from a C&C server over IMAP, and processes incoming messages in a loop. Its command vocabulary is limited; it can only read and write files to the victim server, run commands and retrieve the output, and update/retrieve the current configuration (which is saved persistently in the registry). See the IDB for complete details.

(Note that if you are interested in the forthcoming C++ training class, it is nearing completion, and should be available in Q2 2022. More generally, remote public classes (where individual students can sign up) are temporarily suspended; remote private classes (multiple students on behalf of the same organization) are currently available. If you would like to be notified when public classes become available, or when the C++ course is ready, please sign up on our [no-spam, very low-volume, course notification mailing list](https://www.msreverseengineering.com/training-classes). (Click the button that says "Provide your email to be notified of public course availability".) )

This analysis was performed with IDA Pro 7.7 and Hex-Rays 32-bit. All analysis has been done in Hex-Rays; go there for all the gory details, and don't expect much from the disassembly listing. All of the programmer-created data structures have been recovered and applied to the proper Hex-Rays variables. The functionality has been organized into folders, as in the following screenshot:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/631bb955-0147-4203-9d35-336b8f256bf8/ComLook-folders.png?format=1000w)

The binary was compiled with MSVC 10.0 with RTTI, and uses standard C++ template containers:

-   string/wstring
    
-   shared\_ptr
    
-   vector
    
-   list
    
-   map
    

The primary difficulty in analyzing this sample was that it was compiled in debug mode. Although this does simplify some parts of the analysis (e.g., error message contain the raw STL typenames), it also slows the speed of comprehension due to a lack of inlining, and includes a huge amount of code to validate so-called C++ debug iterators. This makes locating the real programmer-written functionality more time-consuming. STL functions involving iterators that, in a release build, would have consumed less than a page of decompilation, often consumed five or more pages due to debug iterator validation.

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/a772f765-cafa-48ac-93fa-f38c862b4724/ComLook-debug-iterators.png?format=1000w)

#### [Source](https://www.msreverseengineering.com/blog/2022/1/25/an-exhaustively-analyzed-idb-for-comlook)

<br/>
---
