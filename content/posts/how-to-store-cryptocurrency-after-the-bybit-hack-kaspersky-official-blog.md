---
title: "How to store cryptocurrency after the Bybit hack | Kaspersky official blog"
date: 2025-03-19
tags: 
  - "cybersecurity"
  - "data_breaches"
  - "infostealers"
  - "linux"
  - "malware"
  - "privacy"
  - "security"
  - "siem"
---

Why crypto exchanges struggle to guard against targeted attacks, and how to store cryptocurrency threat-free.

February 21 was a dark day for the crypto market as it suffered the largest heist in its history. Attackers made off with around $1.5 billion from Bybit, the world’s second-largest crypto exchange, with experts citing it as the biggest theft – of anything – of all time. Although neither this loss nor the withdrawal of a further $5 billion by panicked investors were fatal for Bybit, the incident underscores the fundamental flaws in the modern crypto ecosystem, and serves up some valuable lessons for regular users.

## How Bybit was robbed

Like all major crypto exchanges, Bybit secures stored cryptocurrency with multi-layered protection. Most funds are stored in cold wallets disconnected from online systems. When current assets need topping up, the required sum is manually moved from the cold wallet to the hot one, and the operation is signed by several employees at once. For this, Bybit uses a multi-signature (multisig) solution from Safe{Wallet}, and each employee involved in the transaction signs it using a private Ledger hardware cryptokey.

The attackers studied the system in detail and, according to independent researchers, compromised a Safe{Wallet} developer machine. Presumably, malicious modifications were made to the code for displaying Safe{Wallet} web application pages. Having conducted their own investigation, the owners of Safe{Wallet} rejected the findings of the two independent information security companies, insisting that their infrastructure had not been hacked.

So what happened? During a routine top-up of $7 million to a hot wallet, Bybit employees saw on their computer screens this exact amount and the recipient’s address, which matched the hot wallet address. But other data got sent for signing instead! For regular transfers, the recipient’s address can (and should!) be checked on the screen of the Ledger device. But when signing multisig transactions, this information isn’t displayed — so Bybit employees essentially made a blind transfer.

As a result, they inadvertently green-lighted a malicious smart contract that moved the entire contents of one of Bybit’s cold wallets to several hundred fake wallets. As soon as the withdrawal from the Bybit wallet was complete, it appears that the code on the Safe{Wallet} website reverted to the harmless version. The attackers are currently busy “layering” the stolen Ethereum — transferring it piecemeal in an attempt to launder it.

By the looks of it, Bybit and its clients were the victims of a targeted supply-chain attack.

## The Bybit case is no one-off

The FBI has officially named a North Korean group codenamed TraderTraitor as the perpetrator. In information-security circles, this group is also known as Lazarus, APT38, or BlueNoroff. Its trademark style is persistent, sophisticated and sustained attacks in the cryptocurrency sphere: hacking wallet developers, robbing crypto exchanges, stealing from ordinary users, and even making fake play-to-earn games.

Before the Bybit raid, the group’s record was the theft of $540 million from the Ronin Networks blockchain, created for the game _Axie Infinity_. In that 2022 attack, hackers infected the computer of one of the game’s developers using a fake job offer in an infected PDF file. This social engineering technique remains in the group’s arsenal to this day.

In May 2024, the group pulled off a smash-and-grab of over $300 million from Japanese crypto-exchange DMM Bitcoin, which went bankrupt as a consequence. Before that, in 2020, more than $275 million was siphoned off the KuCoin crypto exchange, with a “leaked private key” for a hot wallet cited as the reason.

Lazarus has been honing its cryptocurrency theft tactics for over a decade now. In 2018, we wrote about a string of attacks on banks and crypto exchanges using a Trojanized cryptocurrency trading app as part of Operation AppleJeus. Experts at Elliptic estimate that North-Korea-linked actors’ total criminal earnings amount to around $6 billion.

## What crypto investors should do

In the case of Bybit, clients were lucky: the exchange promptly serviced the wave of withdrawal requests that ensued, and promised to compensate losses from its own funds. Bybit remains in business, so clients don’t need to take any particular action.

