---
title: "<div>Inside the Massive Alleged AT&T Data Breach</div>"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

![Inside the Massive Alleged AT&T Data Breach](https://www.troyhunt.com/content/images/2024/03/ezgif-4-4feebf3c7a.jpg)

I hate having to use that word - "alleged" - because it's so inconclusive and I know it will leave people with many unanswered questions. (**Edit:** 12 days after publishing this blog post, it looks like the "alleged" caveat can be dropped, see the addition at the end of the post for more.) But sometimes, "alleged" is just where we need to begin and over the course of time, proper attribution is made and the dots are joined. We're here at "alleged" for two very simple reasons: one is that AT&T is saying "the data didn't come from us", and the other is that I have no way of proving otherwise. But I have proven, with sufficient confidence, that the data is real and the impact is significant. Let me explain:

Firstly, just as a primer if you're new to this story, read BleepingComputer's piece on the incident. What it boils down to is in August 2021, someone with a proven history of breaching large organisations posted what they claimed were 70 million AT&T records to a popular hacking forum and asked for a very large amount of money should anyone wish to purchase the data. From that story:

> From the samples shared by the threat actor, the database contains customers' names, addresses, phone numbers, Social Security numbers, and date of birth.

Fast forward two and a half years and the successor to this forum saw a post this week alleging to contain the entire corpus of data. Except that rather than put it up for sale, someone has decided to just dump it all publicly and make it easily accessible to the masses. This isn't unusual: "fresh" data has much greater commercial value and is often tightly held for a long period before being released into the public domain. The Dropbox and LinkedIn breaches, for example, occurred in 2012 before being broadly distributed in 2016 and just like those incidents, the alleged AT&T data is now in _very_ broad circulation. It is undoubtedly in the hands of thousands of internet randos.

AT&T's position on this is pretty simple:

> AT&T continues to tell BleepingComputer today that they still see no evidence of a breach in their systems and still believe that this data did not originate from them.

The old adage of "absence of evidence is not evidence of absence" comes to mind (just because they can't find evidence of it doesn't mean it didn't happen), but as I said earlier on, I (and others) have so far been unable to prove otherwise. So, let's focus on what we _can_ prove, starting with the accuracy of the data.

The linked article talks about the author verifying the data with various people he knows, as well as other well-known infosec identities verifying its accuracy. For my part, I've got 4.8M Have I Been Pwned (HIBP) subscribers I can lean on to assist with verification, and it turns out that 153k of them are in this data set. What I'll typically do in a scenario like this is reach out to the 30 newest subscribers (people who will hopefully recall the nature of HIBP from their recent memory), and ask them if they're willing to assist. I linked to the story from the beginning of this blog post and got a handful of willing respondents for whom I sent their data and asked two simple questions:

1. Does this data look accurate?
2. Are you an AT&T customer and if not, are you a customer of another US telco?

The first reply I received was simple, but emphatic:

![Inside the Massive Alleged AT&T Data Breach](https://www.troyhunt.com/content/images/2024/03/GI_kjAfbQAA1eHV.jpg)

This individual had their name, phone number, home address and most importantly, their social security number exposed. Per the linked story, social security numbers and dates of birth exist on most rows of the data in encrypted format, but two supplemental files expose these in plain text. Taken at face value, it looks like whoever snagged this data also obtained the private encryption key and simply decrypted the vast bulk (but not all of) the protected values.

![Inside the Massive Alleged AT&T Data Breach](https://www.troyhunt.com/content/images/2024/03/GI_kxrkbYAAzxjz.jpg)

The above example simply didn't have plain text entries for the encrypted data. Just by way of raw numbers, the file that aligns with the "70M" headline actually has 73,481,539 lines with 49,102,176 unique email addresses. The file with decrypted SSNs has 43,989,217 lines and the decrypted dates of birth file only has 43,524 rows. (**Edit:** the reason for this later became clear - there is only one entry per date of birth which is then referenced from multiple records.) The last file, for example, has rows that look just like this:

```
.encrypted_value='*0g91F1wJvGV03zUGm6mBWSg==' .decrypted_value='1996-07-18'
```

That encrypted value is precisely what appears in the large file hence providing an easy way of matching all the data together. But those numbers also obviously mean that not every impacted individual had their SSN exposed, and _most_ individuals didn't have their date of birth leaked. (**Edit:** per above, the same entries in the DoB file are referenced by multiple source records so whilst not every record had a DoB recorded, the difference isn't as stark as I originally reported.)

![Inside the Massive Alleged AT&T Data Breach](https://www.troyhunt.com/content/images/2024/03/GI_xf24asAEPboF.jpg)

As I'm fond of saying, there's only one thing worse than your data appearing on the dark web: it's appearing on the clear web. And that's precisely where it is; the forum this was posted to isn't within the shady underbelly of a Tor hidden service, it's out there in plain sight on a public forum easily accessed by a normal web browser. And the data is real.

That last response is where most people impacted by this will now find themselves - "what do I do?" Usually I'd tell them to get in touch with the impacted organisation and request a copy of their data from the breach, but if AT&T's position is that it didn't come from them then they may not be much help. (Although if you are a current or previous customer, you can certainly request a copy of your personal information regardless of this incident.) I've personally also used identity theft protection services since as far back as the 90's now, simply to know when actions such as credit enquiries appear against my name. In the US, this is what services like Aura do and it's become common practice for breached organisations to provide identity protection subscriptions to impacted customers (full disclosure: Aura is a previous sponsor of this blog, although we have no ongoing or upcoming commercial relationship).

What I can't do is send you your breached data, or an indication of what fields you had exposed. Whilst I did this in that handful of aforementioned cases as part of the breach verification process, this is something that happens entirely manually and is infeasible en mass. HIBP only ever stores email addresses and never the additional fields of personal information that appear in data breaches. In case you're wondering why that is, we got a solid reminder only a couple of months ago when a service making this sort of data available to the masses had an incident that exposed tens of _billions_ of rows of personal information. That's just an unacceptable risk for which the old adage of "you cannot lose what you do not have" provides the best possible fix.

As I said in the intro, this is not the conclusive end I wanted for this blog post... yet. As impacted HIBP subscribers receive their notifications and particularly as those monitoring domains learn of the aliases in the breach (many domain owners use unique aliases per service they sign up to), we may see a more conclusive outcome to this incident. That may not necessarily be confirmation that the data did indeed originate from AT&T, it could be that it came from a third party processor they use or from another entity altogether that's entirely unrelated. The truth is somewhere there in the data, I'll add any relevant updates to this blog post if and when it comes out.

As of now, all 49M impacted email addresses are searchable within HIBP.

**Edit (31 March):** AT&T have just released a short statement making 2 important points:

> AT&T data-specific fields were contained in a data set

> it is not yet known whether the data in those fields originated from AT&T or one of its vendors

They've also been mass-resetting account passcodes after TechCrunch apparently alerted AT&T to the presence of these in the data set. That article also includes the following statement from AT&T:

> Based on our preliminary analysis, the data set appears to be from 2019 or earlier, impacting approximately 7.6 million current AT&T account holders and approximately 65.4 million former account holders

Between originally publishing this blog post and AT&T's announcements today, there have been dozens of comments left below that attribute the source of the breach to AT&T in ways that made it increasingly unlikely that the data could have been sourced from anywhere else. I know that many journos (and myself) reached out to folks in AT&T to draw their attention to this, I'm happy to now end this blog post by quoting myself from the opening para 😊

> But sometimes, "alleged" is just where we need to begin and over the course of time, proper attribution is made and the dots are joined.

Go to Source
