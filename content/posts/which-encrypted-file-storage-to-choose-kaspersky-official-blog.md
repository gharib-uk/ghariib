---
title: "Which encrypted file storage to choose? | Kaspersky official blog"
date: 2025-01-03
categories: 
  - "cybersecurity"
  - "security"
---

Attacks on encrypted file storage: how to choose a safe alternative to Dropbox or OneDrive

No one can deny the convenience of cloud file-storage services like Dropbox or OneDrive. The one drawback is that cybercriminals, intelligence agencies, or the hosting provider itself can view your cloud-based files without authorization. But there’s a more secure alternative: encrypted cloud file-storage. Some call it end-to-end encryption (E2EE) — similar to Signal and WhatsApp. According to the marketing blurb, files are encrypted on your device and sent to the cloud already in secure form — the encryption key remaining in your possession and no one else’s. Not even the provider can sniff this information. But is that really the case?

## Swiss-cheese encryption

The Applied Cryptography Group at ETH Zurich took apart the algorithms of five popular encrypted storage services: Sync.com, pCloud, Icedrive, Seafile, and Tresorit. In each of them, the researchers found errors in the implementation of encryption allowing, to varying degrees, file manipulation, and even access to fragments of unencrypted data. Earlier, they’d discovered flaws in two other popular hosting services —  MEGA and Nextcloud.

In all cases, attacks are carried out from a malicious server. The scenario is as follows: the intruders either hack the encrypted hosting servers, or, by manipulating routers along the client-to-server path, force the victim’s computer to connect to another server mimicking the genuine encrypted hosting server. If this tricky maneuver succeeds, the attackers can theoretically:

- In the case of **com**, plant folders and files with incriminating information, and change the file names and metadata of stored information. Also, the hacked server can send new encryption keys to the client, then decrypt any files downloaded afterwards. Plus, the built-in share function allows the malicious server to decrypt any file shared by the victim, since the decryption key is contained in the link that’s sent when the server is accessed.
- In the case of **pCloud**, plant files and folders, arbitrarily move files and swap file names, delete file fragments, and decrypt files downloaded post-hack.
- In the case of **Seafile**, force the client to use an older version of the protocol, making it easier to bruteforce passwords, swap or delete file fragments, plant files and folders, and modify file metadata.
- In the case of **Icedrive**, plant files consisting of fragments of other files already uploaded to the cloud, change the name and location of stored files, and reorder file fragments.
- In the case of **Tresorit**, manipulate the metadata of stored files— including authorship.
- In the case of **Nextcloud**, manipulate encryption keys — allowing decryption of downloaded files.
- In the case of **MEGA**, restore encryption keys and thus decrypt all files. It’s also possible to plant incriminating files.

The malicious server in each case is a hard-to-implement but not blue-sky component of the attack. In light of the cyberattacks on Microsoft and Twilio, the possibility of compromising a major player is real. And of course, E2EE by definition needs to be resistant to malicious server-side actions.

Without going into technical details, we note that the developers of all the services seem to have implemented bona fide E2EE and used recognized, strong algorithms like AES and RSA. But file encryption creates a lot of technical difficulties when it comes to document collaboration and co-authoring. The tasks required to overcome these difficulties and factor in all possible attacks involving modified encryption keys remain unsolved, but Tresorit has done a far better job than anyone else.

The researchers point out that the developers of the various services made very similar errors independently of each other. This means that the implementation of encrypted cloud storage is fraught with non-trivial cryptographic nuances. What’s needed is a well-developed protocol thoroughly tested by the cryptographic community — such as TLS for websites or the Signal Protocol for instant messengers.

## Costly fixes

The biggest problem with fixing the identified bugs is that not only do the applications and server software need updating, but also, in many cases, user-saved files need re-encrypting. Not every hosting provider can afford these huge computational outlays. What’s more, re-encryption is only possible in cooperation with each user — not unilaterally. Which is probably why fixes are slow in coming:

- **com** responded to the researchers after six months, and only after the appearance of press reports. Having finally woken up, they announced a fix for the problem of key leakage when sharing links, and said they’d to patch the other flaws as well — but without giving a time frame.
- **Tresorit** promised to fix the issue in 2025 (but the problem is less acute for them).
- **Seafile** fixed the issue of protocol version downgrade without commenting on the other flaws.
- **Icedrive** decided not to address the identified issues.
- **pCloud** didn’t respond to the researchers until the appearance of press reports, then announced that the attacks are theoretical and don’t require immediate action.
- **Nextcloud** fixed the issue and majorly reworked the overall approach to E2EE in version 3.12. The updated encryption scheme has yet to be researched.
- **MEGA** significantly lowered the likelihood of an attack by introducing client-side checks.

## What users need to do

Although the issues identified by the Applied Cryptography Group cannot be called purely theoretical, they do not represent a mass threat readily exploitable by cybercriminals. Therefore, hasty action isn’t required; rather — a sober assessment of your situation is needed:

- How sensitive is the data in your storage, and how tempting is it to outsiders?
- How much data do you store in the encrypted service, and is it easy to move to another?
- How important are the collaboration and file-sharing features?

If collaboration isn’t important, while the data stored is critical, the best option is to switch to local file encryption. You can do this in a variety of ways — for example, by storing data in an encrypted container file or an archive with a strong password. If you need to transfer data to another device, you can upload an already encrypted archive to the cloud hosting service.

If you want to combine collaboration and convenience with proper security guarantees, and the amount of stored data isn’t that great, it’s worth moving the data to one of the services that better withstood ETH Zurich’s testing. That means Tresorit first and foremost, but don’t discount MEGA and Nextcloud.

If none of these solutions fits the bill, you can opt for other encrypted hosting services, but with additional precautions: avoid storing highly sensitive data, promptly update client applications, regularly check your cloud drives, and delete outdated or extraneous information.

In any case, remember that the most likely attack on your data will take the shape of an infostealer simply compromising your computer or smartphone. Therefore, encrypted hosting must go hand in hand with full anti-malware protection for all smartphones and computers.

Go to Source
