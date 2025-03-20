---
title: "Book Review: Red Team Blues"
date: 2025-01-15
tags: 
  - "ces"
  - "edge"
  - "hardware"
  - "infosec"
  - "secure-enclave"
  - "technology"
---

As a rule, book reviews are not a thing I usually do.

So when I received an out-of-the-blue email from Cory Doctorow last week asking if I would review his latest book, Red Team Blues, it took a minute to overcome my initial skepticism. While I’m a fan of Cory’s work, this is a narrow/nerdy blog about cryptography, not a place where we spend much time on literature. Moreover, my only previous attempt to review a popular cryptography novel — a quick sketch of Dan Brown’s abysmal _Digital Fortress_ — did not go very well for anyone.

But Cory isn’t Dan Brown. And Red Team Blues is _definitely_ not Digital Fortress.

This became obvious in the middle of the first chapter, when a character began explaining the operation of a trusted execution environment and its various digital signing keys. While it’s always fun to read about gangsters and exploding cars, there’s something particularly nice about a book whose plot hangs around a piece of technology that most people don’t even _think about_. (And if that isn’t your thing, there _are_ exploding cars and gangsters.)

This still leaves the question of how a cryptography blog reviews a work of fiction, even one centered on cryptography. The answer is pretty simple: I’m not going to talk much about the story. If you want that, there are other reviews out there. While I did enjoy the book immensely and I’m hopeful Cory will write more books in this line (with hopefully more cryptography), I’ll mainly focus on the plausibility of the core technical setup.

But even to do that, I have to provide a few basic details about the story. (Note: minor spoilers below, but really only two chapters’ worth.)

The protagonist of Red Team Blues is 67-year-old Martin Hench, an expert forensic accountant with decades of experience tracing and recovering funds for some of the most powerful people in Silicon Valley. Martin is on the brink of retirement, lives in a bus named “the Unsalted Hash” and loves bourbon nearly as much as he despises cryptocurrency. This latter position is presumably a difficult one for someone in Martin’s line of work, and sure enough his conviction is quickly put to the test.

Before long Martin is hired by his old friend Danny Lazer — sort of a cross between Phil Zimmerman, David Chaum and (maybe) Max Levchin — who begs him to take one last career-defining job: namely, to save his friend’s life by saving his newest project: a cryptocurrency called TrustlessCoin.

TrustlessCoin is a private cryptocurrency: not terribly different from real ones like Monero or Zcash. (As a founding scientist of a private cryptocurrency, let me say that none of the things in this novel have ever happened to me, and I’m slightly disappointed in that.)

Unlike standard cryptocurrencies, TrustlessCoin contains one unusual and _slightly horrifying technological twist_. Where standard cryptocurrencies rely on consensus algorithms to construct a public ledger (and zero-knowledge proofs for privacy), TrustlessCoin bases its integrity on the security of mobile Trusted Execution Environments (TEEs). This means that its node software runs inside of systems like Intel’s SGX, ARM’s TrustZone, or Apple’s Secure Enclave Processor.

Now, this idea isn’t entirely unprecedented. Indeed, some real systems like MobileCoin, Secret Network and Intel’s PoET take a fairly similar approach — although admittedly, these rely mainly on server-based TEEs rather than mobile ones. It is, however, an idea _that makes me want to scream like a child who just found a severed human finger in his bowl of cornflakes._

You see, TEEs allow you to run software (more) securely inside of your own device, which is a good and respectable thing to do. But distributed systems often require more: they must ensure that _everyone else_ in the network is also running the software in a similarly-trustworthy environment. If some people aren’t doing so — that is, if they’re running the software on a computer they can tamper with and control — then that can potentially harm the security of the entire network.

TEE designers have been aware of this idea for a long time, and for years have been trying to address this using secure remote attestation. Attestation systems provision each processor with a digital signing key (in turn certified by the manufacturer’s root signing key) that allows the processor to produce _attestations_. These signed messages “prove” to remote parties that you’re actually running the software inside a valid TEE, rather than on some insecure VMWare image or a Raspberry Pi. Provided these systems all work perfectly, everyone in the system can communicate with everyone else and _know_ that they are running the software on secure hardware as well.

The problems crop up _when that assumption breaks down_. If even a single person can emulate the software inside a TEE on their own (non-trusted device or VM) then all of your beautiful assumptions may go out the window. Indeed, something very similar to this recently happened to Secret Network: clever academic researchers found a way to extract a master decryption key from (one) processor, and were then able to use that key to destroy privacy guarantees across the whole network. (Some mitigations have since been deployed.)

It goes without saying that Red Team Blues is not about side-channel attacks on processors. The problem in this novel is vastly worse: Danny Lazer has gone and bribed someone to steal the secret root signing keys for every major mobile secure enclave processor: and, of course, they’ve been all been stolen. Hench’s problem is to figure out whether it’s even possible to get them back. And that’s only the beginning of the story.

As its name implies, Red Team Blues is a novel about the difference between offense and defense: about how much more difficult it is to secure a system than it is to attack one. This metaphor applies to just about every aspect of life, from our assumptions about computer security to the way we live our lives and build our societies.

But setting all these heavy thoughts aside, mostly Red Team Blues is a quick fun read. You can get the eBook without DRM, or listen to an audiobook version narrated by Wil Wheaton (although I didn’t listen to it because I couldn’t put the book down.)

Go to Source
