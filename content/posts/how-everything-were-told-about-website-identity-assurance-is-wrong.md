---
title: "<div>How Everything We're Told About Website Identity Assurance is Wrong</div>"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/green-bar-iso.png)

I have a vehement dislike for misleading advertising. We see it every day; weight loss pills, make money fast schemes and if you travel in the same circles I do, claims that extended validation (EV) certificates actually do something useful:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Why are you still claiming this <a href="https://twitter.com/digicert?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">@digicert</a>? This is extremely misleading, anyone feel like reporting this to the relevant advertising standards authority in their jurisdiction? <a href="https://t.co/enzJUodhdG?ref=troyhunt.com">https://t.co/enzJUodhdG</a> <a href="https://t.co/Rnx6CDexhv?ref=troyhunt.com">pic.twitter.com/Rnx6CDexhv</a></p>— Troy Hunt (@troyhunt) <a href="https://twitter.com/troyhunt/status/1491559543410593794?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">February 9, 2022</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Someone had reached out to me privately and shared the offending page as they'd taken issue with the false claims DigiCert was making. My views on certificate authority shenanigans spinning yarns on EV are well known after having done many talks on the topic and written many blog posts, most recently in August 2019 after both Chrome and Firefox announced they were killing it. When I say "kill", that never meant that EV would no longer _technically_ work, but it killed the single thing spruikers of it relied upon - being visually present beside the address bar. That was 2 and a half years ago, so why is DigiCert still pimping the message about the green bar with the company name? Beats me (although I could gue$$), but clearly DigiCert had a change of heart after that tweet because a day later, the offending image was gone. You can still see the original version in the Feb 9 snapshot on archive.org:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image.png)

But look at it today and everything has been toned down green bar wise:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-1.png)

Problem solved! Yeah, nah... I was especially interested in the new addition on How to Identify Fake Websites and that, dear readers, was the catalyst for this blog post. Let's tear it apart bit by bit.

### Nobody Looks "Beyond the Lock"

This isn't all going to be in the order in which DigiCert has presented the information, instead I'm going to walk through it in a fashion which systematically breaks it apart and shoots it down in flames. Let's start here:

> You should look beyond the lock by clicking on it once to reveal more information.

Turns out the catchphrase of looking beyond the lock is a term they've pushed before in another piece I'll come back to shortly. For now, let's talk about what the term means, starting with how it works in Chrome on the PC:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-2.png)

That's what you get by "clicking it once" - an assurance that the connection is secure which really just reiterates what we already know courtesy of the padlock icon. At this stage we know nothing about site identity, so let's drill down further:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-3.png)

Ah, now we know the cert has been issued to DigiCert Inc. in the US. So, all good right? No, because who are they? I mean, all we know is that the cert has been issued to an entity with that name, we don't know if they are a certificate authority or a company that certifies how many fingers you have on your hand (digits - get it?). This is what Ian Carroll demonstrated a few years back when he got an EV cert for Stripe Inc. Perfectly legit cert issued to a perfectly legit entity, just not the one everyone thought it was.

But this is all more effort and arguably, it's harder to issue an EV cert in this fashion than a domain validated (DV) cert, so... EV is better? No, because per the title of this section, nobody is actually going to look beyond the lock anyway. (Yes, I know there'll be someone somewhere who eventually does, let's just agree that "nobody" is a number that rounds to 0%.) I mean seriously, do you ever do this? Your significant other? Your parents? No, no and no.

But let's humour them and say you did. Let's imagine you're going to do some shopping at Amazon and you look behind the lock, here's what you'll see:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-4.png)

Huh. This is... different. Compare it to the DigiCert one and see how the "Issued to" section is missing? That's because Amazon doesn't have an EV cert, inevitably because they're smart enough to realise it wouldn't do them any good if they did! But you see the problem: if DigiCert wants to make the case that you should inspect a cert by drilling down _2 clicks_ (not one) before trusting the site, that clearly flies in the face of how the web actually works. Same with eBay. Same with Alibaba. Same with the little shop I buy my coffee from. Don't "look beyond the lock" because if you do, you're not going to be buying anything online any more.

Back on the dedicated page for looking beyond the lock, DigiCert takes the bank at peoples.com (which uses an EV cert) and provides multiple examples of how the bank appears in various browsers then concludes as follows:

