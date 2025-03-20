---
title: "An Introduction to Open Source Licensing for complete beginners"
date: 2025-01-20
---

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_720/https://assets.ubuntu.com/v1/b6442458-JAAS%20illustration.svg)

Open source is one of the most exciting, but often misunderstood, innovations of our modern world. I still remember the first time I installed linux on my laptop, saw the vast array of packages I could install on it, all the utilities and libraries that make it work, all the forum threads filled with advice and debugging and troubleshooting, and I thought:

_“Wait, all of this is_ **_free_**_???”_

It’s free, you can use it, and it’s awesome. But you can go a surprisingly long time and still feel like you only have a vague idea of what “open source” actually means. It can take a while to get used to the jargon, the controversies, and the technical details.

If you’re new to open source, or if you’re not new and a bit embarrassed about some of your gaps in knowledge, this article is for you. We’re going to work our way from one concept to the next and by the end you’ll know the basic terms and concepts associated with software development, and why open source is such an interesting form of software development.

To understand what open source means, we need to back up a bit and start by understanding a more basic concept: intellectual property.  
  

# What is intellectual property?

Intellectual property, or IP, is the concept of ownership over something intangible, like an idea, a song, a story, or some computer code. It is distinct from concepts of private property (ownership of land and/or capital by a person or organization) and personal property (ownership of physical objects by a person) because unlike those traditional property concepts, theft of intellectual property doesn’t deprive the owner of the thing itself.

If I have a laptop and you take it, I no longer have the laptop anymore. But what if I write a book on my laptop and you hack into it and publish the book without my permission? In one sense, you didn’t really take the book away from me, I still have it. I can still open it up and read it and enjoy it. But most people have a sense that you’ve done something wrong to me. You may not have deprived me of this intangible thing that is my book, but you’ve done something with it that should be my prerogative, since I’m the one who created it. That’s what intellectual property (in this case, copyright) is all about.

Intellectual property comes in many forms, but this article will focus on copyright, which is the form of IP most relevant to open source software and software in general.

# What are copyrights?

Copyrights give the author of a creative work several exclusive rights related to the work. Among these are the right to copy, modify, and distribute that work. A copyright comes into effect the moment the creative work comes into existence, with no additional action needed from its creator.

In the example I gave before, I held a copyright over the book I wrote on my laptop _the moment I wrote it_, without needing to publish it or register it or anything.

Most copyright laws say that anything made and fixed in a tangible medium by a human that requires some degree of originality is protected by copyright. This includes (among other things) recorded or written music, written words, visual art, designs, and computer code. Interestingly, ideas themselves are not protected by copyright! For the purposes of copyright, the only thing that matters is the output of creative work or the tangible medium (the writing, drawing, code, recording, photograph, etc…) not the underlying ideas.

If I told you about a book I was writing about a tall and handsome UX designer who was extremely rich and could shoot lasers out of his hands, and then you beat me to market with your own book about a tall and handsome UX designer who was extremely rich and could shoot lasers out of his hands, that would be pretty much fair game as long as you didn’t use any of the specific words I had used.

# What are licenses?

Now that we know what copyrights are, we can talk about licenses. A license is a message from the owner of a copyrighted work describing how it can be used.

Let’s come back to my book example. If I decided to publish my book on my website, I could also publish a license along with it allowing people to use it. For example, I might encourage people to record themselves reading it, and publish the recording as an audiobook to Youtube. Or maybe I might tell people they can copy the text of the book and send it to their friends. Maybe I’ll tell people that they can print the book if they want, but they can’t sell their printed copies. Or maybe I’ll tell people they can sell those copies, but they can’t change anything, and they need to credit me as the author. I can get as specific with these terms as I want because I own the copyrights of the book, which means I have the authority over how it’s copied and distributed.

Here’s where software comes in. When a developer writes some computer code, they immediately own the exclusive right to copy, distribute, modify, use and sell that code. Many software developers will compile their code into a binary that can be run on a computer, but don’t publish the source code. But many other developers decide to also publish the source code itself, and give people more freedom over what they do with it. The way they communicate these extra freedoms is by publishing their code with something like an open source license.

# What is Open Source?

Colloquially speaking, open source software is simply software that is shared in such a way that encourages people to use it, learn from it, and build on it. But good faith without legal diligence can always be exploited, there’s a more strict definition of open source that you’ll see people insist on.

According to the open source initiative, or OSI (widely held as an authority on the subject), software is only open source if it is published with a license that meets a set of ten criteria. 

