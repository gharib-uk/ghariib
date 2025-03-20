---
title: "Three questions about Apple, encryption, and the U.K."
date: 2025-03-19
categories: 
  - "apple"
  - "backdoors"
  - "cybersecurity"
  - "encryption"
  - "security"
---

Two weeks ago, the Washington Post reported that the U.K. government had issued a secret order to Apple demanding that the company include a “backdoor” into the company’s end-to-end encrypted iCloud Backup feature. From the article:

> The British government’s undisclosed order, issued last month, requires blanket capability to view fully encrypted material, not merely assistance in cracking a specific account, and has no known precedent in major democracies. Its application would mark a significant defeat for tech companies in their decades-long battle to avoid being wielded as government tools against their users, the people said, speaking under the condition of anonymity to discuss legally and politically sensitive issues.

That same report predicted that Apple would soon be disabling their end-to-end encrypted iCloud backup feature (called Advanced Data Protection) for all U.K. users. On Friday, this prediction was confirmed:

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/02/image-22.png?w=1024)

Apple’s decision to disable their encrypted cloud backup feature has triggered many reactions, including a few angry takes by Apple critics, accusing Apple of selling out its users:

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/02/image-19.png?w=1024)

With all this in mind, I think it’s time to take a sober look at what might really happening here. This will require some speculation and educated guesswork. But I think that exercise will be a lot more helpful to us if we want to find out what’s really going on.

### Question 1: does Apple really care about encryption?

Encryption is a tool that protects user data by processing it using a key, so that only the holder of the appropriate key can read it. A variant called _end-to-end encryption_ (E2EE) uses keys that only the user (or _users_) knows. The benefit of this approach is that data is protected from many threats that face centralized repositories: theft, cyber attacks, and even access by sophisticated state-sponsored attackers. One downside of this encryption is that it can also block governments and law enforcement agencies from accessing the same data.

Navigating this tradeoff has been a thorny problem for Apple. Nevertheless, Apple has _mostly_ opted to err on the side of aggressive deployment of (end-to-end) encryption. For some examples:

1. In 2008, the company began encrypting all iPhone internal data storage by default. This is why you can feel safe (about your data) if you ever leave your iPhone in a cab.

4. In 2011, the company launched iMessage, a built-in messaging service with default end-to-end encryption for all users. This was the first widely-deployed end-to-end encrypted messaging service. Today these systems are recommended even by the FBI.

7. In 2013, Apple launched iCloud Key Vault, which encrypts your backed-up passwords and browser history using encryption that even Apple can’t access.

Apple faced law enforcement backlash on each of these moves. But perhaps the most famous example of Apple’s aggressive stance on encryption occurred during the 2016 Apple v. FBI case, where the company actively fought U.S. government’s demands to bypass encryption mechanisms on an iPhone belonging to an alleged terrorist. Apple argued that satisfying the government’s demand would have required Apple to weaken encryption on all of the company’s phones. Tim Cook even took the unusual step of signing a public letter defending the company’s use of encryption:

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/02/image-21.png?w=1024)

I wouldn’t be telling you the truth if I failed to mention that Apple has also made some big mistakes. In 2021, the company announced a plan to implement client-side scanning of iCloud Photos to search for evidence of illicit material in private photo libraries. This would have opened the door for many different types of government-enforced data scanning, scanning that would work even if data was backed up in an end-to-end encrypted form. In that instance, technical experts quickly found flaws in Apple’s proposal and it was first paused, then completely abandoned in 2022.

This is not intended to be a hagiography for Apple. I’m simply pointing out that the company has, in the past, taken major public risks to deploy and promote encryption. Based on this history, I’m going to give Apple the benefit of the doubt and assume that the company is not racing to sell out its users.

### Question 2: what was the U.K. really asking for?

Way back in 2016, the U.K. passed a bill called the Investigatory Powers Act, sometimes called the “Snooper’s Charter.” At the time the law was enacted, many critics argued that it could be used to secretly weaken security systems, potentially making them much more vulnerable to hacking.