> In the current absence of a uniform way of showing stronger identity and trust across all web browsers, consumers browsing the web and other relying parties need to know for themselves how to identify information about site ownership.

But just as this logic doesn't work for the world's largest e-commerce platforms, it also doesn't work for many banks. For example, one of our "Big 4" down here in Australia:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-13.png)

Or Denmark's largest bank:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-14.png)

And yes, you will find all sorts of banks that _do_ have EV certs, but you almost certainly don't know which ones without checking which also means you don't know which ones _don't_ have it so if you go to that bank and don't see EV, what does that mean? Is it a phishing site? Or just a bank without EV? Remember, _EV only works if people change their behaviour in its absence_ and clearly, that just doesn't happen.

While we're talking banks, have a look at the certificate transparency logs for danskebank.com and search for "extended validation". They've had plenty of EV certs in the past, but have subsequently abandoned them. Now, imagine you're a Danske Bank customer and let's say you're that rare breed that actually clicks (twice) down into the cert details; what would you have done on the first occasion you didn't see EV? Left the site and not banked there any more? No, of course not, and their abandonment of EV without consequences proves how little value they (and many others) actually place in it.

This is what just doesn't seem to be understood by the DigiCert marketing folks because if it was, statements like this wouldn't exist:

> Because of the higher level of authentication and verification provided by an EV certificate, users know that you are who you say you are and feel safe transferring sensitive data.

That's on the page in the original tweet. That same page suggests that EV should be used for account logins, the order process and "front-facing websites" which is... the entire internet? Gotcha, just put it everywhere.

### There's no Useful "Beyond the Lock" on a Massive Portion of Mobile Devices

Let's keep humouring DigiCert: how do you look "beyond the lock" on mobile? You know, those devices that are now massively dominant in the mobile shopping space? The ones that account for about three quarters of all e-commerce sales? Try it on Safari on iOS. Can you figure out how to inspect a site's certificate? You won't, because you can't.

Let's try Chrome. You can't click the lock in the address bar though, you need to hit the little menu icon (the 3 dots), scroll down then click on "Site information". You can do that for this very blog:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-5.png)

Your credit card numbers are safe when you enter them into this site! But because I don't have an EV cert, you don't see an entity name represented anywhere so you have no identity assurance. Let's try DigiCert instead:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-6.png)

Uh... where's "DigiCert Inc"? Not there. Chrome doesn't show the entity name on their mobile browser, at least not on iOS. The DigiCert reference you see is simply the issuing CA in the same way as you see Cloudflare on my cert.

I actually ended up having a lengthy chat to DigiCert support whilst writing this blog post today and I raised this exact issue (among others). They raised 2 interesting points, the first of which was that apparently Chrome on Android _will_ show the full entity name of an EV cert (do please confirm that in the comments, Android friends). The second point was that iOS users should follow the guidance on How to Check Digital Certificates on Computer and iPhone which suggests that they _download a dedicated app_ called TLS  Inspector which will then show the full entity name. Seriously? Someone is about to make an online purchase but wants to verify the identity of the service first so they go to the app store, download TLS inspector, enter the URL of the site they were on _then_ make a judgement call on trust? C'mon guys, this is just getting ridiculous!

This speaks to the entire theme of this post - that the information they're providing is misleading. Nowhere amongst the troves of information DigiCert has provided about checking certificates did I find a single reference to not being able to do that in the browser of what is arguably the world's most recognisable mobile device. Why not? Would it reflect poorly on the efficacy of EV? Yes, it would.

In summary, nobody clicks beyond the lock, if they did they'd almost never see EV and even if they _wanted_ to click, it wouldn't do them any good on a massive chunk of mobile anyway.

### The Website Checker

How else can you establish if a website is fake? Apparently, using a website checker:

> When in doubt, use a website checker to verify if a website is secure. A secure website check can let you know any vulnerabilities on the site, if it is using encryption and what level of verification a site has.

Are people trying to establish the legitimacy of a site _really_ going to do this?! Let's see how it works and I'll give it a run on the BBC's shop:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-18.png)

So... you get penalised by not using a cert from one of those 4 CAs? _That's_ part of establishing whether a site is fake or not?! This feels much more like a poor man's SSL Labs test with a built-in certificate authority bias than a means of establishing if a site is fake or not.

Let's try another one:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-19.png)

Success! Following DigiCert's guidance from the page about identifying fake websites, I've loaded the website checker, checked the site and it's all come back green. Well, unless you actually load it up then it's rather red:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-20.png)

