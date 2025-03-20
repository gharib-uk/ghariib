---
title: "This Week in Security: ClamAV, The AMD Leak, and The Unencrypted Power Grid"
date: 2025-01-25
---

![](https://hackaday.com/wp-content/uploads/2016/01/darkarts.jpg?w=800)

Cisco’s ClamAV has a heap-based buffer overflow in its OLE2 file scanning. That’s a big deal, because ClamAV is used to scan file attachments on incoming emails. All it takes to trigger the vulnerability is to send a malicious file through an email system that uses ClamAV.

The exact vulnerability is a string termination check that can fail to trigger, leading to a buffer over-read. That’s a lot better than a buffer overflow while writing to memory. That detail is why this vulnerability is strictly a Denial of Service problem. The memory read results in process termination, presumably a segfault for reading protected memory. There are Proof of Concepts (PoCs) available, but so far no reports of the vulnerability being used in the wild.  

## AMD Vulnerability Leaks

AMD has identified a security problem in how some of its processors verify the signature of microcode updates. That’s basically all we know about the issue, because the security embargo still isn’t up. Instead of an official announcement, we know about this issue via an Asus beta BIOS release that included a bit too much information.

## I Read the Docs

There’s nothing quite as fun as winning a Capture The Flag (CTF) challenge the wrong way. The setup for this challenge was a simple banking application, with the challenge being to steal some money from the bank’s website. The intended solution was to exploit the way large floating point numbers round small values. Deposit 1e10 dollars into the bank, and a withdraw of $1000 is literally just a rounding error.

The unintended solution was to deposit `NaN` dollars. In JavaScript-speak that’s the special Not a Number value that’s used for oddball situations like dividing by a float that’s rounded down to zero. `NaN` has some other strange behaviors, like always resulting in false comparisons. `NaN > 0`? False. `NaN < 0`? False. `NaN == NaN`? Yep, also false. And when the fake bank web app checks if a requested withdraw amount is greater than the amount in the account? Since the account is set to `NaN`, it’s also false. Totally defeats the internal bank logic. How did the student find this unintended solution? “I read the docs.” Legendary.

## Another Prompt Injection Tool

\[Utku Sen\] has a story and a revamped tool, and it leads to an interesting question about LLMs. The story starts with a novel LLM prompt, that gives more natural sounding responses from AI tools. LLMs have a unique problem, that there is no inherent difference between pre-loaded system prompts, and user-generated prompts. This leads to an attack, where a creative user prompt can reveal the system prompt. And in a case like \[Utku\]’s, the system prompt is the special sauce that makes the service work. He knew this, and attempted to protect against such attacks. Within an hour of releasing the tool to the public, \[Utku\] got a direct message on X with the system prompts.

There’s a really interesting detail, that the prompt injection attack only worked 1 out of 11 times. This sent me down an LLM rabbit hole, asking whether LLMs are deterministic, and if not, why not. The simple answer is the “temperature” control knob on LLMs add some random noise to the output text. There seems to be randomness even when the LLM temperature is turned to zero, caused either by floating point errors, or even a byproduct of doing batched inference. Regardless, prompt injection attacks may only work after several tries.

And that brings us to promptmap tool. It is intended to evaluate a system prompt, and launch multiple attempts to poison or otherwise inject malicious user prompts into the system. And of course, it is now capable of using the approach that successfully revealed \[Utku\]’s system prompt.

## Cloudflare’s Unintentional GPS

There’s a really interesting unintended side effect of using Cloudflare’s CDN network: Users load data from the nearest datacenter. Unique data can be served to a target user, and then the cache can be checked to leak coarse location information. This is novel research, but ultimately not actually all that important from a security perspective. The primary reason is that the same sort of attack has always existed and can be used to extract a much more valuable piece of user identifying data: The user’s IP address.

## The Unauthenticated, Unencrypted radios that control The German Power Grid

\[Fabian Bräunlein\] and \[Luca Melette\] were just looking for radio-controlled light switches, to pull off a modern take on Project Blinkenlights. What they found was the Radio Ripple Control protocol, an unauthenticated, unencrypted radio control protocol. That just happens to control about 40 Gigawatts of power generation across Germany, not to mention street lamps and other bits of hardware.

The worst-case scenario for an attacker is to turn _on_ all of the devices that use grid power, while turning _off_ all of the connected devices that generate power. Too much of an imbalance might even be capable of resulting in the dreaded grid-down scenario, where all the connected power generation facilities lose sync with each other, and everything has to be disconnected. Recovery from such a state would be slow and tedious. And thankfully not actually very likely. But even if this worst-case scenario isn’t very realistic, it’s still a severe vulnerability in how the German grid is managed. And fixes don’t seem to be coming any time soon.

## Bits and Bytes

The Brave browser had a bit of a dishonest downloads issue, where the warning text about a download would show the URL from the referrer header. The danger is that a download may be considered trustworthy, even when it’s actually being served from an arbitrary URL.

If JavaScript in general or next.js in particular is in your security strike zone, you’ll want to check out the write-up from \[Rachid.A\] about cache poisoning in this particular framework, and the nice cache of security bounties it netted.

Zoom has a weird security disclosure for one of their Linux applications, and it contains a description I’ve never seen before: The bug “may allow an authorized user to conduct an escalation of privilege via network access.” Given the CVSS score of 8.8 with an attack vector of network, this should probably be called a Remote Code Execution vulnerability.

Subaru had a problem with STARLINK. No, not the satellite Internet provider, the other STARLINK. That’s Subaru’s vehicle technology platform that includes remote start and vehicle tracking features. That platform had a pair of flaws, the first allowing an attacker to reset any admin’s password. The second is that the Two Factor Authentication protection can be bypassed simply by hiding the pop-up element in the HTML DOM. Whoops! Subaru had the issues fixed in under 24 hours, which is impressive.

And finally, Silent Signal has the intriguing story of IBM’s i platform, and and a compatibility issue with Windows 11. That compatibility issue was Microsoft cracking down on apps sniffing Windows passwords. And yes, IBM i was grabbing Windows passwords and storing them in the Windows registry. What a trip.

Go to Source
