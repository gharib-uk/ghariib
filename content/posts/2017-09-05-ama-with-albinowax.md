---
title: "AMA with albinowax"
date: 2017-09-05T02:00:00+02:00
draft: false
type: posts
categories: 
- 
---
# AMA with albinowax

<br/>

<br/>
### Thank you for doing this interview! Can you please introduce yourself?

I’m James ‘albinowax’ Kettle (it’s a long story), and I live beside the PortSwigger headquarters near Manchester, England. I’ve been bounty hunting for about seven years, since back when Google and Mozilla were pretty much the only targets.

### Q: How do you manage your personal life, work, and bug bounties? Do you do bug bounties as a job or a hobby?

My day job is researching new attack techniques to put into Burp Suite’s scanner, and I use bug bounty websites to evaluate my prototype tools.

### Q: How much time do you spend on Hunting for Bugs? On average, how many bugs do you think you report per month?

I don’t bounty hunt day to day, so I tend to find no bugs for ages followed by a ton in a few hours when I have a good idea like [targeting load balancers](http://blog.portswigger.net/2017/07/cracking-lens-targeting-https-hidden.html) or [abusing CORS](http://blog.portswigger.net/2016/10/exploiting-cors-misconfigurations-for.html).

### Q: How long did it take you until you found your first significant/high impact/payout vulnerability?

My first ever payout was $1500, which I obtained [by selling a zeroday broker a vulnerability in their own website](https://www.youtube.com/watch?v=wgkj4ZgxI4c&feature=youtu.be&list=PLpr-xdpM8wG8RHOguwOZhUHkKiDeWpvFp&t=57). This was only a few months after I got into security, back when CSRF worked on pretty much everything. Good times.

### Q: Of all the bugs you’ve found, what was your favorite/most interesting?

Probably the [series of password reset poisoning bugs I found in Django](http://www.skeletonscribe.net/2013/05/practical-http-host-header-attacks.html). I think that was the first ever password reset poisoning attack, and Django kept ignoring my patch advice meaning I could bypass their fix, leading to three $3,000 payouts for a single issue.

### Q: When and how did you have your breakthrough? When did you realize hacking and bug bounties was something you wanted to dedicate your time to? Please share your insights and the problems you faced to becoming an established bug bounty hacker?

As soon as I realised I could try to hack stuff without worrying about being arrested, I was on board. I think the great thing about bug bounties is, there really aren’t any barriers. If you look hard enough you’ll find a good bug and get paid.

### Q: What do you do to keep up with all the new trends?

Twitter, mostly.

### Q: Do you collaborate with other hackers? Can you name a few?

Only infrequently so far, but I’m currently experimenting with working together with other bounty hunters to beta test my prototypes.

Technical Questions
===================

### Q: How do you approach a target? What is your routine like? What is your recon process like? What kind of information do you seek in your information gathering process? And how does this information help you?

I like to attack every website at the same time. To do that, I’ve [built a pipeline](http://blog.portswigger.net/2017/07/cracking-lens-targeting-https-hidden.html#pipeline) that uses DNS data from scans.io to identify targets, and fires payloads using ZGrab.

### Q: Do you use any tools? Do you have your own tools that you have written to automate/facilitate your work? What Burp extensions do you use? Is there a tool that not a lot of people use that you think they should?

I primarily use my own prototype Burp Suite extensions. I’m quite proud of Backslash Powered Scanner, so I’d recommend everyone use that. Also, I think Burp’s manual Collaborator client is currently under-appreciated; it’s seriously powerful.

### Q: This is one of our most popular questions: How do you test for Server Side vulnerabilities such as RCE, SQLi, etc?

Backslash Powered Scanner! It’s all about maximum laziness.

### Q: How often do you find a bug that has been overlooked after a bounty program has been established and a horde of researchers have been digging?

Regularly; that’s what hacking is all about for me.

### Q: Do you think being a pentester, web developer, or being in a related field, helps you with bug bounties? Where should they start?

Being able to code definitely helps, but I don’t think it’s necessary to start. My advice is to just get hacking already.

Time to wrap it up!
===================

### Q: What kind of music do you listen to?

Mostly Metallica.

### Q: What do you do when you aren’t hacking?

Counterstrike.

### Q: What kind of impact/role have bug bounties played in your life?

I might not do much bounty hunting these days, but bug bounties taught me how to hack; the freedom to experiment on live sites and pure result-based reward scheme helped shape the attitude I carry today.

### Q: What is an advice you received as a beginner that helped you with your bug bounty career?

I didn’t really get any advice, or perhaps just didn’t listen. I vividly remember the worst advice though - it was “you can’t do anything bad with an Excel formula”. Turns out, you can [execute arbitrary code](https://www.contextis.com/blog/comma-separated-vulnerabilities). My advice is never to hold back from an attack just because it’s too dumb to work.

### Q: What is one area of hacking (web, mobile, hardware, etc) you wish you knew more about / plan on focusing your learning on?

Always web.

### Q: If someone with basic technical background asked you, “where should I start?”, what are 3 things you would recommend they do before diving into bug bounties?

1.  Read the [WebApp Hacker’s Handbook](http://www.wiley.com/WileyCDA/WileyTitle/productCd-1118026470.html) and afterwards, The Tangled Web
2.  Get practical experience using [OWASP Broken Web Apps](https://code.google.com/archive/p/owaspbwa/)
3.  Buy [Burp Suite Pro](https://portswigger.net/A.ashx?a=3317C1C686432D16) _cough_

### Q: Someone was eager to know, what do you put on your toast?

Chilli

### Q: What’s your worst bug bounty story/experience?

I won’t go into the details here but I may have published something [elsewhere](http://www.skeletonscribe.net/2016/08/reviewing-bug-bounties-hackers.html)…

### Q: If you had to pick one hacker to collaborate with, who would it be?

@lcamtuf - I earned three bounties just by reading The Tangled Web, so it’d be carnage if we collaborated.

### Q: What’s the one feature you would like to see in the platforms?

Reviews of bounty programs.

### Q: What’s your favorite text editor?

Vim

#### [Source](/blog/ama/albinowax/)

<br/>
---
