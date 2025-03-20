---
title: "How to remove metadata from photos, videos, and other files, and why do it at all | Kaspersky official blog"
date: 2025-01-25
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

Metadata can reveal information that should remain confidential. We explain how to remove it.

If you’re anything like me, you probably share plenty of photos, videos and documents, and send lots of voice messages and emails every single day too. But how often do you stop to consider the additional data contained in these files? For each of these files/media contains metadata — which can reveal a lot of interesting details not meant for prying eyes; for example, a photo’s time and location, a document’s editing history, device information, IP address, geolocation, and much more. So, for example, whenever you post an innocent selfie on social media, you’re also making public a whole ton of extra information that you might not necessarily want others to see.

In this article, we explore the pros and cons of metadata and how to remove it.

## What is metadata and what’s it for?

To put it simply, metadata is additional information about a file’s content. Such data is added to files by applications that create or process them, operating systems, or users themselves. In most cases, metadata is created and updated automatically. For example, for files, this can include the creation date, last modified date, type, owner, and so on. In the case of photos, metadata can include the date and location, exposure settings, camera or smartphone model, and so on, recorded in Exif format. Specifically which data is stored depends on the camera/smartphone model and settings.

Some metadata is “visible” and easy to edit. For example, audio files contain special tags describing the content — author, artist, album, track name, genre, etc. — that can be easily changed in any media player.

Other metadata is less evident. Did you know, for example, that from the metadata of an office document you can easily discover who edited it, when, for how long, and using which programs? In some cases, you can even restore the entire edit history from the first keystroke.

Of course, metadata wasn’t originally designed to be “the perfect stalking tool”, but simply a useful feature. However, you can end up sharing more than you intended; for example, your employer or client could find out how much time you actually spent working on a document, and the Exif data of a selfie you post online can reveal what smartphone you use and where you were at the time. Metadata can also help catch criminals or uncover fraudulent schemes.

For example, in 2019, U.S. law enforcement managed to arrest the fraudster Hicham Kabbaj, who’d been sending his former employer invoices for equipment supplies from a shell company called Interactive Systems for four years. Of course, no equipment was actually supplied, but a total of six million dollars was transferred into Interactive System’s accounts. The fraudster was eventually caught out because of simple oversight: four of the 52 invoices were in the MS Word _.doc_ format, and the metadata listed the author as KABBAJ.

Besides the police, malicious actors can also use metadata. In 2016, we conducted an experiment to try to determine a person’s location from a single photo. For us, this was just a fun exercise, but criminals could have very different motives.

Or consider a slightly more complex scenario: your innocent PDF file somehow ends up in the hands of a malicious actor. How it got there doesn’t matter — let’s say they introduced themselves as your colleague. In this case, the contents of the file may be of no interest to the criminal. What’s important to them, however, is that you’ve already taken the bait (so the attack can continue) and leaked the PDF’s metadata — revealing the software and version you used to create it. With this knowledge, the attacker can send you malware specifically designed to exploit a vulnerability in your particular system. Protecting yourself from this kind of scenario requires a combination of measures: ignoring suspicious messages, removing metadata, and updating your software promptly.

## How to remove metadata

You can remove metadata using built-in tools or third-party programs and services. We recommend the former, as then your metadata won’t end up in the hands of third parties this way. Third-party tools act as an extra layer between you and the “cleaned” file. This layer could potentially retain metadata, which criminals could somehow get hold of.

So now let’s look at how to remove metadata from photos and videos, and DOC and PDF files using built-in tools.

### Photos and videos

#### On Windows

In **File Explorer**, right-click on the file, select **Properties**, and go to the **Details** tab. At the bottom of the screen, click **Remove Properties and Personal Information**, and in the window that opens, either keep the default option **Create a copy with all possible properties removed**, or manually select the properties you want to remove, and click **OK**.

#### On macOS and iOS.

Apple operating systems let you remove or modify the date, time, and geolocation. However, location data is only recorded for photos and videos taken with geolocation services enabled.

To remove or modify metadata on a macOS device, open the **Photos** app, go to the **Image** menu, select **Location**, and click **Hide Location**. Here you can also **Revert to Original Location** — which raises the question of where this data is actually stored — or **Assign Location** to one or more photos after you **Copy Location** from another photo. Additionally, in the **Image** menu, you can **Adjust Date and Time** of the capture.

On an iPhone or iPad, open the **Photos** app, select the photo to edit, and tap the ⓘ info button, or simply swipe up on the photo. Here, you can **Adjust** the date, time, and location. For location, you can either select **No Location** or assign any other location to the photo. (This is useful if you’re posting photos taken in a studio near your home, while pretending to be in, say, Maldives.) To edit multiple photos at once, select them all, tap the three-dot button **(…)**, then choose **Adjust Date & Time** or **Adjust Location**.

#### On Android

On Android devices, you can remove or modify location data using the **Google Photos** app. Select the photo or video, tap the three-dot **More** icon, select **Edit**, and tap **Remove location**.

### DOC files

If you’re using Word, go to the **File** tab and select **Info**. Then click **Check for Issues**, followed by **Inspect Document** and **Inspect**. Under **Document Properties and Personal Information**, click **Remove All**.

Windows users can also remove DOC file metadata using **File Explorer**, just as they would with photos and videos.

### PDF files

If you’re using Adobe Acrobat, go to **File**, then **Document properties**, and select **Description**. In the window that opens, you can manually edit the author, subject, keywords, and title of the document. Clicking **Additional Metadata** opens a window displaying all the document’s metadata.

You can also remove PDF metadata using **File Explorer** in the same way as for photos and videos.

## Security Measures

So, what’s the main way to protect yourself from malicious actors exploiting your metadata? Two words: exercising caution. In addition, for maximum security, follow these extra precautions:

- **Set your social media profiles to private.** This way, attackers won’t be able to use the metadata from your old photos and videos.
- **Use a comprehensive security solution.** It will act as a safety net — protecting your payment and personal data even if you fall victim to a cybercriminal.
- **Remove metadata regularly.** At first, this may seem like a lot of extra work just to send a simple selfie, but over time, removing metadata will become second nature.

Go to Source
