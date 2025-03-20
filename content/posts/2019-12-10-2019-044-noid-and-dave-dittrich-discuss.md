---
title: "2019-044-Noid and Dave Dittrich discusses recent keybase woes - Part 1"
date: Tue, 10 Dec 2019 03:10:25 +0000
draft: false
type: posts
---
# 2019-044-Noid and Dave Dittrich discusses recent keybase woes - Part 1

<br/>

<br/>
Patreon donor goodness: Scott S. and Ion S.

@\_noid\_ @davedittrich

Their response:

 “it’s not a bug, it’s a feature”

    “Don’t write a blog post that will point out the issue”

    “You pointing out our issues makes things more difficult for us”

    “It’s a free service, why are you hurting us?”

https://keybase.io/docs/bug\_reporting

  
  

Nov 22nd

Noid (@\_noid\_) Keybase discussion blog post

[https://www.whiskey-tango.org/2019/11/keybase-weve-got-privacy-problem.html](https://www.whiskey-tango.org/2019/11/keybase-weve-got-privacy-problem.html)

Reddit post showing potential SE attacks occurring: [https://www.reddit.com/r/Keybase/comments/e6uou3/hi\_guys\_i\_received\_a\_message\_today\_that\_is/](https://www.reddit.com/r/Keybase/comments/e6uou3/hi_guys_i_received_a_message_today_that_is/) 

Keybase’s decision to fix it came out after The Register asked them about the issue…

Dec 4th

[https://keybase.io/blog/dealing-with-spam](https://keybase.io/blog/dealing-with-spam)

Dec 5th.

[https://www.theregister.co.uk/2019/12/05/keybase\_struggles\_with\_harassment/](https://www.theregister.co.uk/2019/12/05/keybase_struggles_with_harassment/)

  
  

Problems with the implementation:  

        Requiring admins for Keybase to decide what’s wrong or if they need to be deleted

        Additional dummy accounts being created on other sites (keybase, twitter, git, reddit, etc), generating problems for those services (as if Twitter doesn’t have enough issues with bots/shitty people)

        Cryptocurrency = trolls/phishing/SE attempts to get folks to hand over their lumens (what’s the motivation of creating the coin?)

        They’ve already opened the spam door, and they’ll not be able to shut it.

Once they took the VC and aligned themselves with Stellar, the attack surface changes

    From Account takeover (integrity attacks) to deception (social engineering)

What is keybase?

    Social network?

    E2E chat

Encrypted file share/storage?

    CryptoCurrency Company? 

    Secure git repo protector?

Which ones do they do well?  

How could they have solved the spam issue?

    Made the cryptocoin a separate application?

        Even their /r/keybase is filling up with spammers asking about their Lumens

How could they fix it?

    You can’t contact someone unless that person allows you to.

    Allow someone to contact you, but do not allow adding to teams without permission

[https://news.ycombinator.com/item?id=21719702](https://news.ycombinator.com/item?id=21719702) (ongoing HN thread)

Noid isn’t the only person with issues in Keybase: [https://vicki.substack.com/p/keybase-and-the-chaos-of-crypto](https://vicki.substack.com/p/keybase-and-the-chaos-of-crypto)

[https://it.slashdot.org/story/19/12/06/1610259/keybase-moves-to-stop-onslaught-of-spammers-on-encrypted-message-platform](https://it.slashdot.org/story/19/12/06/1610259/keybase-moves-to-stop-onslaught-of-spammers-on-encrypted-message-platform)

[https://keybase.io/docs-assets/blog/NCC\_Group\_Keybase\_KB2018\_Public\_Report\_2019-02-27\_v1.3.pdf](https://keybase.io/docs-assets/blog/NCC_Group_Keybase_KB2018_Public_Report_2019-02-27_v1.3.pdf) 

  
  

Stephen Carter's definition of “integrity.”

_Integrity, as I will use the term, requires three steps: (1) discerning what is right and what is wrong, (2) acting on what you have discerned, even at personal cost; and (3) saying openly that you are acting on your understanding of right from wrong._

 — Stephen Carter, “Integrity.” Harper-Collins. [https://www.harpercollins.com/9780060928070/integrity/](https://www.harpercollins.com/9780060928070/integrity/)

Can the person \[who took the controversial act\] explain their reasoning, based on principles they can articulate and would follow even if it meant they paid a price? Or do they selectively choose principles in arbitrary ways so as to fit the current circumstances in order to guarantee they get an outcome that benefits them?

noid’s blog post clearly documents the timeline of interactions with Keybase, including: (1) providing detailed steps to reproduce; (2) suggesting mitigations that could be implemented in the architecture; (3) providing guidance to users to protect themselves when the vulnerability disclosure was made public; and (4) justifying his decision to go public by citing and following a vulnerability disclosure policy of a major industry leader in this area, Google:

### _Following Google Security’s guidelines for issues being actively exploited in the wild, I chose to release this information 7 days after I last heard from Keybase._

### The ACM Code of Conduct has several sections that could apply here:

### 1.1 Contribute to society and to human well-being, acknowledging that all people are stakeholders in computing.

### 1.2 Avoid harm.

### 1.6 Respect privacy.

### 2.1 Strive to achieve high quality in both the processes and products of professional work.

### 2.7 Foster public awareness and understanding of computing, related technologies, and their consequences.

### 3.1 Ensure that the public good is the central concern during all professional computing work.

### 3.7 Recognize and take special care of systems that become integrated into the infrastructure of society.

The right to privacy of your information, as well as the right to choose with whom you associate and communicate, are both arguably duties based on the concept of autonomy (i.e., your right to choose).

In biomedical and behavioral research, the principle involved here is known as Respect for Persons and is best recognized as the idea of informed consent. Giving users autonomy in making their data public, but not giving them autonomy in who they allow to communicate with them and add them to “teams,” could be viewed as conflicting as regards this principle.

This is in fact precisely what noid brought up in his initial communication with Keybase:

_I had a random guy I don’t follow add me to a team and start messaging me about cryptocurrency stuff. This really shouldn’t be default behavior. This can result in a spam or harassment vector (hence why I’m reluctant to post it on the open forum). Ideally the default behavior should be that no one can add you to a team_ **_without your consent_**_. Then maybe have an option of allowing those you follow to be able to do so, and as a final option let anyone add you to a team (but make sure folks know this isn’t recommended)._

#### [Source](http://brakeingsecurity.com/2019-044-noid-and-dave-dittrich-discusses-recent-keybase-woes-part-1)

<br/>
---
