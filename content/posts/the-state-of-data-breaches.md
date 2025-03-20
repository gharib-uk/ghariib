---
title: "The State of Data Breaches"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

![The State of Data Breaches](https://www.troyhunt.com/content/images/2024/06/ezgif-1-0d8c25890a.jpg)

I've been harbouring some thoughts about the state of data breaches over recent months, and I feel they've finally manifested themselves into a cohesive enough story to write down. Parts of this story relate to very sensitive incidents and parts to criminal activity, not just on behalf of those executing data breaches but also very likely on behalf of some organisations handling them. As such, I'm not going to refer to any specific incidents or company names, rather I'm going to speak more generally to what I'm seeing in the industry.

### Breach Disclosure is Still a Painful Time Suck

Generally, when I disclose a breach to an impacted company, it's already out there in circulation and for all I know, the company is already aware of it. Or not. And that's the problem: a data breach circulating broadly on a popular clear web hacking forum doesn't mean the incident is known by the corporate victim. Now, if I can find press about the incident, then I have a pretty high degree of confidence that someone has at least tried to notify the company involved (journos generally reach out for comment when writing about a breach), but often that's non-existent. So, too, are any public statements from the company, and I very often haven't seen any breach notifications sent to impacted individuals either (I usually have a slew of these forwarded to me after they're sent out). So, I attempt to get in touch, and this is where the pain begins.

I've written before on many occasions about how hard it can be to contact a company and disclose a breach to them. Often, contact details aren't easily discoverable; if they are, they may be for sales, customer support, or some other capacity that's used to getting bombarded with spam. Is it any wonder, then, that so many breach disclosures that I (and others) attempt to make end up going to the spam folder? I've heard this so many times before after a breach ends up in the headlines - "we did have someone try to reach out to us, but we thought it was junk" - which then often results in news of the incident going public before the company has had an opportunity to respond. That's not good for anyone; the breached firm is caught off-guard, they may very well direct their ire at the reporter, and it may also be that the underlying flaw remains unpatched, and now you've got a bunch more people looking for it.

An approach like security.txt is meant to fix this, and I'm _enormously_ supportive of this, but in my experience, there are usually two problems:

1. When a firm uses one, they get bombarded with beg bounties and legitimate reports get lost in all the junk
2. There has only ever been one single instance of a company I've disclosed to having a security.txt file

That one instance was so exceptional that, honestly, I hadn't even looked for the file before asking the public for a security contact at the firm. Shame on me for that, but is it any wonder?

Once I do manage to make contact, I'd say about half the time, the organisation is good to deal with. They often already know of HIBP and are already using it themselves for domain searches. We've joked before (the company and I) that they're grateful for the service but never wanted to hear from me!

The other half of the time, the response borders on open hostility. In one case that comes to mind, I got an email from their lawyer after finally tracking down a C-suite tech exec via LinkedIn and sending them a message. It wasn't threatening, but I had to go through a series of to-and-fro explaining what HIBP was, why I had their data and how the process usually unfolded. When in these positions, I find myself having to try and talk up the legitimacy of my service without sounding conceited, especially as it relates to publicly documented relationships with law enforcement agencies. It's laborious.

My approach during disclosure usually involves laying out the facts, pointing out where data has been published, and offering to provide the data to the impacted organisation if they can't obtain it themselves. I then ask about their timelines for notifying impacted customers and welcome their commentary to be included in the HIBP notifications sent to our subscribers. This last point is where things get more interesting, so let's talk about breach notifications.

### Breach Notifications Are Still Not What We Thought They Would Be

This is perhaps one of my greatest bugbears right now and whilst the title will give you a pretty good sense of where I'm going, the nuances make this particularly interesting.

I suggest that most of us believe that if your personal information is compromised in a data breach, you'll be notified following this discovery by the organisation responsible for the service. Whether it's one day, one week, or even a month later isn't really the issue; frankly, any of these time frames would be a good step forward from where we frequently find ourselves. But constantly, I'm finding that companies are taking the position of consciously not notifying individuals _at all_. Let me give you a handful of examples:

During the disclosure process of a recent breach, it turned out the organisation was already aware of the incident and had taken "appropriate measures" (their term was something akin to that being vague enough to avoid saying what had been done, but, uh, "something" had been done). When pressed for a breach notice that would go to their customers, they advised they wouldn't be sending one as the incident had occurred more than 6 months ago. That stunned me - the outright admission that they wouldn't be communicating this incident - and in case you're thinking "this would never be allowed under GDPR", the company was HQ'd well within that scope being based in a major European city.

Another one that I need to be especially vague about (for reasons that will soon become obvious), involved a sizeable breach of customer data with the folks exposed inhabiting every corner of the globe. During my disclosure to them, I pushed them on a timeline for notifying victims and found their responses to be indirect but almost certainly indicating they'd never speak publicly about it. Statements to the effect of "we'll send notifications where we deem we're legally obligated to", which clearly left it up to them to make the determination. I later learned from a contact close to the incident that this particular organisation had an impending earnings call and didn't want the market to react negatively to news of a breach. "Uh, you know that's a whole different thing if they deliberately cover that up, right?"

