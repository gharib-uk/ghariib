---
title: "Kali Linux 10 Release - Moto - The Birth of Kali Linux"
date: Wed, 13 Mar 2013 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 10 Release - Moto - The Birth of Kali Linux
https://www.kali.org/blog/kali-linux-1-0-0-release/images/kali-the-birth.jpg
<br/>

<br/>
Kali Linux, the rising It&rsquo;s been 7 years since we released our first version of BackTrack Linux, and the ride so far has been exhilarating. When the dev team started talking about BackTrack 6 (almost a year ago), each of us put on paper a few &ldquo;wish list goals&rdquo; that we
<br/>
Kali Linux, the rising
----------------------

It’s been 7 years since we released our first version of [BackTrack Linux](https://www.backtrack-linux.org/), and the ride so far has been exhilarating. When the dev team started talking about BackTrack 6 (almost a year ago), each of us put on paper a few “wish list goals” that we each wanted implemented in our “next version”.

### Scrapping it all and starting afresh

It soon became evident to us that with our 4 year old development architecture, we would not be able to achieve all these new goals without a massive restructure, so, **we massively restructured**. We realized it would be easier to start afresh, using new technologies and processes than to try to patch up our existing environment to conform to Debian policies and FSH. This realization brought upon the next question…

### Ubuntu vs. Debian

Once we realized we are free from the bonds of our old environment, we started musing about the base platform we want to build our next penetration testing distribution - the main players on our table were Debian and Ubuntu. With both options heavily weighed and gently avoiding philosophical rants about the pros and cons of each, Debian was our final choice.

### What about the OffSec courses?

Surprisingly enough, with all the new changes we have made in Kali, the user experience remains pretty much the same. Apart from a couple of path changes due to our new FHS compliance, our students should feel little difference between Kali and BackTrack.

### Where’s my /pentest?

Gone. Kaput. Kwisha. Dissipated. FSH compliance has removed the _**/pentest**_ structure from our distribution. Although the _**/pentest**_ directory tree was a signature of our previous distributions for many years, it always brought with it policy questions which could never be satisfactorily answered. For example, when does a tool go in _**/pentest**_, and when should it be placed in the $PATH? Where should a tool like “sqlmap” be placed? Should it be in _**/pentest/web**_, or _**/pentest/database**_? With our new FSH compliant packages, there’s no guesswork left. Everything is in the path and accessible directly, as it should be.

### Kali Linux - what’s in a name?

Hindu Goddess of time and change? Philippine martial art? Cool word in Swahili? None of the above. “Kali” is simply the name we came up with for our new distribution. Why change the name in the first place? With all these significant changes in our distribution, we felt that we needed to convey this in the project name. “BackTrack 6” didn’t do justice to our efforts in the past year, and wouldn’t convey our new message to our users. What’s the new message? We’ll let you find out for yourself.

#### [Source](https://www.kali.org/blog/kali-linux-1-0-0-release/)

<br/>
---
