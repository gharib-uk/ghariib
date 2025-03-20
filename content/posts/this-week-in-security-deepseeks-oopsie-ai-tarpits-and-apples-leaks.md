---
title: "This Week in Security: DeepSeek’s Oopsie, AI Tarpits, And Apple’s Leaks"
date: 2025-02-01
---

![](https://hackaday.com/wp-content/uploads/2016/01/darkarts.jpg?w=800)

DeepSeek has captured the world’s attention this week, with an unexpected release of the more-open AI model from China, for a reported mere $5 million training cost. While there’s lots of buzz about DeepSeek, here we’re interested in security. And DeepSeek has made waves there, in the form of a ClickHouse database unintentionally opened to the world, discovered by the folks from Wiz research. That database contained chat history and log streams, and API keys and other secrets by extension.

Finding this database wasn’t exactly rocket science — it reminds me of my biggest bug bounty win, which was little more than running a traceroute and a port scan. In this case it was domain and sub domain mapping, and a port scan. The trick here was knowing to try this, and then understanding what the open ports represented. And the ClickHouse database was completely accessible, leaking all sorts of sensitive data.

## AI Tarpit

Does it really grind your gears that big AI companies are training their models on your content? Is an AI crawler ignoring your `robots.txt`? You might need help from Nepenthes. Now before you get too excited, let’s be clear, that this is a malicious software project. It will take lots of CPU cycles, and it’s explicitly intended to waste the time of AI crawlers, while also feeding gibberish into their training models.

The project takes the form of a website that loads slowly, generates gibberish text from a Markov chain, and then generates a handful of unique links to other “pages” on the site. It forms the web equivalent of an infinite “maze of twisty little passages, all alike”.

While the project has been a success, confirmed by the amount of time various web crawlers have spent lost inside, AI companies are aware of this style of attack, and mitigations are coming.

Check out the demo, but don’t lose too much time in there.  
https://arstechnica.com/tech-policy/2025/01/ai-haters-build-tarpits-to-trap-and-trick-ai-scrapers-that-ignore-robots-txt/

## Is The QR Code Blue and Black?

<figure>

![](https://hackaday.com/wp-content/uploads/2025/01/f83ec372200ea15a.png?w=400)

<figcaption>

Or is it White and Gold

</figcaption>

</figure>

This is a really interesting bit of research happening on a Mastodon thread. The initial hack was a trio of QR codes, pointing to three different news sites, interleaved beneath a lenticular lens. Depending on the angle from which it was viewed, this arrangement led to a different site. That provoked \[Christian Walther\] to question whether the lens was necessary, or if some old-school dithering could pull off the same trick. Turns out that it sure can. One image, two URL. We’d love to see this extended to QR codes that register differently under different lighting, or other fun tricks. Head over to Elliot’s coverage for more on this one.

## SLAPing and FLOPing Apple

Apple’s A and M chips have a pair of recently discovered speculative execution flaws, FLOP and SLAP. That’s False Load Out Predictions and Speculation in Load Address Predictions . FLOP uses mispredicted memory contents to access data, and SLAP uses mispredicted memory addresses. The takeaway is that Javascript running on one page can leak bytes from another web page.

Both of these attacks have their own wrinkles and complexities. SLAP has only been demonstrated in Safari, and is triggered by training the address prediction on an address layout pattern that leads into memory outside the real buffer. By manipulating Safari into loading another page in the same process as the attacker page, this can be used to leak buffer data from that other page.

FLOP is much more powerful, and works in both Safari and Chrome, and is triggered by training the CPU that a given load instruction tends to return the same data each time. This can be used in Safari to pull off a type confusion speculation issue, leading to arbitrary data leakage from any memory address on the system. In Chrome the details are a bit different, but the result is still an arbitrary memory read primitive.

The worst case scenario is that a compromised site in one tab can pull data from the rest of the system. There’s an impressive demo where a compromised tab reads data from ProtonMail running in a different tab. Apple’s security team is aware of this work, and has stated that it does not consider these attacks to be immediately exploitable as real world attacks.

## Bits and Bytes

WatchTowr is back with the details on another Fortigate vulnerability, and this time it’s a race condition in the jsconsole management interface, resulting in an authentication bypass, and jumping straicht to super\_admin on the system.

Unicode continues causing security problems, to no great surprise. Windows has a “Best-Fit” character conversion facility, which attempts to convert Unicode characters to their nearest ASCII neighbors. That causes all sorts of problems, in the normal divergent-parser-behavior way. When a security check happens on the Unicode text, but the Best-Fit conversion happens before the text is actually used, the check is neatly bypassed by the text being Best-Fit into ASCII.

And finally, Google’s Project Zero has an in-depth treatment of COM object exploitation with IDispatch. COM objects can sometimes be accessed across security boundaries, and sometimes those remote objects can be used to execute code. This coverage dives into the details of how the IDispatch interface can be used to trigger this behavior. Nifty!

Go to Source
