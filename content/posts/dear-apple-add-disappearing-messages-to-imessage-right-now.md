---
title: "Dear Apple: add “Disappearing Messages” to iMessage right now"
date: 2025-03-19
categories: 
  - "apple"
  - "cybersecurity"
  - "iphone"
  - "messaging"
  - "security"
---

This is a cryptography blog and I always feel the need to apologize for any post that isn’t “straight cryptography.” I’m actually getting a little tired of apologizing for it (though if you want some hard-core cryptography content, there’s plenty here and here.)

Sometimes I have to remind my colleagues that out in the real world, our job is not to solve exciting mathematical problems: _it’s to help people communicate securely_. And people, at this very moment, need a lot of help. Many of my friends are Federal employees or work for contractors, and they’re terrified that they’re going to lose their job based on speech they post online. Unfortunately, “online” to many people includes thoughts sent over private text messages — so even private conversations are becoming chilled. And while this is hardly the worst thing happening to people in this world, it’s something that’s happening to my friends and neighbors.

(And just in case you think this is partisan: many of the folks I’m talking about are Republicans, and some are military veterans who work for the agencies that keep Americans safe. They’re afraid for their jobs and their kids, and that stuff transcends politics.)

So let me get to the point of this relatively short post.

### Apple iMessage is encrypted, but is it “secure”?

A huge majority of my “normie” friends are iPhone users, and while some have started downloading Signal app (and you should too!) — many of them favor one communications protocol: Apple iMessage. This is mostly because iMessage is just the built-in messaging app used on Apple phones. If you don’t know the branding, all you need to know is that iMessage is “blue bubbles” you get when talking to other Apple users.

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/03/image.png?w=532)

Apple boasts that the iMessage app encrypts your messages end-to-end, and that it has done so since 2011. This means that all your messages and attachments are encrypted under keys that Apple does not know_._ The company has been extremely consistent about this, discusses it in their platform security guide, and in recent years they’ve even extended their protocol to provide post-quantum security. (A few years ago my students and I found a bug in the protocol, but Apple fixed it quickly — so I am very personally confident in their encryption.)

This is nice. And it’s all true.

But encryption _in transit_ is only one part of the story. After delivering your messages with its uncompromising post-quantum security, Apple allows two things that aren’t so great:

1. iMessages stick around your phone forever unless you manually delete them (a process that may need to happen on both sides, and is painfully annoying.)

4. iMessages are automatically backed up to Apple’s iCloud backup service, if you have that feature on — and since Apple sets this up as the default during iPhone setup, most ordinary people do.

The combination of these two features turn iMessage into a Star-Trek style Captain’s Log of your entire life. Searching around right now, I can find messages from 2015. Even though my brain tells me this was three years ago, I’m reliably informed that this is a whole decade in the past.

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/03/image-1.png?w=1024)

Now, while the messages above are harmless, I want to convince you that this is often a very bad thing. People want to have private conversations. They _deserve to have private conversations_. And their technology should make them feel safe doing so. That means they should know that their messaging software has their back and will make sure those embarrassing or political or risque text messages are not stored forever on someone’s phone or inside a backup.

### We know exactly how to fix this, and every other messenger did so long ago

If you install WhatsApp, Facebook Messenger, Signal, Snap or even Telegram (please don’t!) you’ll encounter a simple feature that addresses this problem. It’s usually called “disappearing messages“, but sometimes goes by other names.

I’m almost embarrassed to explain what this feature does, since it’s like explaining how a steering wheel works. _Nevertheless_. When you start a chat, you can decide how long the messages should stick around for. If your answer is forever, you don’t need to do anything. However, if it’s a sensitive conversation and you want it to be ephemeral in the same way that a phone call is, you can pick a time, typically ranging from 5 minutes to 90 days. When that time expires, your messages just get erased — on both your phone and the phones of the people you’re talking to.

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/03/img_0219.jpg?w=700)

A separate feature of disappearing messages is that some platforms will omit these conversations from device backups, or at least they’ll make sure expired messages can’t be restored. This makes sense because _those conversations are supposed to be ephemeral_: people are clearly not expecting those text messages to be around in the future, so they’re not as angry if they lose them a few days early.

