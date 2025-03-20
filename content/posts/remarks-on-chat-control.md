---
title: "Remarks on “Chat Control”"
date: 2025-01-15
tags: 
  - "backdoors"
  - "ces"
  - "technology"
---

_On March 23 I was invited to participate in a panel discussion at the European Internet Services Providers Association (EuroISPA). The focus of this discussion was on recent legislative proposals, especially the EU Commission’s new “chat control” content scanning proposal, as well as the future of encryption and fundamental rights. These are the introductory remarks I prepared._

Thank you for inviting me today.

I should start by making brief introduction. I am a professor of computer science and a researcher in the field of applied cryptography. On a day-to-day basis this means that I work on the design of encryption systems. Most of what I do involves building things: I design new encryption systems and try to make existing encryption technologies more useful.

Sometimes I and my colleagues also _break_ encryption systems. I wish I could tell you this didn’t happen often, but it happens much more frequently than you’d imagine, and often in systems that have billions of users and that are very hard to fix. Encryption is a very exciting area to work in, but it’s also a _young_ area. We don’t know all the ways we can get things wrong, and we’re still learning.

I’m here today to answer any questions about encryption in online communication systems. But mainly I’m here because the EU Commission has put forward a proposal that has me very concerned. This proposal, which is popularly called “chat control”, would mandate _content scanning_ technology be added to private messaging applications. This proposal has not been properly analyzed at a technical level, and I’m very worried that the EU might turn it into law.

Before I get to those technical details, I would like to address the issue of where encryption fits into this discussion.

Some have argued that the new proposal is not about _encryption_ at all. At some level these people are correct. The new legislation is fundamentally about privacy and confidentiality, and where law enforcement interests should balance against those things. I have opinions about this, but I’m not an EU citizen. Unfortunately this is a fraught debate that Europeans will have to have among themselves. I don’t envy you.

What concerns me is that the Commission does not appear to have a strong grasp on the _technical implications_ of their proposal, and they do not seem to have considered how it will harm the security of our global communications systems. And this does affect me, because the security of our communications infrastructure is not localized to any one continent: if the 447 million citizens of the EU vote to weaken these technical systems, it could affect all consumers of computer security technology worldwide.

So why is encryption so critical to this debate?

Encryption matters because it is the single best tool we have for securing private data. My time here is limited, but if I thought that using all of it to convince you of this single fact was necessary, I would do that. Literally every other approach we’ve ever used to protect valuable data has been compromised, and often quite badly. And because encryption is the only tool that works for this purpose, any system that proposes to scan private data must — _as a purely technical requirement_ — grapple with the technical challenges it raises when that data is protected with end-to-end encryption.

And those technical implications are significant. I have read the Impact Assessment authored by the Commission, and I hope I am not being rude to this audience when I say that _I found it deeply naive and alarming._ My impression is that the authors do not understand, at a purely technical level, that they are asking technology providers to deploy systems that none of them know how to build safely. Nor has the Commission consulted people with the technical and scientific expertise that would be needed to make this proposal viable.

In order to explain my concerns, I need to give some brief background on how content scanning systems work: both historically, and in the context that the EU is proposing.

Modern content scanning systems are a new creation. They have only been deployed since only about 2009, and widely deployed only after about 2011. These systems normally evaluate messages uploaded to a server, often a social network or public repository. In historical systems — that is, older systems without end-to-end encryption — they would process unencrypted _plaintext_ data, usually to look for known _child sexual abuse media_ files (or CSAM.) Upon finding such an image, they undertake various reporting: typically alerting employees at the provider, who may then escalate to the police.