It's not DigiCert's fault that someone has stood up a phishing site here, but this demonstrates how telling people to use a website checker does absolutely nothing of any use whatsoever. Want to check if the certificate is valid? That's what your browser does without you even having to lift a finger. _That's_ what's useful, not which CA issued the cert.

### Seals Should be Clubbed

I saw a talk by a couple of very clever security folks in Amsterdam back in 2015 where they spoke to their ingeniously named paper: Clubbing Seals: Exploring the Ecosystem of Third-party Security Seals. From their conclusions:

> Through a series of automatic and manual experiments, we discovered that third-party security seals are severely lacking in their thoroughness and coverage of vulnerabilities. We uncovered multiple rudimentary vulnerabilities in websites that were certified to be secure and showed that websites that use third-party security seals do not follow security best practices any better than websites that do not use seals. In addition, we proposed a novel attack where seals can be used as vulnerability oracles and describe how an attacker can abuse seal providers to discover the exact exploit for any given vulnerable seal-using website.

It's worth reading the paper to understand the clever technical way in which they used seals to expose vulnerabilities, but for now I'm just going to focus on the super obvious stuff that everyone can easily grasp.

DigiCert is actively pimping "DigiCert Secured" seal as a means of establishing the trustworthiness of a site:

> A site seal signals that the site is authentic, and you can usually click on a site seal to reveal more information about the website and how it was verified. Seals that do nothing when clicked should not be trusted, as they are likely illegitimate copies of seals.

Geez, so much material 🍿

Ok, so firstly, a site seal is just an image. That is all. Anybody can put any image they like on a page, yes, even DigiCert's. Here's all you need:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-11.png)

That's on their Report Seal Misuse page where, if someone believes the DigiCert sea... screw it, let's just call it what it is - the DigiCert Secured image - is being used inappropriately they can report it. That'll stop the hackers! But even that page is nonsensical:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-12.png)

Are we talking seals or certificates? Seals are images on a page that anyone can place there of their own free volition whilst certificates must be correctly signed and issued to verified customers. It's a very odd conflation of terms.

Which brings me to the second point: bar a very small handful of people, nobody has any idea who DigiCert is. If you're a phisherperson and you believe in the value of seals but don't want to infringe on DigiCert's IP, just make your own! Find the clipart, add the text, job done!

Thirdly - and I can't believe this even rates a mention - non legitimate seals can still be made to do stuff when you click on them! I mean seriously, that whole bit about "seals that do nothing when clicked" is just nonsensical and you can easily emulate DigiCert's behaviour by popping a new window when someone clicks on it:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-9.png)

If it was me, I'd pop that window from a legitimate looking domain name and just like the _real_ DigiCert Secured site, it wouldn't even need an EV cert so nobody can verify the identity of the site serving it! Some thick irony right there.

Part of the problem with site seals is the impartiality of the empirical data used to promote them. For example, in DigiCert's All Pros, No Cons infographic:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-21.png)

Sources? Try these:

1. The Baymard Institute, 2020
2. DigiCert Customer Surveys, 2020
3. DigiCert Blog: https://www.digicert.com/blog/website-seals-affect-user-trust/

We can discard the last 2 because of the obvious bias, but what about the first one? What does the Baymard Institute have to say about site seals in 2020? It's hard to say because it's such a generic reference with no link to the actual research. I think it's How Users Perceive Security During the Checkout Flow (Incl. New ‘Trust Seal’ Study 2021) because CheapSSL Security links through to there from their own resource pimping commercial certificates. They talk about abandonment at a similar rate (17%) and go on to say (emphasis theirs):

> Generally, we see during testing that **adding any visual icon** will help increase the user’s general level of perceived security.

They continue with this absolute zinger:

> In each test since 2016 we also included a completely **“homemade / fake” seal** not issued by a 3rd party, with no meaning whatsoever beyond the icon itself. Note how the homemade seal performed significantly better than the SSL seals issued by established vendors except Norton.

I found this _after_ writing the piece above suggesting people should just make their own and I've gotta admit, I feel just a little bit smug about that now 😊

But hey, I always say we should judge not by what is said but rather what is done, so let's see what Baymard themselves do given all the troves of research data they have. EV certificate? Nope:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-22.png)

Ouch!

What about a seal at the point of purchase, the very place we need to convince people to trust the site and not join the 17% who go elsewhere. Let's go shopping:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-23.png)

