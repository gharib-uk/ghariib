---
title: "Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/02/image-11.png)

I found myself going down a previously unexplored rabbit hole recently, or more specifically, what I thought was "a" rabbit hole but in actual fact was an ever-expanding series of them that led me to what I refer to in the title of this post as "6 rabbits deep". It's a tale of firewalls, APIs and sifting through layers and layers of different services to sniff out the root cause of something that seemed very benign, but actually turned out to be highly impactful. Let's go find the rabbits!

### The Back Story

When you buy an API key on Have I Been Pwned (HIBP), Stripe handles all the payment magic. I _love_ Stripe, it's such an awesome service that abstracts away so much pain and it's dead simple to integrate via their various APIs. It's also dead simple to configure Stripe to send notices back to your own service via webhooks. For example, when an invoice is paid or a customer is updated, Stripe sends information about that event to HIBP and then lists each call on the webhooks dashboard in their portal:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/01/image-7.png)

There are a whole range of different events that can be listened to and webhooks fired, here we're seeing just a couple of them that are self explanatory in name. When an invoice is paid, the callback looks something like this:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/01/image-9.png)

HIBP has received this call and updated it's own DB such that for a new customer, they can now retrieve an API key or for an existing customer whose subscription has renewed, the API key validity period has been extended. The same callback is also issued when someone upgrades an API key, for example when going from 10RPM (requests per minute) to 50RPM. It's super important that HIBP gets that callback so it can appropriately upgrade the customer's key and they can immediately begin making more requests. When that call doesn't happen, well, let's go down the first rabbit hole.

### The Failed API Key Upgrade 🐰

This should never happen:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/01/image-10.png)

This came in via HIBP's API key support portal and is pretty self-explanatory. I checked the customer's account on Stripe and it did indeed show an active 50RPM subscription, but when drilling down into the associated payment, I found the following:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/02/image-11.png)

Ok, so at least I know where things have started to go wrong, but why? Over to the webhooks dashboard and into the failed payments and things look... suboptimal:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/01/image-12.png)

Dammit! Fortunately this is only a small single-digit percentage of all callbacks, but every time this fails it's either stopping someone like the guy above from making the requests they've paid for or potentially, causing someone's API key to expire even though they've paid for it. The latter in particular I was really worried about as it would nuke their key and whatever they'd built on top of it would cease to function. Fortunately, because that's such an impactful action I'd built in heaps of buffer for just such an occurrence and I'd gotten onto this issue quickly, but it was disconcerting all the same.

So, what's happening? Well, the response is HTTP 403 "Forbidden" and the body is clearly a Cloudflare challenge page so something at their end is being triggered. Looks like it's time to go down the next rabbit hole.

### Cloudflare's Firewall and Logs 🐰 🐰

Desperate just to quickly restore functionality, I dropped into Cloudflare's WAF and allowed all Stripe's outbound IPs used for webhooks to bypass their security controls:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/01/image-13.png)

This wasn't ideal, but it only created risk for requests originating from Stripe and it got things up and running again quickly. With time up my sleeve I could now delve deeper and work out precisely what was going on, starting with the logs. Cloudflare has a really extensive set of APIs that can control a heap of features of the service, including pulling back logs (note: this is a feature of their Enterprise plan). I queried out a slice of the logs corresponding to when some of the 403s from Stripe's dashboard occurred and found 2 entries similar to this one:

```
{"BotScore":1,"BotScoreSrc":"Verified Bot","CacheCacheStatus":"unknown","ClientASN":16509,"ClientCountry":"us","ClientIP":"54.187.205.235","ClientRequestHost":"haveibeenpwned.com","ClientRequestMethod":"POST","ClientRequestReferer":"","ClientRequestURI":"[redacted]","ClientRequestUserAgent":"Stripe/1.0 (+https://stripe.com/docs/webhooks)","EdgeRateLimitAction":"","EdgeResponseStatus":403,"EdgeStartTimestamp":1674073983931000000,"FirewallMatchesActions":["managedChallenge"],"FirewallMatchesRuleIDs":["6179ae15870a4bb7b2d480d4843b323c"],"FirewallMatchesSources":["firewallManaged"],"OriginResponseStatus":0,"WAFAction":"unknown","WorkerSubrequest":false}
```

