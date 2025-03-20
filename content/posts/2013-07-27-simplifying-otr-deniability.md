---
title: "Simplifying OTR deniability"
date: Sat, 27 Jul 2013 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Simplifying OTR deniability

<br/>

<br/>
At Open Whisper Systems we help develop [TextSecure](https://play.google.com/store/apps/details?id=org.thoughtcrime.securesms), an encrypted chat application for Android. TextSecure was designed as a general purpose SMS/MMS client which would also automatically encrypt conversations when communicating with other TextSecure users. For those encrypted sessions, TextSecure uses a compact derivative of the well-known [OTR protocol](http://www.cypherpunks.ca/otr/).

We’re currently in the process of transitioning TextSecure to use a device’s data channel as a transport for communication with other TextSecure users whenever possible. This enables communication with the upcoming [TextSecure for iOS](/blog/iphone-rsn), helps users avoid SMS fees, and obscures conversation metadata from telcos.

The transition to a new transport is also a good opportunity for us to evaluate and introduce additional cryptographic protocol changes. Below is one cryptographic protocol change we’re thinking of making that we’d welcome feedback on.

[_Read more..._](https://signal.org/blog/simplifying-otr-deniability/)

#### [Source](https://signal.org/blog/simplifying-otr-deniability/)

<br/>
---
