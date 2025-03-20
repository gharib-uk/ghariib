---
title: "The Evolution of iOS Passcode Security"
date: 2025-02-01
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "general"
  - "ios"
  - "iphone"
  - "passcode"
  - "pin"
  - "screen-lock"
  - "security"
  - "security-awareness"
  - "unlock"
  - "vulnerabilities"
---

![](https://blog.elcomsoft.com/wp-content/uploads/2023/04/passcodes.jpg)

Over the years, Apple has continuously refined its security mechanisms to deter unauthorized access to their devices. One of the most significant aspects of this evolution is the increasingly sophisticated passcode protection system in iOS devices. This article explores how the delay between failed passcode attempts has evolved over time, highlighting changes that have made iOS screen lock protection more secure.

## Early Stages

In the early days of iOS, passcode attempts were practically unlimited. The delay between failed attempts would increase over time, but in extreme cases, the wait time could reach absurd numbers -sometimes even displaying “Next attempt in 100 million years.” There are plenty of screenshots of these exaggerated lockout periods available on the net.

## iOS 10 and iOS 11

Back in 2018, we performed a number of tests to verify the amount of the delay introduced between unsuccessful unlock attempts. In iOS 10 and 11, Apple introduced an effective mechanism to deter brute-force attacks on passcodes, which implemented progressively increasing delays after each unsuccessful attempt. The results for iOS 10 and 11 were identical:

- **5 attempts** : 1 minute
- **6 attempts** : 5 minutes
- **7 attempts** : 15 minutes
- **8 attempts** : 15 minutes
- **9 attempts** : 1 hour
- **10 attempts** : 1 hour
- **11 attempts** : Erase data or “iPhone is disabled”

## iOS 12: Still Unchanged

With the release of iOS 12, Apple maintained a similar structure, keeping the delays largely unchanged. We re-run the tests, using devices running iOS 10 and iOS 12. The results showed that both devices followed the same pattern of increasing delays, culminating in an hour-long wait before the final attempts. At that time, we stopped the test early for the iOS 10 device to preserve the jailbreak,  which was essential for low-level extraction back in the days, while allowing the iOS 12 device to bwcome disabled after multiple failed attempts. Once again, the result for iOS 12 was identical to iOS 10 and iOS 11:

- **5 attempts** : 1 minute
- **6 attempts** : 5 minutes
- **7 attempts** : 15 minutes
- **8 attempts** : 15 minutes
- **9 attempts** : 1 hour
- **10 attempts** : 1 hour
- **Erase data or “iPhone is disabled”**

Interestingly, back then Apple had these numbers wrong in their official documentation. In https://support.apple.com/en-gb/ht204306, Apple stated: “If you enter the wrong passcode on an iOS device six times in a row, you’ll be locked out and a message will say that your device is disabled.” We have not been able to confirm the number of failed attempts being “six times in a row”. In our tests, the delay occurs after entering the wrong passcode 5 times in a row, after which we experience a progressively increasing delay. This mistake was puzzling, yet Apple had already corrected the documentation, and the wrong number is no longer present in the official documentation. The original version can still be accessed on Web Archive.

## 2023 and Beyond

By 2023, the delay scheme was still the same in the then-current versions of iOS as documented in Analyzing iPhone PINs published in April 2023, so we actually stopped looking. However, it recently came to our attention that Apple’s latest iteration of the passcode delay system introduces even longer waiting periods of 3 hours and 8 hours before the device finally becomes disabled:

- **1-3 attempts** : No delay
- **4th attempt** : 1 minute
- **5th attempt** : 5 minutes
- **6th attempt** : 15 minutes
- **7th attempt** : 1 hour
- **8th attempt** : 3 hours
- **9th attempt** : 8 hours
- **10 or more attempts** : Device is disabled and must connect to a Mac or PC

This new approach significantly increases the time required for an attacker to make numerous incorrect guesses, effectively rendering manual brute-force methods impractical.

We tried to trace when the change had actually occurred by reviewing the changes in Apple documentation over time. The change seemingly occurred some time between 08.2023 and 10.2024; unfortunately intermediary versions of the documentation are not available on Web Archive. August 2023: old scheme. October 2024: updated scheme.

## Your Old Passcode May Just Work

There is one more interesting feature that was introduced in iOS 17 that makes it possible to temporarily use an old passcode if the new one is forgotten. However, some users posted evidence of abnormal behavior (e.g. Reddit discussion, Apple forums). We’ve seen posts reporting that the old password was still valid way past the 72-hour window.

Recently, Apple introduced a feature that allows users to fully reset their device directly from the lock screen if they confirm that they have forgotten their passcode. Surprisingly, this option was missing for a long time, forcing users to either enter 10 incorrect passcodes to trigger a state where the device required a computer connection for further instructions, or search online for DFU mode reset guides. This uncertainty around the reset process led to the rise of cheap third-party solutions marketed as tools to bypass forgotten passcodes. In reality, these tools simply performed a reset (which, of course, can be done for free), resulting in complete data loss while leaving the device still tied to its original Apple ID.

## The Advantage of Longer Lockout Delays

The new delay scheme (where lockouts extend to 3 and 8 hours after multiple failed attempts) has one major benefit: it reduces the likelihood of accidental lockouts if one’s phone gets into the hands of a curious child. Previously, 2.5 hours of button-mashing could permanently disable a device. Now, a child is unlikely to have access to the device for the full 12-hour lockout period needed to trigger a full wipe.

## Screen Time Passcodes

Speaking of child protection, Screen Time passcodes (used for parental controls and restrictions) have their own delay system. Delays are progressively increased, reaching 1 hour after the 11th attempt, yet without data deletion. We decided to test the current state of Screen Time passcodes. On iOS 18, the delays are:

- First 5 attempts: No delay
- 6th attempt: 1-minute delay
- 7th attempt: 5-minute delay
- 8th attempt: 15-minute delay
- 9th attempt: 1-hour delay

We didn’t test beyond this point. Also, the Screen Time passcode format remains restricted to only 4 digits, as it always has.

## Passcodes: Myths, Facts, and Surprising Details

While PIN codes and passcodes might not seem like the most exciting topic, there are still some interesting details worth exploring. Beyond the basics, we’ve uncovered a few facts and debunked some myths about how passcode security actually works. From misconceptions about brute-force protection to myths about bypassing lockout timers, there’s more to iOS passcode security than meets the eye.

### Do Data Actually Get Deleted After 10-11 Failed Attempts?

The answer is both yes and no, or, rather, “it depends”.

- On 32-bit devices, data is not erased. In fact, on iOS 7 and earlier, passcode-free extraction is possible.
- On newer devices, the data itself is not physically wiped; only the encryption keys in metadata are deleted. This still renders the data inaccessible as pretty much everything on the iPhone is encrypted.
- On some 64-bit devices (up to iPhone 7) running iOS 12 or earlier, it was still possible to brute-force the passcode and recover data even after the device was in a “disabled” state.

### Getting an Extra Attempt by Updating iOS: Myth

A common rumor suggests that if a device is disabled after too many failed attempts, updating iOS (e.g., via 3uTools) grants one extra passcode attempt. Our testing found no evidence to support this. Occasionally, an extra attempt may appear, but it is kina “fake” as even the correct passcode will not work.

### Do These Delays Affect Forensic Brute Force Attempts?

No, the delays only apply to on-device passcode entry through the UI. For forensic tools that can brute-force passcodes, these delays have no impact. Brute-force speeds vary:

- On 32-bit devices, rates range from 5-20 attempts per second.
- On newer devices, speeds drop significantly. For example, a test reported on LinkedIn showed that an iPhone SE 2020 (A13 chip, iOS 15.6.1) was limited to about 2 attempts per minute; 595,000 passcodes took 192 days to process.

### Is There Any Passcode Brute-Force Solution for iPhone 12 and Later?

As of now, there is no known way to brute-force passcodes on iPhones with the A14 chip or newer (iPhone 12 and later). This is likely due to the transition to a new Secure Enclave generation. More details on Secure Enclave changes can be found in Apple’s documentation: Secure Enclave feature summary.

### Apple’s Theoretical Brute-Force Speeds vs. Reality

Apple’s official documentation (source) claims:

> “The iteration count is calibrated so that one attempt takes approximately 80 milliseconds. In fact, it would take more than five and a half years to try all combinations of a six-character alphanumeric passcode with lowercase letters and numbers.”

In reality, these numbers don’t match observed brute-force speeds. Even iPhone 7 does not reach 12 attempts per second. Additionally, Apple’s calculation assumes a 6-character lowercase alphanumeric passcode, which is not a typical choice. If the user has a simple 6-digit numeric passcode, brute-force at this speed would take significantly less time, which is not the case in real life.

### Repeated Identical Passcodes Are Not Counted

If the same passcode is entered multiple times in a row, iOS counts them as a single attempt rather than incrementing the failure counter.

## Conclusion

The evolution of iOS passcode security reflects Apple’s approach to protecting access to their devices with progressively increasing delays between unsuccessful unlock attempts. From the initial implementation in iOS 10 and 11 to the current implementation, the system is designed to deter unauthorized access to user’s data.

Go to Source