Beyond the basic technical functionality, a disappearing messages feature says something. It announces to your users that a conversation is actually intended to be private and short-lived, that its blast radius will be contained. You won’t have to think about it years or months down the line when it’s embarrassing. This is valuable not just for what it does _technically_ but for the confidence it gives to users, who are already plenty insecure after years of abuse by tech companies.

### Why won’t Apple add a disappearing messages feature?

I don’t know. I honestly cannot tell you. It is baffling and weird and wrong, and completely out of step with the industry they’re embedded in. It is particularly bizarre for a company that has formed its entire brand image around stuff like this:

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/03/image-3.png?w=940)

To recap, nearly every single other messaging product that people use in large numbers (at least here in the US) has some kind of disappearing messages feature. Apple’s omission is starting to be very unique.

I do have some friends who work for Apple Security and I’ve tried to talk to them about this. They usually try to avoid me when I do stuff like this — sometimes they mention lawyers — but when I’m annoying enough (and I catch them in situations where exit is impossible) I can occasionally get them to answer my questions. For example, when I ask “_why don’t you turn on end-to-end encrypted iCloud backup by default_,” they give me thoughtful answers. They’ll tell me how users are afraid of losing data, and they’ll tell me sad stories of how difficult it is to make those features as usable as unencrypted backup. (I _half_ believe them.)

When I ask about disappearing messages, I get embarrassed sighs and crickets. Nobody can explain why Apple is so far behind on this basic feature even as an option, long after it became standard in every other messenger. Hence the best I can do is speculate. Maybe the Apple executives are afraid that governments will pressure them if they activate a security feature like this? Maybe the Messages app is written in some obsolete language and they can’t update it? Maybe the iMessage servers have become sentient and now control Tim Cook like a puppet? _These are not great answers, but they are better than anything the company has offered_ — and everyone at Apple Security kind of knows it.

In a monument to misaligned priorities, Apple even spent time adding post-quantum encryption to its iMessage protocol — this means Apple users are now safe from quantum computers that don’t exist. And yet users’ most intensely private secrets can still be read off their phone or from a backup by anyone who can guess their passcode and use a search box. This is not a good place to be in 2025, and Apple needs to do better.

### A couple of technical notes

Since this is a technical blog I feel compelled to say a few things that are just a tad more detailed than the plea above.

First off, Gene Hoffman points me to a small feature in the Settings of your phone called “Keep Messages” (buried under “Messages” in Settings, and then scrolling way down.) This determines how long your messages will be kept around on your own phone. You cannot set this per conversation, but you can set it to something shorter than “Forever”, say, one year. This is a big decision for some people to make, however, since it will immediately delete any old messages you actually cared about.

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/03/img_0220.jpg?w=1014)

More importantly (as mentioned in comments) this only affects _your_ phone. Messages erased via this process will remain on the phones of your conversation partners.

Second, if you really want to secure your iMessages, you should turn on Apple’s Advanced Data Protection feature. This will activate end-to-end encryption for your iCloud backups, and will ensure that nobody but you can access those messages.

_This is not the same thing as disappearing messages, because all it protects is backups. Your messages will still exist on your phone and in your encrypted backups. But it at least protects your backups better._

Third, Apple advertises a feature called Messages in iCloud, which is designed to back up and sync your messages between devices. Apple even advertises that this feature is end-to-end encrypted!

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/03/image-4.png?w=1024)

I hate this phrasing because it is _disastrously_ misleading. Messages in iCloud may be encrypted, however… If you use iCloud Backup without ADP (which is the default for new iPhones) the Messages in iCloud encryption key itself will be backed up to Apple’s servers in a form that Apple itself can access. And so the content of the Messages in iCloud database will be completely available to Apple, or anyone who can guess your Apple account password.

![](https://blog.cryptographyengineering.com/wp-content/uploads/2025/03/image-5.png?w=1024)

None of this has anything to do with disappearing messages. However: that feature, with proper anti-backup protections, would go a long way to make these backup concerns obsolete.