That's one of Stripe's outbound IP's on 54.187.205.235 and the "FirewallMatchesRuleIDs" collection has a value in it. Ergo, something about this request triggered the firewall and caused it to be challenged. I'm sure many of us have gone through the following thought process before:

What did I change?

Did I change _anything?_

Did _they_ change something?

Except "they" could have been either Cloudflare or Stripe; if it wasn't me (and I was fairly certain it wasn't), was it a Cloudflare change to the rules or a Stripe change to a webhook payload that was now triggering an existing rule? Time to dig deeper again so it's over to the Cloudflare dashboard and down into the WAF events for requests to the webhook callback path:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/01/image-28.png)

Yep, something proper broke! Let's drill deeper and look at recent events for that IP:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/01/image-16.png)

As you dig deeper through troubleshooting exercises like this, you gradually turn up more and more information that helps piece the entire puzzle together. In this case, it looks like the "Inbound Anomaly Score Exceeded" rule was being triggered. What's that? And why? Time to go down another rabbit hole.

### The Cloudflare OWASP Core Ruleset 🐰 🐰 🐰

So, deeper and deeper down the rabbit holes we go, this time into the depths of the requests that triggered the managed rule:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/01/image-18.png)

Well that's comprehensive 🙂

There's a lot to unpack here so let's begin with the ruleset that the previously identified "Inbound Anomaly Score Exceeded" rule belongs to, the Cloudflare OWASP Core Ruleset:

> The Cloudflare OWASP Core Ruleset is Cloudflare’s implementation of the OWASP ModSecurity Core Rule SetOpen external link (CRS). Cloudflare routinely monitors for updates from OWASP based on the latest version available from the official code repository.

That link is yet another rabbit hole altogether so let me summarise succinctly here: Cloudflare uses OWASP's rules to identify anomalous traffic based on a customer-defined paranoia level (how strict you want to be) and then applies a score threshold (also customer-defined) at which an action will be taken, for example challenging the request. What I learned as this saga progressed is that the "Inbound Anomoly Score Exceeded" rule is actually a rollup of the rules beneath it. The OWASP score of "26" is the sum of the 6 rules listed beneath it and once it exceeds 25, the superset rule is triggered.

Further - and this is the really important bit - Cloudflare routinely updates the rules from OWASP which makes sense because these are ever-evolving in response to new threats. And when did they last upgrade the rules? It looks like they announced it right before I started having issues:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/01/image-19.png)

Whilst it's not entirely clear from above when this release was scheduled to occur, I did reach out to Cloudflare support and was advised it had already taken place:

> Please note that we did bump the OWASP version, which we are integrating with to 3.3.4 as noted on our scheduled changes.

So maybe it's not Cloudflare's fault _or_ Stripe's fault, but OWASP's fault? In fairness to all, I don't think it's anyone's fault per se and is instead just an unfortunate result of everyone doing their best to keep the bad guys out. Unless... it really is Stripe's fault because there's something in the request payload that was always fishy and is now being caught? But why for only some requests and not others? Next rabbit!

### Cloudflare Payload Logging 🐰 🐰 🐰 🐰

Sometimes, people on the internet lose their minds a bit over things they really shouldn't. One of those things, in my experience, is Cloudflare's interception of traffic and it's something I wrote about in detail nearly 7 years ago now in my piece on security absolutism. Cloudflare plays an _enormously_ valuable role in the internet's ecosystem and a substantial part of the value comes from being able to inspect, cache, optimise, and yes, even reject traffic. When you use Cloudflare to protect your website, they're applying rulesets like the aforementioned OWASP ones and in order to do that, _they must be able to inspect your traffic!_ But they don't log it, not all of it, rather just "metadata generated by our products" as they refer to it on their logs page. We saw an example of that earlier on with Stripe's request from their IP showing it triggered a firewall rule, but what we didn't see is the _contents_ of that POST request, the actual _payload_ that triggered the rule. Let's go grab that.

