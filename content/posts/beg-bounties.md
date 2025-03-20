---
title: "Beg Bounties"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

![Beg Bounties](https://www.troyhunt.com/content/images/2021/11/ElBZmahX0AEUKOc.jpg)

When someone passed me hundreds of thousands of records on kids taken from CloudPets a few years ago, I had a nightmare of a time getting in touch with the company. They'd left a MongoDB instance exposed to the public without a password and someone had snagged all their data. Within the data were references that granted access to voice recordings made by children, stored in an S3 bucket that also had no auth. So, why didn't CloudPets respond to attempts to contact them? Their CEO later explained it very succinctly:

<blockquote class="twitter-tweet" data-conversation="none"><p lang="en" dir="ltr">"We did have a reporter, try to contact us multiple times last week, you don't respond to some random person about a data breach.</p>— Michael Kan (@Michael_Kan) <a href="https://twitter.com/Michael_Kan/status/836593923304767488?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">February 28, 2017</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Problem is, random people are _precisely_ the sorts of people that find data breaches. I mean, who _wouldn't_ be random in this situation?!

A little later and I'm attempting disclosure to Adult-FanFiction:

![Beg Bounties](https://www.troyhunt.com/content/images/2021/11/Picture1.png)

I always try to provide enough information to independently verify the incident thus making it easy to establish the legitimacy of my message. I'd done this so many times by then that I was _very_ conscious of how scammy these messages can look. Alas, all reasonable measures were exhausted without response, I loaded the data into Have I Been Pwned (HIBP) and _then_ they took notice:

![Beg Bounties](https://www.troyhunt.com/content/images/2021/11/Adult-FanFiction.png)

In order of sentences: Yes there is, that's bullshit, that's true, that's also bullshit, no they won't, they never did. That public forum post was later removed, but I always back these things up because you just never know when common sense may prevail 🙂

Why are companies so skittish about responding to disclosure notices like these? In part, because there are those amongst us who attempt to run what amounts to a digital protection racket in order to make some quick bucks. Here's an example:

![Beg Bounties](https://www.troyhunt.com/content/images/2021/11/image.png)

Ooh, sounds nasty! Here's the attachment (shout out to Mr Robot):

![Beg Bounties](https://www.troyhunt.com/content/images/2021/11/Capture--002-.PNG)

He's sent it to the email address I have published in my security.txt file which I put out there because I genuinely want to know if someone finds a security vulnerability in any of my things. And that's a real possibility; I've created dozens of courses on infosec (including one that includes a module on clickjacking!), I've written hundreds of blog posts on the topic, I've travelled the world running my Hack Yourself First workshop (over 100 times now), I've somehow even ended up in US Congress talking about cybersecurity! But I can make mistakes. Coding mistakes. Configuration mistakes. Or someone else makes one in a library or platform I'm dependent on and wammo! It is I who is pwned. But not this time:

![Beg Bounties](https://www.troyhunt.com/content/images/2021/11/image-1.png)

That link Mayank shared leads to a page that literally has this at the bottom of it (conveniently cropped off the earlier image he attached):

![Beg Bounties](https://www.troyhunt.com/content/images/2021/11/image-2.png)

This is why my email above says "beg bounty" and it's exactly what it sounds like - someone begging for a bounty. Sophos wrote up a bunch of good examples earlier this year and they typically amount to easily discoverable configurations that are publicly observable and minor in nature. DMARC records. A missing CSP. Anything that as Sophos puts it, is "scaremongering for profit". And just to be crystal clear, these are "reports" submitted to website operators _who do not have a published bug bounty_. I love bug bounties (2 of my Pluralsight courses are on them with friend and Bugcrowd founder Casey Ellis), but we're not talking about organisations with the resources to invest in formal programs that pay money (which, incidentally, Pluralsight also runs). No, we're talking about resources like my blog and free community projects like HIBP. Hence the "beg" component of the bounty.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Feel free to use this as often as you’d like. <a href="https://t.co/1kVSqjLeut?ref=troyhunt.com">pic.twitter.com/1kVSqjLeut</a></p>— Steve Edwards (@sedward5) <a href="https://twitter.com/sedward5/status/1319653212681609217?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">October 23, 2020</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Want to be a bounty beggar? It's dead simple, you just use tools like Qualys' SSL Labs, dmarcian or Scott Helme's Security Headers, among others. Easy point and shoot magic and you don't need to have any idea whatsoever what you're doing! But hold on, why does that HIBP report on Scott's site only score a "B"? Where's the CSP? And the referrer-policy? They're on HIBP (go and check for yourself in your browser), but they're not there on the interstitial page Cloudflare is serving up to Scott's crawler as part of the anti-automation defences I've put in place. But hey, that takes knowledge and understanding to establish which is why I've received beg bounty requests for precisely this in the past.

I was finally prompted to write this after yet another run-in with someone seeking a beg bounty. Here's the thread:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I fucking hate beg bounties 😡 <a href="https://t.co/Giv4JRRaty?ref=troyhunt.com">pic.twitter.com/Giv4JRRaty</a></p>— Troy Hunt (@troyhunt) <a href="https://twitter.com/troyhunt/status/1456944042353172487?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 6, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

It was _immediately_ clear that Hammad was going to beg for a bounty, but it was a quiet Saturday night here and I thought it would be entertaining to see just how far down the rabbit hole he wanted to go. So, I responded, positively:

<blockquote class="twitter-tweet" data-conversation="none"><p lang="und" dir="ltr"><a href="https://t.co/1OEkJDHHcH?ref=troyhunt.com">pic.twitter.com/1OEkJDHHcH</a></p>— Troy Hunt (@troyhunt) <a href="https://twitter.com/troyhunt/status/1456944048527183878?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 6, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

I took my usual signature off the email (the one you can see in an earlier screen grab), just to ensure Hammad held onto the glimmer of hope that he may successfully extract some hard-earned money from me. Of course, he took the bait:

<blockquote class="twitter-tweet" data-conversation="none"><p lang="und" dir="ltr"><a href="https://t.co/epM9YTSz1L?ref=troyhunt.com">pic.twitter.com/epM9YTSz1L</a></p>— Troy Hunt (@troyhunt) <a href="https://twitter.com/troyhunt/status/1456944054269214720?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 6, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

It's like dealing with scam phone calls: if you want to see where they lead, you need to play the game and not come on too strong too early. Hammad continued a few minutes later:

<blockquote class="twitter-tweet" data-conversation="none"><p lang="und" dir="ltr"><a href="https://t.co/uoaGstoASH?ref=troyhunt.com">pic.twitter.com/uoaGstoASH</a></p>— Troy Hunt (@troyhunt) <a href="https://twitter.com/troyhunt/status/1456944060296413189?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 6, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

And there we have it. The beg. It was always going to come, he just neglected to mention it in the first message. Maybe he forgot? Or maybe he's done this enough times now (which subsequent replies to this thread with his previous attempts suggest) that he's learned enough social engineering to know not to go too hard on the first approach. This dismayed me:

<blockquote class="twitter-tweet" data-conversation="none"><p lang="und" dir="ltr"><a href="https://t.co/yfVR8UkXBv?ref=troyhunt.com">pic.twitter.com/yfVR8UkXBv</a></p>— Troy Hunt (@troyhunt) <a href="https://twitter.com/troyhunt/status/1456944066143272965?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 6, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

On a (slightly) more serious note for a moment, this is what specifically pisses me off: I don't know how many disclosures I've done of both serious security vulnerabilities and complete breaches of data (100+, surely), but I have never, ever - _not even once_ - asked for money. But Hammad isn't me:

<blockquote class="twitter-tweet" data-conversation="none"><p lang="und" dir="ltr"><a href="https://t.co/hhBpyBQG6y?ref=troyhunt.com">pic.twitter.com/hhBpyBQG6y</a></p>— Troy Hunt (@troyhunt) <a href="https://twitter.com/troyhunt/status/1456944072791248900?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 6, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Now, as many people subsequently pointed out in the thread, the irony is that Hammad _did_ actually already do the "work" for free and whether I paid him or not, the effort had already been invested. Ok, patience exhausted, time to put the signature back in and take Hammad to school:

<blockquote class="twitter-tweet" data-conversation="none"><p lang="und" dir="ltr"><a href="https://t.co/We6F17ZOZY?ref=troyhunt.com">pic.twitter.com/We6F17ZOZY</a></p>— Troy Hunt (@troyhunt) <a href="https://twitter.com/troyhunt/status/1456944080936599557?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 6, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

I did genuinely have this blog post in draft and had been adding bits to it as the Hammads of the world popped their begs into my inbox. If he stopped responding here, then that would have been the end of it; I might have just added a couple more notes and come back to this post in the distant future. However, after this next reply I knew where my Sunday afternoon was going:

<blockquote class="twitter-tweet" data-conversation="none"><p lang="und" dir="ltr"><a href="https://t.co/1KtYck2WEF?ref=troyhunt.com">pic.twitter.com/1KtYck2WEF</a></p>— Troy Hunt (@troyhunt) <a href="https://twitter.com/troyhunt/status/1456947217839714311?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 6, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

I bet it was clickjacking! Maybe on hack-yourself-first.com 🤣

Clearly, I didn't forget and I also didn't forgive and he probably should have expected me (sorry, couldn't help myself!) If I'm honest, I was surprised at how much traction this thread got and I woke up on Sunday morning to hundreds of mentions and thousands of likes across the tweet thread. It struck a chord. _Many_ of you fucking hate beg bounties and shared my lack of sympathy for Muhammad Kamran (the full name will help SEO the next time someone he targets starts searching for him 🙂). However, there was a very small single-digit number of people that disagreed, and I want to address those arguments here:

<blockquote class="twitter-tweet" data-conversation="none"><p lang="en" dir="ltr">I'm not happy with this approach either. If he is a scammer with no vuln found then go ahead and waste his time. I wouldn't call people trying to make money as a begger. He sounds kind and sincere to me. I don't public humiliate a person asking to wash my windows for a dollar</p>— JohnΞ (@j3g) <a href="https://twitter.com/j3g/status/1457094782736470017?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 6, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

First sentence - good! Onto the "scammer" comment and it raises an interesting question: is this a scam? I agree with this use of the term in that this behaviour amounts to "a dishonest scheme" and, depending on the definition you read, the intent is to "deceive and defraud". Attempting to scare people with an alleged vulnerability then withholding information about it until a financial commitment is made all whilst claiming to be a "white hat" is dishonest, deceptive, and fraudulent. As for the rest, firstly, "beg bounty" is becoming a pretty broadly adopted term as you saw from the Sophos article. A quick nod also to Michael Argast and Chester Wisniewski (who, incidentally, wrote the earlier mentioned Sophos article) for their role in coining the phrase:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I love the fact a term I coined with <a href="https://twitter.com/chetwisniewski?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">@chetwisniewski</a> after running into a bunch of these a couple of years ago is getting tweeted by <a href="https://twitter.com/troyhunt?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">@troyhunt</a>, one of my infosec heros. 😆 <a href="https://t.co/heOQjWgySz?ref=troyhunt.com">https://t.co/heOQjWgySz</a></p>— Michael Argast 💉💉🤗 (@michaelargast) <a href="https://twitter.com/michaelargast/status/1456994041279639554?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 6, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

As for Hammad having a "kind and sincere" demeanour, you get more flies with honey than vinegar. _Of course he's going to be nice!_ Have you ever had someone try to scam you who started out being obnoxious? No, you'd hang up on them right away. Gaining someone's trust by coming across as approachable and building rapport is scamming 101. It's also no excuse whatsoever for this behaviour. As for the "wash my windows for a dollar" remark, firstly, IRL analogies explaining digital concepts are terrible. Secondly, if we really want to go down that path then the correct analogy is a random stranger telling you there's something important wrong with your car, but they won't tell you what it is unless you give them money. A lot less nice, that example, isn't it?

<blockquote class="twitter-tweet" data-conversation="none"><p lang="en" dir="ltr">sorry Troy, but I don't think that your answers were appropriate. While these people obviously want some free money, you don't need to lower yourself to that level, you could have handled it better. Remember that this could have been an example which people would follow</p>— opsxcq (@opsxcq) <a href="https://twitter.com/opsxcq/status/1456952074206425088?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 6, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

The responses to that tweet speak for themselves (Hammad, is that you?) but the last sentence hit a nerve. _This is exactly the example I would like people to follow!_ I'd like them to waste the time of the beg bounty hunter, give them a stern talking to and shame them publicly. Look, if you really want to inject some warm fuzzies into your own messaging then suggest the likes of Hammad go and get involved as a genuine security researcher at Bugcrowd or HackerOne. There has never been a better time for _actual_ security researchers to get involved in this industry and there is no shortage of opportunities that don't involve shaking down unsuspecting victims for cash. And people _have_ been actively sharing their own experiences with the same guy:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">One must simply love these "White Hat Hackers"... <a href="https://t.co/xfxgVuuX4t?ref=troyhunt.com">pic.twitter.com/xfxgVuuX4t</a></p>— Ondřej Surý (@oerdnj) <a href="https://twitter.com/oerdnj/status/1455820710492975106?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 3, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none"><p lang="en" dir="ltr">That name is familiar… I found this back in 2019 <a href="https://t.co/7rxcZl0Tms?ref=troyhunt.com">pic.twitter.com/7rxcZl0Tms</a></p>— Parrilla (@nubeblog) <a href="https://twitter.com/nubeblog/status/1457084154219221004?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 6, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none"><p lang="en" dir="ltr">muhammad hammad loves <a href="https://twitter.com/elhackernet?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">@elhackernet</a> too 😂 <a href="https://t.co/JJ9q9uqUTu?ref=troyhunt.com">pic.twitter.com/JJ9q9uqUTu</a></p>— elhacker.NET (@elhackernet) <a href="https://twitter.com/elhackernet/status/1457459693262184450?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 7, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none"><p lang="en" dir="ltr">Yea he has been floating around begging for a while. <a href="https://t.co/yRAuUQMvQ6?ref=troyhunt.com">pic.twitter.com/yRAuUQMvQ6</a></p>— (╯°□°）╯︵ zɔɹʎp ʎpuɐ (@adyrcz) <a href="https://twitter.com/adyrcz/status/1457035316552998912?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 6, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

This illustrates why on balance, I'm comfortable using the name Muhammad Kamran or Muhammad Hammad in this post as others have done in their tweets; I'd like for the next person he tries to scare into coughing up cash to search for his name, find this post and understand the principle of the beg bounty and why it can be safely ignored. Equally, I'd be perfectly happy if someone used my name to highlight my approach to disclosure, because this is what it looks like:

![Beg Bounties](https://www.troyhunt.com/content/images/2021/11/image-3.png)

I picked this example because it's very recent and if you read my tweet thread about the Thingiverse breach, a very frustrating experience. _But it's the right way to go about it!_ Open. Honest. Transparent. But above all, bereft of ulterior motives. I only wanted one thing out of the Thingiverse disclosure and that was simply for them to be aware of the breach and to inform their customers accordingly.

Getting back to my original point, is it any wonder companies are standoff-ish when someone like myself attempts to report a genuine _serious_ security issue? Just look at a Twitter search for me asking for security contacts at a company. It shouldn't be this way and shady approaches by bounty beggars makes it all the harder. I'm sitting on literally _billions_ of records from undisclosed data breaches because the burden of disclosure is so high.

Finally, I'm a _massive_ supporter of the security.txt standard and have been for many years now, and I just hate hearing stories from people about how it's being abused for beg bounties. This is literally one of the barriers to entry: as soon as security contacts are published, organisations have to deal with garbage reports that create noise and cause genuine, _legitimate_ reports to sink amongst all the crap they're dealing with.

This is why I have no patience for beg bounties and no hesitation exposing those who partake in them to the detriment of absolutely everyone other than themselves. If you receive this garbage, respond with a stern word and a link to this post... and perhaps a tweet thread of your own 🙂

<blockquote class="twitter-tweet" data-conversation="none"><p lang="en" dir="ltr">From now on, I won't ignore this kind of emails. This is going to be my answer, thanks <a href="https://twitter.com/troyhunt?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">@troyhunt</a> !</p>— Our Code World (@ourcodeworld) <a href="https://twitter.com/ourcodeworld/status/1457308441941262341?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">November 7, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Go to Source
