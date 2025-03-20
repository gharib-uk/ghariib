---
title: "AMA with securinti"
date: 2018-02-27T01:00:00+01:00
draft: false
type: posts
categories: 
- 
---
# AMA with securinti

<br/>

<br/>
### Thank you for doing this interview! Can you please introduce yourself?

I’m Inti De Ceukelaire from Oilsjt, Belgium. I grew up being a typical script kiddie browsing the web for hacks and cheats for the video games I played. Whenever I bought a new console or handheld, I would immediately go on the internet to look for softmods or jailbreaks.

In 2011, I bought a brand new Playstation Portable and things changed. I came home from the store, went online to check how to jailbreak the device and was devastated to find out that there was no jailbreak available the latest firmware at that time (6.35). I came to the conclusion that if I wanted a jailbreak, I would have to find the exploit myself. I locked myself into my room for days and literally tried anything that came into my mind. I was extremely lucky to find a buffer overflow in the browser’s bookmark feature that allowed arbitrary code execution. Check the crappy video I made as a proof of concept [here](https://www.youtube.com/watch?v=Hv8Hlh5GQ0g).

I found out being a hacker is so much cooler than being a skid so I bought some books to learn about web application security. The Google VRP program just started so there were plenty of vulnerabilities to discover back then. I believe I found about 5 XSS issues on Google in my first week of hacking, most of which were duplicates. But the few bucks I got motivated me to quit my student job at the local shopping mall (I hated it) and spend my free time bug bounty hunting. That was probably one of the best decisions I’ve ever made, next to joining HackerOne and Intigriti.

### Q: How do you manage your personal life, work, and bug bounties? Do you do bug bounties as a job or a hobby?

I work as a creative developer / social media guy at Studio Brussel, a Belgian radio station. I get a lot of time to play with the Facebook API and improve my coding skills. I don’t spend as much time on bug bounty hunting as I’d like to. It varies from month to month, but usually no more than a few hours a week. Sometimes I report 20 bugs a month, sometimes I don’t report anything for three months. Quality over quantity!

### Q: How long did it take you until you found your first significant/high impact/payout vulnerability?

Sad but true: I found my most significant bugs by accident. The Playstation vulnerability that got me into the bug bounty scene was a pretty cool, but didn’t really come down to skills. The first five-figure bounty I got was for a bug I discovered while trying to return some shoes my wife ordered at an online shop. I’m so glad she didn’t like them!

### Q: Of all the bugs you’ve found, what was your favorite/most interesting?

I think my [Ticket Trick](https://medium.freecodecamp.org/how-i-hacked-hundreds-of-companies-through-their-helpdesk-b7680ddc2d4c) was pretty interesting. It was a logic flaw that allowed remote attackers to gain access to internal company assets such as their private Slack instance, Twitter account and support portal. What made this bug so cool is that, once you know how it works, it’s fairly simple, yet it was never discovered before. Testing every single bug bounty program was really exciting and there was no rush because I knew I was the only one aware of it.

### Q: When and how did you have your breakthrough? When did you realize hacking and bug bounties was something you wanted to dedicate your time to? Please share your insights and the problems you faced to becoming an established bug bounty hacker?

My personal breakthrough came when I realized the low-hanging fruit was simply not worth looking into. Most of the bugs I found in my first year were reflected cross site scripting issues that didn’t really have an impact. I was in it solely for the money and didn’t really enjoy looking for the bugs. As time passed by I realized I could earn more money by investing some more time on a target to improve my skills to find the really juicy stuff. I decided to stick with the next program I was invited for and I’m still working on it. I didn’t find the best bugs up until recently. It’s been three years since I started working on that program.

### Q: What do you do to keep up with all the new trends?

Twitter and the bug bounty community Slack. The cool thing with our community is that we help each other moving forward by sharing knowledge. Technically, you may lose out on some bug bounties by sharing your methods with other hackers, but I’ve found that the return the community is much more rewarding than playing this game on your own.

### Q: Do you collaborate with other hackers? Can you name a few?

The first member of the community I got in touch with was @smiegles. We were both working on Yahoo and shared some of our findings in a Skype chat. Then @itsecurityguard, @ziot and @nahamsec joined and we decided to open the discussion to other hackers. This group chat would later evolve to the Bug Bounty Forum Slack channel as we know it.

At H1-702 in 2016, I met @arneswinnen, also from Belgium. I wasn’t aware of any other bug bounty hunters from my country so it was really cool to meet him and exchange experiences. Later @preben\_ve and @jerome joined HackerOne’s CTF’s and we united under the name #TeamBelgium.

Technical Questions
===================

### Q: How do you approach a target? What is your routine like? What is your recon process like? What kind of information do you seek in your information gathering process? And how does this information help you?

Disclaimer: I am probably one of the most inefficient hackers out there. I really don’t have a routine. I use Sublist3r, knock, dirsearch, nmap, GitHub, BitBucket, Pastebin, HaveIBeenPwned, Censys, WayBack machine, Twitter, Google and Shodan for recon, but for big targets that have been looked at, I usually focus on the documentation. RTFM is the best advice I could give to pwn a target.

### Q: Do you always look for all vulnerabilities types when you approach a website?

No. I usually start with one main goal. I recently challenged myself to find a way for an external website to discover the age or gender of a Facebook user without having to ask for permission. It only took a few hours to find one. I would have never found that vulnerability just by testing for the classic textbook vulnerabilities.

### Q: Do you use any tools? Do you have your own tools that you have written to automate/facilitate your work? What Burp extensions do you use? Is there a tool that not a lot of people use that you think they should?

I have a confession to make: I only started using Burp in 2017. As I said, I’m probably not the most efficient hacker out there, but you don’t need Burp to find cool stuff. Sometimes it even helps to close burp for a while when stuck on a target, you’ll approach it from a different perspective.

Other than that, the usual, standard tools like sqlmap, nmap, dirsearch, knock, sublist3r, … Whenever I need something non-standard and I’m not in a rush, I write the tool myself to improve my programming skills. I’m still far behind looking at the custom tools and dashboards other hackers have built though. I really need to work on that.

### Q: This is one of our most popular questions: How do you test for Server Side vulnerabilities such as RCE, SQLi, etc?

I only focus on the blind stuff. Blind RCE, blind SQLi, blind template injection, … whatever. As long as it’s blind, I’ll take it. Not necessarily because it’s easier (it’s not), but mainly because it is often overseen by other hackers, can have a huge impact and requires the hacker to think about the application, what’s going on behind the scenes and to look for creative ways to blindly execute a payload.

### Q: How often do you find a bug that has been overlooked after a bounty program has been established and a horde of researchers have been digging?

I try to avoid duplicates as much as possible. I don’t go after the low hanging fruit but instead look for a specific pattern or behavior. For example: I may spend a day or two fiddling with the account deletion functionality. Most of the times I won’t get anything useful out of it, but if I’m lucky enough and discover a specific behavior that introduces a security vulnerability related to this functionality, I can test lots of bug bounty programs without having much risk of getting duped.

### Q: Do you think being a pentester, web developer, or being in a related field, helps you with bug bounties? Where should they start?

Yes. You don’t need to be a 1337 programmer to get into bug bounty hunting, but knowing the basics will definitely help. I would recommend reading HackerOne’s hacktivity and Pete Yaworski’s web hacking 101.

Time to wrap it up!
===================

### Q: What kind of music do you listen to?

Anything from heavy metal to classical music. When I’m hacking, I prefer that kind of hip hop that makes you feel like a 1337 matrix gangsta.

### Q: What do you do when you aren’t hacking?

Traveling, going out with friends and ironing. I spend way too much time ironing and I absolutely hate it. If there was some kind of automatic ironing machine, I’d immediately buy it. It’d be a bounty well spent!

### Q: What kind of impact/role have bug bounties played in your life?

Bug bounties changed my life. I have my own appartement, don’t have to worry about money, had some awesome experiences (like the HackerOne events abroad) and made a ton of friends in the community. I am incredibly grateful for that, I mean, ten years ago we could go to jail for this! I try sharing some of this luck with my friends and family. For example, whenever I get a big bounty, I take my wife out for a #BugBountyDinner.

### Q: What is an advice you received as a beginner that helped you with your bug bounty career?

As a beginner I would just submit a ton of low quality reports with little impact. Instead of ignoring my reports some security engineers would offer helpful feedback. I always admired Neal Poole from Facebook for his long and informative replies on why something is not considered a bug. I remember him showing some support after the third duplicate in a row. Showing that you care really pays off for program owners in the long run.

### Q: What is one area of hacking (web, mobile, hardware, etc) you wish you knew more about / plan on focusing your learning on?

I’d love to learn more about hardware hacking and malware. It’s on my list, but I keep getting distracted by bug bounty programs. For bug bounty programs in 2018, I will be focussing on logic flaws with a financial impact in webshops.

### Q: If someone with basic technical background asked you, “where should I start?”, what are 3 things you would recommend they do before diving into bug bounties?

Don’t ask where to start, just start. The internet has plenty of resources on web application security. Believe in yourself, set some goals and don’t stop until you achieve them. Create a twitter account, then follow @fransrosen and anyone he follows. Follow the people they follow. Repeat. Then focus on one target and one vulnerability class. It will take some time. Persist.

### Q: Someone was eager to know, what do you put on your toast?

I’m a nutella guy.

### Q: What’s your worst bug bounty story/experience?

There was this one company that almost sued me over a blog post they initially gave permission to publish after proofreading it, but later changed their mind.

### Q: If you had to pick one hacker to collaborate with, who would it be?

Hands down: @jobert.

### Q: What’s the one feature you would like to see in the platforms?

I’d like to be able to communicate with a program to ask questions about the scope or the application.

### Q: What’s your favorite text editor?

Atom and vim.

#### [Source](/blog/ama/inti/)

<br/>
---
