---
title: "How Spoutible’s Leaky API Spurted out a Deluge of Personal Data"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

![How Spoutible’s Leaky API Spurted out a Deluge of Personal Data](https://www.troyhunt.com/content/images/2024/02/ezgif-4-50602b781b.jpg)

Ever hear one of those stories where as it unravels, you lean in ever closer and mutter “No way! _No way! NO WAY!_” This one, as far as infosec stories go, had me leaning and muttering like never before. Here goes:

Last week, someone reached out to me with what they claimed was a Spoutible data breach obtained by exploiting an enumerable API. Just your classic case of putting someone else's username in the URL and getting back data about them, which at first glance I assumed was another scraping situation like we recently saw with Trello. They sent me a file with 207k scraped records and a URL that looked like this:

```
https://spoutible.com/sptbl_system_api/main/user_profile_box?username=troyhunt
```

But they didn't send me my account, in fact I didn't even have an account at the time and if I'm honest, I had to go and look up exactly what Spoutible was. The penny dropped as I read into it: Spoutible emerged in the wake of Elon taking over Twitter, which left a bunch of folks unhappy with their new social overlord so they sought out alternate platforms. Mastodon and Bluesky were popular options, Spoutible was another which was clearly intended to be an alternative to the incumbent.

In order to unravel this saga in increasing increments of "no way!" reactions, let's just start with the basics of what that API endpoint was returning:

```
{
  err_code: 0,
  status: 200,
  user: {
    id: 735525,
    username: "troyhunt",
    fname: "Troy",
    lname: "Hunt",
    about: "Creator of Have I Been Pwned. Microsoft Regional Director. Pluralsight author. Online security, technology and “The Cloud”. Australian.",
```

Pretty standard stuff and I'd expect any of the major social platforms to do exactly the same thing. Name, username, bio and ID are all the sorts of data attributes you'd expect to find publicly available via an API or rendered into the HTML of the website. These fields, however, are quite different:

```
email: "[redacted]",
ip_address: "[redacted]",
verified_phone: "[redacted]",
gender: "M",
```

Ok, that's now a "no way!" because I had no expectation at all of any of that data being publicly available (note: phone number is optional, I chose to add mine). It's certainly not indicated on the pages where I entered it:

![How Spoutible’s Leaky API Spurted out a Deluge of Personal Data](https://www.troyhunt.com/content/images/2024/02/image.png)![How Spoutible’s Leaky API Spurted out a Deluge of Personal Data](https://www.troyhunt.com/content/images/2024/02/image-1.png)![How Spoutible’s Leaky API Spurted out a Deluge of Personal Data](https://www.troyhunt.com/content/images/2024/02/image-10.png)

But it's also not that different to previous scraping incidents; the aforementioned Trello scrape exposed the association of email addresses to usernames and the Facebook scrape of a few years ago did the same thing with phone numbers. That's not unprecedented, but this is:

```
password: "$2y$10$B0EhY/bQsa5zUYXQ6J.NkunGvUfYeVOH8JM1nZwHyLPBagbVzpEM2",
```

_No way!_ Is it... real? Is that genuinely a bcrypt hash of my own password? Yep, that's exactly what it is:

![How Spoutible’s Leaky API Spurted out a Deluge of Personal Data](https://www.troyhunt.com/content/images/2024/02/image-2.png)

**The Spoutible API enabled any user to retrieve the bcrypt hash of any other user's password.**

I had to check, double check then triple check to make sure this was the case because I can only think of one other time I've ever seen an API do this...

<TangentialStory>

During my 14 years at Pfizer, I once reviewed an iOS app built for us by a low-cost off-shored development shop. I proxied the app through Fiddler, watched the requests and found an API that was returning every user record in the system and for each user, their corresponding password in plain text. When quizzing the developers about this design decision, their response was - and I kid you not, this isn't made up - "don't worry, our users don't use Fiddler" 🤦‍♂️

</TangentialStory>

I cannot think of any reason ever to return any user's hashed password to any interface, including an appropriately auth'd one where only the user themselves would receive it. There is _never_ a good reason to do this. And even though bcrypt is the accepted algorithm of choice for storing passwords these days, it's far from uncrackable as I showed 7 years ago now after the Cloudpets breach. Here I used a small dictionary of weak, predictable passwords and easily cracked a bunch of the hashes. Weak passwords like... "spoutible". Wondering just how crazy things would get, I checked the change password page and found I could easily create a password of 6 or more characters (so long as it didn't exceed 20 characters) with no checks on strength whatsoever:

![How Spoutible’s Leaky API Spurted out a Deluge of Personal Data](https://www.troyhunt.com/content/images/2024/02/image-3.png)

Strong hashing algorithms like bcrypt are weakened when poor password choices are allowed and strong password choices (such as having more than 20 characters in it), are blocked. For exactly the same reason breached services advise customers to change their passwords even when hashed with a strong algorithm, all Spoutible users are now in the same boat - change you password!

But fortunately these days many people make use of 2 factor authentication to protect against account takeover attacks where the adversary knows the password. Which brings us to the next piece of data the API returned:

```
2fa_secret: "7GIVXLSNKM47AM4R",
2fa_enabled_at: "2024-02-03 02:26:11",
2fa_backup_code: "$2y$10$6vQRDRDHVjyZdndGUEKLM.gmIIZVDq.E5NWTWti18.nZNQcqsEYki",
```

_Oh wow!_ Why?! Let's break this down and explore both the first and last line. The 2FA secret is the seed that's used to generate the one time password to be used as the second factor. If you - as an attacker - know this value then 2FA is rendered useless. To test that this was what it looked like, I asked Stefán to retrieve my data from the public API, take the 2FA secret and send me the OTP:

![How Spoutible’s Leaky API Spurted out a Deluge of Personal Data](https://www.troyhunt.com/content/images/2024/02/image-4.png)

It was a match. _If_ Stefán could have cracked my bcrypted password hash (and he's a smart guy so "spoutible" would have definitely been in his word list), he could have then passed the second factor challenge. And the 2FA backup code? Thinking that would also be exactly what it looked like, I'd screen grabbed it when enabling 2FA:

![How Spoutible’s Leaky API Spurted out a Deluge of Personal Data](https://www.troyhunt.com/content/images/2024/02/image-5.png)

Now, using the same bcrypt hash checker as I did for the password, here's what I found:

![How Spoutible’s Leaky API Spurted out a Deluge of Personal Data](https://www.troyhunt.com/content/images/2024/02/image-6.png)

What I just don't get is if you're going to return the 2FA secret anyway, why bother bcrypting the backup code? And further, it's only a 6 digit number, do you know how long it takes to crack a bcrypted 6 digit number? Let's find out:

<blockquote class="twitter-tweet"><p lang="und" dir="ltr">570075, 2m59s</p>— Martin Sundhaug (@sundhaug92@mastodon.social) (@sundhaug92) <a href="https://twitter.com/sundhaug92/status/1753977138133242028?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">February 4, 2024</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Many other people worked it out in single-digit minutes as well, but Martin did it fastest at the time of writing so he gets the shout-out 😊

You know how I said you'd keep leaning in further and further? Yeah, we're not done yet because then I found this:

```
em_code: "c62fcf3563dc3ab38d52ba9ddb37f9b1577d1986"
```

Maybe I've just seen too many data breaches before, but as vague as this looks I had a really good immediate hunch of what it was but just to be sure, I logged out and went to the password reset page:

![How Spoutible’s Leaky API Spurted out a Deluge of Personal Data](https://www.troyhunt.com/content/images/2024/02/image-7.png)

Leaning in far enough now, anticipating what's going to happen next? Yep, it's exactly what you thought:

![How Spoutible’s Leaky API Spurted out a Deluge of Personal Data](https://www.troyhunt.com/content/images/2024/02/image-8.png)![How Spoutible’s Leaky API Spurted out a Deluge of Personal Data](https://www.troyhunt.com/content/images/2024/02/image-9.png)

**_NO WAY!_ Exposed password reset tokens meant that anyone could immediately takeover anyone else's account 🤯**

After changing the password, no notification email was sent to the account holder so just to make things even worse, if someone's account was taken over using this technique they'd have absolutely no idea until they either realised their original password no longer worked or their account started spouting weird messages. There's also no way to see if there are other active sessions, for example the way Twitter shows them:

![How Spoutible’s Leaky API Spurted out a Deluge of Personal Data](https://www.troyhunt.com/content/images/2024/02/image-13.png)

Further, changing the password doesn't invalidate existing sessions so as best as I can tell, if someone has successfully accessed someone else's Spoutible account there's no way to know and no way to boot them out again. That's going to make recovering from this problematic unless Spoutible has another mechanism to invalidate all active sessions.

The one saving grace is that the token was rotated after reset so you can't use the one in the image above, but of course the _new one_ was now publicly exposed in the API! And there's no 2FA challenge on password reset either but of course even if there was, well, you already read this far so you know how that could have been easily circumvented.

There's just one more "oh wow!" remaining, and it's the ease with which the vulnerable API was found. Spoutible has a feature called Pods and when you browse to that page, people listening to the pod are displayed with the ability to hover over their profile and display further information. For example, here's Rosetta and if we watch the request that's made in the dev tools...

![How Spoutible’s Leaky API Spurted out a Deluge of Personal Data](https://www.troyhunt.com/content/images/2024/02/image-12.png)

By design, all the personal information including email and IP address, phone number, gender, bcrypt hashed password, 2FA secret and backup code _and_ the code that can be immediately used to reset the password is returned to every single person that uses this feature. How many times has this API spouted troves of personal data out to people without them even knowing? Who knows, but I do know it wasn't the only API doing that because the one that listed the pods _also_ did it:

![How Spoutible’s Leaky API Spurted out a Deluge of Personal Data](https://www.troyhunt.com/content/images/2024/02/image-11.png)

Because the vulnerable APIs was requested organically as a natural part of using the service as it was intended, Spoutible almost certainly won't be able to fully identify abuse of it. To use the definition of the infamous Missouri governor who recently attempt to prosecute a journalist for pressing F12, _everyone_ who used those features inadvertently became a hacker.

Just one last finding and I've not been able to personally validate it so let's keep it out of "oh wow!" scope: the individual that sent me the data and details of the vulnerability said that the exposed data includes access tokens for other platforms. A couple of months ago, Spoutible announced cross-posting to Mastodon and Bluesky and my own data does have a "cross\_posting\_auth" node, albeit set to null. I couldn't see anywhere within the UI to enable this feature, but there are profiles with values in there. During the disclosure process (more on that soon), Spoutible did say that those value were encrypted and without evidence of a private key compromise, they believe they're safe.

Here's my full record as it was originally returned by the vulnerable API:

<script src="https://gist.github.com/troyhunt/47c486211aaefd5902b20e0260be73d6.js"></script>

To be as charitable as possible to Spoutible, you could argue that this is largely just the one vulnerability that is the inadvertent exposure of internal data via a public API. This is data that has a legitimate purpose in their system and it may simply be a case of a framework automatically picking all entity attributes up from the data tier and returning them via the UI. But it's the circumstances that allowed this to happen and then exacerbated the problem when it did that concern me more; clearly there's been no security review around this feature because it was so easily discoverable (at least there certainly wasn't review whilst it was live), nor has been any thought put in to notifying people of potential account takeovers or providing them with the means to invalidate other sessions. Then there are periphery issues such as very weak password rules that make cracking bcrypt so much easier, weak 2FA backup codes and pointless bcrypting of them. Not major issues in and of themselves, but they amplify the problems the exposed data presents.

Clearly this required disclosure before publication, unfortunately Spoutible does not publish a security.txt file so I went directly to the founder Christopher Bouzy on both Twitter and email (obviously I could have reached out on Spoutible, but he's very active on Twitter and my profile has more credibility there than a brand new Spoutible account). Here's the timeline, all AEST:

1. 4 Feb, 15:30: Initial outreach asking for security contact
2. 4 Feb, 17:27: Response from Spoutible
3. 4 Feb, 18:31: Full details provided to Spoutible
4. 4 Feb, 19:48 (or earlier): API is fixed
5. 5 Feb 01:28 (or earlier): Announcement made about the incident
6. 5 Feb 07:52: Spoutible confirmed all em\_code values have been rotated

To give credit where it's due, Spoutible's response time was excellent. In the space of only about 4 hours, the data returned by the API had a huge number of attributes trimmed off it and now aligns with what I'd expect to see (although the 207k previously scraped records obviously still contain all the data). I'll also add that Christopher's communication with me commendable; he's clearly genuinely passionate about the platform and was dismayed to learn of the vulnerability. I've dealt with many founders of projects in the past that had suffered data breaches and it's especially personal for them, having poured so much of themselves into it.

Here's their disclosure in its entirety:

![How Spoutible’s Leaky API Spurted out a Deluge of Personal Data](https://www.troyhunt.com/content/images/2024/02/image-14.png)

The revised API is now returning over 80% less data and looks like this:

<script src="https://gist.github.com/troyhunt/649eb212a736a55929477f687416f800.js"></script>

If you're a detail person, yes, the forward slashes are no longer escaped and the remaining fields are ordered slightly differently so it looks like the JSON encoder has changed. In case you're interested, here's a link to a diff between the two with a little bit of manipulation to make it easier to see precisely what's changed.

As to my own advice to Spoutible users, here are the actions I'd recommend:

1. Change your Spoutible password and change any other account you reused that password on
2. If you had 2FA turned on for Spoutible, turn it off then back on again so that it generates a different secret
3. If you enabled cross-posting to Mastodon or Bluesky, out of an abundance of caution you should invalidate the keys on those platforms
4. Recognise that your email address, IP address, phone number if you added it and any intentionally publicly visible data associated to your profile may have been exposed

The 207k exposed email addresses that were sent to me are now searchable in Have I Been Pwned and my impacted subscribers have received email notifications.

Go to Source