This was due to a critical feature of the new law: it enables the U.K. government to issue secret “Technical Capability Notices” that can force a provider, such as Apple, to _secretly_ _change the operation of their system_ — for example, altering an end-to-end encrypted system so that Apple would be forced to hold a copy of the user’s key. With this modification in place, the U.K. government could then demand access to any user’s data on demand.

By far the most concerning part of the U.K. law is that it does not clearly distinguish between U.K. customers and non-U.K. customers, such as those of us in the U.S. or other European nations. Apple’s lawyers called this out in a 2024 filing to Parliament:

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/02/image-14.png)

In the _worst-case_ interpretation of the law, the U.K. might now be the arbiter of all cybersecurity defense measures globally. Her Majesty’s Government could effectively “cap” the amount of digital security that customers anywhere in the world can depend on, without users even knowing that cap was in place. This could expose vast amounts of data to state-sponsored attackers, such as the ones who recently compromised the entire U.S. telecom industry. Worse, because the U.K.’s Technical Capability Notices are secret, companies like Apple would be effectively forced to lie to their customers — convincing them that their devices are secure, when in fact they are not.

It goes without saying that this is a very dangerous road to start down.

### Question 3: how might Apple respond to a broad global demand from the U.K.?

Let us imagine, hypothetically, that this worst-case demand is exactly what Apple is faced with. The U.K. government asks Apple to secretly modify their system for all users globally, so that it is no longer end-to-end encrypted anywhere in the world.

(_And if you think about it practically:_ that flavor of demand seems almost unavoidable in practice. Even if you imagine that Apple is only being asked only to target users in the U.K., the company would either need to build this capability globally, or it would need to deploy a new version or “zone”1 for U.K. users that would work differently from the version for, say, U.S. users. From a technical perspective, this would be tantamount to admitting that the U.K.’s version is somehow operationally distinct from the U.S. version. That would invite reverse-engineers to ask very pointed questions and the secret would almost certainly be out.)

But if you’re Apple, you absolutely cannot entertain, or even engage with this possibility. The minute you engage with it, you’re dead. One single nation — the U.K. — becomes the governor of all of your security products, and will now dictate how they work globally. Worse, engage with this demand would open a hell-mouth of unfortunate possibilities. Do you tell China and Europe and the U.S. that you’ve given the U.K. a backdoor into their data? What if they object? What if they want one too?

There is nothing down that road but catastrophe.

So if you’re Apple and faced with this demand from the U.K., engaging with the demand is not really an option. You have a relatively small number of choices available to you. In order of increasing destructiveness:

1. Hire a bunch of very expensive lawyers and hope you can convince the U.K. to back down.

4. Shut down iCloud end-to-end encryption in the U.K. and hope that this renders the issue moot.

7. ???

10. Exit the U.K. market entirely.

If we can believe the reporting so far, I think it’s safe to say that Apple has almost certainly tried the legal route. I can’t even imagine what the secret court process in the U.K. looks like (does it involve wigs?) but if it’s anything like the U.S.’s FISA courts, I would tend to assume that it is unlikely to be a fair fight for a target company, particularly a foreign one.

In this model, Apple’s decision to disable end-to-end encrypted iCloud Backup means we have now reached Stage 2. U.K. users will no longer be able to sign up for Apple’s end-to-end encrypted backup as of February 21. (We aren’t told how existing users will be handled, but I imagine they’ll be forced to voluntarily downgrade to unencrypted service, or else lose their data.) Any request for a backdoor for U.K. users is now completely moot, because effectively the system no longer exists for U.K. users.

At this point I suppose it remains to see what happens next. Perhaps the U.K. government blinks, and relaxes its demands for access to Apple’s keys. In that case, I suppose this story will sink beneath the waves, and we’ll never hear anything about it ever again, at least until next time.

In another world, the U.K. government keeps pushing. If that happens, I imagine we’ll be hearing quite a bit more about this in the future.

_Top photo due to Rian (Ree) Saunders._

_Notes:_

1. Apple already deploys a separate “zone” for many of its iCloud security products in China. This is due to Chinese laws that mandate domestic hosting of Apple server hardware and keys. We have been assured by Apple (in various reporting) that Apple does not violate its end-to-end encryption for the Chinese government. The various people I’d expect to quit — if that claim was not true — all seem to be still working there.
