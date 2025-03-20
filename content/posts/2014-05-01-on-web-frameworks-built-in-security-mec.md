---
title: "On web frameworks built-in security mechanisms and common pitfalls"
date: 2014-04-30T22:48:00.001-07:00
draft: false
type: posts
categories: 
- ikki,opensource,web frameworks,web security
---
# On web frameworks built-in security mechanisms and common pitfalls

<br/>

<br/>
Modern web application frameworks are expected to provide built-in security mechanisms against common flaws, such as Cross-Site Request Forgery and injection attacks. Developers can benefit from these protections as they don't need to create ad-hoc defense mechanisms and they can rather focus on building features.

  
Citing the [OWASP Framework Security project](https://www.owasp.org/index.php/OWASP_Framework_Security_Project#tab=Main)  

> The most effective way to bring security capabilities to developers is to have them built into the framework.

  

Although built-in security features have clearly improved web security, using a framework doesn't necessarily guarantee a bullet-proof application. When theory and practice diverge, things can still go wrong:

1.  **Frameworks are not immune to bugs.** They are software. As such, they can be affected by security issues too. Security mechanisms can be bypassed or abused. 
2.  **Poor or inconsistent documentation.** Using appropriate APIs and invoking those calls in the right way is a crucial aspect for leveraging all security mechanisms. Unfortunately, the quality of the documentation doesn't always facilitate the job of developers. 
3.  **Negligence.** Developers still need to read (and understand) the documentation. Building secure software is complicated and requires in-depth understanding of all subtle details.

Although dealing with security issues in production environments is always painful, fixing application framework bugs is even more complicated.  As they usually impact an high number of websites, weaponized exploits are often available in a few hours after the disclosure. On the other side, not all vendors are sufficiently agile to provide a patch. Moreover, the resolution with homegrown fixes may not be trivial. Finally, developers and QA engineers do not necessarily have visibility on the actual code changes, thus they're forced to perform full regression testing to make sure that the application still works as expected.

  

Despite that, **security bugs are generally the most evident problem**. High impact security flaws in common frameworks generate Hacker News threads, flames in security mailing lists and even receive mainstream attention. Good developers and blue teams follow security mailing, vulnerability feeds and vendor announcements. The probability of stepping into an advisory is close to one.

  

On the contrary, **poor documentation and negligence are silent threats**. You won't find as many blog posts or security advisories talking about 'insecure' API usage or misconfigurations.

For instance, everyone in the security community uses the [CVE](http://cve.mitre.org/) acronym, but just few folks know what CCE stands for (btw, it's [Common Configuration Enumeration](https://cve.mitre.org/data/board/archives/2006-08/msg00000.html)).

  

> Since the very first days, the CVE Editorial Board has recognized the need to address both software flaws (aka vulnerabilities) and mis-configurations (aka exposures). The CCE project is logical next step in the evolution of CVE to finally address the 'E' in CVE.

  
To reinforce my point, let's think together about real-life examples for each category:  

1.  **Frameworks are not immune to bugs.** Apache Struts and the countless OGNL expressions code execution bugs ([CVE-2014-0094](http://www.cvedetails.com/cve/CVE-2014-0094/), [CVE-2013-2251](http://www.cvedetails.com/cve/CVE-2013-2251/), [CVE-2013-2135](http://www.cvedetails.com/cve/CVE-2013-2135/), [CVE-2013-2134](http://www.cvedetails.com/cve/CVE-2013-2134/), [CVE-2012-0838](http://www.cvedetails.com/cve/CVE-2012-0838/), ...), Ruby's Action Pack parsing flaw ([CVE-2013-0156](http://www.cvedetails.com/cve/CVE-2013-0156/)), Spring's Expression Language injections  ([CVE-2011-2730](http://www.cvedetails.com/cve/CVE-2011-2730/)), PHP Lavarel [cookie forgery to RCE](https://labs.mwrinfosecurity.com/blog/2014/04/11/laravel-cookie-forgery-decryption-and-rce/), ....and many others. Just a few examples off the top of my head
2.  **Poor or inconsistent documentation.** [Scrypt API](http://vnhacker.blogspot.com/2014/04/fairy-tales-in-password-hashing-with.html) misuse, ... what else?..  [PHP htmlspecialchars](http://docs.php.net/manual/en/function.htmlspecialchars.php)
3.  **Negligence****.** [Ruby Mass Assignment](http://code.tutsplus.com/tutorials/mass-assignment-rails-and-you--net-31695), [Java SecureRandom](http://www.cigital.com/justice-league-blog/2009/08/14/proper-use-of-javas-securerandom/), ...it's getting hard

  

### It's up to us, the community.

Improving application security is not just discovering and fixing security bugs. It's making sure that we have the right foundations and we build secure software on top of that. We need to trust our tools and know how to use them.

  

Collaboration and open-source are crucial aspects to win this game. As Github successfully demonstrated, code collaboration is a fertile ground. Encouraging code review and transparency creates opportunities for developers and the security community to improve code quality and other software development artifacts - including documentation.

  

Inspire your company to contribute back to the open-source projects on which you rely. As a developer, spend time crafting easy-to-use APIs accompanied with clear documentation. If you're a security researcher, don't stop after you discover a bug: submit a patch and help the project to prevent similar issues. Small things that can really make the difference.

#### [Source](http://blog.nibblesec.org/feeds/7826134053376974790/comments/default)

<br/>
---
