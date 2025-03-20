---
title: "AMA with arneswinnen"
date: 2017-09-25T02:00:00+02:00
draft: false
type: posts
categories: 
- 
---
# AMA with arneswinnen

<br/>

<br/>
### Thank you for doing this interview! Can you please introduce yourself?

I’m Arne Swinnen from Belgium, 28 years old. I started hacking with the CTF-team of my University KU Leuven around 10 years ago. My bug bounty endeavours began in the summer of 2015, namely Instagram (Facebook bug bounty program). Currently, I work on bug bounty platforms HackerOne, Bugcrowd, Intigriti, Zerocopter and Bounty Factory, besides the well-established “standalone” programs from Facebook, Google, Paypal and some others.

### Q: How do you manage your personal life, work, and bug bounties? Do you do bug bounties as a job or a hobby?

I’m doing bug bounty hunting full-time since August 2016, so for a good year now. The year before I was still a full-time employee, so I only dedicated evenings, weekends and holidays to bug hunting back then.

### Q: How much time do you spend on Hunting for Bugs? On average, how many bugs do you think you report per month?

I roughly spend between 40-50 hours a week on bug bounty hunting and reading the latest write-ups, 10 out of 12 months a year. I could certainly spend even more time on bug hunting, but I want to avoid burning out like some other hunters have experienced.

