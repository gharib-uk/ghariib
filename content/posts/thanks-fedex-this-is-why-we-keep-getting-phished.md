---
title: "Thanks FedEx, This is Why we Keep Getting Phished"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

![Thanks FedEx, This is Why we Keep Getting Phished](https://www.troyhunt.com/content/images/2024/02/Screenshot-2024-02-22-at-12.26.43-unnumbered-1.png)

I've been getting a lot of those "your parcel couldn't be delivered" phishing attacks lately and if you're a human with a phone, you probably have been too. Just as a brief reminder, they look like this:

![Thanks FedEx, This is Why we Keep Getting Phished](https://www.troyhunt.com/content/images/2024/02/IMG_4658-1.jpg)![Thanks FedEx, This is Why we Keep Getting Phished](https://www.troyhunt.com/content/images/2024/02/IMG_4659.jpg)![Thanks FedEx, This is Why we Keep Getting Phished](https://www.troyhunt.com/content/images/2024/02/IMG_4660.jpg)

These get through all the technical controls that exist at my telco and they land smack bang in my SMS inbox. However, I don't fall for the scams because I look for the warning signs: a sense of urgency, fear of missing out, and strange URLs that look nothing like any parcel delivery service I know of. They have a pretty rough go of convincing me they're from Australia Post by putting "auspost" somewhere or other within each link, but I'm a smart human so I don't fall for this (that's a joke, read why humans are bad at URLs).

However... I _am_ expecting a parcel. It's well into the 2020's and post COVID so I'm _always_ expecting a parcel, because that's just how we buy stuff these days. And so, when I received the following SMS earlier this week I was expecting a parcel _and_ I was expecting phishing attacks:

![Thanks FedEx, This is Why we Keep Getting Phished](https://www.troyhunt.com/content/images/2024/02/Screenshot-2024-02-22-at-12.26.43-unnumbered-1.png)

So... which is it? Parcel or phish? Let's see what the people say:

<blockquote class="twitter-tweet" data-conversation="none"><p lang="en" dir="ltr">Referring to the parent tweet, is this message legit and should I pay the duty and taxes?</p>— Troy Hunt (@troyhunt) <a href="https://twitter.com/troyhunt/status/1759827511402537256?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">February 20, 2024</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Whoa - that's an 87% "dodgy AF" vote from over 4,000 respondents so yeah, that's pretty emphatic. Why such an overwhelmingly suspicious crowd? Let's break that message down into 7 "dodgy AF" signs:

![Thanks FedEx, This is Why we Keep Getting Phished](https://www.troyhunt.com/content/images/2024/02/Screenshot-2024-02-22-at-12.26.43-2.jpeg)

1. Phishers commonly make typos in their messaging and I know "FedEx" always capitalises the "E". And what's with the "-Exp"? Dodgy AF!
2. Why does the shipment number look so short? And why is it identical to the requested payment below? Dodgy AF!
3. Ah, so it's _urgent_ is it? Urgency is a core tenet of social engineering as it encourages people to act without properly thinking it though. Dodgy AF!
4. Why are the "D" and the "T" capitalised? Dodgy AF!
5. This is a US-headquartered global delivery parcel service, why aren't they telling me the currency? Or even using a dollar sign? Dodgy AF!
6. Does this even need explaining? What's this "bpoint.com.au" service? It's definitely not a FedEx domain nor an Aussie gov one if we're talking duty and taxes. Dodgy AF!
7. So... _you're_ going to give me the contact details for any "query" (not "queries", so there's another grammatical red flag), the very practice we're now moving away from for one simple reason: because it's dodgy AF!

And so, I was with the 87% of other people. However... I was expecting a package. From FedEx. Coming from outside Australia so it may attract duty and taxes. And I _really_ want to get this package because it's a new 3D printer from Prusa, and they're awesome!

There's a sage piece of advice that's always relevant in these cases and it's very simple: if in doubt, go the website in question and verify the request yourself. So, I went to the purchase confirmation from Prusa, found the shipping details and followed the link to the FedEx website. Now it was simply a matter of finding the section that talks about tax, except...

![Thanks FedEx, This is Why we Keep Getting Phished](https://www.troyhunt.com/content/images/2024/02/IMG_2736.jpg)

Dodgy. A. F.

I went all through that page and couldn't find a single reference to duty, nor for anything tax related. Try as I might, I couldn't establish the authenticity of the SMS by going directly to the (alleged) source. But what I _could_ easily establish is that if you follow that link in the SMS, you can change the tracking number, the customer name and the amount _to absolutely anything you want!_

![Thanks FedEx, This is Why we Keep Getting Phished](https://www.troyhunt.com/content/images/2024/02/image-18.png)

This is all done by simply changing the URL parameters; I'm not modifying the browser DOM or intercepting traffic or doing anything fancy, it's literally just query string parameter tampering reflected XSS style. This feels like every phishing site ever, not a payment service run by Australia's largest bank. Seriously, BPOINT is provided by the Commonwealth Bank and after the experience above, I'm at the point of reaching out to them and making a disclosure. Except that this is how the system was obviously designed to work and it's a completely parallel issue to phishy FedEx SMSs. Speaking of which, the very next morning I got another one from the same sender:

![Thanks FedEx, This is Why we Keep Getting Phished](https://www.troyhunt.com/content/images/2024/02/image-19.png)

I don't know if this makes it better or worse 🤦‍♂️ Let's just jump into the highlights, both good and bad:

1. My shipping number is now actually in the text of the email - yay!
2. The words "duty" and "taxes" are now represented in the correct case - yay!
3. The words "PAY NOW" are capitalised which seems... dodgy AF!
4. And my favourite bit of all: the "link" isn't actually a link at all because it contains no scheme, no domain and no path, just the query string parameters! Dodgy AF!

It's quite unbelievable what they've done with the link because it makes the SMS entirely unactionable. It's impossible to click anywhere and pay the money. And while I'm here, why are all the query string parameter names now capitalised? It's like there's a completely different (broken) process somewhere generating these links. Or scammers just aren't consistent...

Because "dodgy AF" is the prevailing theme, I needed to dig deeper, so I searched for the 1800 number. One of the first results was for a Reverse Australia page for that number which upon reading the first 3 comments, perfectly summed up the sentiment so far:

![Thanks FedEx, This is Why we Keep Getting Phished](https://www.troyhunt.com/content/images/2024/02/image-20.png)

And the more you read both on that site and other top links in the search results, the more people are totally confused about the legitimacy of the messages. There's only one thing to do - call FedEx. Not by the number in the (still potentially phishy) SMS, but rather via the number on their website. So, click the "Support" menu item, down to "Customer Support" and we end up here:

![Thanks FedEx, This is Why we Keep Getting Phished](https://www.troyhunt.com/content/images/2024/02/image-21.png)

I'll save you the pain of reading the response that ensued, suffice to say that it only referred to email communications and boiled down to suggesting you read the domain of the sender. But I did manage to pin the system down on a phone number which as you'll see, is completely different to the one in the SMS messages:

![Thanks FedEx, This is Why we Keep Getting Phished](https://www.troyhunt.com/content/images/2024/02/image-22.png)

So, I call the number and follow the voice prompts, selecting options via the keypad to route me through to the duty and taxes section. But eventually, several steps deep into the process, the system stops responding to key presses! "1" doesn't work and neither does "2" so without a response, the same message just repeats. But it does offer an alternative and suggestions I call 132610. _That's the number I called in the first place to get stuck in this infinite loop!_

I try again, this time following a different series of prompts that eventually asks for a tracking number and then proceeds to tell me precisely what the website already does! But it also provides the option to speak to a customer service operator and I'm actually promptly put through. The operator explains that my shipment is valued at US$799 which converts to AU$1,215.97 and it therefore subject to some inbound fees. "Great, but how much and does it match what's in the phishy SMSs I've received?" He promises someone will call be back shortly...

And then, out of the blue 3 days after the initial phishy SMS arrived, an email landed in my inbox:

![Thanks FedEx, This is Why we Keep Getting Phished](https://www.troyhunt.com/content/images/2024/02/image-23.png)

The dollar figure, the BPOINT address and the messaging all lined up with the SMSs, but that's just merely correlation and if someone had both my phone number and email address they could easily attempt to phish both with the same details. But then, I looked at the attachment to the email and found this:

![Thanks FedEx, This is Why we Keep Getting Phished](https://www.troyhunt.com/content/images/2024/02/IMG_2739.jpg)

**_IT'S THE MISSING LINK!!!_**

My complete Prusa invoice was attached along with the order number, price and shipping details. In other words, 87% of you were wrong 😲

On a more serious note, Aussies alone are losing north of AU$3B annually to scams, and that's obviously only a drop in the ocean compared to the global scale of this problem. Our Australian Communications and Media Authority body (ACMA) recently reported 336M blocked scam SMSs and technical controls like these are obviously great, but absent from their reporting was the number of scam messages they _didn't_ block. There's an easy explanation for this omission: they simply don't know how many are sent. But if I were to take a guess, they've merely blocked the tip of the iceberg. This is why in addition to technical controls, we reply on human controls which means helping people identify the patterns of a scam: requests for money, a sense of urgency, grammar and casing that's a bit off, odd looking URLs. You know, stuff like this:

![Thanks FedEx, This is Why we Keep Getting Phished](https://www.troyhunt.com/content/images/2024/02/Screenshot-2024-02-22-at-12.26.43-unnumbered-1.png)

What makes this situation so ridiculous is that while we're all watching for scammers attempting to imitate legitimate organisations, FedEx is out there imitating scammers! Here we are in the era of burgeoning AI-driven scams that are becoming increasingly hard for humans to identify, and FedEx is like "here, hold my beer" as they one-up the scammers at their own game and do a perfect job of being completely indistinguishable from them.

Ah well, as I ultimately lament in these situations, it's a good time to be in the industry 😊

Go to Source
