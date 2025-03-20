---
title: "<div>You Can't Trust Hackers, and Other Data Breach Verification Tales</div>"
date: 2025-01-23
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
---

![You Can't Trust Hackers, and Other Data Breach Verification Tales](https://www.troyhunt.com/content/images/2025/01/2025-01-22_17-20-03.png)

It's hard to find a good criminal these days. I mean a really trustworthy one you can be confident won't lead you up the garden path with false promises of data breaches. Like this guy yesterday:

![You Can't Trust Hackers, and Other Data Breach Verification Tales](https://www.troyhunt.com/content/images/2025/01/image-17.png)

For my international friends, JB Hi-Fi is a massive electronics retailer down under _and they have my data!_ I mean by design because I've bought a bunch of stuff from them, so I was curious not just about my own data but because a breach of 12 million plus people would be massive in a country of not much more than double that. So, I dropped the guy a message and asked if he'd be willing to help me verify the incident by sharing my own record. I didn't want to post any public commentary about this incident until I had a reasonable degree of confidence it was legit, not given how much impact it could have in my very own backyard.

Now, I wouldn't normally share a private conversation with another party, but when someone sets out to scam people, that rule goes out the window as far as I'm concerned. So here's where the conversation got interesting:

![You Can't Trust Hackers, and Other Data Breach Verification Tales](https://www.troyhunt.com/content/images/2025/01/image-21.png)

_He guaranteed it for me!_ Sounds legit. But hey, everyone gets the benefit of the doubt until proven otherwise, so I started looking at the data. It turns out my own info wasn't in the full set, but he was happy to provide a few thousand sample records with 14 columns:

1. customer\_id\_
2. first\_name
3. last\_name
4. FullName
5. gender
6. email\_address\_
7. mobile\_country\_
8. mobile\_number\_
9. dob
10. postal\_street\_1\_
11. state\_
12. postal\_code\_
13. city\_
14. account\_status

Pretty standard stuff, could be legit, let's check. I have a little Powershell script I run against the HIBP API when a new alleged breach comes in and I want to get a really good sense of how unique it is. It simply loops through all the email addresses in a file, checks which breaches they've been in and keeps track of the percentage that have been seen before. A unique breach will have anywhere from about 40% to 80% previously seen addresses, but this one had, well, more:

![You Can't Trust Hackers, and Other Data Breach Verification Tales](https://www.troyhunt.com/content/images/2025/01/image-23.png)

Spot the trend? Every single address has one breach in common. Hmmm... wonder what the guy has to say about that?

![You Can't Trust Hackers, and Other Data Breach Verification Tales](https://www.troyhunt.com/content/images/2025/01/image-18.png)

_But he was in the server! And he grabbed it from the dashboard of Shopify!_ Must be legit, unless... what if I compared it to the actual full breach of Dymocks? That's a local Aussie bookseller (so it would have a lot of Aussie-looking email addresses in it, just like JB Hi-Fi would), and their breach dated back to mid-2023. I keep breaches like that on hand for just such occasions, let's compare the two:

![You Can't Trust Hackers, and Other Data Breach Verification Tales](https://www.troyhunt.com/content/images/2025/01/image-20.png)

Wow! What are the chances?! He's going to be _so_ interested when he hears about this!

![You Can't Trust Hackers, and Other Data Breach Verification Tales](https://www.troyhunt.com/content/images/2025/01/image-19.png)

And that was it. The chat went silent and very shortly after, the listing was gone:

![You Can't Trust Hackers, and Other Data Breach Verification Tales](https://www.troyhunt.com/content/images/2025/01/image-22.png)

It looks like the bloke has also since been booted off the forum where he tried to run the scam so yeah, this one didn't work out great for him. That $16k would have been so tasty too!

I wrote this short post to highlight how important verification of data breach claims is. Obviously, I've seen _loads_ of legitimate ones but I've also seen a lot of rubbish. Not usually this blatant where the party contacting me is making such demonstrably false claims about their own exploits, but very regularly from people who obtain something from another party and repeat the lie they've been told. This example also highlights how useful data from previous breaches is, even after the email addresses have been extracted and loaded into HIBP. Data is so often recycled and shipped around as something new, this was just a textbook perfect case of making use of a previous incident to disprove a new claim. Plus, it's kinda fun poking holes in a scamming criminal's claims 😊

Go to Source
