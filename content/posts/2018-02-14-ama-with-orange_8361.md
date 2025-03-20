---
title: "AMA with orange_8361"
date: 2018-02-14T01:00:00+01:00
draft: false
type: posts
categories: 
- 
---
# AMA with orange_8361

<br/>

<br/>
### Thank you for doing this interview! Can you please introduce yourself?

Hi, my name is Orange Tsai, come from Taiwan. I am 25 years old now. The first time I started to learn security is about 16.

In web security, I love server-side vulnerabilities more than client-side. To take control of a server is more fun for me. So, I love Remote Code Execution in particular. I have reported RCE to several vendors, such as Facebook, GitHub, Apple, Uber, Yahoo and Imgur.

Even so, I still love to study all techniques no matter it is in server-side or client-side, in web or binary. For example, I also found XSS on Google, Onavo(Facebook), Apple and reported a Internet Explorer memory corruption RCE to Microsoft.

If you want to know more about me, here is my blog and twitter :)

-   [blog.orange.tw](http://blog.orange.tw)
    
-   [@orange\_8361](https://twitter.com/orange_8361)
    

### Q: How do you manage your personal life, work, and bug bounties? Do you do bug bounties as a job or a hobby?

I like to do research more. Bug Bounty is just a hobby to me. I usually do bug hunting when I get stuck in some research(You know, not every research you can get a result), or found something cool(like weird features, weird designs that may lead to security problems) and I will try to find is there any Bug Bounty Program which I can leverage.

For example, I was interested in Java Web Framework few months ago. When I traced the root cause of CVE-2013-3827. I noticed that the vulnerability can be easily found by Google Hacking. So I started hunting for vulnerable vendors and found some juicy information Apple.

### Q: How much time do you spend on Hunting for Bugs? On average, how many bugs do you think you report per month?

Not often. I usually hunt bugs when I found something interesting!

For example, you can see the report about [How I found SQL Injection on Uber](https://hackerone.com/reports/150156).

It’s just a vacation trip, but…

### Q: How long did it take you until you found your first significant/high impact/payout vulnerability?

Hmmm…

I think the first high impact bug is [RCE in b.login.yahoo.com](http://blog.orange.tw/2013/11/yahoo-bug-bounty-part-2-loginyahoocom.html) (Sorry it’s only in Chinese)

At the first, I just saw the [news](https://www.htbridge.com/news/what_s_your_email_security_worth_12_dollars_and_50_cents_according_to_yahoo.html) and knew Yahoo is going to launch a Bug Bounty Program.

In that time, I also noticed the Struts2 vulnerabilities. I think I am lucky because I just use the dork **“site:yahoo.com ext:do”** and find the vulnerable site quickly.

I spent whole the night to bypass the WAF and wrote a working OGNL PoC to get the RCE and bounty :P

### Q: Of all the bugs you’ve found, what was your favorite/most interesting?

Oops, it’s very hard to make a choice. Could I choose two? :P

The most interesting:

[How I Hacked Facebook, and Found Someone’s Backdoor Script](http://blog.orange.tw/2016/04/bug-bounty-how-i-hacked-facebook-and-found-someones-backdoor-script.html)

The most favorite:

[How I Chained 4 vulnerabilities on GitHub Enterprise, From SSRF Execution Chain to RCE!](http://blog.orange.tw/2017/07/how-i-chained-4-vulnerabilities-on.html)

### Q: When and how did you have your breakthrough? When did you realize hacking and bug bounties was something you wanted to dedicate your time to? Please share your insights and the problems you faced to becoming an established bug bounty hacker?

Give yourself a target, and reach it! You need pressures

For example:

While I was learning security. I usually give myself a few months to accomplish a small aim, such as, be the top 10 in a [wargame](https://en.wikipedia.org/wiki/Wargame_\(hacking\)) site or hack into some famous sites.

In order to reach the target, You will try to learn everything about the target. Search all the resource on the Internet, try all the possible method that can solve the challenge, use whole the month to achieve the target.

### Q: What do you do to keep up with all the new trends?

I keep all thre trends by RSS. I subscribed several security blogs, forums and mailing lists on [feedly.com](feedly.com)

I also keep trends by social networks:

-   The hackers in China love to use [Weibo](http://weibo.com/), [ZhiHu](https://www.zhihu.com/) and a private study group [XiaoMiQuan](https://www.xiaomiquan.com/)
    
-   The hackers in U.S., Europe and other places love to use [Twitter](https://twitter.com/)
    
-   [GitHub](http://github.com/), I followed some hackers in GitHub and see what projects they Stared and Forked
    

### Q: Do you collaborate with other hackers? Can you name a few?

No, always alone :(

Technical Questions
===================

### Q: How do you approach a target? What is your routine like? What is your recon process like? What kind of information do you seek in your information gathering process? And how does this information help you?

I think most of the recon process are the same. FuzzDB, DirBuster, NMAP, Google Hacking…etc.

I usually found something interesting in:

-   pastebin.org
-   github.com
-   gist.github.com
-   google.com
-   netcraft.com - Domain / IP history
-   …

About the DNS Re-con

-   Big Data(I love Big Data :P)
    
    -   [Internet Census 2012](http://census2012.sourceforge.net/)
    -   [scans.io](https://scans.io/)
    -   [fofa.io](https://fofa.so/)
    -   [Zoomeye.org](http://zoomeye.org/)
    -   [Shodan.io](http://shodan.io/)
-   Whois
    
-   Reverse Whois
    
    -   [Benmi](https://www.benmi.com/)
    -   [DomainTools](https://www.domaintools.com/)
-   DNS bruteforcing
    
    -   I wrote a Python scripts by myself
-   SSL Certifications
    
    -   [Censys](https://censys.io/)
    -   [crt.sh](https://crt.sh/)
    -   Certificate Transparency - [Facebook](https://developers.facebook.com/tools/ct)
    -   Certificate Transparency - [Google](https://www.google.com/transparencyreport/https/ct/)
-   Reverse IP range
    
    -   dig
    -   [Robtex](https://www.robtex.com/)
    -   [You get signal](http://www.yougetsignal.com/tools/web-sites-on-web-server/)
    -   [Bing ip](https://www.bing.com/)
-   Passive DNS
    
    -   [VirusTotal](https://www.virustotal.com/)
    -   [PassiveTotal](https://www.passivetotal.org/)
    -   [DNSDB](https://www.dnsdb.info/)
    -   [sitedossier](http://www.sitedossier.com/)

hmmm… It is hard to list all. I don’t usually use them. But, maybe useful for you :)

### Q: Do you always look for all vulnerabilities types when you approach a website?

Yes, always.

Bugs can be anywhere. So, don’t miss any opportunity.

Look for as many vulnerabilities as possible!

### Q: Do you use any tools? Do you have your own tools that you have written to automate/facilitate your work? What Burp extensions do you use? Is there a tool that not a lot of people use that you think they should?

Tools:

I don’t often use tools. I think Firefox, BurpSuite, Google, Python, NMAP and a Linux can do everything.

But this is AMA. I need to write something. So I open my working directory and list some of my tools. However, most of tools are for the POST-Exploitation… haha. I think you still can check them:

-   [NBTool / DNSLogger](https://wiki.skullsecurity.org/Nbtool)
-   [Tiny-Shell](https://github.com/orangetw/tsh)
-   [Mimikatz](https://github.com/gentilkiwi/mimikatz)
-   [Caidao](https://github.com/tennc/webshell/tree/master/caidao-shell) (It’s a interface that you can just upload a one-line webshell to website, and you can control the server with this tool)
-   [NaviCat](https://www.navicat.com/en/) (Easy to manage MySQL, SQL Server, Oracle)
-   [ProxyChains](http://proxychains.sourceforge.net/) (It’s useful with SSH -D. In Window, I use [Proxifier](https://www.proxifier.com/))
-   [xss.swf](https://github.com/evilcos/xss.swf)

I also wrote some scripts and maintained a custom dictionary to help me scan the DNS and sensitive files and directories.

Burp Extensions:

-   Active Scan++
-   J2EE Scan
-   Backslash Powered Scanner
-   HTTPoxy Scanner
-   Java Deserialization Scanner
-   Retrie.js

### Q: This is one of our most popular questions: How do you test for Server Side vulnerabilities such as RCE, SQLi, etc?

SQLi:

-   page=1‘
-   page=1‘’
-   page=1%cc‘
-   page=1’-sleep(10)-’
-   page=1"
-   page=1"“
-   page=1%cc”
-   page=1"-sleep(10)-”
-   page=1/1
-   page=1/0
-   page=1/sleep(10)
-   page=1\\
-   …

RCE:

-   I usually use “sleep 10” as my payload
-   page=1\`sleep 10
-   page=1|sleep 10
-   page=1$(sleep 10)
-   page=1%0asleep 10%0a
-   …

LFI:

-   page=1
-   page=./1
-   page=.//1
-   page=././1
-   page=../pages/1
-   page=\\1
-   …

I think finding server-side vulnerabilities is not about payloads. It depended on how you process the context, how you find the TINY detail that no one notice on each request.

However, the most important thing is, learned all the tricks, techniques and skills as much as possible. Whatever the skill, trick and technique in Web Security or not. It will open your mind!

### Q: Do you think being a pentester, web developer, or being in a related field, helps you with bug bounties? Where should they start?

I believe that hacking is an art. Information Security is not just a domain of Computer Science. It’s a combination of multiple domains. So I think learning other skills is helpful to you.

For example:

When you are facing some websites which are using CGI. You need the skill of Reverse Engineering and Binary Exploitation.

Also, some techniques in Cryptography are also good to you. Padding Oracle, Length Extension Attack, Bit-Flipping Attack, more and more… These are really possible be found in real web applications!

Finally, read CVEs, vulnerabilities and technique reports as much as possible. Oh, don’t just read. You need to analyse, know the root cause and if there is a PoC or Exploit, try to implement by yourself. The devil is in the details.

Time to wrap it up!
===================

### Q: What kind of music do you listen to?

C-pop. Rene Liu, Lala Hsu

### Q: What do you do when you aren’t hacking?

Surf on the Internet.

### Q: What kind of impact/role have bug bounties played in your life?

I think Bug Bounty is a way that keep me motivated.

When you find cool bugs and get listed in Hall of fames for example it gives you confidence and that results in more motivations which drives you to keep learning and dig deeper. This is a good circle!

### Q: What is an advice you received as a beginner that helped you with your bug bounty career?

Just play for fun. The money is not important. Learning something new is more important than the money!

### Q: What is one area of hacking (web, mobile, hardware, etc) you wish you knew more about / plan on focusing your learning on?

Advanced binary exploitation. Although I can do binary pwning stuffs now. It’s still not enough. I need to learn more techniques and get more experiences on large applications.

### Q: If someone with basic technical background asked you, “where should I start?”, what are 3 things you would recommend they do before diving into bug bounties?

1.  Find a good target
2.  Don’t be frustrated if nothing found
3.  Hack to Learn

There are some resources that you can read. For example:

-   [Bug Bounty Reference](https://github.com/ngalongc/bug-bounty-reference/) by [@ngalongc](https://twitter.com/ngalongc)
-   [The Bug Hunters Methodology](https://github.com/jhaddix/tbhm) by [@jhaddix](https://twitter.com/jhaddix)
-   [XSSChallengeWiki](https://github.com/cure53/XSSChallengeWiki/wiki) by [cure53](https://github.com/cure53)
-   [HTML5 Security Cheatsheet](https://github.com/cure53/H5SC) by [cure53](https://github.com/cure53)
-   [Awesome Penetration Testing](https://github.com/enaqx/awesome-pentest) by [enaqx](https://github.com/enaqx/)

CTF is also a good way to learn web tricks (I know CTF becomes more harder and harder nowadays. But you still can participate some CTF for beginners, like [CSAW CTF](https://ctf.csaw.io) or [PicoCTF](https://picoctf.com/))

I have open-sourced all my CTF Web challenges on [My-CTF-Web-Challenges](https://github.com/orangetw/My-CTF-Web-Challenges). There are lots of web tricks you can check :P

### Q: Someone was eager to know, what do you put on your toast?

🍊

### Q: What’s your worst bug bounty story/experience?

I found a clever XSS in a popular product (The XSS uses an interesting feature in Struts2 and I think not so much people know that) and reported to the vendor. But the vendor only gave me a $20 coupon in their online shop store.

The most sadness is, the hoodie I like in their online store is 50 dollars…

### Q: If you had to pick one hacker to collaborate with, who would it be?

I want to pick TWO!

@filedescriptor @pwntester

### Q: What’s your favorite text editor?

Sublime Text

#### [Source](/blog/ama/orange/)

<br/>
---