Because the contents of a POST request can contain sensitive information, Cloudflare doesn't log it. Obviously they see it in transit (that's how OWASP's rules can be applied to it), but it's not stored anywhere and even if _you_ want to capture it, they don't want to be able to see it. That's where payload logging (another Enterprise plan feature) comes in and what's really neat about that is every payload must be encrypted with a public key retained by Cloudflare whilst only you retain the private key. The setup looks like this:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/01/image-21.png)

Pretty self-explanatory and once done, right under where we previously saw the additional logs we now have the ability to decrypt the payload:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/01/image-25.png)

As promised, this requires the private key from earlier:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/01/image-24.png)

And now, finally, we have the actual payload that triggered the rule, seen here with my own test data:

```
[ " },n "billing_reason": "subscription_update",n "charge": null,n "collection_method": "charge_automatically",n "created": 1674351619,n "currency": "usd",n "custom_fields": null,n "customer": "cus_MkA71FpZ7XXRlt",n "customer_address": ", " },n "customer_email": "troy-hunt+1@troyhunt.com",n "customer_name": "Troy Hunt 1",n "customer_phone": null,n "customer_shipping": null,n "customer_tax_exempt": "none",n "customer_tax_ids": [nn ],n "default_payment_method": null,n "default_source": null,n "default_tax_rates": [nn ],n "description": "You can manage your subscription (i.e. cancel it or regenerate the API key) at any time by verifying your email address here: https://haveibeenpwned.com/API/Key",n "discount": null,n "discounts": [nn ],n "due_date": null,n "ending_balance": -11804,n "footer": null,n "from_invoice": null,n "hosted_invoice_url": "https://invoice.stripe.com/i/acct_1EdQYpEF14jWlYDw/test_YWNjdF8xRWRRWXBFRjE0aldsWUR3LF9OREo5SlpqUFFvVnFtQnBVcE91YUFXemtkRHFpQWNWLDY0ODkyNDIw02004bEyljdC?s=ap",n "invoice_pdf": "https://pay.stripe.com/invoice/acct_1EdQYpEF14jWlYDw/test_YWNjdF8xRWRRWXBFRjE0aldsWUR3LF9OREo5SlpqUFFvVnFtQnBVcE91YUFXemtkRHFpQWNWLDY0ODkyNDIw02004bEyljdC/pdf?s=ap",n "last_finalization_error": null,n "latest_revision": null,n "lines": ", " ", " ],n "discountable": false,n "discounts": [nn ],n "invoice_item": "ii_1MSsXfEF14jWlYDwB1nfZvFm",n "livemode": false,n "metadata": ", " },n "period": ", " },n "plan": ", " },n "nickname": null,n "product": "prod_Mk4eLcJ7JYF02f",n "tiers_mode": null,n "transform_usage": null,n "trial_period_days": null,n "usage_type": "licensed"n },n "price": ", " },n "nickname": null,n "product": "prod_Mk4eLcJ7JYF02f",n "recurring": ", " },n "tax_behavior": "unspecified",n "tiers_mode": null,n "transform_quantity": null,n "type": "recurring",n "unit_amount": 15000,n "unit_amount_decimal": "15000"n },n "proration": true,n "proration_details": ", " "il_1MMjfcEF14jWlYDwoe7uhDPF"n ]n }n },n "quantity": 1,n "subscription": "sub_1MMjfcEF14jWlYDwi8JWFcxw",n "subscription_item": "si_N6xapJ8gSXdp7W",n "tax_amounts": [nn ],n "tax_rates": [nn ],n "type": "invoiceitem",n "unit_amount_excluding_tax": "-14304"n },n ", " ],n "discountable": true,n "discounts": [nn ],n "livemode": false,n "metadata": ", " },n "period": ", " },n "plan": ", " },n "nickname": null,n "product": "prod_Mk4lTSl4axd9mt",n "tiers_mode": null,n "transform_usage": null,n "trial_period_days": null,n "usage_type": "licensed"n },n "price": ", " },n "nickname": null,n "product": "prod_Mk4lTSl4axd9mt",n "recurring": ", " },n "tax_behavior": "unspecified",n "tiers_mode": null,n "transform_quantity": null,n "type": "recurring",n "unit_amount": 2500,n "unit_amount_decimal": "2500"n },n "proration": false,n "proration_details": ", " },n "quantity": 1,n "subscription": "sub_1MMjfcEF14jWlYDwi8JWFcxw",n "subscription_item": "si_NDJ98tQrCcviJf",n "tax_amounts": [nn ],n "tax_rates": [nn ],n "type": "subscription",n "unit_amount_excluding_tax": "2500"n }n ],n "has_more": false,n "total_count": 2,n "url": "/v1/invoices/in_1MSsXfEF14jWlYDwxHKk4ASA/lines"n },n "livemode": false,n "metadata": ", " },n "next_payment_attempt": null,n "number": "04FC1917-0008",n "on_behalf_of": null,n "paid": true,n "paid_out_of_band": false,n "payment_intent": null,n "payment_settings": ", " },n "period_end": 1674351619,n "period_start": 1674351619,n "post_payment_credit_notes_amount": 0,n "pre_payment_credit_notes_amount": 0,n "quote": null,n "receipt_number": null,n "rendering_options": null,n "starting_balance": 0,n "statement_descriptor": null,n "status": "paid",n "status_transitions": ", " },n "subscription": "sub_1MMjfcEF14jWlYDwi8JWFcxw",n "subtotal": -11804,n "subtotal_excluding_tax": -11804,n "tax": null,n "test_clock": null,n "total": -11804,n "total_discount_amounts": [nn ],n "total_excluding_tax": -11804,n "total_tax_amounts": [nn ],n "transfer_data": null,n "webhooks_delivered_at": 1674351619n }n },n "livemode": false,n "pending_webhooks": 1,n "request": ", " },n "type": "invoice.paid"n}" ]
```

