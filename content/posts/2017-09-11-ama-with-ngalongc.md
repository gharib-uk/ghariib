---
title: "AMA with ngalongc"
date: 2017-09-11T02:00:00+02:00
draft: false
type: posts
categories: 
- 
---
# AMA with ngalongc

<br/>

<br/>
### Thank you for doing this interview! Can you please introduce yourself?

I am Ron Chan, a Hong Kong bug hunter. I started to learn about hacking in 2016 April, I have no clue where to start at that time, then I found an interesting online hacking course, OSCP. I paid for 60 days of labs and start to pwn them one by one. By the time I finished my OSCP exam, I found something called bug bounty, I find it through Orange Tsai’s Facebook SQLi to RCE blog post. The technical details alone of the bug is amazing, and the idea of bug bounty amazed me the most. I never knew people can make legal money through hacking. After that I start to read more and more bug bounty write-ups, and eventually I found my first bug in Yahoo Pay, I can buy anything with any price I want. And then I decide to dedicate my time to bug bounty. Since then, Yahoo and Uber have been my favourite program.

### Q: How do you manage your personal life, work, and bug bounties? Do you do bug bounties as a job or a hobby?

Personal life: 20% Work: 10% Bug Bounty: 70%

### Q: How much time do you spend on Hunting for Bugs? On average, how many bugs do you think you report per month?

I had a rough start, I can only report 5-10 low impact bugs per month in first six months, for the recent six months, I report 20-40 bugs per month, it really depends on the luck. If that month Yahoo decide to have some major change in Flickr, then I may find more for that month.

### Q: How long did it take you until you found your first significant/high impact/payout vulnerability?

My first bug in Yahoo has pretty high impact, I can buy anything with any price. I am super lucky that only Hong Kong residents could have access to the Yahoo Pay function. And there are not many bug hunters in Hong Kong.

### Q: Of all the bugs you’ve found, what was your favorite/most interesting?

Google Bug Hunter’s Account Takeover, you can read it [here](https://ngailong.wordpress.com/2017/08/07/how-i-could-steal-your-google-bug-hunter-account-with-two-clicks-in-ie/) It is interesting because Google actually didn’t do anything wrong, in terms of technical vulnerability. They were just encoding the character in 302 response header, it is IE’s weird behavior that lead to this finding.

### Q: When and how did you have your breakthrough? When did you realize hacking and bug bounties was something you wanted to dedicate your time to? Please share your insights and the problems you faced to becoming an established bug bounty hacker?

The way I see my major breakthrough is right after reading zseano’s tutorial, he showed how a open redirect could be leveraged to a full fb linked account takeover. This is not just a technical details that strike me the most, but the concept to increase the impact even it is trivial bug(open redirect). After reading his blog, I started to go through the existing bug bounty programs one by one, and it turns out quite a few of the programs are vulnerable to this open redirect+ fb ATO attack. I was not just looking for open redirect + fb account ATO. I also applied his concept to Oauth login flow exploit. That’s the moment I found out Uber multiple Oauth bugs, Flickr ATO bugs and earned around ~40k(?) because of his tutorial. Since then I know I can’t ever leave bug hunting because it is so fun.

The major problem before I reach this breakthrough is that I don’t have a thorough understanding of what those write up means. I think this is the problem of most of the new hunters. For example, when I first read about fin1te’s self xss to good xss. It seems I can grab the general idea of the method, but deep inside I don’t understand why he needs to use CSP, what is the problem of Login CSRF etc. All these little details make up to a good write up but I failed to understand them. After the break through, all of the little bugs he talked about finally makes sense to me. After knowing have improved myself, I read all the old write ups that I did not understand previously, and the world was never the same.

### Q: What do you do to keep up with all the new trends?

Twitter.

### Q: Do you collaborate with other hackers? Can you name a few?

I worked with cache-money to hack Uber together for a few bugs recently. And I asked filedescriptor a lot of question when I have something I don’t understand, not exactly collaboration, more like tutorial class from him.

Technical Questions
===================

### Q: How do you approach a target? What is your routine like? What is your recon process like? What kind of information do you seek in your information gathering process? And how does this information help you?

I mainly look at Yahoo and Uber, so I routinely check their front page and see what is new in their product, check their engineering blog, follow their twitter, never miss an update when they push the feature to production. Subdomain brute force, nmap, google dorks I do them occasionally but not always, only when I think it is needed.

### Q: Do you always look for all vulnerabilities types when you approach a website?

My weakest area is asset discovery, I seldom use dirbuster or sublist3r. Cause my main target is Yahoo and Uber, they generally have enough function/features that could occupy my time, all I do is follow the UberEng twitter and keep an eye on what is updating on their product. Like the latest tipping features, social features, uberpool features etc. though I live in HK and the features in Uber are ALWAYS two steps later than US Uber, I still find this approach effective. About Yahoo, the attack surface is giant in itself, Yahoo sports, Gemini, news, finance, its more than you can test, so I usually don’t spend the time to do subdomain brute force or dirbuster. Testing the function alone is occupying most of my time. To sums up, I am lazy, test whatever is presented to me.

### Q: Do you use any tools? Do you have your own tools that you have written to automate/facilitate your work? What Burp extensions do you use? Is there a tool that not a lot of people use that you think they should?

I mainly use Burp, and VPS to perform brute force sometimes. JSON beautifier is my recent favourite extension in Burp. Not much automation when I hunt.

### Q: This is one of our most popular questions: How do you test for Server Side vulnerabilities such as RCE, SQLi, etc?

I test for IDOR, and authentication bypass, these two are easy to test and have high impact. IDOR is nothing new, just replace your parameter with victim’s identifier. Authentication bypass is reset password/oauth/saml etc.

### Q: How often do you find a bug that has been overlooked after a bounty program has been established and a horde of researchers have been digging?

Not sure cause I left private program for a long time.

### Q: Do you think being a pentester, web developer, or being in a related field, helps you with bug bounties? Where should they start?

I am a Security Consultant, it doesn’t really help in bug bounty. Two responsibility and different goal.

Time to wrap it up!
===================

### Q: What kind of music do you listen to?

Random youtube trending song

### Q: What do you do when you aren’t hacking?

Spend time with family, friends, like everyone else.

### Q: What kind of impact/role have bug bounties played in your life?

Hobby and major source of income.

### Q: What is an advice you received as a beginner that helped you with your bug bounty career?

Keep reading the write ups and replicate it in your local environment when you don’t understand it.

### Q: What is one area of hacking (web, mobile, hardware, etc) you wish you knew more about / plan on focusing your learning on?

Definitely iOS jailbreak, it’s a another new level of hacking.

### Q: If someone with basic technical background asked you, “where should I start?”, what would recommend they do before diving into bug bounties?

OSCP, the Web application hacker’s handbook, Learn Python the Hard Way.

### Q: Someone was eager to know, what do you put on your toast?

No toast in Hong Kong ;)

### Q: What’s your worst bug bounty story/experience?

A private program, it is a chinese company, it doesn’t acknowledge a ATO bug after fixing it silently. Closed as NA and no reward.

### Q: If you had to pick one hacker to collaborate with, who would it be?

Filedescriptor ;)

### Q: What’s the one feature you would like to see in the platforms?

Bounty bonus in short period of time.

### Q: What’s your favorite text editor?

Vim

#### [Source](/blog/ama/ngalongc/)

<br/>
---
