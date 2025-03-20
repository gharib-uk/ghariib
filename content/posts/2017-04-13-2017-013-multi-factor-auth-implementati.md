---
title: "2017-013-Multi-factor Auth implementations gotchas and solutions with Matt"
date: Thu, 13 Apr 2017 03:03:36 +0000
draft: false
type: posts
---
# 2017-013-Multi-factor Auth implementations gotchas and solutions with Matt

<br/>

<br/>
Most everyone uses some kind of Multi-factor or '2 Factor Authentication". But our guest this week (who is going by "Matt" @infosec\_meme)... Wanted to discuss some gotchas with regard to 2FA or MFA, the issues that come from over-reliance on 2FA, including some who believe it's the best thing ever, and we finally discuss other methods of 2FA that don't just require a PIN from a mobile device or token.

We also discuss it's use with concepts like "beyondCorp", which is google's concept of "Software Defined Perimeter" that we talked about a few weeks ago with @jasonGarbis ([http://traffic.libsyn.com/brakeingsecurity/2017-011-Software\_Defined\_Perimeter.mp3](http://traffic.libsyn.com/brakeingsecurity/2017-011-Software_Defined_Perimeter.mp3))

This is a great discussion for people looking to implement 2FA at their organization, or need ammunition if your boss thinks that all security is solved by using Google Auth.

Direct Link: [http://traffic.libsyn.com/brakeingsecurity/2017-013-Multi-factor\_auth\_gotchas\_with\_Matt.mp3](http://traffic.libsyn.com/brakeingsecurity/2017-013-Multi-factor_auth_gotchas_with_Matt.mp3)

Youtube Channel:  [https://www.youtube.com/channel/UCZFjAqFb4A60M1TMa0t1KXw](https://www.youtube.com/channel/UCZFjAqFb4A60M1TMa0t1KXw)

iTunes Store Link:  [https://itunes.apple.com/us/podcast/brakeing-down-security-podcast/id799131292?mt=2](https://itunes.apple.com/us/podcast/brakeing-down-security-podcast/id799131292?mt=2) 

\---------

Jay Beale’s Class “aikido on the command line: hardening and containment”

**JULY 22-23 & JULY 24-25    AT BlackHat 2017**

[https://www.blackhat.com/us-17/training/aikido-on-the-command-line-linux-hardening-and-containment.html](https://www.blackhat.com/us-17/training/aikido-on-the-command-line-linux-hardening-and-containment.html)

\---------

Join our #Slack Channel! Sign up at **https://brakesec.signup.team**  
  
#RSS: http://www.brakeingsecurity.com/rss  
  
#Google Play Store: https://play.google.com/music/m/Ifp5boyverbo4yywxnbydtzljcy?t=Brakeing\_Down\_Security\_podcast

#iHeartRadio App:  https://www.iheart.com/show/263-Brakeing-Down-Securi/

#SoundCloud: https://www.soundcloud.com/bryan-brake

Comments, Questions, Feedback: [bds.podcast@gmail.com](mailto:bds.podcast@gmail.com)

Support Brakeing Down Security Podcast on #Patreon: [https://www.patreon.com/bds\_podcast](https://www.patreon.com/bds_podcast)

#Twitter: @brakesec @boettcherpwned @bryanbrake

#Player.FM : [https://player.fm/series/brakeing-down-security-podcast](https://player.fm/series/brakeing-down-security-podcast)

#Stitcher Network: [http://www.stitcher.com/s?fid=80546&refid=stpr](http://www.stitcher.com/s?fid=80546&refid=stpr)

#TuneIn Radio App: [http://tunein.com/radio/Brakeing-Down-Security-Podcast-p801582/](http://tunein.com/radio/Brakeing-Down-Security-Podcast-p801582/)

Show Notes:

What does MFA try to solve:

-   Mitigate password reuse
-   Cred theft - Someone stealing credentials from embarassingadultsite.com and turns they work out on a totallyserious.gov RDP server
-   Phishing bad - same as above, except now you convince someone totallyseriousgov.com is legit and they give you credentials

Cred theft:

-   Getting to the point where old mate literally has more password dumps than time
-   [https://www.troyhunt.com/i-just-added-another-140-data-breaches-to-have-i-been-pwned/](https://www.troyhunt.com/i-just-added-another-140-data-breaches-to-have-i-been-pwned/)
-   Honestly not going away, and combined with password reuse makes things pretty bad

Phishing:

-   Happens.
-   META: do we need to back this up with some stats?  [https://blog.barkly.com/phishing-statistics-2016](https://blog.barkly.com/phishing-statistics-2016)

MFA / Bad things happening with that:

-   AU Telecommunications provider sent multifactor SMS to wrong people
-   -   [https://www.itnews.com.au/news/telstra-sending-sms-to-wrong-numbers-after-exchange-fire-449690](https://www.itnews.com.au/news/telstra-sending-sms-to-wrong-numbers-after-exchange-fire-449690)
-   RSA was owned years ago - and had to reissue a bunch of tokens
-   -   [http://money.cnn.com/2011/06/08/technology/securid\_hack/](http://money.cnn.com/2011/06/08/technology/securid_hack/)
    -   [https://bits.blogs.nytimes.com/2011/04/02/the-rsa-hack-how-they-did-it/?\_r=0](https://bits.blogs.nytimes.com/2011/04/02/the-rsa-hack-how-they-did-it/?_r=0)
    -   On the plus side, obviously increased cost to attacker significantly to do that
-   Phishing frameworks are everywhere
-   -   Misc / Turns out U2F makes phishing kind of dead? (Read first amendment)
    -   [https://breakdev.org/evilginx-advanced-phishing-with-two-factor-authentication-bypass/](https://breakdev.org/evilginx-advanced-phishing-with-two-factor-authentication-bypass/)
    -   Appears Backed up by the spec ( ‘Origin’ / [https://fidoalliance.org/specs/fido-u2f-v1.1-id-20160915/fido-u2f-overview-v1.1-id-20160915.pdf](https://fidoalliance.org/specs/fido-u2f-v1.1-id-20160915/fido-u2f-overview-v1.1-id-20160915.pdf))

Phishing/2FA/Solutions?

1.  a) What does multifactor actually solve?
2.  b) Are we (infosec industry) issuing multifactor solutions to people just so people make money?
3.  c)  Do these things give a \*false\* sense of security?
4.  d) What do you think about storing the token on the same box? Especially given an actor on the box is just going to steal creds as they’re entered.

Internal training / is this actually working?

  

Australia Post didn't think so

[https://www.itnews.com.au/news/why-australia-post-ransomwared-its-own-staff-454987](https://www.itnews.com.au/news/why-australia-post-ransomwared-its-own-staff-454987)

Counterpoints:

It's irritating and does break at times ( [https://twitter.com/dguido/status/842448889697447938](https://twitter.com/dguido/status/842448889697447938) )

C: I don’t like running some silly app on my phone

C: I also don’t like running around with a physical token

C: Embedding a Yubico nano in my usb slot leaves me with one usb port left

Also doesn’t solve when someone just steals that token

Does any of it matter:

Beyondcorp / "Lets make the machines state be part of the credential"

[https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/43231.pdf](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/43231.pdf)

-   Tl;dr of paper: TPMs, certificates and a lot of health checks - think of NAC on steroids

Is there some way we (not google) can make it so a credential is worthless?

Solutions:

Duo / “There's an app on my phone and it has context about what wants to do something right now”

Probably a step in the right direction

Kind of like some Aus banks which SMS you before transferring $X to Y account

Okta - (grab links to spec)

META // Does this actually solve it?

OAUTH - (grab links to spec)

Attacking OAUTH - [https://dhavalkapil.com/blogs/Attacking-the-OAuth-Protocol/](https://dhavalkapil.com/blogs/Attacking-the-OAuth-Protocol/)

META // It’s not MFA, but it makes the cost of unrelated compromise significantly lower

META // Engineering things to short lived secrets is a better idea

I think one of the better ideas being put out was by google in 2014, the ‘beyondcorp’ project ([https://research.google.com/pubs/pub43231.html)](https://research.google.com/pubs/pub43231.html\)), simply put:

-   The devices used everywhere are chromebooks run in standard mode rather than developer mode
-   -   (Whitelisting For Free™)
-   Everything is a web app
-   Everything else can’t run due to app whitelisting built-in
-   The device needs to also authenticate before the user can do anything, and is used as part of the judgement for access control engines
-   Everything cares about the machine the user is using - It’s part of the credential
-   Passwords are no longer important and it’s all single sign on
-   -   Suddenly credential theft doesn’t matter
-   The device uses certificates to attest to its current state, so stolen passwords without a valid device don’t matter
-   As the device is a glorified web browser, and has app whitelisting, you’re not going to get code execution on it, malware no longer matters
-   -   Caveat, someone will probably think of some cool technique and that’ll ruin everything
    -   See: Problem of induction / “Black swan event”

Obviously this is a massive undertaking and would require massive overhaul of everything, but it did look like Google were able to pull it off in the end. ([https://research.google.com/pubs/pub44860.html)](https://research.google.com/pubs/pub44860.html\)).

Tavis is banging on LastPass again…  [https://www.ghacks.net/2017/03/21/full-last-pass-4-1-42-exploit-discovered/](https://www.ghacks.net/2017/03/21/full-last-pass-4-1-42-exploit-discovered/)

Duo Security // Beyondcorp

[https://duo.com/blog/beyondcorp-for-the-rest-of-us](https://duo.com/blog/beyondcorp-for-the-rest-of-us)

  
  

More info on Beyondcorp

[https://www.beyondcorp.com](https://www.beyondcorp.com)

Misc// Hey google wrote a paper on U2F a while back

[http://fc16.ifca.ai/preproceedings/25\_Lang.pdf](http://fc16.ifca.ai/preproceedings/25_Lang.pdf)

  

Touched on briefly / “Secure Boot Stack and Machine Identity” at Google - Servers which need to boot up into a given state (Sounds like U/EFI except ‘ Google-designed security chip’)

[https://cloud.google.com/security/security-design/resources/google\_infrastructure\_whitepaper\_fa.pdf](https://cloud.google.com/security/security-design/resources/google_infrastructure_whitepaper_fa.pdf)

  

META // Patrick Gray (sic) interviewed Duo last week and talked about the same thing

[https://risky.biz/RB448/](https://risky.biz/RB448/)

#### [Source](http://brakeingsecurity.com/2017-013-multi-factor-auth-implementations-gotchas-and-solutions-with-matt)

<br/>
---
