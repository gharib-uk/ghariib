---
title: "AMA with 0xteknogeek"
date: 2018-07-26T02:00:00+02:00
draft: false
type: posts
categories: 
- 
---
# AMA with 0xteknogeek

<br/>

<br/>
### Thank you for doing this interview! Can you please introduce yourself?

My name is Joel Margolis aka teknogeek. I’m from Connecticut and New York and have been hacking for 10 years. I started out hacking when I was a kid by brute-forcing the family laptop password to play video games. Ever since, I’ve been hooked on breaking, fixing, and breaking again everything I can get my hands on. However, typically I do mobile security almost exclusively nowadays! I usually hack on private programs, but some of my favorite public programs to hack on are Uber, Yahoo, and Airbnb.

### Q: How do you manage your personal life, work, and bug bounties? Do you do bug bounties as a job or a hobby?

I’m a full-time student and work multiple jobs, so managing it all can be really tough. A few of the things that I use to help me are Todoist and Google Calendar. Not only do they work hand-in-hand, but they also make it really simple to manage a lot of things. Now when I think of something that I have to do, I enter it into Todoist and won’t forget about it. I also enter my entire schedule into Google Calendar, color coded and everything. When someone asks me about something, I can take out my phone and know everything that I have going on all at once. I had a weird introduction to bug bounty hunting, which was at H1-702. Previously, I had been doing contracting work reverse engineering mobile apps. When I went to H1-702 I met some amazing hackers who explained that all the stuff I was doing could be used to find big bugs. With contracting becoming harder to manage in college, I decided to move a portion of the time I had dedicated towards that and put it towards bug bounty hunting part-time.

### Q: How much time do you spend on Hunting for Bugs? On average, how many bugs do you think you report per month?

How much time I spend really depends on how successful my hunting has been going and how many other things I have to do that day. Sometimes, I’ll spend a few days in a row just hunting for bugs because I can feel something good coming or if I’m in the middle of a good bug. Typically I submit anywhere between 0-5 bugs a month, but I always aim to get at least one every month.

### Q: How long did it take you until you found your first significant/high impact/payout vulnerability?

Due to the strange start to my bug bounty history, I actually found a high vulnerability on Uber while at H1-702. Not only was it my first bug on the platform, but it was my first paid bug too!

### Q: Of all the bugs you’ve found, what was your favorite/most interesting?

I would have to say my favorite/most interesting bug was either a recent command injection vulnerability I found on a private program or a set of AWS S3 bucket credentials that were hardcoded into an application on a different private program.

### Q: When and how did you have your breakthrough? When did you realize hacking and bug bounties was something you wanted to dedicate your time to? Please share your insights and the problems you faced to becoming an established bug bounty hacker?

I started breaking into mobile apps about 4 years ago. Being able to reverse engineer something from a finished product to something I could use and manipulate myself was appealing and always a new challenge to tackle. When I found out that I could take those skills and use them to find paid bugs and not get in trouble, I was drawn into bug bounty hunting. While I had a fairly recent introduction to the bug bounty community, the mobile security background I have has taken me years to accumulate. To talk about some of the problems and insights I encountered when becoming a hacker, let me first address some common misconceptions. First, people think that hacking is super difficult and you need years of computer knowledge and experience to find anything. Wrong! Some of the craziest bugs have been found with simple scans and common payloads. Second, people think that bug public programs are impossible to find new bugs on with all the other hackers looking at the same things you are. There are countless times where dozens of hackers spot the same odd behavior but only one person figures out how to abuse it! Lastly, get to know your target. It’s really hard to break something you don’t understand. Spend at least 20-30 minutes of your recon devoted to understanding and using the product yourself. This will help you better grasp why they might do something and how an attack would work in action.

### Q: What do you do to keep up with all the new trends?

To keep up with new trends, Twitter is an excellent resource. Additionally, I use Feedly as a news aggregator for security and tech news and stay active in the security communities around me and at school.

### Q: Do you collaborate with other hackers? Can you name a few?

Of course, collaboration is important! I’ve done collabs in the past with jelmer, edio, uranium238, and many others.

Technical Questions
===================

### Q: How do you approach a target? What is your routine like? What is your recon process like? What kind of information do you seek in your information gathering process? And how does this information help you?

When approaching a target, I like to take a look at the information I might already have. If it’s a mobile app, I will decompile it and start looking for juicy things in the source code before I even install it. Once I have found some interesting things, I will install the application on an emulator or a physical device and start looking at the networking traffic being sent. For my web proxy, I use either Charles or Burp Suite which have proved to work the best in my experience. When I’m looking an app, I like to look for API keys, hardcoded secrets, bad security practices like explicit certificate unpinning, etc. Once I have a look at the network traffic, I like to see if there are things like IDOR, bad authentication, any assumptions that the developer might have made with the mobile app API. Usually, that will give me a very good base level of knowledge and I can start digging deeper into things as I come across them. If the target is a web application, I almost always start by doing subdomain scans, google dorks, looking on GitHub for repositories and secrets, checking for hidden folders like .git, and directory scans. There’s a ton of good resources out there for the right search keywords to use, especially here on BBF! Once I’ve done some scanning recon, I will go hands-on with the application and experiment with it as a user before I dive in further. This allows me to better understand the mistakes that a developer might have made.