But the hack demonstrates once again just how hard it is to secure funds flowing through blockchain systems, and how little can be done to cancel a transaction or refund money. Given the unprecedented scale of the attack, many have called for the Ethereum blockchain to be rolled back to its pre-hack state, but Ethereum developers consider this “technically intractable”. Meanwhile, Bybit has announced a bounty program for crypto exchanges and ethical researchers to the tune of 10% of any funds recovered, but so far only $43 million has materialized.

This has caused some crypto industry experts to speculate that the main fallout from the hack will be a rise in self-custody of crypto assets.

**Self-custody** shifts the responsibility for secure storage from the shoulders of specialists to your own. Therefore, only go down this route if you have total confidence in your abilities to master all security measures and follow them rigidly day by day. Note that regular users without cryptowallet millions are unlikely to face a sophisticated attack targeted specifically at them, while generic mass attacks are easier to deflect.

So, what do you need for secure self-custody of cryptocurrency?

- **Buy a hardware wallet with a screen.** This is the most effective way to protect crypto assets. Do a little research first, and be sure to buy a wallet from a reputable vendor — and directly: never second-hand or from a marketplace. Otherwise, you might get a pre-hacked wallet that swallows up all your funds. When using a wallet to sign transfers, always check the recipient’s address on both the computer screen and the wallet screen to rule out its substitution by a malicious smart contract or a clipper Trojan that replaces cryptowallet addresses in the clipboard.
- **Never store wallet seed phrases in electronic form.** Forget about using files on your computer and photos in your gallery for that — modern Trojans have learned to infiltrate Google Play and the App Store and recognize data in photos stored on your smartphone. Only paper records (or metal engravings, if you prefer) kept inside a safe or in another physically secure place, protected from both unauthorized access and natural disasters, will do. You might consider multiple storage locations, as well as splitting your seed phrase into parts.
- **Don’t keep all your eggs coins in one basket.** For holders of large amounts or different types of crypto assets, it makes sense to use multiple wallets. Small amounts for transactional needs can be stored on a crypto exchange, while the bulk can be divided among several hardware cryptowallets.
- **Use a dedicated computer.** If possible, dedicate a computer for cryptocurrency transactions. Physically restrict access to it (e.g., put it in a safe, a locked cupboard or locked room), use disk encryption and password login, and have a separate account with its own passwords (i.e., different to those on your main computer). Install reliable protection and enable maximum security settings on your “crypto-computer”. Connect it to the internet only for transactions, and use it solely for operations with wallets. Playing games, reading crypto news, and chatting with friends are for another device.
- If dedicating a computer is impractical or uneconomical, **maintain strict digital hygiene on your main computer**. Set up a separate account with low privileges (non-administrator) for crypto operations, and another account — also non-administrator — for work, chat and games. There’s no need to work in administrator mode at all, except to update the system software or significantly reconfigure the computer. Sign in to your dedicated “crypto account” only for operations with wallets, and sign out immediately afterward. Don’t give outsiders access to the computer, and don’t share admin passwords with anyone.
- **Take care when choosing cryptowallet software.** Carefully study the software’s description, make sure that the application has been on the market for a long time, and check that you’re downloading it from the official website, and that the digital signature of the distribution corresponds to the website and the name of the vendor. Perform a deep scan of your computer with an up-to-date security solution before installing and running cryptowallet software.
- **Be careful with updates.** While we usually recommend updating all software right away, in the case of cryptocurrency applications, it’s worth adjusting this policy a little. After the release of a new version, wait about a week and read the reviews before installing it. This will give the community time to catch any bugs or Trojans that may have sneaked into the update.
- **Follow the enhanced computer security measures** described in our post **Protecting crypto investments: four key steps to safety**, which include installing a powerful security solution, such as Kaspersky Premium, on your computer and smartphone, regularly updating your operating system and browsers, and using strong, unique passwords.
- **Expect phishing.** Cryptocurrency fraud can be both multifaceted and sophisticated, so any unexpected messages by email, messenger app and the like should be seen as the start of a scam. Keep on top of all the latest crypto scams by following our blog or Telegram channel, as well as other reputable cybersecurity sources.

> Read more about crypto scams and ways to protect yourself in our dedicated posts:
> 
> - Eight of the most daring crypto thefts in history
> - The top-5 biggest cryptocurrency heists ever
> - Case study: fake hardware cryptowallet
> - Pig butchering: large-scale cryptocurrency fraud
> - and other articles about cryptocurrency.

Go to Source
