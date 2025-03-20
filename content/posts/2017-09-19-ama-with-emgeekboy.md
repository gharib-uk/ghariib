---
title: "AMA with emgeekboy"
date: 2017-09-19T02:00:00+02:00
draft: false
type: posts
categories: 
- 
---
# AMA with emgeekboy

<br/>

<br/>
### Thank you for doing this interview! Can you please introduce yourself?

I’m Sandeep Singh known by the handle ‘Geekboy’ and I’m from India. i got myself into hacking around 6 year ago when I decided to do to ethical hacking certification just for adding anything extra into my profile other than regular studies. I started doing bug bounties almost 3 years back when I saw some friends Facebook posts about rewards from companies like Facebook / Google more, at that time I heard of HackerOne platform so I started on HackerOne and got stick on it, almost 70-80% of my bug reports submissions is on HackerOne with 800+ valid reports submitted to 100+ programs but not to forget there are some other cool platforms as well like BugCrowd & Synack where I participated.

### Q: How do you manage your personal life, work, and bug bounties? Do you do bug bounties as a job or a hobby?

I started bug bounties as full time just after completing my graduation, so i just need to manage times for my friends and family but really you don’t need to worry about managing times if you doing full time, you are your own boss, so do whenever you feel & enjoy whenever you want and that’s the freedom you get if doing it as full time.

### Q: How much time do you spend on Hunting for Bugs? On average, how many bugs do you think you report per month?

As i said i do bb as full time so most of time and usually all nights but not on daily basis, and i had no idea about my average numbers bugs p/m but few days before i used BBstats (still in beta) by Gwen and it shows 41 bugs p/m on average just on HackerOne since almost 3 years, believe me i was surprised too with those stats and now i’m really worried for the submission of this month.

### Q: How long did it take you until you found your first significant/high impact/payout vulnerability?

It took almost 3 months to get 1st valid bug on HackerOne, it was logical one on Square program which allows any remote attacker to block any users to change their email address.

### Q: Of all the bugs you’ve found, what was your favorite/most interesting?

