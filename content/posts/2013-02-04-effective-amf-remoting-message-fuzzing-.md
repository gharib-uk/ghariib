---
title: "Effective AMF Remoting Message fuzzing with Blazer v03"
date: 2013-02-03T22:56:00.001-08:00
draft: false
type: posts
categories: 
- blazer,burp suite,ikki,java
---
# Effective AMF Remoting Message fuzzing with Blazer v03

<br/>

<br/>
  
After several weeks of extensive testing and debugging, [Blazer v0.3](http://code.google.com/p/blazer/downloads/detail?name=Blazer_v0.3.jar&can=2&q=) is finally out!  
It's been a long ride since the first lines of code, back in 2011. In this post, I am going to present all new features and describe Tips&Tricks to make your AMF security testing even more effective.  
  
If you are not familiar with Blazer, have a look at the project page: [http://code.google.com/p/blazer/](http://code.google.com/p/blazer/).  
New to Burp Suite? Have a look at the [video tutorials](http://portswigger.net/burp/tutorials/) and consider to buy [Instant Burp Suite Starter](http://www.packtpub.com/burp-suite-starter/book).  
  

### What's new?

Blazer v0.3 includes a few interesting new features presented during my [DeepSec talk](http://www.slideshare.net/ikkisoft/amf-testing-made-easy-deepsec-2012), but even more important is the result of extensive testing on Windows, Mac OS X and Linux using multiple Java Runtime Environments and recent Burp Suite releases.  
  

-   **Java classes and source code import feature**  
    In addition to JARs, it is now possible to import directories containing _.class_ and _.java_ files. The ability to import source code, in addition to application libraries, allows to partially use Blazer even during black-box security testing.  
    
-   **AMF request/response export functionality (AMF2XML)**  
    Sharing details of security vulnerabilities triggered by AMF messages was annoying, as it was not possible to export AMF requests and responses in an intelligible format. Using the AMF2XML feature, it is now possible to export those messages in a file or console.  
      
    
    [![](http://i.imgur.com/usvJo.png)](http://i.imgur.com/usvJo.png)
    
      
    
-   **Sandbox feature using a custom security manager**   
    The rationale behind the introduction of this feature is to prevent any malicious action caused by application libraries. Blazer uses Java reflection and fairly complex heuristics to automatically instantiate and populate objects by using the application libraries. Application objects are created on the tester's computer and methods are locally invoked to populate attributes before sending the AMF message to the remote service. As a result, untrusted application libraries may end up writing files, opening network sockets or other involuntary IO operations.  
      
    
    [![](http://i.imgur.com/whqQOre.png)](http://i.imgur.com/whqQOre.png)
    
      
    
-   **Numerous bugs and performance issues fixed**  
    I've fixed more than 20 bugs and multiple performance issues, including an annoying GUI refresh bug on OS X and Windows. This version has been extensively tested on multiple platforms; I've specifically delayed the release to make sure that all issues I've encountered during my testing have been fixed.

###   

### BlackBox vs GrayBox testing with Blazer

Blazer is a security tool for _gray-box testing_. It has been designed and built with the assumption that the application libraries are available to the tester. All Java classes exchanged between client and server should be imported in the tool. This is a realistic assumption if you are doing vulnerability research, not if you are performing a standard pentest.  
  
However, starting from this release, it is actually possible to partially use Blazer during black-box testing. If your application is using primitive types and libraries which can be downloaded from the Internet, you can benefit from Blazer's automatic objects generation by manually crafting a fake _.java_ file including all method signatures:  
  
**1.** Decompile the client-side Flex components (e.g. SWF files) or monitor the network traffic in order to enumerate all remote methods. [Deblaze](http://deblaze-tool.appspot.com/) tool can be used for it. 

  

**2.** Create a _.java_ file containing method signatures as observed in the traffic. Something like the following:  

> package flex.samples.product;  
> public class ProductService{  
> public Product getProduct(int prodId){}  
> } 

**3.** In Blazer, import the crafted Java source file and all application libraries referenced in the application. At this stage, Blazer can be used to automatically generate objects and perform fuzzing.  
  

### Tips & Tricks 

Fuzzing complex applications containing multiple custom classes isn't trivial. To improve coverage and effectiveness, the following recommendations can save you precious time:  
  

-   Always [increment](http://www.portswigger.net/burp/help/suite_gettingstarted.html#launching) [the amount of memory](http://www.portswigger.net/burp/help/suite_gettingstarted.html#launching) that your computer makes available to Burp Suite. If you are generating a large number of AMF messages, consider to chain **two** instances of Burp Suite. The first instance can be used to intercept the application requests and launch Blazer. In Blazer, set the proxy within _tab 3_ to point to the second Burp Suite instance. The latter will collect all requests generated by Blazer. In Burp Suite Pro, you can also set [automatic backups](http://www.portswigger.net/burp/help/options_misc.html#automaticbackup) to prevent any data loss.  
    
  
-   As of Burp Suite v1.5.01, [Burp Extender](http://www.portswigger.net/burp/extender/) has a new API. Blazer has been improved to support both old and new Burp Extender APIs. Standard output and error can be displayed within Burp Extender, to a file or in the console screen. During testing, I suggest to redirect those streams to two separate files in order to record all operations and exceptions.  
    
  
-   Balancing the number of _permutations_, _attack vectors_ and _probability_ is the magic sauce of Blazer. Read the original [whitepaper](http://blazer.googlecode.com/files/BH2012_LucaCarettoni_WP_FINAL.pdf)/[presentation](http://blazer.googlecode.com/files/BH2012_LucaCarettoni_PRESO_FINAL.pdf), make sure to understand those settings and tune the tool. Even better, check the implementation of the _ObjectGenerator_ class.  
    
  
-   _Divide et impera_ by breaking up numerous application method signatures into small groups. Start testing a few methods and make sure that you have imported all required application libraries. Finally, review the server responses and monitor the server's status to detect security vulnerabilities. For example - if you are looking for SQL injections - use Burp's [filter by search term](http://portswigger.net/burp/help/proxy_history.html#filter) to identify AMF messages that triggered visible errors and grep for similar strings in the server logs. Blazer appends a custom HTTP header to all AMF requests that can be used to correlate message and method signature. Also, the newest export functionality can be used to review the AMF payload. 

  
Feel free to [email me](mailto:luca.carettoni@ikkisoft.com) if you have any question.  Also, let me know if you find bugs using Blazer!

#### [Source](http://blog.nibblesec.org/feeds/7842457065653159428/comments/default)

<br/>
---