Ok, nothing there other than credit card logos, let's hit the big "Pay with Credit Card" button and see if my confidence can be regained:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-24.png)

No site seal! I got so afraid of the trustworthiness of this merchant that I immediately closed the tab and forfeited my purchased.

So, what does it tell you when even the organisation doing the research doesn't see value in site seals? And further, when one of the world's largest payment providers clearly sees no value in site seals, what do we make of that? It's a very obvious, very simple conclusion.

### None of us Can Read URLs

I'm going to drop in a talk by Emily Schechter I saw at LocoMocoSec in Hawaii in 2018, set to precisely the position I'd like you to watch from (keep going until you get the idea):

<iframe width="100%" height="480" src="https://www.youtube.com/embed/UD-ukjVoeLc?start=95" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

With that in mind, let's go back to DigiCert's sage advice:

> One key indicator of a fake site is a misspelled URL. Fraudsters may change up a URL name slightly, like using amaz0n.com, or they may change the domain extension — like amazon.org instead of amazon.com.

So, how do we know that amazon.org is the wrong address? Are they seriously expecting people to remember which TLDs are used by the various websites they use? This is a particularly bad example to choose because not only does the .org redirect to the primary site, it's also actually owned by Amazon (the Amazon you think it is!) so it's a terrible example of a "fake" site. Further, the quote above implies that the "correct" Amazon TLD is .com - so what the hell was I looking at earlier on with the .com.au version?! TLDs are frequently geographically contextual so suggesting that "changing the domain extension" is somehow an indicator of a fake site is nonsensical.

Here's another perfect illustration of the problem: I've just discussed the "DigiCert Secured" site seal and how when placed on a merchant's site and clicked, should take the user to a site that reveals "more information about the website and how it was verified". Ok, let's imagine that the site that loads after clicking on the seal is digicert-secured.com. Seem legit? It's not misspelled, there's no character substitution and the TLD is... also legit? But it's not DigiCert's. Instead, the digicert-secured.com domain is mine. I decided to keep the site completely empty because I don't need to do anything more to make the point. Ok, so it's _almost_ completely empty, I just added a seal to the page for assurance 😎

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-15.png)

None of us can read URLs and consistently make reliable decisions on trust. Let me drive that point home even further courtesy of this phishing message I received on the weekend:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-25.png)

It looks spammy as hell to me because a) I didn't order anything from that website and b) it has all the hallmarks of a phishing attack, primarily that it is clearly trying to entice me to click through (What package?! I don't remember ordering a package! Now I'm really curious...) And no, the path with the funky string at the end of it isn't a sign of a phish because this is what a legitimate path looks like. For example, here's what was in a recent Amazon order I made (I've no idea which bits if any may disclose something so I've obfuscated various parts of it):

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-8.png)

As for xterminator.co.nz, that's a legitimate pest control website in Auckland, it's just that the path above then routes you through to a phishing page (either the site has been compromised or it's using an open redirect). So, going back to the quote at the start of this section, neither you nor I can read URLs and make reliable decisions about trustworthiness. That's why we have controls like this:

![How Everything We're Told About Website Identity Assurance is Wrong](https://www.troyhunt.com/content/images/2022/02/image-16.png)

That's Safari on iOS, you've already seen Chrome on the desktop's warning when I tested the phishing page that DigiCert's website checker implied was trustworthy.

If humans could read and understand URLs and consistently draw accurate conclusions on trustworthiness we wouldn't need browser controls like these. But they can't.

### Conclusions

I have no problems with certificate authorities selling EV certs or seals, my problem is when they mislead consumers into believing they actually do something they don't. I feel so sympathetic to the poor folks responsible for their use within organisations who already have a gazillion increasingly complex cyber things to consider without also having to deal with misleading advertising from incumbent players trying to breathe life into a dead product. Expect more of that too, with the shenanigans around QWAC (already being derided by Scott Helme and described by the EFF as "hurting internet security") which presents a new gravy train for the companies that gave a big chunk of their business to Let's Encrypt. But hey, at least that will present more opportunity to write entertaining pieces like this one 😊

Lastly, I will happily donate digicert-secured.com to DigiCert if they'd like it. They can keep my seal, add their own seal, whatever, it's theirs. I've made the point and will happily bear the domain registration cost in the process. Good deal, I reckon!

Go to Source