### Q: Do you always look for all vulnerabilities types when you approach a website?

I don’t look for all vulnerability types when I approach a website, but over time I have built up little bells/triggers in my head that will go off when I see something. For example, whenever I see a string that’s encoded in Base64, I know it almost instantly even though I can’t decode it in my head. These little triggers help me pick up on things as I scan over what I’m looking at. The vulnerabilities that you find aren’t always going to be the same as other ones that you have found of the same type. However, I frequently use past bugs as reference knowledge for new bugs I find.

### Q: Do you use any tools? Do you have your own tools that you have written to automate/facilitate your work? What Burp extensions do you use? Is there a tool that not a lot of people use that you think they should?

Like I said, I use both Charles and Burp as web proxies depending on my use-case. Sometimes I prefer the Charles layout over Burp while other times I need the pentesting features provided by Burp. I have done a bit of automation for myself with scripts to decompile, organize, and scan a target, but all of it is private for personal use. The #1 tool that I use the most, however, is Visual Studio Code. It’s a fantastic robust text editor with plugins, a large community, and an open source code base. It’s definitely my go-to for opening large amounts of files and folders.

### Q: This is one of our most popular questions: How do you test for Server Side vulnerabilities such as RCE, SQLi, etc?

Try to figure out what the code on the server side is actually doing! Think about what the query or command string might look like based on the action being performed. Then, test to see if you’re right. Additionally, try to get as much information as you can from side-channels. What version of PHP are they using, are they using Linux or Windows, what commands are available on the system, is the system using something custom or distributed?

### Q: How often do you find a bug that has been overlooked after a bounty program has been established and a horde of researchers have been digging?

I’m not sure how often I find something that’s been overlooked. Often times, I am looking at new programs and especially private ones. Additionally, mobile security doesn’t have a lot of hackers so the amount of people who have looked at the same things is pretty low.

### Q: Do you think being a pentester, web developer, or being in a related field, helps you with bug bounties? Where should they start?

Having experience as both a pentester and web developer has been one of the most valuable tools available to me. Being a web developer will allow you to see the challenges you face as a pentester from the other side of the equation. If you know how a web developer might design something, you can better think about the options available to you as the attacker in the same scenario. I would definitely recommend learning basic web development and coding skills, especially: HTML, JavaScript, Python, Ruby, and PHP; these will help you immensely down the line! Check out Code Academy and FreeCodeCamp for more resources.

Time to wrap it up!
===================

### Q: What kind of music do you listen to?

Electronic, Jazz, Hip-Hop

### Q: What do you do when you aren’t hacking?

Play video games, learn something totally new

### Q: What kind of impact/role have bug bounties played in your life?

They have allowed me to meet so many amazing people, granted me one-time opportunities, expanded my security knowledge, helped me figure out better time management, and given me the money to buy my own car!

### Q: What is an advice you received as a beginner that helped you with your bug bounty career?

Don’t give up and always be confident in your own abilities! You never know when you might find RCE :)

### Q: What is one area of hacking (web, mobile, hardware, etc) you wish you knew more about / plan on focusing your learning on?

I wish I knew more about web and hardware hacking! I’d really like to apply my technical skills to more web hacking to get better at spotting bugs. Additionally, one of my personal goals is to write an iOS jailbreak by the end of 2018. I don’t have anywhere close to the knowledge I need to do that yet, but it’s something I am excited to learn a lot more about!

### Q: If someone with basic technical background asked you, “where should I start?”, what are 3 things you would recommend they do before diving into bug bounties?

Look at the OWASP Top 10 list and understand how each vulnerability happens and can be prevented, read over a handful of bug bounty write-ups to understand the approach people take and pick what you want to hack and learn a lot about it!

### Q: Someone was eager to know, what do you put on your toast?

Butter with jelly or jam

### Q: What’s your worst bug bounty story/experience?

I spent weeks communicating with a platform about an XSS vulnerability that they validated incorrectly only to have it closed as out-of-scope. Takeaway: always read the entire scope!

### Q: If you had to pick one hacker to collaborate with, who would it be?

Frans Rosen :)

### Q: What’s the one feature you would like to see in the platforms?

Putting their full scope in the designated scope area on HackerOne, it makes readability much easier!

### Q: What’s your favorite text editor?

Visual Studio Code

#### [Source](/blog/ama/teknogeek/)

<br/>
---