But enough of what's _present_ in the payload, it's what's _absent_ that especially struck me. No obvious XSS patterns, nor SQL injection or any other suspicious looking strings. The request looked totally benign, so why did it trigger the rule?

I wanted to compare the payload of a blocked request with a similar request that wasn't blocked, but they're only logged at Cloudflare when they trigger a rule. No problem, it's easy to grab the full request from Stripe's webhook history so I found one that passed and one that failed and diff'd them both:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/01/image-27.png)

This clearly isn't the full 200 lines, but it's a very similar story over the remainder of the files; tiny differences largely down to dates, IDs, and of course, the customers themselves. No suspicious patterns, no funky characters, nothing visibly abnormal. It's a bit pointless to even mention it because they're near identical, but the payload on the left is the one that passed the firewall whilst the payload on the right was blocked.

Next rabbit hole!

### Cloudflare's Internal Rules Engine 🐰 🐰 🐰 🐰 🐰

Completely running out of ideas and options, focus moved to the folks inside Cloudflare who were already aware there was an issue:

<blockquote class="twitter-tweet" data-conversation="none"><p lang="en" dir="ltr">We are actively looking into this and will likely release an update to the Cloudflare OWASP ruleset soon</p>— Michael Tremante (@MichaelTremante) <a href="https://twitter.com/MichaelTremante/status/1616413793709395971?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">January 20, 2023</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

