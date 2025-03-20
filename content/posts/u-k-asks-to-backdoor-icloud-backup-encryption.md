---
title: "U.K. asks to backdoor iCloud Backup encryption"
date: 2025-03-19
categories: 
  - "apple"
  - "backdoors"
  - "cybersecurity"
  - "iphone"
  - "security"
---

_I’m supposed to be finishing a wonky series on proof systems (here and here) and I promise I will do that this week. In the midst of this I’ve been a bit distracted by world events._

Last week the Washington Post published a bombshell story announcing that the U.K. had filed “technical capability notices” demanding that Apple modify the encryption used in their Advanced Data Protection (ADP) system for iCloud. For those who don’t know about ADP, it’s a system that enables full “end-to-end” encryption for iCloud backups, including photos and other data. This means that your backup data should be encrypted under your phone’s passcode — and critically, neither Apple nor hackers can access it. The U.K. request would secretly weaken that feature for at least some users.

Along with Alex Stamos, I wrote a short opinion piece (version without paywall) at the Wall Street Journal and I wanted to elaborate on the news a bit more here.

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/02/image-12.png?w=1024)

### What’s iCloud and what’s ADP?

Most Apple phones and tablets are configured to automatically back their contents up to a service called iCloud Backup, which maintains a nearly-mirror copy of every file on your phone. This includes your private notes, contacts, private iMessage conversations, personal photos, and so on. So far I doubt I’m saying anything too surprising to the typical Apple user.

What many people don’t know is that in normal operation, this backup is not end-to-end encrypted. That is, Apple is given a decryption key that can access all your data. This is good in some ways — if you lose your phone and also forget your password, Apple _might_ still be able to help you access your data. But it also creates a huge weakness. Two different types of “bad guys” can walk through the hole created by this vulnerability: one type includes hackers and criminals, including sophisticated state-sponsored cyber-intrusion groups. The other is national governments: typically, law enforcement and national intelligence agencies.

Since Apple’s servers hold the decryption key, the company can be asked (or their servers can be hacked) to provide a complete backup copy of your phone at any moment. Notably, since this all happens on the server side, you’ll never even know it happened. Every night your phone sends up a copy of its contents, and then you just have to hope that nobody else obtains them.

This is a bad situation, and Apple has been somewhat slow to remedy it. This is surprising, since Google has enabled end-to-end encrypted backup since 2018. Usually Apple is the company leading the way on privacy and security, but in this case they’re trailing? Why?

In the past we’ve seen various hints about this. For example, in 2020, Reuters published a story claiming that the FBI had convinced Apple to drop its plans for encrypted backups as a result of agency pressure. This is bad! Of course, Apple never confirmed this, but Apple never confirms anything.

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/02/image-13.png?w=1024)

In 2021, Apple proposed a byzantine new system for performing client-side scanning of iCloud Photos, in order to detect child sexual abuse material (CSAM). Technical experts pointed out that this was a bizarre architecture, since client-side scanning is something you do when you can’t scan photos on the server — usually because that data is encrypted. However at that time Apple refused to countenance the notion that they were considering end-to-end encryption for backup data.

Then, at the end of 2022, Apple finally dropped the other shoe. The new feature they were deploying was called Advanced Data Protection (ADP), and it would finally enable end-to-end encryption for iCloud Backup and iCloud Photos. This was an opt-in mode and so you’d have to turn it on manually. But if you did this, your backups would be encrypted securely under your phone’s passcode — something you _should_ remember because you have to type it in every day — and even Apple would not be able to access them.

The FBI found this very upsetting. But, in a country with a Fourth and First Amendment, at least in principle, there wasn’t much they could do if a U.S. firm wanted to deploy software that enabled users to encrypt their own data.

<figure>

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/02/image-16.png?w=1024)

<figcaption>

_Go into “Settings”, type “Advanced data” and turn it on._

</figcaption>

</figure>

### But… what about other countries?

Apple operates in hundreds of different countries, and not all of them have laws similar to the United States. I’ve written before about Apple’s stance in China — which, surprisingly, does not appear to involve any encryption backdoors. But of course, this story involves countries that are closer to the US, both geographically and politically.

In 2016, the U.K. passed the Investigatory Powers Act (IPA), sometimes known as the “Snooper’s Charter.” The IPA includes a clause that allows His Majesty’s government to submit Technical Capabiilty Notices to technology firms like Apple. A Technical Capability Notice (TCN) under U.K. law is a secret request in which the government demands that a technical provider _quietly modify the operation of its systems so that they no longer provide the security feature advertised to users_. In this case, presumably, that would involve weakening the end-to-end encryption system built into iCloud/ADP so that the U.K. could request downloads of encrypted user backups _even without access to the user’s passcode or device._

The secrecy implicit in the TCN process is a massive problem here, since it effectively requires that Apple lie to its users. To comply with U.K. law, they must swear that a product is safe and works one way — and this lying must be directed to both civilian users and U.S. government users, commercial users, and so on — while Apple is forced to actively re-design their products to work differently. The dangers here should be obvious, along with the enormous risks to Apple’s reputation as a trustworthy provider. I will reiterate that this is not something that even China has demanded of Apple, as far as we know, so it is quite alarming.

The second risk here is that the U.K. law does not obviously limit these requests to U.K. customers. In a filing that Apple submitted back in 2024, the company’s lawyers make this point explicitly:

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/02/image-14.png?w=1024)