AirBnb one, i blogged about it [here](https://www.geekboy.ninja/blog/airbnb-bug-bounty-turning-self-xss-into-good-xss-2/), it was interesting for me as it was chain of multiple low severity bugs to get the ATO, and this is why programs should care about those small bugs at 1st place as well.

### Q: When and how did you have your breakthrough? When did you realize hacking and bug bounties was something you wanted to dedicate your time to? Please share your insights and the problems you faced to becoming an established bug bounty hacker?

I started hacking just as an optional carrier option for safe side, but when i actually got into hacking it was curiously/interest to know everything, how it happens , how i can do it and etc, after having some experience in web application security i switched to bug hunting just after i released that we can even eary also by seating home, so what else anyone want ? all these things happens while i was doing my graduation so i had no family pressure/tension of having job so that was good thing in my case, things which are not good at the time when i started is lack of resources, awareness about bug bounties but now we have tons of resources/talks to start with bug bounties if you are coming with right intent.

### Q: What do you do to keep up with all the new trends?

Twitter, Slack.

### Q: Do you collaborate with other hackers? Can you name a few?

Yeah, if i do then most of time it’s [Parth](https://twitter.com/Parth_Malhotra).

Technical Questions
===================

### Q: How do you approach a target? What is your routine like? What is your recon process like? What kind of information do you seek in your information gathering process? And how does this information help you?

For the start i check with policy/scope section of the program and see what’s in scope and what’s not, once i got those information, of eg: if `*.site.com` is in scope, i start with subdomain scan in both ways active/passive discovery, for bruteforcing i do use MassDNS and for passive there are various sources like crt.ch,shodan,censy/google, in meantime i go through with all the functions core application which is most focused part in my testing. after understanding all the application i go through all the checklist whatever comes in mind against the application, it was hectic to maintain those checklist and get it whenever you need, but HUNT made it easy, just import that burp extension into your burp and it’s always with you, in fact both extensions are handy to use, credit goes to Bugcrowd team for this. apart from this i usually check for subdomain takeovers as well in case Frans didn’t took over it already.

### Q: Do you always look for all vulnerabilities types when you approach a website?

Yes, but it depends how much time i’m spending on the target, like for the start i go with obvious issue like XSS, CSRF and more, if things looks interesting i dig deeper and spend more time on target to find as much as possible bugs i can.

### Q: Do you use any tools? Do you have your own tools that you have written to automate/facilitate your work? What Burp extensions do you use? Is there a tool that not a lot of people use that you think they should?

Tools: - Sublister, Aquatone, MassDNS (For subdomain discovery) - DirSeach (For content discovery) - Second Order (for crawling endpoints & JS files) - Actarus (recon & management tool)

Burp Extensions: - Reflected Parameters (XSS) - Autorize (For privilege escalation) - Collaborator Everywhere (For detecting external service interaction) - JSON Decoder - Backslash Powered Scanner

### Q: This is one of our most popular questions: How do you test for Server Side vulnerabilities such as RCE, SQLi, etc?

It’s all about paying attention to parameters/functions, prams like url/file where any external reference getting called and then verify for possible SSRF/LFI and other things and sometimes we can use some tricks to escalate basic SSRF to make it critical, eg: in case where you can pass URL to parameter and see the content back, just check for if it’s running on Amazon cloud try pull some data, in case direct url call is block try with 302 redirect to call the url, use different IP notation to bypass the blacklisting. pay attention to upload forms, play with extensions, path etc, and always know the running environment of application where we can check for already published exploit like Struts, ImageTragick, python pickling etc.

### Q: How often do you find a bug that has been overlooked after a bounty program has been established and a horde of researchers have been digging?

Very rare, as i had very bad habit of not looking for bugs in old or already hunted programs but after seeing some demo by PHP hacker Jobert in SFO and couple of newly discovered bugs in public programs i changed my mind to check old programs after certain period of time.

### Q: Do you think being a pentester, web developer, or being in a related field, helps you with bug bounties? Where should they start?

I jumped into bug bounties after my studies so not sure about pentester but having background in development is big plus, if you developer you already know everything, they just need to change their thought process for breaking the applications instead of developing.

Time to wrap it up!
===================

### Q: What kind of music do you listen to?

Classical indian remix :D

### Q: What do you do when you aren’t hacking?

Playing computer games, hanging out with friends, traveling all cool places in my country.

### Q: What kind of impact/role have bug bounties played in your life?

I will say complete life changing, it’s my primary source of income with the work i love to do most will complete freedom.

### Q: What is an advice you received as a beginner that helped you with your bug bounty career?

In early days of bug hunting after initial failures i was about give up but few friends who are active in bug hunting that time advised me to have Patience & keep doing what i’m doing which actually works.

### Q: What is one area of hacking (web, mobile, hardware, etc) you wish you knew more about / plan on focusing your learning on?

Mobile Application reversing & IOT

### Q: If someone with basic technical background asked you, “where should I start?”, what are 3 things you would recommend they do before diving into bug bounties?

If someone with basic technical background asked you, “where should I start?”, what are 3 things you would recommend they do before diving into bug bounties?

-   Follow writeup blogs, [here is mine](https://www.geekboy.ninja/blog/) as well, [hacktivity](https://hackerone.com/hacktivity?sort_type=popular&filter=type%3Aall&page=1&range=forever) page, [web hacking 101](https://leanpub.com/web-hacking-101).
-   Engage with community, share/understand how things works.
-   Respect other hackers and program’s policy.

### Q: Someone was eager to know, what do you put on your toast?

Amul butter :\[

### Q: What’s your worst bug bounty story/experience?

Few fake programs who signed up on platform for reports and never came back to me or anyone else.

### Q: If you had to pick one hacker to collaborate with, who would it be?

Frans Rosén (The hacker i admire most)

### Q: What’s the one feature you would like to see in the platforms?

Enhanced profile page of hackers.

### Q: What’s your favorite text editor?

Sublime

#### [Source](/blog/ama/geekboy/)

<br/>
---
