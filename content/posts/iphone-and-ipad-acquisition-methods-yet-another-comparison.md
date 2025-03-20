---
title: "iPhone and iPad Acquisition Methods: Yet Another Comparison"
date: 2025-01-15
categories: 
  - "apple"
  - "mobile"
tags: 
  - "ces"
  - "hardware"
  - "ios"
  - "report"
---

![](https://blog.elcomsoft.com/wp-content/uploads/2025/01/extraction-methods-2_1200x630.png)

Welcome to the world of mobile forensics, where extracting data is the first (and arguably the most critical) step. Whether you’re working with an ancient Apple device or attempting to break into the latest iPhone 16 Pro Max, there is a method for every gadget – each with its own share of challenges. We love explaining the differences between the extraction techniques, detailing their pros and contras, but sometimes you are limited to the one and only method that is the most likely to succeed.

## A word on consent extraction

Consent extraction simply refers to any means of accessing data an iPhone where the device’s passcode is known. With the passcode in hand, the investigator gains access to the device, bypassing many security hurdles. This allows for the use of a wide range of extraction techniques, such as bootloader exploits or agent-based acquisition. For all modern iPhones and iPads consent extraction is the only way supported by our tools, yet in certain cases some very limited amount of information can be obtained from the device even if the passcode is not known.

Date extraction is just the first and the most challenging step in mobile forensics. There are several extraction methods for Apple devices, and we are about to give a detailed comparison between them.

## **Bootloader exploit**

While this extraction method is our favorite for being the safest and most reliable, is it now showing its age. The latest iPhone fully supported by this method is the eight generations old iPhone 7 range, while even the formally supported iPhone 8, 8 Plus, and iPhone X can be only extracted if they are running an old version of iOS (particularly, iOS 11-13 are fully supported, iOS 14-15 require a passcode reset prior to extraction, while iOS 16 extractions are practically unavailable unless the user never set up a passcode in the iPhone). Always use this method when compatible.

**What**

- Low-level non-invasive extraction method
- Exploits checkm8 and other similar vulnerabilities in i-device bootloaders
- Supports multiple generations of Apple hardware

**Pros**

- Forensically sound with consistent, reproduceable results
- BFU extraction possible, extracting limited information if the passcode is unknown
- Does not tamper the device in any way
- Full file system and keychain are acquired
- Supports Apple TV (up to the original 4K model), Apple Watch (up to S3) and the original HomePod

**Cons**

- Works for legacy devices only
- Requires a macOS or Linux computer to run

In addition, some very ancient iPhones based on the A6 SoC support full passcode unlock. We are currently working on passcode unlock for Apple A7 through A10 based devices.

## **Agent acquisition**

This extraction method allows accessing the same scope of data as the bootlooder exploit, yet it works for much newer devices than that. The range of supported devices starts with the iPhone 8/X, which may or may not support bootloader-based acquisition depending on the version of iOS they run, and all the way up to the iPhone 15 Pro Max. Compatibility depends on the version of iOS installed on the device being investigated as agent-based extraction utilizes vulnerabilities in the OS kernel to obtain the required level of privileges and access the data on the device.

**What**

- Low-level extraction method; minimally invasive
- Exploits various chains of vulnerabilities in the iOS or iPadOS
- Supports iPhone and iPad devices

**Pros**

- Works with recent device models
- Full file system and keychain are acquired

**Cons**

- Limited iOS version support
- Sideloading the extraction agent requires additional safeguards
- May not work with some managed devices

This extraction method is not bulletproof as there are things that may cause the extraction agent to fail even if the device is fully compatible. That could be MDM or specific ScreenTime settings, for example. Still, this method is great, and we strongly recommend it.

## **Advanced logical acquisition**

If the device is not compatible with either low-level method, you can still try logical extraction.

**What**

- High-level extraction method that can only access permissible data via official Apple protocols

**Pros**

- Widely compatible, simple and risk-free when applied properly
- Also works with Apple TV and Apple Watch (though gets less data)
- Fully compatible with all device models and OS versions

**Cons**

- Keychain extraction is only partial
- Cannot get important system data
- Cannot get data from many applications
- Only extracts data that is explicitly permitted (by app developers and Apple)
- Various hurdles to overcome (such as backup password or “Stolen Device Protection” that requires users’ biometrics in addition to passcode)

## **Manual inspection**

While logical extraction is widely compatible, certain situations may require a different type of analysis. Here comes manual inspection.

**What**

- Access individual files and transfer them out by sharing via AirDrop or cloud services
- Screenshots, photos or video recordings of the device being inspected

**Pros**

- Quick, simple
- Compatible with all devices

**Cons**

- The opposite of forensically sound
- Invasive, makes numerous changes on the device
- Questionable admissibility of evidence collected during manual inspection
- Questionable admissibility of any evidence extracted from the device at a later point
- Very limited amount of data is available

## **Cloud acquisition**

This method works great both standalone and in combination with other methods, often allowing to fill the gaps. Cloud extraction is the only method that can be used without the device itself, though the list of requirements is long enough.

**What**

- Downloading data from various cloud services (in case of Apple devices, from iCloud)

**Pros**

- No need for the physical device
- Extracts everything from the user’s cloud account, including data from other devices

**Cons**

- Need full authentication credentials
- End-to-end encryption requires a passcode/password from a trusted device
- iCloud data is not always available (e.g. insufficient space, disabled sync, disabled cloud backups and so on)
- iCloud may contain very old data
- Potential legal issues

## **Summary**

Below is a quick summary of the methods reviewed above:

|  | **SoC** | **iOS** | **Data** |
| --- | --- | --- | --- |
| **Bootloader exploit** | A4-A11 | ALL | FFS, keychain |
| **Acquisition agent** | A11-A17 | 12.0 – 16.6.1 | FFS, keychain |
| **Extended logical** | ALL | ALL | backup, logs, media |
| **Manual** | ALL | ALL | limited |
| **Cloud** | ALL | ALL | backups, synced |

To summarize, here are some tips:

- **Know your options.** Start by figuring out which extraction methods are compatible with the device you’re working on – even if only theoretically.
- **Understand the trade-offs.** Each method has its quirks, limitations, and potential impacts on the device, so keep those in mind.
- **Aim for Full File System extraction.** If possible, this should always be your go-to for the most complete data retrieval.
- **Handle with care.** Use any method carefully to avoid damaging the device, altering its data or otherwise affecting admissibility.

Of course, you’ll also need the right tools. Grab Elcomsoft iOS Forensic Toolkit for pulling data from the device itself and Elcomsoft Phone Breaker for iCloud extraction.

And remember, extraction is just the beginning. Once you’ve got the data, you’ll need to parse it, decrypt it (if needed), analyze it, and finally compile it into a report. To make sure everything is solid, don’t rely on a single piece of software; cross-checking with multiple tools is always a smart move. The different tools support different types of data, including system artefacts and application-specific data across various versions; universal solutions simply do not exist. Besides, even the most reliable tools can have bugs, which are especially prominent in new versions, where issues often arise.

Finally, don’t forget abou the other devices that are often overlooked: iPods, Apple Watches, Apple TVs, and HomePods. These gadgets may contain unique data unavailable from other sources or inaccessible due to screen lock passcodes. Furthermore, such devices can sometimes help circumvent security on newer devices: for example, a screen lock passcode on an older device might also work to unlock a newer one. This is why analyzing older devices is still valuable and in fact a very common practice.

Go to Source