For example, in order for a project to be considered open source by the OSI, not only must the source code be available to inspect, but the source code must also be allowed to be modified and redistributed. It also can’t dictate what the software can be used for, or what other software is redistributed along with it. 

The OSI also hosts a list of licenses that they consider truly open source. If you take the OSI’s word for it, the question of whether something is open source comes down to a very simple question: is the license on the OSI’s list?

There’s nothing forcing you to take the OSI’s word for it, though. You can publish your creative works in whatever way you want, and there’s nothing to stop you from calling it open source (or “open”) without meeting the criteria set out by the OSI. But you may find that the argument over what’s open source and what’s not goes a lot deeper than you think…

# What _isn’t_ Open Source?

In order to understand what open source _is_, it’s helpful to understand what _it is not_.

## Proprietary software

Proprietary software is precisely the opposite of open source software. Proprietary software is usually distributed without the source code available, and the copyright holder retains most of their default privileges when it comes to modifying and redistributing that software. Most software is like this.

For example, you can install the Adobe suite on your computer, but you need to pay a subscription fee, use Adobe’s cloud platform, and you can’t modify the software from the form in which you receive it.

## Freeware

Freeware is software that is free in the sense that you don’t need to pay for it. You may notice that almost all open source software is also free in this way. But when you hear something called “Freeware”, that usually means that it’s not open source, it’s just free of charge. Therefore, freeware is usually proprietary.

For example, you can download the macOS operating system for free, but you can’t look at or modify the source code, and the license for macOS says that you can’t install it on any hardware that isn’t made by Apple. Which means no running macOS on your Samsung smart fridge, regardless of how undeniably cash-money that might be.

### Bonus round: Shareware

Shareware is basically like freeware, but is typically distributed with the intention that people will redistribute it. This was particularly common in the days before high-bandwidth internet. Developers that wanted to let people try their software for free couldn’t rely on downloads, because not everyone would be able to download that much data. Instead, developers would release their sample software and encourage people to burn it onto floppy disks and hand them out to their friends. It was a particularly popular way of distributing video game demos, such as those for Doom and Quake.

## Source-available

Source-available software generally lets you read the source code, but restricts you from modifying it or redistributing it.

For example, Epic’s Unreal Engine. According to the Unreal Engine End User License, you can download and use the Unreal Engine, and you can look at its source code, but you can’t publicly redistribute modified versions of the code.

# What is Free Software?

Surely it’s just software that’s free, right?

We need to distinguish between two related but distinct meanings of the word _free_. There’s _free_ in the sense of “free beer”, and free in the sense of freedom.

For proponents of “Free Software” (which is capitalized), it’s not enough for something to cost nothing for it to be free. It’s not even enough for it to be open source. Free Software, in the strict sense, is software that respects and upholds your freedoms. Which freedoms? Per the explanation from GNU:

- The freedom to run the program as you wish, for any purpose (freedom 0).
- The freedom to study how the program works, and change it so it does your computing as you wish (freedom 1). Access to the source code is a precondition for this.
- The freedom to redistribute copies so you can help others (freedom 2).
- The freedom to distribute copies of your modified versions to others (freedom 3). By doing this you can give the whole community a chance to benefit from your changes. Access to the source code is a precondition for this.

I’ve listed out the freedoms because I want you to notice something: this is a fundamentally different kind of thing compared to open source as described by the OSI. Whereas open source is a _legal_ definition, Free Software is a _moral_ definition. Whereas the goal of open source is to compete with proprietary software as a commercially viable alternative, the goal of Free Software is to fundamentally rethink how we relate to software on a personal level.

# What is copyleft?

Picture this: You’ve published some open source software, hoping that curious and collaborative people will use it, look at the source code, learn something from it, and maybe even help you find bugs or add features. But then, a big corporation comes along, takes your code, and uses it in their proprietary software product. On the one hand, your code is being freely used (by the big corporation). On the other hand that freedom only lasted one generation, so to speak.

This is the problem that “copyleft” licenses are meant to solve. A copyleft license gives the user of a piece of software the ability to create and distribute derivative works, _under the condition that those derivative works are also published with a copyleft license_. This means that once you’ve published your work under a copyleft license, it will stay copyleft forever.

# What is a permissive license?

Picture this: You’ve written some software, and you want to publish it with an open source license, so that people can do whatever they want with it. Your friend comes along, and tells you that you should use a copyleft license, in order to make sure that your work doesn’t become proprietary, and make sure that people need to credit you, and make sure that the software stays free. But maybe you don’t care about all that. In your mind, you want the users of your software to do whatever they want with it, including making it proprietary or making it closed-source. This is the motivation behind permissive licenses.

