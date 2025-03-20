---
title: "Let’s talk about AI and end-to-end encryption"
date: 2025-01-17
categories: 
  - "ai"
  - "cybersecurity"
  - "encryption"
  - "privacy"
  - "security"
tags: 
  - "imessage"
---

Recently, I came across a fantastic new paper by a group of NYU and Cornell researchers entitled “How to think about end-to-end encryption and AI.” I’m extremely grateful to see this paper, because while I don’t agree with _every single one_ of it’s conclusions, it’s a good first stab at an incredibly important set of questions.

I was particularly glad to see this paper, but this exact topic has been very much on my mind this past few months. On the one hand, my interest stems from the rapid deployment of new AI assistant systems like Google’s scam call protection and Apple Intelligence, both of which aim to put AI basically everywhere on your phone — even, potentially, right in the middle of your encrypted messages. On the other hand, it comes from a recent European debate over “mandatory content scanning” laws that would require AI to scan virtually every private message you send.

While these two subjects seem very different, at a certain point I’ve came to believe that maybe they aren’t. And worse, as someone who has been fighting in the “crypto wars” for over a decade, this has forced me to ask some questions about what the future of end-to-end encryption will look like, and if it even _has_ a future.

But let’s start from an easier direcction.

### What is end-to-end encryption, and what does AI have to do with it?

From a privacy perspective, the most important story of the past decade or so has been the rise of end-to-end encrypted communications platforms. Prior to 2011, most cloud-connected devices simply uploaded their data to unsecured servers (or, if you were lucky, kept it stored locally.) For many people this meant their data was exposed to hackers, litigation, government subpoenas, and even exploitation by the platforms themselves. With the exception of a few advanced users who knew how to use tools like PGP and OTR, we end-users mostly had to suffer the consequences.  
  
And (spoiler alert) oh boy, did those consequences turn out to be terrible.

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/01/image-7.png?w=1024)

After 2011 our approach to data storage began to change. Messaging apps like Signal, Apple iMessage and WhatsApp began to roll out default end-to-end encryption for private messaging. This approach ensured that servers would _never see the plaintext content of your messages._ Manufacturers like Google, Samsung and Apple also began encrypting the data stored locally on your phone, a move that quickly triggered exciting arguments with law enforcement. More recently, providers like Google have introduced default end-to-end encryption for cloud backups, and (somewhat belatedly) their competitors have begun to follow.

Nothing about this project was easy — I stress, it involved an enormous amount of engineering work, by hard-working brilliant people. And yet it was simple in a way. All of the data encrypted by these projects shared a common feature: none of it _needed to be processed by a server._

The obvious problem is that while encrypted data hides content from servers, it also makes it difficult for servers to compute on that data.1 For backups and private messages this is basically just fine, since that content is mostly just stored on the servers and downloaded for use by clients. But for data that actually requires serious processing, the options are more limited. When this is the case — for example, if your phone applies text recognition to your photo library — designers have typically been forced into a binary choice: they can send the plaintext off to a server, in the process bringing back many of the old vulnerabilities, or else they can limit their processing to whatever can be performed on the device itself.

The problem with the second solution is that the typical phone has a limited amount of processing power, RAM and battery. This is why your iPhone will typically process photos while it’s plugged in at night, rather than burning through your battery while you’re on the go. Moreover, some phone hardware can barely do any processing. While a top-end phone will run you $1400 or more, it’s still possible to buy a cheap Android phone for a couple hundred bucks. There’s a vast world of difference between these devices in terms of what they can do, but manufacturers want you to have it all.

This finally brings us to the matter of AI — which will probably end up being the biggest privacy story of _this_ decade, and possibly the next.

Unless you’ve been living under a rock, you’ve probably noticed the explosion of powerful new AI models, as well as Silicon Valley’s excitement to find new applications for this technology. Phone and messaging companies are no exception. Even if they don’t quite know _how_ AI will be useful to their customers, firms like Google and Apple have decided that AI models are the future. We’ve already seen the deployment of applications such as text message summarization, detection of scam phone calls, offensive image recognition, and new systems that help you compose text.

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/01/image-2.png?w=1024)

And this is just the beginning.

If you listen to the AI futurists — and in this case, I think you should — all of this is really just a palate cleanser for the _really_ neat stuff still to come: namely, the deployment of “AI agents,” i.e., Your Plastic Pal Who’s Fun To Be With. _In theory_ those systems could remove your need to ever touch your phone again: they’ll answer your text messages for you, order your food, swipe your dating profile, negotiate with your lenders, and generally anticipate your every want or need. The only ingredient they’ll need to make this future come true is _virtually unrestricted access to all your private data_, plus a whole gob of computing power to process it.

Which finally brings us to the sticky part.

