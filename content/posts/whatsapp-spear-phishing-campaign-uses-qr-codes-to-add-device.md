---
title: "WhatsApp spear phishing campaign uses QR codes to add device"
date: 2025-01-19
---

A cybercriminal campaign linked to Russia is deploying QR codes to access the WhatsApp accounts of high-profile targets like journalists, members of think tanks, and employees of non-governmental organizations (NGOs), according to new details revealed by Microsoft.

The group, which Microsoft tracks by the name “Star Blizzard,” is also referred to as Coldriver by other researchers. Last year, the group created impersonation accounts where members posed as experts in a field that their targets might be interested in—or that was somehow affiliated with the target. Once a relationship had been established, the target would receive a phishing link or a document that contained a phishing link.

But over time, that tactic became widely known, and part of the cybercriminals’ infrastructure was taken down. Now, it seems the group has changed tactics and is sending QR codes instead of malicious links to the targets that they have established an initial relationship with.

These QR codes do not take the target to a malicious website, nor will they join them to the promised WhatsApp group on “the latest non-governmental initiatives aimed at supporting Ukraine NGOs,” as is claimed in one of the cybercriminal lures.

In reality, the link in the QR code is intentionally broken. The idea is that the target will respond with a remark about the broken link. When that happens the cybercriminals send out a shortened URL to a website that displays another QR code.

<figure>

![obfuscated and shortened link](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/wrapped_and_shortened_link.jpg)

<figcaption>

_Screenshot courtesy of Microsoft_

</figcaption>

</figure>

> “I apologize for the inconvenience with the QR code. Kindly try this alternative link: US-Ukraine NGOs Group  
> It should work without any issues.

By scanning this QR code and following the instructions on the website they confirm the addition of an extra device to the WhatsApp account of the target. With that access the group can read the messages in their WhatsApp account and use existing browser plugins, particularly those designed for exporting WhatsApp messages from an account accessed via WhatsApp Web.

## How to stay safe

These spear phishing campaigns are highly targeted and you’ll probably never see an invite to this group. But cybercriminals tend to copy ideas that work, so you may see them in another form.

There are a few simple rules that will help you avoid this kind of phishing.

- Always hover over links before clicking them.

- When you find a shortened URL, think about the possible reason for shortening. Was there a real need to do this or is it just meant to hide the destination?

- When still in doubt, unshorten the URL.

- When following instructions on a website, scrutinize whether the prompts on your device actually match the expected ones. WhatsApp will double-check whether you want to add a device to the account.

- Double-check whether the sender is who they claim to be through another method of contact.

* * *

**We don’t just report on phone security—we provide it**

Cybersecurity risks should never spread beyond a headline. Keep threats off your mobile devices by downloading Malwarebytes for iOS, and Malwarebytes for Android today.

Go to Source
