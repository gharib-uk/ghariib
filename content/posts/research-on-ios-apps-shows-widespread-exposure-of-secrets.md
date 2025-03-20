---
title: "Research on iOS apps shows widespread exposure of secrets"
date: 2025-03-19
---

Researchers found that most of the apps available on Apple’s App Store leak at least one hard-coded secret.

The researchers looked at 156,000 iOS apps and discovered more than 815,000 hardcoded secrets, including very sensitive secrets like keys to cloud storage, various Application Programming Interfaces (APIs), and even payment processors.

The researchers noted how:

“The average app’s code exposes 5.2 secrets, and 71% of apps leak at least one secret.”

Secrets hard-coded in the source code of the apps are considered exposed because they are relatively easy to find and abuse by cybercriminals.

While you may think that’s the publisher’s problem, these hard-coded secrets can have serious consequences for the an app’s users, particularly when these are credentials which provide access to cloud storage services like AWS S3 buckets or Azure Blob Storage. The researchers found 78,000 apps which exposed cloud storage buckets.

We have posted plenty of examples of exposed AWS S3 buckets over the years, often leading to millions of exposed customer records. Depending on the type of app these records can contain financial data, location data, and other personal information.

Unless you’re able to reverse engineer an app, there is not a lot you can do after the fact. But you can keep this information in mind before you install an app. Do you trust the developers to follow best practices and do you really need it? Also keep a tight rein on the permissions you allow an app.

## Protecting yourself after a data breach

There are some actions you can take if you are, or suspect you may have been, the victim of a data breach.

- **Check the vendor’s advice.** Every breach is different, so check with the vendor to find out what’s happened, and follow any specific advice they offer.

- **Change your password.** You can make a stolen password useless to thieves by changing it. Choose a strong password that you don’t use for anything else. Better yet, let a password manager choose one for you.

- **Enable two-factor authentication (2FA).** If you can, use a FIDO2-compliant hardware key, laptop or phone as your second factor. Some forms of two-factor authentication (2FA) can be phished just as easily as a password. 2FA that relies on a FIDO2 device can’t be phished.

- **Watch out for fake vendors.** The thieves may contact you posing as the vendor. Check the vendor website to see if they are contacting victims, and verify the identity of anyone who contacts you using a different communication channel.

- **Take your time.** Phishing attacks often impersonate people or brands you know, and use themes that require urgent attention, such as missed deliveries, account suspensions, and security alerts.

- **Consider not storing your card details**. It’s definitely more convenient to get sites to remember your card details for you, but we highly recommend not storing that information on websites.

- **Set up identity monitoring.** Identity monitoring alerts you if your personal information is found being traded illegally online, and helps you recover after.

## Check your digital footprint

If you want to find out what personal data of yours has been exposed online, you can use our free Digital Footprint scan. Fill in the email address you’re curious about (it’s best to submit the one you most frequently use) and we’ll send you a free report.

SCAN NOW

Go to Source