At present, most phones just don’t have quite enough compute, memory and battery power to reliably run the very high-end models we will need to realize many of these applications. This means, for at least some users — and possibly _most_ users for the foreseeable future if models keep getting bigger and more powerful and more proprietary — much of this processing (and the data we need processed) will have to be offloaded to remote servers.

And this is (only) one reason that I say that AI could be the biggest privacy story of the current decade. Not only will we be sending more of our data off-device, but it’ll be examined by increasingly powerful systems. In principle those systems will know everything about us, our friends, our private conversations, maybe even our deepest innermost thoughts. We are about to face many hard questions about these systems, including some difficult questions about _whether they will actually be working for us at all._

But I’ve gone about four steps farther than I intended to. Let’s start with the easier questions.

### What does AI mean for end-to-end encrypted messaging?

Modern end-to-end encrypted messaging systems provide a very specific set of technical guarantees. Concretely, an end-to-end encrypted system is designed to ensure that plaintext message content is not available anywhere except for the end-devices of the participants and (here comes the Big Asterisk) _anyone the participants or their devices choose to share it with._

The italicized part of that sentence is pretty important, and we technologists probably don’t stress it enough. End-to-end encrypted messaging systems are intended to deliver data securely. _They don’t dictate what happens to it next._ If you take screenshots of your messages, back them up in plaintext, copy/paste your data onto Twitter, or hand over your device in response to a civil lawsuit — well, that has really nothing to do with end-to-end encryption. Once the data has been delivered from one end to the other, end-to-end encryption washes its hands and goes home

Of course there’s a difference between _technical guarantees_ and the promises that individual service providers make to their customers. For example, Apple says this about its iMessage service:

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/01/image-3.png?w=1024)

That promise can be a little bit confusing! For example, imagine that Apple keeps its promise to deliver messages securely, but then your (Apple) phone goes ahead and uploads (the plaintext) message content to a different set of servers where Apple really can read it. Apple is absolutely using end-to-end encryption in the dull technical sense — but is the statement above really accurate? Is Apple keeping its broader promise that it “can’t decrypt the data”?

(NB: I am not picking on Apple here! This is one paragraph from a long and detailed security overview. In a separate legal document, Apple’s lawyers have written a zillion surprisingly-readable words to cover their asses. My point here is just to point out that a _technical guarantee_ and a _user promise_ can be two different things.)

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/01/img_9881.jpg?w=1024)

Or let’s consider another situation. Each time you start a WhatsApp group chat, you receive a message like the one at the right. Now imagine that some other member of the group — not you, but one of your friends — decides use something like a text summarization service operated by WhatsApp. Is this message still accurate? If not, how should it change?

In general, what we’re asking here is a question about _informed_ _consent._ Consent is complicated because it’s both a moral concept and also a legal one, and the law is different in every corner of the world. The NYU/Cornell paper does an excellent job on the legal parts, but I _really_ _don’t want you to get your hopes up._ I expect that some companies will do a good job informing their users because they want to earn those users’ trust. Others, well — here in the US, they’ll bury your consent inside of an inscrutable “terms of service” document you never read. In the EU they’ll probably give you a cookie banner:

<figure>

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/01/image-4.png?w=1024)

<figcaption>

_This is obviously a joke. But, like, maybe not?_

</figcaption>

</figure>

The TL;DR here is that in the near term we’re headed to a world where we should expect increasing amounts of our data to be processed by AI, and some of that processing will (very likely) be off-device. Good end-to-end encrypted services will actively inform you that this is happening, so maybe you’ll have the chance to opt-in or opt-out. But if it’s _truly ubiquitous_ (as our futurist friends tell us it will be) then probably your options will be pretty limited.

<figure>

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/01/image-5.png?w=672)

<figcaption>

Some ideas for avoiding AI inference on your private messages (cribbed from Mickens.)

</figcaption>

</figure>

Fortunately all is not lost. In the short term, some folks have been thinking hard about this problem.

### Trusted Hardware and Apple’s “Private Cloud Compute”

The good news is that our coming AI future isn’t going to be an immediate privacy disaster. And in large part that’s because a few privacy-focused companies like Apple have anticipated the problems that AI inference will create — namely the need to outsource processing to better hardware — and they’ve tried to do something about it.

Since, as we already discussed, Apple can’t rely on every Apple device possessing enough computing power (or RAM and battery) to perform inference locally, they anticipate the need to push their queries off to some remote data center. The connection from your phone to the data center can be encrypted easily, of course. But the the remote machine is an obvious potential target. It will need to receive highly confidential data from your phone, and it will need that data in cleartext in order to perform ML inference on it. The presence of all this (cleartext) private data on this computer poses a risk. It could be stolen by hackers, of course, but worse: Apple’s Business Development executives might be tempted to read this data and find ways to sell it.