![](https://blog.cryptographyengineering.com/wp-content/uploads/2023/03/image.png?w=1024)

Historical scanning systems such as Microsoft’s PhotoDNA used a _perceptual hashing_ algorithm to reduce each image to a “fingerprint” that can be checked against a database of _known_ _illicit content_. These databases are maintained by child safety organizations such as NCMEC. The hashing algorithms themselves are deliberately imperfect: they are designed to produce similar fingerprints for files that appear (to the human eye) to be identical, even if a user has slightly altered the file’s data.

A first limitation of these systems is that their inaccuracy can be exploited. It is relatively easy, using techniques that have only been developed recently, to make new images that _appear_ to be harmless licit media files, but that will produce a fingerprint that is identical to harmful illicit CSAM.

A second limitation of these hash-based systems is that they cannot detect _novel_ CSAM content. This means that criminals who post _newly-created_ abuse media are effectively invisible to these scanners. Even a decade ago, the task of finding novel CSAM would have required human operators. However, recent advances in AI have made it possible to train deep neural networks on such imagery, so that these networks can try to detect new examples of it:

![](https://blog.cryptographyengineering.com/wp-content/uploads/2023/03/image-2.png?w=1024)

Of course, the key word in any machine-based image recognition system is “_try_.” All image recognition systems are somewhat fallible (see example at right) and even when they work well, they often fail to differentiate between licit and illicit content. Moreover these systems can be exploited by malicious users to produce surprising results. I’ll come back to that in a moment.

![](https://blog.cryptographyengineering.com/wp-content/uploads/2023/03/image-3.png?w=1024)

But allow me to return to the key challenge: integrating these systems with encrypted communication systems.

In end-to-end encrypted systems, such as WhatsApp or Apple iMessage or Signal, server-side scanning is no longer viable. The problem here is that private data is encrypted when it reaches the server, and cannot be scanned. The Commission proposal isn’t specific about how these systems should be handled, but it hints that this scanning should be done _on the user’s device_ _before the content is encrypted_. This approach is called _client side scanning_.

There are several challenges here.

First, client-side scanning represents an _exception_ to the privacy guarantees of encrypted systems. In a standard end-to-end encrypted system, your data is private to you and your intended recipient. In a system with client-side scanning, your data is confidential… with an _asterisk_. That is, the data itself will be private unless the scanning system determines a violation has occurred, at which point your confidentiality will be (silently) revoked and unencrypted data will be transmitted to the provider (and thus, anyone who has compromised your provider.)

This ability to selectively disable encryption creates new opportunities for attacks. If an attacker can identify the conditions that will cause the model to reduce the confidentiality of youe encryption, she can generate new — and apparently harmless — content that will cause this to happen. This will very quickly overwhelm the scanning system, rendering it useless. But it will also seriously reduce the privacy of many users.

A mirror version of this attacker exists as well: he will use knowledge of the model to _evade_ these systems, producing new imagery and content that appear unchanged, but that these systems cannot detect at all. Your most sophisticated criminals — most likely the ones who _create_ this awful content in the first place — will hide in plain sight.

Finally, a more alarming possibility exists: many neural-network classifiers allow for the _extraction_ of the images that were used to train the model. This means every complex neural network model may potentially contain images of abuse victims, who would be exposed to further harm if these models were revealed.

The only known defense against all of these attacks is to tightly protect the models themselves: that is, the ensure that the complex systems of neural network weights and/or hash fingerprints _are never revealed_. Historical server-side systems to to great lengths to protect this data, even making their very algorithms confidential. This was feasible in server-side scanning systems because the data only exists on a centralized server. It does not work well with client-side scanning, where models must be distributed to users’ phones. And so without some further technical ingredient, models cannot exist either on the server or on the user’s device.

The only serious proposal that has attempted to address this technical challenge was devised — and then subsequently abandoned — by Apple in 2021. That proposal aimed only at detecting known content using a perceptual hash function. The company proposed to use advanced cryptography to “split” the evaluation of hash comparisons between the user’s device and Apple’s servers: this ensured that the device never received a readable copy of the hash database.

Apple’s proposal failed for a number of reasons, but its technical failures provided important lessons that have largely been ignored by the Commission. While Apple’s system protected the hash database, _it did not protect the code of the proprietary neural-network-based hash function Apple devised_. Within two weeks of the public announcement, users were able to extract this code and devise both the collision attacks and evasion attacks I mentioned above.

<figure>

![](https://blog.cryptographyengineering.com/wp-content/uploads/2023/03/image-6.png?w=1024)

<figcaption>

One of the first “meaningful” collisions against NeuralHash, found by Gregory Maxwell.

</figcaption>

</figure>

<figure>

![](https://blog.cryptographyengineering.com/wp-content/uploads/2023/03/image-5.png?w=1024)

<figcaption>

Evasion attacks against Apple’s NeuralHash, from Struppek _et_ al. (source)

</figcaption>

</figure>

The Commission’s Impact Assessment deems the Apple approach to be a success, and does not grapple with this failure. I assure you that this is not how it is viewed within the technical community, and likely not within Apple itself. One of the most capable technology firms in the world threw all their knowledge against this problem, and were embarrassed by a group of hackers: essentially before the ink was dry on their proposal.

This failure is important because it illustrates the limits of our capabilities: at present we do not have an efficient means for evaluating complex neural networks in a manner that allows us to keep them secret. And so model extraction is a real possibility in all proposed client-side scanning systems _today_. Moreover, as my colleagues and I have shown, even “traditional” perceptual hash functions like Microsoft’s PhotoDNA are vulnerable to evasion and collision attacks, once their code becomes available. These attacks will proliferate, if only because 4chan is a thing: and because some people on the Internet love nothing more than hurting other Internet users.

<figure>

![](https://blog.cryptographyengineering.com/wp-content/uploads/2023/03/image-7.png?w=812)

<figcaption>

From Prokos _et al._ (source)

</figcaption>

</figure>

<figure>

![](https://blog.cryptographyengineering.com/wp-content/uploads/2023/03/image-4.png?w=1024)

<figcaption>

This example shows how a neural-network based hash function (NeuralHash) can be misled, by making imperceptible changes to an image.

</figcaption>

</figure>

In practice, the Commission’s proposal — if it is implemented in production systems — invites a range of technical attacks that we simply do not comprehend today, and that scientists have barely begun to think about. Moreover, the Commission is not content to restrain themselves to scanning for _known CSAM content_ as Apple did. Their desire to target _previously unknown content_ as well as textual content such as “grooming behavior” poses risks from many parties and requires countermeasures against abuse and surveillance that are completely undeveloped.

Worse: the “grooming behavior” requirement implies that untested, perhaps not-yet-developed AI language models will be a core part of tomorrow’s security systems. This is worrisome, since these models have failure modes and exploit opportunities that we are only beginning to explore.

In my discussion so far I have only scratched the surface of this issue. My analysis today does not consider even more basic issues, such as how we can trust that the purveyors of these opaque models are honest, and that the model contents have not been altered: perhaps by insider attack or malicious outside hackers. Each of these threats was once theoretical, and I have seen them all occur in just the last several years. Nor does it consider how the scope of these systems might be increased by future governments, and how this infrastructure will make future abuses more likely.

In conclusion, I hope that the Commission will rethink its hurried schedule and give this proposal enough time to be evaluated by scientists and researchers here in Europe and around the world. We should seek to understand these technical details as a _precondition_ for mandating new technologies, rather than attempting to “build the airplane while we are flying in it”, which is very much what this proposal will encourage.

Thank you for your time.

Go to Source
