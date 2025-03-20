---
title: "AMA with jackhcable"
date: 2017-08-22T02:00:00+02:00
draft: false
type: posts
categories: 
- 
---
# AMA with jackhcable

<br/>

<br/>
### Thank you for doing this interview! Can you please introduce yourself?

My name is Jack Cable and I’m from Chicago. I got into hacking by inadvertently finding a vulnerability in a financial site which allowed me to send negative amounts of money to other users, effectively stealing money from their account. Since then, I’ve participated in a mix of public and private bug bounty programs on HackerOne and Cobalt.

### Q: How do you manage your personal life, work, and bug bounties? Do you do bug bounties as a job or a hobby?

I’m a student and do bug bounties on the side (I plan to major in either math or computer science). I don’t really have a set timeframe for hacking, just whenever I have the time and feel like it.

### Q: How much time do you spend on Hunting for Bugs? On average, how many bugs do you think you report per month?

On average, I probably spend around 5 hours a week hunting bugs, but this can vary. Since I started, I’ve reported 300 vulnerabilities on Cobalt and HackerOne, which comes out to 15 vulnerabilities a month.

### Q: How long did it take you until you found your first significant/high impact/payout vulnerability?

Immediately! The first vulnerability I found was a critical vulnerability on a financial site (ChangeTip) which allowed me to steal any amount of money.

### Q: Of all the bugs you’ve found, what was your favorite/most interesting?

Several, I seem to have a knack for finding ways to get an infinite amount of money on financial sites. One of those, in particular, was in a private HackerOne program which allowed a user to create a transaction for a negative amount, but wouldn’t actually process the transaction unless it was a positive amount. I thought this was unexploitable until I found an undocumented parameter which allowed me to send multiple transactions at a time; I could then send $10 to one user and $-9 to another. This would only charge my account $1 (the sum of the transactions), but generated $10 in another user’s account.

### Q: When and how did you have your breakthrough? When did you realize hacking and bug bounties was something you wanted to dedicate your time to? Please share your insights and the problems you faced to becoming an established bug bounty hacker?

I’ve always loved the challenge of trying to break into systems, and that has motivated me to explore white hat hacking and bug bounty programs. The hardest part is likely getting started with public programs, I would suggest looking at newest programs on HackerOne and starting from there.

### Q: What do you do to keep up with all the new trends?