Apple’s approach to this problem is called “Private Cloud Compute” and it involves the use of special _trusted hardware_ devices that run in Apple’s data centers. A “trusted hardware device” seems like an oxymoron, but really it’s just a kind of computer that is highly locked-down in both the physical and logical sense. Apple’s machines are house-made, and use custom silicon features such as Apple’s Secure Boot system to ensure that only “allowed” operating system software is loaded. The OS then uses code-signing to ensure that only allowed software images can run. Apple ensures that no long-term state is stored on these machines, and also bounces your request to a different random server every time you connect. The machines “attest” to the application software they run, meaning that they announce a hash of the software image (which must itself be stored in a verifiable transparency log to prevent any new software from surreptitiously being added.) Apple even says it will publish its software images (though unfortunately not the source code) so that security researchers can check them over for bugs.

The goal of this system is to make it hard for both attackers _and_ Apple employees to exfiltrate data from these devices. While this is a vastly weaker guarantee than encryption — where Apple would have to break cryptography, rather than software, to obtain data — it is still better than sending the data to a server that Apple’s employees can simply log into.

Although Apple’s PCC approach is currently unique to Apple, we can hope that other privacy-focused services will soon crib the idea from Apple — after all, imitation is the best form of flattery — and then if we have to use AI everywhere, at least the result will be a bit more secure than it might otherwise be.

### Who does your AI agent actually work for?

There are _so_ many important questions that I haven’t discussed in this post, and I feel guilty because I’m not going to get to them. I have not, for example, discussed the very important problem of the data used to _train_ (and fine-tune) future AI models, something that opens entire oceans of privacy questions. If you’re interested, these problems are discussed in the NYU/Cornell paper.

But rather than go there, I want to turn this discussion towards a different question: namely, _who are these models actually going to be working for?_

Just to reiterate, over the past couple of years I’ve been watching the UK and EU debate new laws that would mandate automated “scanning” of private, encrypted messages. The EU’s proposal is focused on detecting both existing and _novel_ child sexual abuse material (CSAM); it has at various points also included proposals to detect audio and textual conversations that represent “grooming behavior.” The UK’s proposal is a bit wilder and covers many different types of illegal content, including hate speech, terrorism content and fraud — one proposed amendment even covered “images of immigrants crossing the Channel in small boats.”

<figure>

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/01/image-6.png?w=1024)

<figcaption>

A German immigrant.

</figcaption>

</figure>

While _known_ CSAM scanning can be done without ML, nearly every other form of content scanning would require powerful ML inference that can operate on private data. This is obvious when you consider what might be required to detect novel CSAM content. And of course, audio or textual “grooming behavior” and hate speech detectors are going to require even more — not only speech-to-text capabilities, but _also_ some form of powerful ML scanner that can read and understand complex human conversations In a nutshell, these laws are asking to mandate a heck of a lot of AI.content.

These proposals haven’t been implemented yet, to some degree part because that sort of AI scanning system is technically challenging to build for encrypted content. But I invite you to imagine a world where _we’ve already built it, and it’s running on your phone_. Now imagine further that the thing we built was not some highly-limited and specialized model, but rather _a powerful AI “agent” that can answer all sorts of questions_ _about you_ — and even do so in plain language. Given the right API, the government could ask your agent all sorts of sophisticated questions about your behavior and data, questions like: “does this user have any potential CSAM,” or “have they written anything that could potentially be hate speech in their private notes,” or “do you think maybe they’re cheating on their taxes?” And the model would probably be able to give a very good answer.

What’s most worrying is that, while many governments have opposed scanning laws on pure privacy grounds, others have been relatively enthusiastic — only pausing due to the obvious technical difficulty of realizing these proposals today. In the future, many of these technical difficulties may melt away.

This future worries me because it doesn’t really matter if your model is running locally, or if it uses trusted cloud hardware — _once a sufficiently-powerful general-purpose agent has been deployed on your phone_, the only question that remains is who gets to talk to it. And that isn’t really a technical question anymore, it’s a policy question. Will it be only you? Or will we prioritize the government’s interest in monitoring its citizens over various fuddy-duddy notions of individual privacy.

And while I’d like to say we’ll always do the right thing here, maybe I’m just not so confident anymore.

_Notes:_

1. (A quick note: some will suggest that Apple should use fully-homomorphic encryption \[FHE\] for this calculation, so the private data can remain encrypted. This is _theoretically_ possible, but unlikely to be practical. The best FHE schemes we have today really only work for evaluating very tiny ML models, of the sort that would be practical to run on a weak client device. While schemes will get better and hardware will too, I suspect this barrier will exist for a long time to come.)

Go to Source
