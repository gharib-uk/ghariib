---
title: "AMA with uraniumhacker"
date: 2017-10-09T02:00:00+02:00
draft: false
type: posts
categories: 
- 
---
# AMA with uraniumhacker

<br/>

<br/>
### Thank you for doing this interview! Can you please introduce yourself?

My name is uraniumhacker and in HackerOne I go with @uranium238. I am 17 years old and currently live in Los Angeles, California. I am originally from Nepal. I have been hacking for about 1.5 years now. I got started through CTF competitions. I competed in a CTF event about 3 years ago and had no idea what I was doing. This motivated me to learn more about cyber security and focus on the world of hacking. I have been hacking on HackerOne most of the time and usually test Yahoo, Uber and other programs. I also like to hack on Google because their security team is super cool and have a fast response time.

### Q: How do you manage your personal life, work, and bug bounties? Do you do bug bounties as a job or a hobby?

I am a full time student at university, and also member of a security club at my university so I do bug bounty only when I have free time after work and college. At the moment, I use bug bounty as a way to learn more about different bugs and web app sec so that I can be better than what I was a day before.

### Q: How long did it take you until you found your first significant/high impact/payout vulnerability?

One of my most impactful/paid bug was found about 6 months into bug bounty. This was found when testing Uber and was found in G Suite. When I found G Suite bug that allowed me to redirect a site if it had certain A records without even verifying it, I started to look into G Suite more. G Suite also has email functionality so I decided to give it a shot. Soon after testing, I found that I could actually use any unclaimed domains that have MX set to G Suite and start listening to emails. G Suite blocked unverified domain from accessing Gmail but they did not block email routes so I was able to use that to retrieve emails for companies. If I remember correctly, they fixed the vulnerability pretty fast and paid out pretty good for it as well.

### Of all the bugs you’ve found, what was your favorite / most interesting?

My most interesting bug has always been and will always be my research on MX records and taking over some of them to listen to emails. Originally this came up with idea from a friend and then was discovered in Uber. Since then, I have used the same kind of idea to do research on MX and have found multiple companies using MX records pointing to domains that either do not exist or they did not claim. This expanded later on as a bug I found on Google suite.

### Q: When and how did you have your breakthrough? When did you realize hacking and bug bounties was something you wanted to dedicate your time to? Please share your insights and the problems you faced to become established Bug bounty hacker?

Main breakthrough was after I found a bug in my school’s gradebook system which allowed me to change password of other users. At that point, I was kind of into bug bounty but had not committed to succeeding in it. Since then, I took it as a challenge to do my best and find at least one High-Critical bug a month. All hackers start from zero in bug bounty (technically with 100 in HackerOne). For me the challenge was to make sure I understand each vulnerability types. I used to ask a lot of questions when I started at first. Whether it was messaging people in the industry that I knew or asking hackers. Sometimes it felt bad to just message and ask when clearly I could have researched it myself. That made me the person who I am today. Despite me constantly messaging these hackers and security personnels they always helped me. Today, I always research before I ask questions and also I am pretty active in helping anyone who needs help. You can message me on Twitter or Slack and I usually respond within 2 hrs.

### Q: What do you do to keep up with all the new trends?

For security: I follow many security researchers in Twitter to make sure I see different research from them and learn from them. I also follow /r/netsec regularly for different security pattern. Finally, HackerOne Zero Daily is another way I am aware of new trends.

For features: If I am testing a company and need to know new features, I usually follow their engineers and security team along with their page in Social media. That way when they let users know a new feature is released, I can test it out right away.

### Q: Do you collaborate with other hackers? Can you name a few?

I do :) It is fun to hack with other hackers because you get to learn a lot of things when working together. Some hackers I work with frequently are: @0x0ibram, @zlz, @dawgyg, @nahamsec, @jobert, @teknogeek and @edoverflow.

Technical Questions
===================

### Q: How do you approach a target? What is your routine like? What is your recon process like? What kind of information do you seek in your information gathering process? And how does this information help you?

This is going to be long so bear with me.

When I approach a target, I approach as a real user and not an attacker. Basically let us say I get invited to a program called Example. I do not jump directly into finding all the bugs I can. I like finding critical bugs that will make the company panic. So what I do is run some recon tools that will gather things like subdomains, directories, ports, different IP ranges etc. To make things easier, I basically have a tool that acts as wrapper all the recon tools and generates result in a nice UI. It outputs every result and if something is vulnerable to common attack like subdomain takeover, it does it for me so that I do not have to waste my time signing in to AWS or other third party service.

Next, the same recon tool also searches for vulnerabilities that I have found before so that I do not keep looking for them in other programs. While that happens, I start browsing the website. Usually I spend about 1-2 hrs using the website trying to note any special pages that could have good impacts. I then go old school on this so I actually take out a notebook and start writing notes. These notes later is converted to a map of the website based on what I see as the critical part. Then I spend the next 1 hr or so actually visualizing bugs.

