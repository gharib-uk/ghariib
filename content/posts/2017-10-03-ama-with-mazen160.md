---
title: "AMA with mazen160"
date: 2017-10-03T02:00:00+02:00
draft: false
type: posts
categories: 
- 
---
# AMA with mazen160

<br/>

<br/>
### Thank you for doing this interview! Can you please introduce yourself?

Mazin Ahmed. I’m originally from Sudan, and based in the United Arab Emirates. I started participating on bug bounty programs since 2013. I have been intrested in hacking and security since a young age. I liked breaking locks, having things do things that is not supposed to do. In 2013, I dedicated myself into the information security career.

### Q: How do you manage your personal life, work, and bug bounties? Do you do bug bounties as a job or a hobby?

My life is a mix of family, university, work, and bug bounty hunting. I’m working as an information security specialist and a penetration tester. Also, I’m establishing my first security start-up this year.

A typical day is starting at 5:00AM, I spend a time at checking messages and emails. Then I start reading news, new write-ups, researches, tools, and exploits. At, 7:00AM, I head to university. I finish around 5PM, I come back, where I take a rest, and start working again on my TODO lists until 12PM. I close the day by reviewing the day’s tasks and preparing tasks for the next day. Sometimes the time changes, but this is my typical day routine.

I do bug bounty hunting whenever I have time from work. In previous years, I have been doing bug bounty hunting the entire time. I enjoy bug bounty hunting, especially cool and challenging programs.

In case you find it boring, I really love my life :)

### Q: What platforms or popular (public) programs have you hacked on?

I’m mainly active on BugCrowd and HackerOne. Popular programs that I reported security vulnerabilities on are: Facebook, Twitter, Oracle, LinkedIn, United States Department of Defense, and many…

### Q: How much time do you spend on Hunting for Bugs? On average, how many bugs do you think you report per month?

I used to spend 30 hours a week on bug bounty hunting. Currenlty, due to time shortage, I spend 7-10 hours a week on bug bounty hunting.

### Q: How long did it take you until you found your first significant/high impact/payout vulnerability?

It took me around a week as far as I remember. It was a cross-site scripting vulnerability on an Oracle-owned service.

### Q: Of all the bugs you’ve found, what was your favorite/most interesting?

Most of the bugs are interesting in a way or another actually. I liked where I wrote struts-pwn, the exploit for CVE-2017-5638, and used it to scan a number of US. Military services. I found 4 full-blown RCEs!

Also, I remember the time I got multiple SQL injection on Symantec that allowed me to access 60+ DBs of Symantec.

I’m living in a nice hacking adventure! :)

### Q: When and how did you have your breakthrough? When did you realize hacking and bug bounties was something you wanted to dedicate your time to? Please share your insights and the problems you faced to becoming an established bug bounty hacker?

I have been interested in information security for years. It’s my hobby essentially.

### Q: What do you do to keep up with all the new trends?

-   Feedly: with reddit/r/netec, a number of exploit databases and websites. Different news websites.
-   Twitter.
-   Slack – bugbountyforum

If you’re not using Feedly, you should definitely use it.

### Q: Please share your insights and the problems you faced to become established Bug bounty hacker?

I self-taght my self web security, I didn’t learn in it a school or university. It was difficult initially, as I didn’t know where and how start. With enough dedication, it was possible to reach the current level. I’m always learning, and will never stop at a certain place.

### Q: Do you collaborate with other hackers? Can you name a few?

I collabrate with many awesome hackers. I mostly collabarate with: Sebastian Neef @gehaxelt, Justin Kennedy @jstnkndy, @EdOverflow, Ahmed Aboul-Ela @aboul3la, Ben Sadeghipour @nahamsec, Oliver Beg @smiegles.

Technical Questions
===================

### Q: How do you approach a target? What is your routine like? What is your recon process like? What kind of information do you seek in your information gathering process? And how does this information help you?