I don’t report an overly lot of bugs, I have an average of 15-20 bugs per month. I do tend to focus mostly on server-side issues, which are somewhat less prevalent than client-side ones. I’m also allergic to informative and N/A, so I only report issues of which I’m 99% sure the company will appreciate and resolve it. Issues of which I’m not 99% sure are set aside to combine in a chained PoC with higher impact to ensure they are accepted (e.g. open redirect & login CSRF in [Authentication bypass on Airbnb via OAuth tokens theft](https://www.arneswinnen.net/2017/06/authentication-bypass-on-airbnb-via-oauth-tokens-theft/)).

### Q: How long did it take you until you found your first significant/high impact/payout vulnerability?

It did take quite some time for me to find my first high impact vulnerability on Instagram, I would say roughly four weeks of effort. Usually I do a couple of “sweeps” on a bug bounty program, and it was only during the second and third sweep on Instagram that I located several [Authentication Brute-Force vulnerabilities](https://www.arneswinnen.net/2016/05/instabrute-two-ways-to-brute-force-instagram-account-credentials/) and an [Account Takeover vulnerability without user interaction](https://www.arneswinnen.net/2016/03/how-i-could-compromise-4-locked-instagram-accounts/).

### Q: Of all the bugs you’ve found, what was your favorite/most interesting?

Personal favorite: [How I Could Steal Money from Instagram, Google and Microsoft](https://www.arneswinnen.net/2016/07/how-i-could-steal-money-from-instagram-google-and-microsoft/) Most technically interesting: [Authentication bypass on Uber’s Single Sign-On via subdomain takeover](https://www.arneswinnen.net/2017/06/authentication-bypass-on-ubers-sso-via-subdomain-takeover/) Most impact: - RCE via PHP Object Injection (thanks [Ambionics](https://www.ambionics.io/blog/php-generic-gadget-chains)) on private H1 program - Race condition via Burp Intruder (thanks [Josip Franjković](https://www.ambionics.io/blog/php-generic-gadget-chains)) on private H1 program

### Q: When and how did you have your breakthrough? When did you realize hacking and bug bounties was something you wanted to dedicate your time to? Please share your insights and the problems you faced to becoming an established bug bounty hacker?

As mentioned before, I CTF’ed a lot during my time at University, which triggered my interest in hacking. After I graduated I worked as a penetration tester for four years. During this time I noticed that I could really enjoy hunting bugs continuously, whereas some of my colleagues got bored after hunting for a while and took another career path.

As for most bug bounty hunters, the beginning was the hardest. I picked out Instagram since I didn’t find too many write-ups about it online at the time, and I really gave it my all, given my penetration testing background. This eventually yielded 10+ different bugs, after which the bug bounty story really started for me and I discovered the bounty platforms.

### Q: What do you do to keep up with all the new trends?

I’m always on the lookout for new write-ups in the #blogs BBF slack channel and on Twitter. Besides, I regularly visit reddit.com/r/netsec to keep up with general IT Security news.

### Q: Do you collaborate with other hackers? Can you name a few?

I personally haven’t collaborated much with other hackers up until now. Belgian bug hunters @intidc, @preben\_ve, @jeromekleinen and myself did have a test run for #teambelgium during the latest H1-702 event with promising results, so I’m looking forward to collaborate more with other hackers in the future.

Technical Questions
===================

### Q: How do you approach a target? What is your routine like? What is your recon process like? What kind of information do you seek in your information gathering process? And how does this information help you?

Recon-wise, I’ve setup a semi-automated system to discover as many subdomains as possible of a target that has a wildcard, e.g. `*.example.com`. Nothing extremely fancy, just the regular combination of tools such as Sublist3r, MassDNS and scans.io parsing. I do give more attention to CNAME and NS records for [takeover vulnerabilities](https://labs.detectify.com/2014/10/21/hostile-subdomain-takeover-using-herokugithubdesk-more/). More information about automation of these techniques can be found [here](http://offsecbyautomation.com/). CNAME records also often reveal the hosting infrastructure, which can be very helpful for additional exploitation, e.g. [SSRF](https://gist.github.com/BuffaloWill/fa96693af67e3a3dd3fb).

Up next, I browse the application a first time while keeping an eye on burp history (or Logger++) constantly while doing so. A HD screen which can display multiple open windows (burp & several browsers) really helps during this process to maintain an overview of what’s happening. I always look at session management and authentication mechanisms first on all auth-related endpoints I can find (website, mobile API, developer API, etc), as the attack surface often is big and these vulnerabilities are generally high impact. Hereafter, I’ll deep-dive and start hunting for server-side issues (see question below).

I also work in sweeps, it helps me to understand the application a lot better. During the first sweep I try to find all functionality and its related endpoints fast while looking for common bugs such as simple IDORs or SSRFs. I’ve noticed in my Instagram endeavours that, during a second and third sweep, more complex bugs can be discovered, since you already know the application’s base functionality and technologies from the previous sweeps. It allows you to recognize exotic behaviour more easily and thus dig much deeper on the most interesting parts.

### Q: Do you always look for all vulnerabilities types when you approach a website?

No. I only actively look for issues that require (almost) no user interaction, such as RCE, XXE, SSTI, Authentication Bypass, SSRF, IDOR, Privilege Escalation and Blind XSS. I believe that these are the kind of issues that are doing companies the most harm currently next to social engineering, but the latter is typically out of scope for bug bounty programs.

I have only reported a handful of client-side vulnerabilities such as reflected XSS and CSRF, as I tend to only report them when they literally blow up in my face, or can aid in a chained PoC with increased impact (e.g. overly scoped cookie in [Authentication bypass on Ubiquity’s Single Sign-On via subdomain takeover](https://www.arneswinnen.net/2016/11/authentication-bypass-on-sso-ubnt-com-via-subdomain-takeover-of-ping-ubnt-com/)).

### Q: Do you use any tools? Do you have your own tools that you have written to automate/facilitate your work? What Burp extensions do you use? Is there a tool that not a lot of people use that you think they should?

I mainly use Burp and some of its popular extensions from the bApp store, including Active Scan++, Additional Scanner Checks, Authmatrix, Backslash Powered Scanner, Collaborator Everywhere, Logger++ and JSON Decoder. Occasionally I write my own burp plugin when necessary (e.g. Instagram). I use Burp’s Active Scanner a lot on selected parts of requests via the helpful [Scan Manual Insertion Point](https://github.com/PortSwigger/scan-manual-insertion-point) plugin to gain some time during testing. I also regularly use the Burp Collaborator Client which is built-in in Burp Suite. It allows quick detection of a myriad of vulnerabilities via out of band techniques, mostly through DNS lookups & HTTP requests.

Besides burp and its plugins, I also have my own DNS and webserver setup for out of band exploitation through e.g. blind RCE or XXE. Additionally, I use the open-source [FakeDns github project](https://github.com/Crypt0s/FakeDns) from @crypt0s to perform the “Time-of-check to Time-of-use” SSRF attack which is mentioned in [Orange Tsai’s awesome SSRF Black Hat USA 2017 presentation](https://www.blackhat.com/docs/us-17/thursday/us-17-Tsai-A-New-Era-Of-SSRF-Exploiting-URL-Parser-In-Trending-Programming-Languages.pdf).

Next to that, I have two rooted Android phones and a jailbroken Iphone at my disposal to test mobile applications.

### Q: This is one of our most popular questions: How do you test for Server Side vulnerabilities such as RCE, SQLi, etc?

RCE-wise, I’ve been having some luck lately with deserialization issues (both Java & PHP) and [SSRF exploitation via Amazon metadata](https://www.hackerone.com/blog-How-To-Server-Side-Request-Forgery-SSRF). SQLi-wise, I don’t really try this a lot manually anymore as they are becoming less and less prevalent, unless I know the technology stack of my target is particularly vulnerable (looking at you, PHP+MYSQL). I generally leave this up to Burp Scanner in other cases.

Blind XSS-wise, I mainly throw standard payloads from [XSSHunter.com](https://xsshunter.com/) in fields that I believe may be displayed in back-end admin panels and hope for the best. For STTI, I use the standard payloads mentioned in [Portswigger’s extensive blogpost on the matter](http://blog.portswigger.net/2015/08/server-side-template-injection.html). XXE and SSRF are always tested manually in combination with Burp Collaborator Client for blind cases (egress filtering). Authentication bypass is typically specific to the target itself, except for brute-force. I always [test IPv6 support](https://hackerone.com/reports/127844), but few companies have adopted it so far. To evade uniqueness checks on user UUIDs during registration to achieve Account Takeover, I’ve had some luck with [unicode-related tricks](http://websec.github.io/unicode-security-guide/).

### Q: How often do you find a bug that has been overlooked after a bounty program has been established and a horde of researchers have been digging?

Pretty often. This is basically the real strength of crowdsourced security: the more eyes that look at a target, the more vulnerabilities are found. Everyone has a different angle to approach a target, no individual hacker is perfect. This is very important to keep in mind while bug hunting on established programs. It’s hard to accept as a hunter, but my personal goal is to make the number of server-side vulnerabilities that I overlook as small as possible, hereby knowing that 100% will most likely never be achieved.

### Q: Do you think being a pentester, web developer, or being in a related field, helps you with bug bounties? Where should they start?

Having a pentesting background definitely helped me. On one hand, I already had knowledge about the most common web vulnerabilities. On the other hand, it learned me how to write a decent report.

Time to wrap it up!
===================

### Q: What kind of music do you listen to?

It ranges from Rock to Tropical Dance, it really depends on my mood.

### Q: What do you do when you aren’t hacking?

Spend time with family and friends.

### Q: What kind of impact/role have bug bounties played in your life?

Pretty major impact. I’m doing this full-time now for more than a year. Bug bounty hunting allows me to work from home as opposed to spending 2-3 hours commuting on a daily basis. I have a lot more time and less stress now.

I became a full-time bug bounty hunter after having participated in only three bug bounty programs, namely Instagram, Uber and one private program. It was kind of a risky move, but I haven’t regretted it for a second so far.

### Q: What is an advice you received as a beginner that helped you with your bug bounty career?

Persist. There’s always something that has been overlooked.

### Q: What is one area of hacking (web, mobile, hardware, etc) you wish you knew more about / plan on focusing your learning on?

I do wish to improve my mobile reverse engineering skills, as this often yields a larger attack surface in terms of endpoints. I am also considering more low-level bug hunting, such as fuzzing for heap overflows in browsers and other targets, but this is a whole different ball game.

### Q: If someone with basic technical background asked you, “where should I start?”, what are 3 things you would recommend they do before diving into bug bounties?

1.  Read as much as you can. Good starting points are the [Web Application Hacker’s 2. Handbook](http://eu.wiley.com/WileyCDA/WileyTitle/productCd-1118026470.html), [HackerOne Hacktivity](https://hackerone.com/hacktivity) and other [bug bounty write-ups](https://github.com/djadmin/awesome-bug-bounty). Test your theoretical knowledge with practical examples, e.g. [vulnerable apps and VMS](https://www.owasp.org/index.php/OWASP_Vulnerable_Web_Applications_Directory_Project).
2.  Pick out a public bug bounty program and hack away. Persist, don’t give up after a couple of days without success. My first instagram vulnerability took me more than two weeks to discover.

### Q: Someone was eager to know, what do you put on your toast?

I don’t eat toast.

### Q: What’s your worst bug bounty story/experience?

I’ve received the “informational” status for two reports on the same program of which I’m still convinced they highlighted an actual security issue. Apart from that, I’ve come across the occasional disagreement about bounty amounts, but that being said, I consider myself lucky to not have experienced major disappointments during my bug bounty hunting endeavours so far.

### Q: If you had to pick one hacker to collaborate with, who would it be?

It would be [@mongo](https://bugcrowd.com/mongo). His stats on Bugcrowd are mighty impressive, month after month, year after year.

### Q: What’s the one feature you would like to see in the platforms?

I would like to have the ability to reach out to a program, to ask whether they consider certain behaviour to be a security risk worthy of reporting or not. Currently, I often do not immediately report in such situations, as I’m allergic to information and N/A statuses. I currently do this on an ad-hoc basis with some programs I’ve been active on for a while now, but I believe platforms could foresee this functionality in the future.

### Q: What’s your favorite text editor?

I’m a Vim person.

#### [Source](/blog/ama/arneswinnen/)

<br/>
---
