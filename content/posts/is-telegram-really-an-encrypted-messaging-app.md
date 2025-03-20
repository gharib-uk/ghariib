---
title: "Is Telegram really an encrypted messaging app?"
date: 2025-01-15
categories: 
  - "general"
tags: 
  - "ces"
  - "intel"
---

This blog is reserved for more serious things, and ordinarily I wouldn’t spend time on questions like the above. But much as I’d like to spend my time writing about exciting topics, sometimes the world requires a bit of what Brad Delong calls “Intellectual Garbage Pickup,” namely: correcting wrong, or mostly-wrong ideas that spread unchecked across the Internet.

This post is inspired by the recent and concerning news that Telegram’s CEO Pavel Durov has been arrested by French authorities for its failure to sufficiently moderate content. While I don’t know the details, the use of criminal charges to coerce social media companies is a pretty worrying escalation, and I hope there’s more to the story.

But this arrest is not what I want to talk about today.

What I do want to talk about is one specific detail of the reporting. Specifically: the fact that nearly every news report about the arrest refers to Telegram as an “encrypted messaging app.” Here are just a few examples:

![](https://blog.cryptographyengineering.com/wp-content/uploads/2024/08/image-1.png?w=1024)

![](https://blog.cryptographyengineering.com/wp-content/uploads/2024/08/image-2.png?w=1024)

![](https://blog.cryptographyengineering.com/wp-content/uploads/2024/08/image-3.png?w=1024)

This phrasing drives me nuts because in a very limited technical sense it’s _not wrong._ Yet in every sense that matters, it fundamentally misrepresents what Telegram is and how it works in practice. And this misrepresentation is bad for both journalists and particularly for Telegram’s users, many of whom could be badly hurt as a result.

Now to the details.

### Does Telegram have encryption or doesn’t it?

Many systems use encryption in some way or another. However, when we talk about encryption in the context of modern private messaging services, the word typically has a very specific meaning: it refers to the use of default end-to-end encryption to protect users’ message content. When used in an industry-standard way, this feature ensures that every message will be encrypted using encryption keys that are only known to the communicating parties, and not to the service provider.

From your perspective as a user, an “encrypted messenger” ensures that each time you start a conversation, your messages will only be readable by the folks you intend to speak with. If the operator of a messaging service tries to view the content of your messages, all they’ll see is useless encrypted junk. That same guarantee holds for anyone who might hack into the provider’s servers, and also, for better or for worse, to law enforcement agencies that serve providers with a subpoena.

Telegram clearly fails to meet this stronger definition for a simple reason: it does not end-to-end encrypt conversations by default. If you want to use end-to-end encryption in Telegram, you must manually activate an optional end-to-end encryption feature called “Secret Chats” for every single private conversation you want to have. The feature is explicitly not turned on for the vast majority of conversations, and is only available for one-on-one conversations, and never for group chats with more than two people in them.

As a kind of a weird bonus, activating end-to-end encryption in Telegram is oddly difficult for non-expert users to actually do.

For one thing, the button that activates Telegram’s encryption feature is not visible from the main conversation pane, or from the home screen. To find it in the iOS app, I had to click at least four times — once to access the user’s profile, once to make a hidden menu pop up showing me the options, and a final time to “confirm” that I wanted to use encryption. And even after this I was not able to actually have an encrypted conversation, since _Secret Chats only works if your conversation partner happens to be online_ when you do this.

<figure>

![](https://blog.cryptographyengineering.com/wp-content/uploads/2024/08/image-7.png?w=1024)

<figcaption>

_Starting a “secret chat” with my friend Michael on the latest Telegram iOS app. From an ordinary chat screen this option isn’t directly visible. Getting it activated requires four clicks: **(1)** to get to Michael’s profile (left image), **(2)** on the “…” button to display a hidden set of options (center image), **(3)** on “Start Secret Chat”, and **(4)** on the “Are you sure…” confirmation dialog. After that I’m still unable to send Michael any messages, because Telegram’s Secret Chats can only be turned on if the other user is also online._

</figcaption>

</figure>

Overall this is quite different from the experience of starting a new encrypted chat in an industry-standard modern messaging application, which simply requires you to open a new chat window.

While it might seem like I’m being picky, the difference in adoption between default end-to-end encryption and this experience is likely very significant. The practical impact is that the vast majority of one-on-one Telegram conversations — and literally _every single group chat_ — are probably visible on Telegram’s servers, which can see and record the content of all messages sent between users. That may or may not be a problem for every Telegram user, but it’s certainly not something we’d advertise as particularly well encrypted.

(If you’re interested in the details, as well as a little bit of further criticism of Telegram’s actual encryption protocols, I’ll get into what we know about that further below.)

### But wait, does default encryption really matter?

Maybe yes, maybe no! There are two different ways to think about this.

One is that _Telegram’s lack of default encryption is just fine_ _for many people_. The reality is that many users don’t choose Telegram for encrypted private messaging at all. For plenty of people, Telegram is used more like a social media network than a private messenger.

Getting more specific, Telegram has two popular features that makes it ideal for this use-case. One of those is the ability to create and subscribe to “channels“, each of which works like a broadcast network where one person (or a small number of people) can push content out to millions of readers. When you’re _broadcasting_ messages to thousands of strangers in public, maintaining the secrecy of your chat content isn’t as important.

Telegram also supports large public group chats that can include thousands of users. These groups can be made open for the general public to join, or they can set up as invite-only. While I’ve never personally wanted to share a group chat with thousands of people, I’m told that many people enjoy this feature. In the large and _public_ instantiation, it also doesn’t really matter that Telegram group chats are unencrypted — after all, who cares about confidentiality if you’re talking in the public square?

But Telegram is not limited to just those features, and many users who join for them will also do other things.

Imagine you’re in a “public square” having a large group conversation. In that setting there may be no expectation of strong privacy, and so end-to-end encryption doesn’t really matter to you. But let’s say that you and five friends step out of the square to have a side conversation. Does _that_ conversation deserve strong privacy? It doesn’t really matter what you want, because _Telegram won’t provide it_, at least not with encryption that protects you from sharing your content with Telegram servers.

Similarly, imagine you use Telegram for its social media-like features, meaning that you mainly consume content rather than producing it. But one day your friend, who also uses Telegram for similar reasons, notices you’re on the platform and decides she wants to send you a private message. Are you concerned about privacy now? And are you each going to manually turn on the “Secret Chat” feature — even though it requires four explicit clicks through hidden menus, and even though it will prevent you from communicating immediately if one of you is offline?

My strong suspicion is that many people who join Telegram for its social media features also end up using it to communicate privately. And I think Telegram knows this, and tends to advertise itself as a “secure messenger” and talk about the platform’s encryption features precisely because they know it makes people feel more comfortable. But in practice, I also suspect that very few of those users are actually _using Telegram’s encryption_. Many of those users may not even realize they have to turn encryption on manually, and think they’re already using it.

Which brings me to my next point.

### Telegram knows its encryption is difficult to turn on, and they continue to promote their product as a secure messenger

Telegram’s encryption has been subject to heavy criticism since at least 2016 (and possibly earlier) for many of the reasons I outlined in this post. In fact, many of these criticisms were made by experts including myself, in years-old conversations with Pavel Durov on Twitter.1

Although the interaction with Durov could sometimes be harsh, I still mostly assumed good faith from Telegram back in those days. I believed that Telegram was busy growing their network and that, in time, they would improve the quality and usability of the platform’s end-to-end encryption: for example, by activating it as a default, providing support for group chats, and making it possible to start encrypted chats with offline users. I assumed that while Telegram might be a follower rather than a leader, it would eventually reach feature parity with the encryption protocols offered by Signal and WhatsApp. Of course, a second possibility was that Telegram would abandon encryption entirely — and just focus on being a social media platform.

What’s actually happened is a lot more confusing to me.

Instead of improving the usability of Telegram’s end-to-end encryption, the owners of Telegram have more or less kept their encryption UX unchanged since 2016. While there have been a few upgrades to the underlying encryption algorithms used by the platform, the user-facing experience of Secret Chats in 2024 is almost identical to the one you’d have seen eight years ago. This, despite the fact that the number of Telegram users has grown by 7-9x during the same time period.

At the same time, Telegram CEO Pavel Durov has continued to aggressively market Telegram as a “secure messenger.” Most recently he issued a scathing criticism of Signal and WhatsApp on his personal Telegram channel, implying that those systems were backdoored by the US government, and only Telegram’s independent encryption protocols were really trustworthy.

While this might be a reasonable nerd-argument if it was taking place between two platforms that both supported default end-to-end encryption, Telegram really has no legs to stand on in this particular discussion. Indeed, it no longer feels amusing to see the Telegram organization urge people away from default-encrypted messengers, while _refusing to implement essential features that would widely encrypt their own users’ messages_. In fact, it’s starting to feel a bit malicious.

### What about the boring encryption details?

This is a cryptography blog and so I’d be remiss if I didn’t spend at least a little bit of time on the boring encryption protocols. I’d also be missing a good opportunity to _let my mouth gape open in amazement_, which is pretty much what happens every time I look at the internals of Telegram’s encryption.

I’m going to handle this in one paragraph to reduce the pain, and you can feel free to skip past it if you’re not interested.

According to what I _think_ is the latest encryption spec, Telegram’s Secret Chats feature is based on a custom protocol called MTProto 2.0. This system uses 2048-bit\* finite-field Diffie-Hellman key agreement, with group parameters (I think) chosen by the server.\* (Since the Diffie-Hellman protocol is only executed interactively, this is why Secret Chats cannot be set up when one user is offline.\*) MITM protection is handled by the end-users, who must compare key fingerprints. There are some weird random nonces provided by the server, which I don’t fully understands the purpose of\* — and that in the past used to actively make the key exchange totally insecure against a malicious server (but this has long since been fixed.\*) The resulting keys are then used to power the most amazing, non-standard authenticated encryption mode ever invented, something called “Infinite Garble Extension” (IGE) based on AES and with SHA2 handling authentication.\*

NB: Every place I put a “\*” in the paragraph above is a point where expert cryptographers would, in the context of something like a professional security audit, raise their hands and ask _a lot_ of questions. I’m not going to go further than this. Suffice it to say that Telegram’s encryption is unusual.

If you ask me to guess whether the protocol and implementation of Telegram Secret Chats is secure, I would say _quite_ _possibly._ To be honest though, it doesn’t matter how secure something is if people aren’t actually using it.

### Is there anything else I should know?

Yes, unfortunately. Even though end-to-end encryption is one of the best tools we’ve developed to prevent data compromise, it is hardly the end of the story. One of the biggest privacy problems in messaging is the availability of loads of meta-data — essentially data about who uses the service, who they talk to, and when they do that talking.

This data is not typically protected by end-to-end encryption. Even in applications that are broadcast-only, such as Telegram’s channels, there is plenty of useful metadata available about _who is listening to a broadcast._ That information alone is valuable to people, as evidenced by the enormous amounts of money that traditional broadcasters spend to collect it. Right now all of that information likely exists on Telegram’s servers, where it is available to anyone who wants to collect it.

I am not specifically calling out Telegram for this, since the same problem exists with virtually every other social media network and private messenger. But it should be mentioned, just to avoid leaving you with the conclusion that encryption is all we need.

_Main photo “privacy screen” by Susan Jane Golding, used under CC license._

Notes:

1. I will never find all of these conversations again, thanks to Twitter search being so broken. If anyone can turn them up I’d appreciate it.

Go to Source