Information gathering is always the key. I like how we can sometimes finish an egagement without even touching the company’s network using open-source intellegince (yes, there are actual hackers that does that!).

I use an internally-built application my start-up is developing that does almost all the pre-testing. From this point, testing is way much easy for me.

Scripting is a mandatory skill that every person in security should learn. I write a lot of quick Shell and Python scripts to automate work.

### Q: Do you always look for all vulnerabilities types when you approach a website?

I try my best to cover as much as possible. I take longer time on larger scopes to make sure I don’t miss anything.

I’m interested in high severity that can have an actual real-time impact, and creative bugs that might not necessary severe, but creative how it could be exploited.

### Q: Do you use any tools? Do you have your own tools that you have written to automate/facilitate your work? What Burp extensions do you use? Is there a tool that not a lot of people use that you think they should?

I use Burp Suite professional for web-related testing. I also, use Dirsearch for directory brute-force. BFAC for backup-artificats testing. SQLMAP for SQL injection exploitation. I like httpscreenshot, very interesting approach for pre-testing.

I spend time in building my fuzzing lists manually. I like to take time to get ready before any engagment or bug bounty hunting session.

I also enjoy taking open-source security tools and improve it, then using it in bug bounty hunting.

### Q: This is one of our most popular questions: How do you test for Server Side vulnerabilities such as RCE, SQLi, etc?

I mostly test it manually. I focus on OOB and time-based testing on server-side vulnerabilities. I always setup an external server to receive connections. Also, Burp Suite Collaborator Client is very handy.

### Q: How often do you find a bug that has been overlooked after a bounty program has been established and a horde of researchers have been digging?

Always have in mind that there is something that was missed. Also, have in mind that companies are constantly pushing code. New code means potential created vulnerability. 

### Q: Do you think being a pentester, web developer, or being in a related field, helps you with bug bounties? Where should they start?

Of course, knowing how things work will make your works way easier. I recommend learning programming and scripting.

Time to wrap it up!
===================

### Q: What kind of music do you listen to?

Anything that sounds good :)

### Q: What do you do when you aren’t hacking?

I spend time with family and friends.

Favourite sport is mixed-martial arts and generally all sports that are full of intense.

### Q: What kind of impact/role have bug bounties played in your life?

I haven’t reached 20 yet, and I’m satisfied with my professional life, and what I have reached till today (and I have hundreds of goals and plans for the future). Probably the best thing to say for a person in my age.

Bug bounty helped me approaching the world of information security. Without bug bounty programs, I don’t think I would have make it.

### Q: What is an advice you received as a beginner that helped you with your bug bounty career?

Never give up learning, reading, and practicing. Never stop at a certain place,

### Q: What is one area of hacking (web, mobile, hardware, etc) you wish you knew more about / plan on focusing your learning on?

I’m planning to work more on reverse engineering. It’s a very interesting area for sure. Also, hardware hacking is really interesting.

### Q: If someone with basic technical background asked you, “where should I start?”, what are 3 things you would recommend they do before diving into bug bounties?

I have written a guideline on “starting in Infosec - 101”. Feel free to read it at http://blog.mazinahmed.net/2017/08/starting-in-infosec-101.html

### Q: Someone was eager to know, what do you put on your toast?

What normal human beings put on their toasts :)

### Q: What’s your worst bug bounty story/experience?

Overall, I don’t face a lot of bad experiences. But here are my worst experieneces: - Marking an account takeover as an invalid finding. - Marking an RCE as won’t fix issue.

### Q: If you had to pick one hacker to collaborate with, who would it be?

I can’t specify a person, as every hacker I know is very talented at a certain area. I would collaborate with as much hackers as I can :)

### Q: What’s the one feature you would like to see in the platforms?

Pay on triage should be mandatory for programs.

### Q: What’s your favorite text editor?

ATOM, nano, gedit, and geany are my favourites

#### [Source](/blog/ama/mazen160/)

<br/>
---