Visualizing bugs for me goes this way: When taking notes, I write certain parameters that might be serious. After that, I start to write what kind of bug it might have and which params. Burp comes handy on this, because I save all the params that I visited in Burp so I go back to them and see each of them. Once I do that and have some pages to target with specific vulnerability, I actually put them to use and see if it actually works. For some programs, I have had good success with this for example with Yahoo so far I have ~90% rate with most of the theoretical bugs actually being true. In some pentests I have done, the success rates ranges between 80-90%.

For recons I use tools that are available online for free for example: Shodan, Censys and certificate tools to gather information about the company. Sublist3r and HostileSubBruteforcer to gather subdomains. Github to look for leaked keys, usernames, password etc.. And some Google dorks to find files in the company. I usually approach the whole recon process by a tool that I made. It is still a work on progress and will be made to be a whole website in about 6 months and then publicly be disclosed for others to use. This tool basically does the whole recon process for you.

Going through everything, somethings that I use often: 1. Shodan - both their CLI and UI 2. Censys - specific search parameters 3. IFTT - as a trigger to detect execution of bXSS. 4. Google Dork - find special directory or pages that can help exploiting a bug. 5. Facebook & Google Ceritificate transparency - find old and new subdomains. These sometimes help with subdomain takeover 6. Tools in Github like: - Sublist3r - list subdomains - Gitrob - to analyze github repos - HostileSubBruteforcer (This helps a lot with subdomain takeover, shout out to @nahamsec). 7. HackerOne policy update to know of new potential targets

### Q: Do you always look for all vulnerabilities types when you approach a website?

Not always. I imagine vulnerability types as I use the website but I never test the site. If it is a target, I use it first then test it. If it is a site that I do not have permission to test, I do not even attempt looking for vulnerabilities on them because that is basically a type of unethical hacking. This also stands for simple bugs like XSS.

Going bug bounty wise: I do not look for all vulnerability types when approaching a target. This is because low hanging fruits are often times a duplicate. What I usually do is approach it as a pentest and look for specific vulns in the code. For example, I would completely test an app and try to find as much IDORs as I can. If there are chances of having an IDOR in low impact place I will test it at the end (which is probably after a week). This way I can reduce the number of duplicates and submit valid and high quality reports to companies.

### Q: Do you use any tools? Do you have your own tools that you have written to automate/facilitate your work? What Burp extensions do you use? Is there a tool that not a lot of people use that you think they should?

Burp is one of the most used tools for me. I basically have it running always so that if I am browsing a target, all the request it mades is linked. Other than that, I have a chrome extension that I made which does almost the same as Burp. It basically saves all the request until I stop it and then saves it to a .txt file for me to analyze. Other than that, I have my own tools that I periodically update and run to do recons. After all tools, I like to do manual testing for many applications. When finding common low hanging vuln, the chrome extension I mentioned earlier looks for CSRF in most cases and has about 85% success rate so far.

### Q: This is one of our most popular questions: How do you test for Server Side vulnerabilities such as RCE, SQLi, etc?

RCE and SQLi are two of my favorite bugs (I am sure they are of others as well). For RCE there is not an exact general guidelines that I follow. Often times, I like to see how a website functions. For example if it seems that it is trying to retrieve something from another website, I put a open FTP protocol and then check the log to see how that responds. If there is something weird that happens then I note that and do other tests. In the end, I approach back to the parameter and play with for about 1-2 hrs and see if I can find a pattern. Another way to do RCE tests are using looking for jenkins instances and such by trying either to bruteforce their subdomain or using Shodan to see if it has detected some services like Jenkins running on them.

Same goes for SQLi except it is more structured because there are some tests that can be done to detect SQLi easily. I usually approach it almost the same way as RCE, except I check it only if i know for certain that it works on retrieving something from the server. After that, I do a recon on the website to see how it is structured and test different sql injections. Common test to run is a simple quote (‘) or backslash checks as Albinowax mentioned in his talk at BlackHat: https://www.blackhat.com/docs/eu-16/materials/eu-16-Kettle-Backslash-Powered%20Scanning-Hunting-Unknown-Vulnerability-Classes-wp.pdf

### Q: How often do you find a bug that has been overlooked after a bounty program has been established and a horde of researchers have been digging?

It has been quite few times I have had such encounter. Honestly telling, I never tested some public applications because I thought every bug would have been found but testing them now has made me realize that is not always the case. This in my opinion is primarily because each hackers approach a target differently. We each have our expertise and weakness and we learn every day. So while a hacker might target an app one way another will target it in different manners. This basically opens door to different vulnerabilities being found everyday in companies like Uber, Facebook, Google etc.

### Q: Do you think being a pentester, web developer, or being in a related field, helps you with bug bounties? Where should they start?