There’s tons of great blogs and write-ups available to learn from. I suggest reading any of the blogs available [here](https://github.com/djadmin/awesome-bug-bounty) as well as [HackerOne’s newsletter](https://www.hackerone.com/zerodaily). Some great write-ups are posted on the Bug Bounty Forum Slack group as well.

### Q: Do you collaborate with other hackers? Can you name a few?

I mostly work myself, though I would be open to collaborating.

Technical Questions
===================

### Q: How do you approach a target? What is your routine like? What is your recon process like? What kind of information do you seek in your information gathering process? And how does this information help you?

I’ve recently been trying to improve my recon process and incorporate more tools. For finding subdomains, I use Jason Haddix’s [domain tool](https://github.com/jhaddix/domain), after which I test to see which subdomains are responding.

I’ve had the most success with medium-sized sites, where I try to comprehensively understand every feature of the site. For finding dynamic content, I explore every page a site has to offer and supplement this with specific Google searches for content (a good resource is here). Additionally, I look through JavaScript source code to find undocumented API endpoints which may be vulnerable.

Once I’ve discovered dynamic content, I test for whatever it might be vulnerable to. This includes anything from XSS to SQLi, and less commonly looked at vulnerabilities like race conditions (I’ll elaborate more on these later).

### Q: Do you always look for all vulnerabilities types when you approach a website?

Typically, yes, I look for everything. I’ve found that on larger applications it’s easy to get overwhelmed by the amount of content, and best to section yourself off to one portion of a site and do a comprehensive test for many vulnerability types.

### Q: Do you use any tools? Do you have your own tools that you have written to automate/facilitate your work? What Burp extensions do you use? Is there a tool that not a lot of people use that you think they should?

Tools I use:

-   [XSS Hunter](https://xsshunter.com/) for blind XSS
-   [Domain](https://github.com/jhaddix/domain) or [Sublist3r](https://github.com/aboul3la/Sublist3r) for finding subdomains
-   [Wfuzz](http://www.edge-security.com/wfuzz.php) for discovering content
-   [Sqlmap](http://sqlmap.org/) for potential SQL injections
-   [XSS Hamster](https://github.com/bugbountyforum/XSS-Radar) for fuzzing XSS :)

### Q: This is one of our most popular questions: How do you test for Server Side vulnerabilities such as RCE, SQLi, etc?

I don’t have much experience with RCE or SQLi (actually found my first RCE yesterday!), but I hope I can offer some tips on an often overlooked vulnerability: race conditions. A race condition occurs when an application has a delay between checking a state (i.e. if a user has enough money in their account to send a transaction) and executing the user’s request.

I’ll detail a few race conditions I’ve found in a variety of contexts. To test for race conditions in BurpSuite, right click a request and select ‘Copy as curl command’. Then, in the command line, execute the request in the form ‘(request) & (request)’. This repeats the request 2 times, sending it asynchronously.

**ChangeTip - race condition in transferring money (with permission to disclose):**

ChangeTip was a bitcoin tipping site that allowed users to send off-chain bitcoin transactions to other users. A user had a pocket with a USD balance and a BTC balance, and could transfer money between them.

Consider if a user had a balance of $1 in their USD balance. In BurpSuite, intercept a request to transfer $1 from the USD balance to BTC, and execute this asynchronously twice, as outlined above. The user now has $-1 in their USD balance and $2 in their BTC balance. This can be repeated to get any amount of money in an account:

![Poc](../../../images/poc.png)

(unfortunately, I was not rewarded $12 billion)

This is a more basic type of race condition, where repeating the same request multiple times leads to interesting results. I’ve found this common in financial sites, either in sending money or canceling pending transactions.

**Instacart - race condition in redeeming coupons (https://hackerone.com/reports/157996)**

Another common type of race condition is in redeeming promo codes, gift cards, or coupons. In Instacart, I found that I could redeem the same promo code twice, which would give double credit to my account. After a fix was issued, I discovered a bypass by redeeming two different promo codes. This demonstrates another case of race conditions, where it is necessary to test different values.

**Race condition on private program exposes SQL query**

The last race condition I’ll outline was on a private program. A user could process a unique, one time action with an item. I executed this request twice, and this exposed an unintentional error message containing the SQL query. This was because the database had the item set to be unique, and the server normally checked if the user had already processed the action. In this case, though, the server verified for both requests that the user had not yet added the item, leading to an SQL error being returned.

### Q: How often do you find a bug that has been overlooked after a bounty program has been established and a horde of researchers have been digging?

Many times! There’s always more bugs to find, [my favorite example](https://hackerone.com/reports/163464) is with a find in Legal Robot, where the information of every user was being sent to everyone via websockets. This was when they launched after having a private program.

### Q: Do you think being a pentester, web developer, or being in a related field, helps you with bug bounties? Where should they start?

Yes, I had a background as a web developer before starting hacking. It helps both ways, my knowledge as a web developer has helped me learn vulnerability types and my hacking experience has allowed me to build more secure applications (I dare you to break mine!).

Time to wrap it up!
===================

### Q: What kind of music do you listen to?

Indie folk / folk rock

### Q: What do you do when you aren’t hacking?

I’m a student, so school + homework takes up a lot of my time. I enjoy working on math, board games, traveling, and coding.

### Q: What kind of impact/role have bug bounties played in your life?

I’ve gotten to know tons of awesome hackers and have been able to learn from them. I also plan to pay for college with them, so that’s a huge help.

### Q: What is an advice you received as a beginner that helped you with your bug bounty career?

There are going to be days when it’s difficult to find any vulnerabilities, and days when bugs come easily. It’s important to keep trying to approach targets in different ways, and switch targets once in a while.

### Q: What is one area of hacking (web, mobile, hardware, etc) you wish you knew more about / plan on focusing your learning on?

Hardware hacking or reverse engineering seems interesting, since it would be a completely different challenge compared to web hacking,

### Q: If someone with basic technical background asked you, “where should I start?”, what are 3 things you would recommend they do before diving into bug bounties?

1.  Learn programming. It helps tons to look at hacking from the other side.
2.  Familiarize yourself with basic vulnerability types and how to find them. [The OWASP Top 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) is a good place to start.
3.  Read through disclosed reports and write-ups.

### Q: Someone was eager to know, what do you put on your toast?

Nutella’s good on just about anything.

### Q: What’s your worst bug bounty story/experience?

I’ve had several cases where a clear vulnerability was not accepted. It’s important to learn where to pick your battles, and move on from them.

### Q: If you had to pick one hacker to collaborate with, who would it be?

Probably Frans Rosén – subdomain takeover ninja.

### Q: What’s the one feature you would like to see in the platforms?

An ability to collaborate on writing a report and share reputation/bounties would be nice, and could encourage hackers to work together.

#### [Source](/blog/ama/jackhcable/)

<br/>
---