And when you think about it — this part I am admittedly speculating about — it seems really surprising that the U.K. would make these requests of a U.S. company without at least speaking to their counterparts in the United States. After all, the U.K. and the U.S. are part of an intelligence alliance known as Five Eyes. They work together on this stuff! There are at least two possibilities here:

1. The U.K. is operating alone in a manner that poses serious cybersecurity and business risks to U.S. companies.

4. The U.S. and the U.K. intelligence (and perhaps some law enforcement agencies) have discussed the request, and both see significant benefit from the U.K. possessing this capability.

We can’t really know what’s going on here, but both options should make us uncomfortable. The first implies that the U.K. is going rogue and possibly harming U.S. security and business, while the latter implies that some level of U.S. agencies are at tacitly signing off on a capability that could be used illegally against U.S. users.

What we do know from the recent Post article is that Apple was allegedly so uncomfortable with the recent U.K. request that they are “likely to stop offering encrypted storage in the U.K.“, _i.e.,_ they were at least going to turn off Advanced Data Protection in the U.K. This might or might not have resolved the controversy with the U.K. government, but it at least it indicated that Apple is not going to quietly entertain these requests.

## What about other countries?

There are about 68 million people in the U.K., which is not a small number. But compared to other markets Apple sells in, it’s not a big place.

In the past, U.S. firms like WhatsApp and Signal have in the past made plausible threats to exit the U.K. market if the U.K. government makes good on threats to demand encryption backdoors. I have no doubt that Apple is willing to go to the mat for this as well if they’re forced to — _as long as we’re only talking about the U.K._ This is really sad for U.K. users, who deserve to have nice things and secure devices.

But there are bigger markets than the U.K. The European Union has 449.2 million customers and has been debating laws that would demand some access to encrypted messaging. China has somewhere between two to three times that. These are big markets to risk over encryption! Moreover, Apple builds a lot of its phones (and phone parts) in China. While I’m an optimist about human ethics — even within big companies — I doubt that Apple can convince its shareholders that their relationship with China is worth risking, over something abstract like the value of trust, or over end-to-end encrypted messages or iCloud.

And that’s what’s at stake if Apple gives in to the U.K. demands. If Apple gives in here, there’s zero reason for China not to ask for the same thing, perhaps this time applied to Apple’s popular iMessage service. And honestly, they’re not wrong? Agreeing to the U.K.’s request might allow the U.K. and Five Eyes to do things that would harm China’s own users. In short, abandoning Apple’s principles one place means they ultimately have to give in anywhere (or worse), or — _and this is a realistic alternative —_ Apple is forced to leave many parts of the world. Both are bad for the United States, and both are bad for people in all countries.

### So what should we do legally?

If you read the editorial, it has a simple recommendation. The U.S. should pass laws that forbid U.S. companies from installing encryption backdoors at the request of foreign countries. This would put companies like Apple in a bind. But it would be a good bind! To satisfy the laws of one nation, Apple would have to break the laws of their home country. This creates a “conflict of laws” situation where, at very least, simple, quiet compliance against the interest of U.S. citizens and customers is no longer an option for Apple — even if the shareholders might theoretically prefer it.

I hope this is a policy that many people could agree on, regardless of where they stand politically.

### So what should we do technically?

I am going to make one more point here that can’t fit in an editorial, but deserves to be said anyway. We wouldn’t be in this jam if Apple had sucked it up and deployed end-to-end encrypted backup more broadly, and much earlier in the game.

Over the past decade I’ve watched various governments make a strong push to stop device encryption, add “warrant” capability to end-to-end encrypted messaging, and then install scanning systems to monitor for illicit content. Nearly all of these attempts failed. The biggest contributor to the failure was widespread deployment and adoption of encryption.

Once a system is widely deployed and people realize it’s adding value and safety to a system, they are loath to mess with that system. You see this pattern first with on-device encryption, and then with messaging. A technology is at first controversial, at first untenable, and then suddenly it’s mandatory for digital security. Even law enforcement agencies eventually start begging people to turn it on:

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/02/image-15.png?w=1024)

A key ingredient for this transition to occur is that lots of people must be leaning on that technology. If 1-2% of the customer base uses optional iCloud encryption, then it’s easy to turn the feature off. Annoying for a small subset of the population, maybe, but probably politically viable for governments to risk it. The same thing is less true at 25% adoption, and it is not true at 50% or 100% adoption.

Apple built the technology to deploy iCloud end-to-end encryption way back in 2016. They then fiddled around, not deploying it _even as an option_, for more than six years. Finally at the end of 2022 they allowed people to opt-in to Advanced Data Protection. But even then they didn’t make it a default, they don’t ask you if you want to turn it on during setup. They barely even advertise it to anyone.

If someone built an encryption feature but nobody heard about it, would it still exist?

This news from the U.K. is a wake up call to Apple that they need to move more quickly. They may feel intimidated due to blowback from Five Eyes nations, and that might be driving their reticence. But it’s too late, the cat is out of the bag. People are noticing their failure to turn this feature on and — while I’m certain there are excellent reasons for them to go slow — the silence and slow-rolling is starting to look like weakness, or even collaboration with foreign governments.

Hell, even Google offers this feature, on by default!

So what I would like, as a technologist, is for Apple to act serious about this technology. It’s important, and the ball is very much in Apple’s court to start pushing those numbers up. The world is not a safe place, and it’s not getting safer. Apple should do the right thing here because they want to. But if not, they should do it because doing otherwise is too much of a liability.
