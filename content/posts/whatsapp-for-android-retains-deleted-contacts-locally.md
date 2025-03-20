---
title: "WhatsApp for Android Retains Deleted Contacts Locally"
date: 2025-01-06
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "security"
  - "security-awareness"
tags: 
  - "advisories"
  - "facebook"
  - "whatsapp"
---

## Summary

WhatApp for Android retains contact info locally after contacts get deleted. This would allow an attacker with physical access to the device to check if the WhatsApp user had interactions with specific contacts, even though they have been deleted.

## Vulnerability Details

When a contact is deleted on WhatsApp, their information about security code changes is retained (while the chat content is not). The only way to get rid of that is to select “Clear Chat” for the contact before deleting it. Even deleting the chat itself doesn’t do it unless the “Clear Chat” operation is done first. The “security code change notifications” option must be enabled in order for this to work.  
  
Someone getting access to the user’s device can figure out whether they ever chatted with specific contacts, even if those contacts and their chats are no longer on the device. This is a privacy issue – especially for people like journalists and those living in dangerous countries.

Since WhatsApp uses Android’s contact app for contact information but supports chats with numbers that aren’t contacts, our theory is that the application retains information about security code changes even for contacts no longer on the device. There seems to be a discrepancy between how the “Clear chat” option and “Delete Chat” options are implemented in the application, with the first option deleting security notification data.

To reproduce:

1. Delete a chat with a contact that had security code changes before.
2. Delete the contact from the device via the Android Contacts app.
3. Re-add contact to the device via the Android Contacts app.
4. Start a new chat in WhatsApp with that contact but do not send any messages.
5. Observe that security code changes are listed with dates in the chat.
6. Select “Clear Chat” to remove the security code changes, and repeat sterps 1-4. Observe that the security code changes no longer appear.

![](https://wwws.nightwatchcybersecurity.com/wp-content/uploads/2021/12/screen-shot-2021-12-30-at-4.13.16-pm.png?w=612)

Tested on WhatsApp for Android, app version 2.21.20.20, running on Android 12.

## Vendor Response

We haven’t retested on a more recent version but our recommendation to users is to use the “Clear Chat” option in order to prevent this.

The vendor will not be fixing this issue, here is their response:

> _As part of the attack scenario you describe getting access to a person’s WhatsApp account to obtain private data, as you mention yourself, people do have a way to remove these messages from their account, if a bad actor gets access to their WhatsApp account prior to that person deleting that information then they will be able to view this information. As such, we are closing this report._

## References

CWE: CWE-212 – Improper Removal of Sensitive Information Before Storage or Transfer

Facebook # 10102482597361835

## Timeline

2021-10-24: Initial report sent to the vendor, report ID assigned  
2021-10-27: Vendor asks for more info, additional info and screenshots sent  
2021-11-03: Vendor sent interim status report, still investigating  
2021-11-09: Vendor rejects the vulnerability and closes the report  
2021-12-30: Public disclosure  

Go to Source