What followed was a period of back and forth initially with Cloudflare, then Stripe as well with everyone trying to nut out exactly where things were going wrong. Essentially, the process went like this:

_Is Cloudflare inadvertently blocking the requests?_

_Is the OWASP ruleset raising false positives?_

_Is Stripe issuing requests that are deemed to be malicious?_

And round and round we went. At one time, Cloudflare identified a change in the OWASP ruleset which appeared to have resulted in their implementation inadvertently triggering the WAF. They rolled it back and... the same thing happened. We deferred back to Stripe on the assumption that _something_ must have changed on their end, but they couldn't identify any change that would have any sort of material impact. We were stumped, but we also had an easy fix just one last rabbit hole away...

### Fine Tuning the Cloudflare WAF 🐰 🐰 🐰 🐰 🐰 🐰

The joy of a managed firewall is that someone else takes all the rigmarole of looking after it away. I'm going to talk more about that in the summary shortly but clearly, that also creates risk as you're delegating control of traffic flow to someone else. Fortunately, Cloudflare gives you a load of configurability with their managed rules which makes it easy to add custom exceptions:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/02/image.png)

This meant I could create a simple exception that was much more intelligent than the previous "just let all outbound Stripe IPs in" by filtering down to the _specific_ path those webhooks were flowing in to:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/02/image-1.png)

And finally, because sequence matters, I dragged that rule right up to the top of the pile so it would cause matching inbound requests to skip all the other rules:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/02/image-2.png)

And finally, there were no more rabbits 😊

### Lessons Learned

I know what you're thinking - "what was the _actual_ root cause?" - and to be honest, I still don't know. I don't know if it was Cloudflare or OWASP or Stripe or if it even impacted other customers of these services and to be honest, yes, that's a little frustrating. But I learned a bunch of stuff and for that alone, this was a worthwhile exercise I took three big lessons away from:

Firstly, understanding the plumbing of how all these bits work together is super important. I was lucky this wasn't a time critical issue and I had the luxury of learning without being under duress; how rules, payload inspection and exception management all work together is really valuable stuff to understand. And just like that, as if to underscore my first point, I found this right before hitting the publish button on the blog post:

![Down the Cloudflare / Stripe / OWASP Rabbit Hole: A Tale of 6 Rabbits Deep 🐰 🐰 🐰 🐰 🐰 🐰](https://www.troyhunt.com/content/images/2023/02/image-3.png)

I added a couple more OWASP rules to the exception in Cloudflare (things like a MySQL rule that was adding 5 points), and we were back in business.

Secondly, I look at the managed WAF Cloudflare provides _more_ favourably than I did before simply because I have a better understanding of how comprehensive it is. I want to write code and run apps on the web, that's my focus, and I want someone else to provide that additional layer on top that continuously adapts to block new and emerging threats. I want to _understand_ it (and I now do, at least certainly better than before), but I don't want managing it day in and day out to be my job.

And finally, IMHO, Stripe needs a better mechanism to report on webhook failures:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">In live mode you are notified after 3 days of trying. You can also query the events (<a href="https://t.co/0mujOPssV0?ref=troyhunt.com">https://t.co/0mujOPssV0</a>) to create a running list of statuses on web hooks that have been sent and alert on that via your own app.</p>— Blake Krone (@blakekrone) <a href="https://twitter.com/blakekrone/status/1615903694159073282?ref_src=twsrc%5Etfw&amp;ref=troyhunt.com">January 19, 2023</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Waiting until stuff breaks really isn't ideal and whilst I'm sure you could plug into the (very extensive) API ecosystem Stripe has, this feels like an easy feature for them to build in. So, Stripe friends, when you read this that's a big "yes" vote from me for some form of anomalous webhook response alerting.

This experience was equal parts frustration and fun and whilst the former is probably obvious, the latter is simply due to having an opportunity to learn something new that's a pretty important part of the service I run. May my frustrated fun story here make your life easier in the future if you face the same problems 😊

Go to Source
