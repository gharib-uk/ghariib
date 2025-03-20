---
title: "4.8 million healthcare records left freely accessible"
date: 2025-01-03
categories: 
  - "security"
---

Your main business is healthcare, so your excuse when you get hacked is that you didn’t have the budget to secure your network. Am I right?

So, in order to prevent a ransomware gang from infiltrating your network, you could just give them what they want—all your data.

The seemingly preferred method to accomplish this is to leave the information unprotected and unencrypted in an exposed Amazon S3 bucket.

An S3 bucket is like a virtual file folder in the cloud where you can store various types of data, such as text files, images, videos, and more. There is no limit to the amount of data you can store in an S3 bucket, and individual instances can be up to 5 TB in size.

Security researcher Jeremiah Fowler is always looking for exposed cloud storage. And recently he found one that contained over 4.8 million documents with a total size of 2.2 TB.

He soon found out that it belonged to a Canadian company offering AI software solutions to support optometrists in delivering enhanced patient care, called Care1. Care1 Canada provides software tools that “take patient care to the next level.”

The information Jeremiah found included eye exam results, which detailed patient PII, doctor’s comments, and images of the exam results. The database also contained lists of patients which included their home addresses, Personal Health Numbers (PHN), and details regarding their health.

In the Canadian healthcare system, a Personal Health Number (PHN) is a unique lifetime identifier that is used to share a patient’s health information among healthcare providers.

This type of healthcare information can be used in phishing attacks, identity theft, and can cause health privacy issues. Ransomware gangs know this is highly coveted, which is why ThreatDown numbers regularly show that 5 to 6% of ransomware attacks are targeting the healthcare industry.

* * *

**We don’t just report on threats – we help safeguard your entire digital identity**

Cybersecurity risks should never spread beyond a headline. Protect your—and your family’s—personal information by using identity protection.

Go to Source
