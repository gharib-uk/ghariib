---
title: "Android devices track you before you even sign in"
date: 2025-03-19
---

Google is spying on Android users, starting from even before they have logged in to their Google account.

That’s what researchers from Dublin’s Trinity College found after they conducted a measurement study to investigate the cookies, identifiers and other data stored on Android devices by Google Play Services.

As the company behind the Android Operating System (OS), the Google Play Store, the most popular search engine in the world, and part of the leading company in digital advertising (Alphabet), Google has obtained a position where it would be hard not to profit from.

However, the ways in which Google uses all of these market shares should not be at the expense of the users and their privacy. So, what the researchers found might be worse than you expected. Or not.

The researchers found that multiple identifiers are used to track the user of an Android handset, even before they have opened a Google app or signed in to their Google account. Pre-installed apps like the Google Play Services and Google Play Store send cookies, identifiers and other data to Google servers.

Without user consent, the researchers flagged at least five types of identifiers:

- Advertising analytics cookies

- Tracking cookies

- The Google Android ID

- Analytics cookies used for A/B testing

- Multiple other cookies and identifiers which can uniquely identify the handset

Since there is no ask for consent, there is no way to opt-out. Ironically, Google explains one of the advertising analytics cookies, the DSID cookie, as “used to identify a signed-in user on non-Google sites so that the user’s ads personalization setting is respected accordingly.” So, Google uses an unannounced cookie to make sure advertisers are able to respect your settings. Ever considered not telling them who I am?

The researchers found little hope for a user who wants to get around this:

> “Users currently have little control over the data that apps store on an Android handset. It is possible to use the Settings app to clear the data stored by an app. This deletes all the data in the app’s data folder, and is akin to re-installing the app. There is no ability to selectively delete cookies etc, unlike within a web browser, and no ability to prevent their storage in the first place.”

The Google Android ID requires some explanation as well. This unique identifier is used in traffic between the device and Google Play Services and Google Play Store. This ID is created immediately after the first connection is made to the device by Google Play Services. Once a user logs in to their Google account, the account and the Google Android ID are linked together, which likely qualifies it as personally identifiable information (PII). The ID is persistent to the extent that logging out of the Google account does not remove it. The only way to remove it, and its data, is to factory-reset the device.

When asked for a response, a Google spokesperson told The Register:

> “This report identifies a number of Google technologies and tools that underpin how we bring helpful products and services to our users.”

Personally, I feel Google could do better by informing users about its tracking methods, as well as ask for consent. But as we pointed out when we looked at the recent new Android System SafetyCore app, Google has no qualms installing secret services for “our own good.”

Perhaps it would be better for Google to let us know what it’s doing, and we’ll decide whether we want that or not.

* * *

**We don’t just report on threats – we help safeguard your entire digital identity**

Cybersecurity risks should never spread beyond a headline. Protect your—and your family’s—personal information by using identity protection.

Go to Source
