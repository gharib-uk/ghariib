---
title: "Twitter to RSS with Google Cloud Function"
date: Mon, 24 Dec 2018 13:41:57 GMT
draft: false
type: posts
---
# Twitter to RSS with Google Cloud Function





2023-Oct-07 - I noticed this stopped working for me today. Not sure how long it has been broken. I’ve stopped using Twitter myself due to the lunatic leadership and have no interest in investigating why it is broken. See you on Mastodon I don’t like having to install additional software to

**2023-Oct-07** - I noticed this stopped working for me today. Not sure how long it has been broken. I’ve stopped using Twitter myself due to the lunatic leadership and have no interest in investigating why it is broken. See you on [Mastodon](https://mastodon.social/@grepular/)

* * *

I don’t like having to install additional software to follow people/organisations on Twitter. I already have an RSS client. Why should I have to install a Twitter client too?

There are services like [https://www.twitrss.me/](https://www.twitrss.me/) which gate Twitter to RSS, but they sometimes get ratelimited, as you’re sharing an API with many other people.

So I’ve created and released [funcTwitter](https://gitlab.com/mikecardwell/functwitter) (GPL-3.0). It’s a simple NodeJS application that gates Twitter to RSS. It can be deployed as a [Google Cloud Function](https://cloud.google.com/functions/), meaning you don’t need to run any special software on your own host. Google Cloud offers a generous permanent free tier for Cloud Functions, so you can easily do this for free for personal use.

You need to create a Google Cloud account and a Twitter development account. Details are in the documentation. Once setup, you can just point your RSS feed reader at URLs like:

[https://YOUR\_UNIQUE\_CLOUD\_FUNCTION\_URL?secret=A\_RANDOM\_SECRET\_YOU\_CREATED&account=THE\_TWITTER\_ACCOUNT\_TO\_FOLLOW](https://YOUR_UNIQUE_CLOUD_FUNCTION_URL?secret=A_RANDOM_SECRET_YOU_CREATED&account=THE_TWITTER_ACCOUNT_TO_FOLLOW)

The secret is basically an authentication token to prevent anybody else from calling the function.

You can also follow feeds based on searches by just switching out the “account” query string parameter with a “search” one instead:

[https://YOUR\_UNIQUE\_CLOUD\_FUNCTION\_URL?secret=A\_RANDOM\_SECRET\_YOU\_CREATED&search=YOUR\_SEARCH\_TERM](https://YOUR_UNIQUE_CLOUD_FUNCTION_URL?secret=A_RANDOM_SECRET_YOU_CREATED&search=YOUR_SEARCH_TERM)

[![](https://www.grepular.com/images/amazon/serverless_computing.jpg)](https://www.grepular.com/redir?key=amazon_serverless_computing "Beginning Serverless Computing")

#### [Source](https://www.grepular.com/Twitter_to_RSS_with_Google_Cloud_Function)

<br/>
---