In general, there’s a spectrum between copyleft and permissive. You’ll typically see comparisons between licenses, where one is said to be “more permissive” than the other. The more permissive a license is, the more it says “do whatever you want, I don’t care”. The less permissive a license is, the more it says “do what you want, _within these limits_.”

Because “permissive” sounds freedom-y, it seems strange that permissive licenses are the ones that can lead to open source software becoming closed source. But the copyleft philosophy is that if you give people the freedom to restrict other people’s freedom, the net effect is less freedom. According to this philosophy, a permissive license is sort of like the parent who lets their kid stay out as late as they want, eat candy for dinner, and skip school. The kid might have more freedom in the short term, but it could end up limiting their options later on.

But there are reasons to release your work under a permissive license other than a commitment to absolute anarchy. If your project is very small, or it didn’t take you very long to create, or you plan on abandoning it entirely, it might just be easier to release it into the world with no strings attached rather than to spend time worrying about how to license it. For projects like hackathons or daily programming exercises that aren’t likely to turn into critical infrastructure in someone else’s technology stack, a big huge license like the GPL might be overkill. The license file itself might even be bigger than the entirety of the code that it licenses!

# What is a CLA?

Open source licenses generally cover the intellectual property considerations of source code as it is distributed, used, modified, and redistributed. But most open source projects also have a way for people to contribute changes to the software. For example, active projects hosted on GitHub usually let people submit pull requests (PRs). This ecosystem, where project maintainers receive code changes from contributors, is central to open source culture.

However, this makes the IP aspects of open source projects tricky. Generally speaking, any changes a contributor makes to an open source project are their own IP, and theoretically, the contributor could choose their own licenses that are different from those governing the rest of the software project.

Why does this matter? Well, imagine that I create some software and publish it on GitHub with an open source license. Then you come along and write a new feature, and submit a PR to add it to the project. I think your new feature is great, and I accept the changes. Everyone’s happy. But now, I create a premium version of the software that I sell for 5 bucks, and I keep all of the money. “Wait!” you say, “You can’t take this open source project and sell it for profit!” “Why not?” I say, “It’s my project.” “But”, you reply, “One of the premium features that you’re selling is _my_ work. I never gave you permission to sell it!” And the thing is, in this situation, you’re right.

The feature you wrote is your intellectual property, and by accepting it into my project, I limited what I could do with the project as a whole.

This is why many projects have a contributor license agreement, or CLA. If a project wants to accept contributions from the public, and also wants to sustain itself and make money, it will only accept changes from contributors who agreed to certain terms laid out in the CLA. Usually, those terms basically amount to “You can contribute, but you give us permission to use those contributions as we see fit.”

CLAs are sometimes seen as controversial in the Free Software movement. For some, CLAs introduce unnecessary friction in the contribution process.  Others argue that CLAs are necessary for ensuring that an open source project can be monetized, without which it would be impossible to sustain an open source business.

The topic of CLAs is complex, and I’m really only scratching the surface of the arguments for and against them. As a newcomer to open source, you don’t need to have an opinion about them right away. But you should know that they exist, and do more research to decide whether they’re right for you.

# I’m not a developer, but I think open source sounds really cool. Should I make things under an open source license?

That’s a very interesting question. The issue is that open source licenses are tailor-made for software. Because of the particular way software development comprises source code and compiled code, technology and artistry, design and implementation, open source licenses contain a lot of language and mechanisms that don’t make any sense for creative works that aren’t software.

For this reason, I would recommend using a Creative Commons license, rather than an Open Source license, for creative works like designs, artwork, and writing.

## What is Creative Commons?

Creative commons is a non-profit organization that provides licenses that anyone can use to publish their creative works. Creative commons licenses are similar to open source licenses in spirit: they usually allow people to take the licensed work and do whatever they want with it, under certain conditions that the creator can set.

For example, maybe you want to record a song and allow people to remix it, as long as they credit you. Or maybe you want to give away your art for free, but you don’t want people using it to sell anything. Or maybe you just want to waive all copyright claims over your creations. There are usually creative commons licenses that satisfy any conditions that you as a creator can think of. In my opinion, non-developers who are excited about open source should consider using Creative Commons licenses.

# Conclusion

I’ve only really scratched the surface in this article, but I hope it’s filled in some of the gaps that are most often left when people try to explain what’s going on with open source. The topics covered here are what I consider to be the most important foundational ideas on which everything else is built.

For those of you who learned something new, let us know what you’d like to know next!

Go to Source