An important point to make here, though, is that when it comes to companies themselves disclosing they've been breached, disclosure to _individuals_ is often not what people think it is. In the various regulatory regimes we have across the globe, the legal requirement often stops at notifying the _regulator_ and does not extend to notifying the _individual victims_. This surprises many people, and I constantly hear the rant of "But I'm in \[insert your country here\], and we have laws that demand I'm notified!" No, you almost certainly don't... but you should. We _all_ should.

You can see further evidence by looking at recent Form 8-K SEC filings in the US. There are many examples of filings from companies that _never_ notified the individuals themselves, yet here, you'll clearly see disclosure to the regulator. The breach is known, it's been reported in the public domain, but good luck ever getting an email about it yourself.

### Companies Prioritise Downplaying Severity and Covering Their Arses

During one disclosure, I had the good fortune of a very close friend of mine working for the company involved in an infosec capacity. They were clearly stalling, being well over a week from my disclosure yet no public statements or notices to impacted individuals. I had a quiet chat with my contact, who explained it as follows:

> Mate, it's a room full of lawyers working out how to spin this

Meanwhile, millions of records of customer data were in the hands of criminals, and every hour that went by was another hour victims went without any knowledge whatsoever that their personal info had been exposed. And as much as it pains me to say this, I get it: the company's priority is the company or, more specifically, the shareholders. That's who the board is accountable to, and maintaining the corporate reputation and profitability of the firm is their number one priority.

I see this all the time in post-breach communication too. One incident that comes to mind was the result of some _egregiously_ stupid technical decisions. Once that breach hit the press, the CEO immediately went on the offence. Blame was laid firstly at those who obtained the data, then at me for my reporting of the incident (my own disclosure was absolutely "by the book").

### Data Breach Victims are Making it Worse

I'm talking about class actions. I wrote about my views on this a few years ago and nothing has changed, other than it getting worse. I regularly hear from data breach victims about them wanting compensation for the impact a breach has had on them yet when pushed, most struggle to explain why. We've had multiple recent incidents in Australia where drivers' licences have been exposed and required reissuing, which is usually a process of going to a local transport office and waiting in a queue. "Are you looking for your time to be compensated for?", I asked one person. We have to rotate our licenses every 5 years anyway, so would you pro-rata that time based on the hourly value of your time and when you were due to be back in there anyway? And if there _has_ been identity theft, was it from the breach you're now seeking compensation for? Or the other ones (both known and unknown) from which your data was taken?

Lawyers are a big part of the problem, and I still regularly hear from them seeking product placement on HIBP. What a time and a place to cash in if you could get your class action pitch right there in front of people at the moment they learn they were in a breach!

Frankly, I don't care too much about individuals getting a few bucks in compensation (and it's only ever a few), and I also don't even care about lawyers doing lawyer things. But I _do_ care about the adverse consequences it has on the corporate victims, as it makes my job a hell of a lot harder when I'm talking to a company that's getting ready to get sued because of the information I've just disclosed to them.

### Summary

These are all intertwined problems without single answers. But there are some clear paths forward:

Firstly, and this seems so obvious that it's frankly ridiculous I need to write it, but there should _always_ be disclosure to individual victims. This may not need to be with the same degree of expeditiousness as disclosure to the regulator, but it has to happen. It _is_ a harder problem for businesses; submitting a form to a gov body can be infinitely easier than emailing potentially hundreds of millions of breached customers. However, it is, without any doubt, the right thing to do and there should be legal constructs that mandate it.

Simultaneously providing protection from frivolous lawsuits where no material harm can be demonstrated and throwing the book at firms who deliberately conceal breaches also seems reasonable. No company is ever immune from a breach, and so frequently, it occurs not due to malicious behaviour by the organisation but a series of often unfortunate events. Ambitious lawyers shouldn't be in a position where they can make hell for a company at their worst possible hour unless there there is significant harm _and_ negligence that can be clearly attributed back to the incident.

And then there's all the periphery stuff that pours fuel on the current dumpster fire. The aforementioned beg bounties that cause companies to be suspicious of even the most genuine disclosures, for example. On the other hand, the standoff-ish behaviour of many organisations receiving reports from folks who just want to see incidents disclosed. Flip side again is the number of people occupying that periphery of "security researcher / extortionist" who cause the aforementioned behaviours described in this paragraph. It's a mess, and writing it down like this makes it so abundantly apparent how many competing objectives there are.

I don't see anything changing any time soon, and anecdotally, it's worse now than it was 5 or 10 years ago. In part, I suspect that's due to how all those undesirable behaviours I described above have evolved over time, and in part I also believe the increasingly complexity of external dependencies is driving this. How many breaches have we seen in just the last year that can be attributed to "a third party"? I quote that term because it's often used by organisations who've been breached as though it somehow absolves them of some responsibility; "it wasn't _us_ who was breached, it was those guys over there". Of course, it doesn't work that way, and more external dependencies leads to more points of failure, all of which you're still _accountable_ for even if you've done everything else right.

Ah well, as I often end up lamenting, it's a fascinating time to be in the industry 🤷‍♂️

Go to Source