Being in a computer science field helps a lot. When you start doing bug bounty you will be able to use your talent to target an application. For example, if you are a web developer, you should have some awareness about security issues. Some web developers that I know use their knowledge to create tools that make it easier for them to deploy secure codes. These web developers if they were to join bug bounty can use the same strategy to create tools that can help them to target applications. Pentesters will also have an expertise to bring. They usually do both blackbox and whitebox so having knowledge about security is a plus point for many. Personally, I believe that no matter what your expertise is, to be a better bug bounty hacker you need to have patience and curiosity. Keep learning and be calm as you work with companies. Read books, blogs, watch videos and write your own codes. Breaking your own code helps you realize how to make it secure which assists a lot in bug bounties.

Time to wrap it up!
===================

### Q: What kind of music do you listen to?

Musics help me a lot to focus when doing bug bounties. I mostly listen to Linkin Park because they have been my favorite band since childhood. I also play some musics from the iHeartRadio app.

### Q: What do you do when you aren’t hacking?

Most of the time it is enjoying time with family, studying and working. Once in awhile, I go out with friends to the beach or go to a park and play soccer for a while.

### Q: What kind of impact/role have bug bounties played in your life?

Bug bounties has helped me a lot. It has taught me to be a better at communications, be patience and to work hard. It has also helped me with financially because right now I am basically paying my whole tuition with bug bounty and also I like to use my bug bounty to help others in need whether through charity or by sponsoring events.

### Q: What is an advice you received as a beginner that helped you with your bug bounty career?

Main advice that has helped me to success is: Read & Research. When approaching a target make sure you know what you are dealing with. It is not just a bug that you are going to find, you are trying to find something that can give you valuable information about a company. That is why, I always read api docs, research every day. One reason why I do not hunt for months is because, I take that break as a way to learn more and experience more whether that is learning a new programming language or learning about a new vulnerability type.

### Q: What is one area of hacking (web, mobile, hardware, etc) you wish you knew more about / plan on focusing your learning on?

My ultimate goal is to learn hacking in mobile and hardware. I am more of a web application security guy so mobile and hardware security is relatively new. When testing mobile apps, I test it generally just by using Burp which helps sometimes but not all the times. Hardware is my weakest because I do not have much experience with it.

### Q: If someone with basic technical background asked you, “where should I start?”, what are 3 things you would recommend they do before diving into bug bounties?

Regarding knowledge wise I usually recommend anyone to first start reading Web Application Hackers’ Handbook both Edition 1 and Edition 2.

Once you have some grasp of what each bug can do, start to read Web Hacking 101. I recommend web hacking 101 because once you have some experience with what bugs can do, you can use this book to actually see them in real life. Author, Pete Yaworsk does a wonderful job in explaining each vulnerability ( he explained my Uber bug way better than I can ).

Then next thing to do is research. One thing I usually tell hackers is that before you message someone asking for help or to clear your doubt, do a complete research on what you want to ask. Most of the times, answer to the questions can be found by simply a google searcher. If you still have doubts after that, then first ask permission to message a hacker then do it. Asking for help is never criticized in the industry but that does not mean you ask without even knowing if the question is already answered.

I am going to break the rule here and give a 4th recommendation: Never think that you will not be able to succeed. Success comes with hard work so research continuoulsy and learn and digest new things each day. Never try to get on the top hastily, take your time. When it comes to hacking and mutual respect with companies I believe it is not the matter of how much bugs you found. Quality > Quantity. Make sure your report are detailed and explains a vulnerability clearly. Never take a shortcut.

### Q: Someone was eager to know, what do you put on your toast?

Simple answer: nutella.

### Q: What’s your worst bug bounty story/experience?

So far, I have not had an experience where I am really mad or angry at a company. There is one case however, where I am not mad at that platform/company but instead I am concerned about how the bug was handled. I am going to respect privacy of the company here and not explain everything about the bug. I found a pretty critical bug when doing a recon once. The domain however was out of scope so I decided not to report it. When discussing with the platform personnel, they recommended that I report it but warned that it might be closed as N/A. Personally I did not care if it was going to be closed as N/A, I wanted to make sure the company will fix it and not be impacted by it. As expected the bug was closed as N/A but the platform said they will let the company know so they can fix it. It has been almost a year and the bug is not fixed and it is still a serious issue.

### Q: If you had to pick one hacker to collaborate with, who would it be?

This is a hard question, because I like to collaborate with all hackers and learn from them. Most of them have been in the industry including bug bounty for a long time. However if I had to really pick one it will be @yaworsk. I have seen his work in Web Hacking 101 and also just in regular bug bounties. I would love to work with him on some programs.

### Q: What’s the one feature you would like to see in the platforms?

Most of my feature request are already there. If I were to ask for one then it would be a 2FA service for hackers. I know HackerOne already has one for companies but having one for hackers would be great. That way, there is no risk at all regarding security of bugs we report.

### Q: What’s your favorite text editor?

I like to use just any text editor (except Vim 😐 ). If I am using a VPS then it is mostly just Nano.

#### [Source](/blog/ama/uranium238/)

<br/>
---
