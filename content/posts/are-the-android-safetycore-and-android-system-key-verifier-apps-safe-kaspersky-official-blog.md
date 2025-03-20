---
title: "Are the Android SafetyCore and Android System Key Verifier apps safe? | Kaspersky official blog"
date: 2025-03-19
tags: 
  - "cybersecurity"
  - "data_breaches"
  - "infostealers"
  - "linux"
  - "malware"
  - "privacy"
  - "security"
  - "siem"
---

How and why Android SafetyCore scans images on your smartphone — and how to remove it.

Since February, many users have been complaining about the Android System SafetyCore app suddenly appearing on their Android phones. It has neither UI nor settings, but Google Play says the developer is Google itself, the number of installations exceeds a billion, and the average rating is a dismal 2.4 stars. The purpose of the app is described vaguely: “It provides the underlying technology for features like the upcoming Sensitive Content Warnings feature in Google Messages”. It’s not hard to guess what “sensitive content” stands for, but how and why is Google going to be warning us about it? And how is it going to find out whether the content is indeed sensitive in nature?

First, some reassurance regarding privacy: neither Google nor independent experts have reported any privacy concerns. SafetyCore runs locally — without sending photos or associated information to external servers. When the user receives an image in Google Messages, a machine-learning model that runs locally on the phone analyzes it and blurs it if it detects anything saucy. To remove the blur, the user has to tap the image and confirm that they really want to view the content. A similar thing happens when sending: if the user tries to send an image with nudity, the phone double-checks if it really needs to be sent. Google stresses that it doesn’t send scan results anywhere.

The SafetyCore app handles the image analysis — but it’s not designed for standalone use. Other apps call on SafetyCore when receiving or sending pictures, but it’s up to them how to use the output. So far, AI analysis can only be used in Google Messages: images recognized as “sensitive” will be blurred. In the future, Google promises to make SafetyCore features available to other developers, enabling apps like WhatsApp and Telegram to detect nudes as well. Other apps could be configured to, for example, block adult content or immediately filter such images into spam.

Unlike previous attempts by Google and Apple to protect children from unwanted content, SafetyCore avoids external server analysis, which enhances privacy but strains hardware. Google anticipates that SafetyCore will eventually be installed on all sufficiently powerful (2GB RAM, Android 9+) phones. The feature will be disabled by default for adult users but enabled for minors. If you don’t need this kind of hand-holding, or don’t like having extra apps, you can simply remove SafetyCore from your phone. Unlike numerous other Google services, this app can easily be uninstalled through both Google Play and the “Apps” subsection of the phone settings. However, bear in mind that Google might reinstall the app with a future update.

SafetyCore is the most sophisticated, though not the only, on-device (meaning no cloud usage and no user-data sharing) AI-powered protection system that Google is developing. Alongside SafetyCore, in October 2024 Google announced language models designed to analyze messages from strangers in Google Messages and suggest ending the conversation if the message text resembles a typical scam scheme.

Besides SafetyCore, another app is spawning on devices with no warning — Android System Key Verifier. It also has no UI, can easily be uninstalled, and is designed for secure communication. However, it features no AI-driven analysis. This app enables two users to verify their keys during end-to-end encrypted messaging. WhatsApp and Signal have their own ways of doing this (users scan each other’s QR codes when meeting in person, or they compare long strings of numbers that show up on the screen). Google wants to make this easier for all messaging apps by putting a standard interface into Android.

Users’ main issue with Google, and the reason for the poor ratings, isn’t what the apps do, but how they’re installed: with no warnings, no explanations, and no user choice. A new app just appears on their phones. Many Google Play reviewers worry if it’s a virus, and some claim their phones or specific apps see reduced performance. There were no widespread issues connected to installing these Google apps, but if you’ve any doubts, you can manually delete the app and see if your phone indeed works better.

Go to Source
