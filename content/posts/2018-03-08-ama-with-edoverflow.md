---
title: "AMA with edoverflow"
date: 2018-03-08T01:00:00+01:00
draft: false
type: posts
categories: 
- 
---
# AMA with edoverflow

<br/>

<br/>
### Thank you for doing this interview! Can you please introduce yourself?

My name is Ed. I am a web designer, developer, and security researcher. I am currently studying computer science at the [ETH Zürich](https://www.ethz.ch/en.html) and work for HackerOne as a security analyst. My interest in security actually started with photography — I went from photography, slowly became interested in design, development, and then finally got into security. How do you manage your personal life, work, and bug bounties? Do you do bug bounties as a job or a hobby?

### Q: How do you manage your personal life, work, and bug bounties? Do you do bug bounties as a job or a hobby?

Bug bounty hunting is something that I do in my spare time. I do not depend on it so it is fairly easy to balance it with my everyday life as a student. That being said, if I would like some pocket money then I might invest a bit more time into hunting.

### Q: How much time do you spend on Hunting for Bugs? On average, how many bugs do you think you report per month?

Timewise, I currently spend about four hours. I used to invest a lot more time, but I have found that in order for my interest to remain healthy I must not overdo it and spend ages hunting. I believe I find about three bugs a month on bug bounty programs.

### Q: How long did it take you until you found your first significant/high impact/payout vulnerability?

My first fairly significant issue was on a non-platform based bug bounty program and was discovered a couple of months after having started bug bounty hunting. Admittedly, I had already been interested in security beforehand, but the whole bug bounty concept was new to me.

### Q: Of all the bugs you’ve found, what was your favorite/most interesting?

I cannot single out on specific vulnerability, to be honest. There are so many that I love looking back at and reminding myself of the fun story behind the bug. My favourite vulnerabilities tend to be the ones that really have a story behind them. For instance, I recently came up with the idea of looking into namespaces and seeing whether or not certain targets reserve them. At first, the issue was purely theoretical, but then applying it into context and being surprised by the fact that the little idea I had in my head actually works in practice is what sticks with me.

### Q: When and how did you have your breakthrough? When did you realize hacking and bug bounties was something you wanted to dedicate your time to? Please share your insights and the problems you faced to becoming an established bug bounty hacker?

DEF CON and H1-702 were the turning points in my career. I felt compelled to share the various things that I had learned so far on my bug bounty journey and motivated to look further in order to discover more.

![](https://user-images.githubusercontent.com/18099289/36700329-a4aacb36-1b4f-11e8-90dc-8842455fff44.png)

Tom and I during H1-702 — Image courtesy of HackerOne

### Q: What do you do to keep up with all the new trends?

For general trends, I follow Twitter, blogs, Slack channels, Hacker News, and the Hacktivity feed on HackerOne. On top of that, I love chatting with people and attending local events in order to stay updated.

### Q: Do you collaborate with other hackers? Can you name a few?

All the time! [Ben Sadeghipour](https://twitter.com/NahamSec) gave me one of the biggest pieces of advice during H1-702 and that is to collaborate with other members of the industry. I find myself hacking with people all the time from all around the world. Usually, we collaborate by messaging each other, but sometimes we even go so far as to hack via a video call or in person. The people that I collaborate the most with are [Gerben Javado](https://twitter.com/gerben_javado) and [Tom Hudson](https://twitter.com/TomNomNom). Even [@mongobug](https://twitter.com/mongobug) gives me some advice every now and then. The security industry is full of people that want to help and collaborate; all you have to do is get in touch with them and network.

Technical Questions
===================

### Q: How do you approach a target? What is your routine like? What is your recon process like? What kind of information do you seek in your information gathering process? And how does this information help you?

I love experimenting! This means I spend quite a lot of time practicing in a controlled environment and looking for unusual, but good ways of approaching a target. By doing research on reconnaissance processes I am able to find techniques that popular tools do not use and then scripting them myself. Talking about scripting, it is vital that hunters automate as much of the reconnaissance process as possible. This will give you the upper hand when approaching a target, especially a competitive one or a target with a large scope. My personal reconnaissance wrapper script finds subdomains, open ports, various directories, checks the target’s DNS records, takes screenshots of all the endpoints, and much more. Once my reconnaissance wrapper is done scanning, it stores the results in a git commit. Some of these features are included in the [megplus](https://github.com/EdOverflow/megplus) script. ![megplus in action](https://user-images.githubusercontent.com/18099289/36700626-a3c48968-1b50-11e8-9e25-56264e17c6a0.png) Talking about automation, an aspect that I did not include in my process for a long time was monitoring — mainly keeping track of variations in your reconnaissance script’s output. I now use the results in the git commits to notify me of any changes. A quick `git diff` is all it takes to find differences in your script’s results.

### Q: Do you always look for all vulnerabilities types when you approach a website?

No, definitely not. I find that my approach varies based on the target. From experience, I have noticed that patterns arise when hunting on certain types of targets. For instance, companies from the financial sector that handle payments will require certain functionality that when implemented incorrectly cause certain issues. On top of the target type, I also look for certain features that could potentially open the target to various issues. When I stumble across webhooks, my immediate thought is Server-Side Request Forgery, since the integration has to connect to an external instance with a user-supplied address.

### Q: Do you use any tools? Do you have your own tools that you have written to automate/facilitate your work? What Burp extensions do you use? Is there a tool that not a lot of people use that you think they should?

Whenever I cannot find a tool that covers a certain task, I code it myself. In my opinion, knowing the default set of tools that come with your Linux distribution is key to making your life easier while hunting. Common third-party tools that I use when targeting web applications include Sublis3r, knockpy, dirsearch, [meg](https://github.com/tomnomnom/meg) (actually any tool developed by Tom), [LinkFinder](https://github.com/GerbenJavado/LinkFinder), virtual-host-discovery, wpscan, webscreenshot, [twitterBFTD](https://github.com/misterch0c/twitterBFTD), [broken-link-checker](https://github.com/stevenvachon/broken-link-checker), and Frans Rosén’s little parameter collection bookmark thingy:

Aside from these tools that I used during the hunting phase, I find it is important we do not forget about tools that can help you during the reporting process. Frans Rosén has [a template generator project](https://github.com/fransr/template-generator) that allows me to quickly put together reports by simply filling in the blanks. This is especially useful for reporting issues to competitive programs, as the tool will save you a lot of time. For low hanging fruit I do not even bother writing a report any longer.

### Q: This is one of our most popular questions: How do you test for Server Side vulnerabilities such as RCE, SQLi, etc?

Make sure you are very familiar with your proxy of choice, and practice finding these type of issues in CTFs and in lab environments. Often times, writing my own vulnerable software has helped me get a better understanding of how to exploit server-side vulnerabilities.

### Q: How often do you find a bug that has been overlooked after a bounty program has been established and a horde of researchers have been digging?

Time and time again. I understand the mental barrier that a popular and competitive target presents, but I am constantly amazed by what I find on these popular programs. If you are really struggling on a competitive program, I highly recommend keeping track of new code and features being pushed. This can be achieved by either following the development news feed or coming back to the same target and double-checking to see if a new feature was added. On top of that, sometimes targeting the core functionality of the application and looking for logic flaws can be extremely rewarding on older programs. Take [my recent Keybase reports](https://hackerone.com/reports/307675), for instance, Keybase’s program has been around forever; all I did was target the main component of the application and ask myself about which design issues might not have been considered when creating Keybase. The reason why these issues are sometimes overlooked is because, for one they are not your everyday vulnerability, and two we often assume that the core functionality will be the most rigid component of the application.

### Q: Do you think being a pentester, web developer, or being in a related field, helps you with bug bounties? Where should they start?

Yes, absolutely. Although not a requirement — as I have seen with some successful bug bounty hunters — having a background in a technology-related field might give you a head start. Whenever someone asks me how to get started, I always respond with a very simple philosophy: “Learn to make it; then break it”. By this I mean, learn how to build the components you will be breaking. You are more likely to notice patterns in targets and better understand what is required to exploit the issue. On top of that, being able to code might also help you when writing detailed proof of concepts or a tool to help automate part of your hunting process. In my experience, my development and design background has helped me create unusual and convincing proof of concepts.

Time to wrap it up!
===================

### Q: What kind of music do you listen to?

I listen to a wide variety of music. In my spare time, I like to play the guitar so I love listening to Jimi Hendrix, Guns & Roses, and Van Halen, but I also find myself listening to electronic music including Dubstep and EDM trap.

### Q: What do you do when you aren’t hacking?

Swimming, playing the guitar, studying, going to security-related events, and photography.

### Q: What kind of impact/role have bug bounties played in your life?

I can finally fly to the US, hack the US Airforce, and not get arrested at the border. How crazy is that?

### Q: What is one area of hacking (web, mobile, hardware, etc) you wish you knew more about / plan on focusing your learning on?

Not necessarily hacking related, but currently, I am more focused on better understanding the fundamental concepts of technology. Studying computer science is really helping me with that. I find the bits and bytes that power our technology to be a fascinating topic that I really want to learn so much more about.

### Q: If someone with basic technical background asked you, “where should I start?”, what are 3 things you would recommend they do before diving into bug bounties?

1.  [https://bugbountyguide.com/](https://bugbountyguide.com/)
2.  [https://edoverflow.com/2017/bugbounty-faq](https://edoverflow.com/2017/bugbounty-faq)
3.  [https://google.com](https://google.com)

### Q: Someone was eager to know, what do you put on your toast?

¯\\\_(ツ)\_/¯

### Q: What’s your worst bug bounty story/experience?

I have had cases where the target has threatened to sue me, but one of my earliest experiences with disclosing a vulnerability was to a company that was very grateful for my report and then suddenly asked me where I lived. Immediately, I thought something was up. Why would a company want to know where I live? Were they trying to take legal action against me? Were they going to call the feds on me? After about ten emails trying to figure out what they were after, I finally realised they wanted to offer me a job. I am pretty sure after about the fifth email they lost interest in me because I could not put two and two together.

### Q: If you had to pick one hacker to collaborate with, who would it be?

Russia.  
  
  
  
  
  
  
  
  
  
On a serious note, for a long time it was [filedescriptor](https://twitter.com/filedescriptor) and then that dream became a reality. It was a very successful session.

### Q: What’s the one feature you would like to see in the platforms?

I could write a book on this subject that is how many features I would like to see implemented on bug bounty platforms.

### Q: What’s your favorite text editor?

Sublime Text and Vim. I used to be a hardcore Atom user but had to give up and move to Sublime Text.

#### [Source](/blog/ama/edoverflow/)

<br/>
---
