---
title: "iOS 15 Image Forensics Analysis and Tools Comparison  - Communication and Social Networking Apps"
date: 2025-03-19
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "forensics"
  - "security"
  - "security-awareness"
  - "security-awareness-3-0"
tags: 
  - "work"
---

The fourth episode is dedicated to the most analyzed family of applications: communication and social networking apps.

Before I start, I would like to mention that I have made some corrections to the previous blog post, based on feedback by tool developers. Also, most of them have confirmed to me that they are working on improving their parsing capabilities, based on this blog series. I am happy to support them in that direction!

There are 29 communication and social networking applications available in Josh Hickman’s acquisition. 

The apps are listed below, in alphabetical order.

1. BeReal
2. Burner
3. Clubhouse
4. Discord
5. Facebook Messenger
6. Google Chat
7. Google Meet
8. Google Voice
9. GroupMe
10. imoHD
11. Instagram
12. Kik Messenger
13. Line
14. Mastodon
15. MeWe
16. Session
17. Signal
18. Silent Phone
19. Skype
20. Snapchat
21. Telegram
22. Threema
23. TikTok
24. Truth Social
25. Twitter
26. Viber
27. WhatsApp
28. Wickr Pro
29. Wire

Before getting into the details of every single apps, it’s important to highlight that once an app is installed on an iOS device, app data is stored in a subfolder in the /private/var/mobile/Containers/Data/Application/ folder. Some apps can also store data in a subfolder in the /private/var/mobile/Containers/Shared/AppGroup. When analyzing an application, it’s important to check and eventually investigate both areas.  

APOLLO has no parsers for third-party apps and ArtEx only has a parser for Discord, which did not run correctly on the test image, and a parser for Snapchat.

Each section for each app is structured similarly: first, a list of the most important files and folders is provided, then a description of the tools parsing capabilities for that app, and finally two different tables. The first table provides the details for each action documented by Josh in the iOS 15 image documentation, and the second table provides a summary of the tools’ parsing capabilities based on the test results.

As usual, YES means that the value was parsed and parsed correctly, NO means that the value was not parsed, YELLOW means that it was partially parsed, and BLUE means that the value was found elsewhere (i.e., iOS Notification or iOS Cache).

## BeReal

Relevant information for this application is stored in these files and folders:

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/disk-bereal-RelationshipsContactsManager-contact

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/disk-bereal-RelationshipsFriendsListManager

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/disk-bereal-UploadPostWorker-post-upload

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/disk-bereal-FeedMyPostManager-subject-series

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/AlexisBarreyat.BeReal/fsCachedData

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/AlexisBarreyat.BeReal/Cache.db

**Direct parsing of this application is only supported by OXYGEN**, but it only includes account information, contacts, posts, and cache. AXIOM, PA, XAMN, and iLEAPP were able to parse a comment to a post from iOS notifications. BeReal parsing was introduced by PA in version 7.64.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   05/05/2023   |   12:10   |   16:10   |   Set profile picture   |   HAL   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   17/05/2023   |   08:27   |   12:27   |   Took & Posted BeReal   |   Starbucks/car selfie   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   18/05/2023   |   15:02   |   19:02   |   Took & Posted BeReal   |   Car doors   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   18/05/2023   |   15:03   |   19:03   |   Left comment on other BeReal   |   Nice machine!   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   18/05/2023   |   15:07   |   19:07   |   Other party commented on BeReal   |   (BeReal posted at 15:02) Car doors???   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   20/05/2023   |   13:43   |   17:43   |   Took BeReal & discarded it   |    |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   20/05/2023   |   13:44   |   17:44   |   Took BeReal   |   Trees & playground   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   20/05/2023   |   13:45   |   17:45   |   Sent BeReal   |   (Taken at 13:44)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Contacts   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Posts   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Cache   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |

## Burner

Relevant information for this application is stored in these files and folders:

- /private/var/mobile/Containers/Shared/AppGroup/<APP\_GUID>/Phoenix.sqlite
- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/com.adhoclabs.burner/fsCachedData/
- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/com.adhoclabs.burner/Cache.db

**Direct parsing of this application is supported by AXIOM and OXYGEN**. PA, XAMN, and iLEAPP were able to parse some incoming messages from iOS notifications. Messages and contacts are parsed correctly by both AXIOM and OXYGEN. Audio calls are partially parsed by both tools, as they were not able to determine the call length.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   08/05/2023   |   13:23   |   17:23   |   Obtained phone number   |    |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   08/05/2023   |   13:25   |   17:25   |   Sent message   |   You there?   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   08/05/2023   |   13:27   |   17:27   |   Received message   |   I am. What number is this?   |   YES   |   Notifications   |   YES   |   Notifications   |   NO   |   Notifications   |
|   08/05/2023   |   13:29   |   17:29   |   Sent message   |   Burner. What are you doing?   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   08/05/2023   |   13:33   |   17:33   |   Received message   |   Ah, ok. Nothing in particular. Same as you.   |   YES   |   Notifications   |   YES   |   Notifications   |   NO   |   Notifications   |
|   08/05/2023   |   13:37   |   17:37   |   Sent message   |   Catching up on some DS9. I can’t believe I never watched that when it was on-air   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   08/05/2023   |   13:46   |   17:46   |   Received message   |   It’s a good show. Helps in understanding Picard   |   YES   |   Notifications   |   YES   |   Notifications   |   NO   |   Notifications   |
|   08/05/2023   |   13:48   |   17:48   |   Sent message   |   That’s what I have heard. Definitely an underrated show.   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   08/05/2023   |   13:49   |   17:49   |   Received message   |   It is. Hang on and I’ll send you a picture   |   YES   |   Notifications   |   YES   |   Notifications   |   NO   |   Notifications   |
|   08/05/2023   |   13:50   |   17:50   |   Received picture   |   Pirate   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   08/05/2023   |   13:52   |   17:52   |   Sent message   |   Got it. I’ll send one   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   08/05/2023   |   13:53   |   17:53   |   Sent picture   |   An intelligent man   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   08/05/2023   |   13:54   |   17:54   |   Received message   |   Got it. I’ll call you in a minute.   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   08/05/2023   |   13:55   |   17:55   |   Outgoing audio call   |   (~1:30)   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   08/05/2023   |   13:58   |   17:58   |   Sent message   |   Ok. We got that backwards. You can call me now. Lol   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   08/05/2023   |   13:59   |   17:59   |   Incoming audio call   |   (0:13) (iOS call logs shows accepted call, Burner call log shows as missed call)   |   YES   |   Notifications   |   YES   |   Notifications   |   NO   |   Notifications   |
|   08/05/2023   |   14:00   |   18:00   |   Incoming audio call   |   (0:15) (iOS call logs shows accepted call, Burner call log shows as missed call)   |   YES   |   Notifications   |   YES   |   Notifications   |   NO   |   Notifications   |
|   08/05/2023   |   14:01   |   18:01   |   Received voicemail   |   (0:09)   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Contacts   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Messages   |   YES   |   Notifications   |   YES   |   Notifications   |   NO   |   Notifications   |
|   Calls   |   YES   |   Notifications   |   YES   |   Notifications   |   NO   |   Notifications   |
|   Voicemail   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Attachments   |   NO   |   NO   |   Cache   |   NO   |   NO   |   NO   |
|   Cache   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |

## Clubhouse

Relevant information for this application is stored in these files and folders:

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Preferences/co.alphaexploration.clubhouse.plist

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/co.alphaexploration.clubhouse/csCachedData

**Direct parsing of this application is supported by PA and OXYGEN, but messages were not parsed**. AXIOM, PA, XAMN, and iLEAPP were able to parse some incoming messages from iOS notifications.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   05/05/2023   |   12:19   |   16:19   |   Login   |   Login   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   12:21   |   16:21   |   Received message   |   Made it to Clubhouse.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   12:22   |   16:22   |   Sent message   |   Awesome. Welcome back.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   12:23   |   16:23   |   Received message   |   Thanks. This looks rather limited, eh?   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   12:24   |   16:24   |   Sent message   |   It does. WE can try to create a room in a few minutes.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   12:26   |   16:26   |   Received message   |   Sounds good. We can’t even send photos here, can we?   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   12:28   |   16:28   |   Sent message   |   Nope. Basic DMs.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   12:29   |   16:29   |   Received message   |   I’ll create the room. Give me a moment.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   12:30   |   16:30   |   Entered room   |   (~1:30)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Contacts   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Channels / Rooms   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Messages   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   Cache   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |

## Discord

Relevant information for this application is stored in these files and folders:

- /private/var/mobile/Containers/Data/Application/<GUID>/Documents/mmkv/mmkv.default

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/com.hammerandchisel.discord/Cache.db

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/com.adhoclabs.burner/fsCachedData/

**Direct parsing of this application is supported by AXIOM, PA, OXYGEN, XAMN, and iLEAPP**. Some differences were found in parsing capabilities. **The deleted message was not recovered by any of the tested tools**.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   24/04/2023   |   14:09   |   18:09   |   Sent message   |   I know it says “Android Test Images,” but can we use this server?   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   14:10   |   18:10   |   Received message   |   Sure, I don’t see why not. What are you up to?   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   14:12   |   18:12   |   Sent message   |   Not much. Just sitting here generating data. Seems that’s my thing. Lol.   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   14:13   |   18:13   |   Received message   |   You could do worse.   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   14:14   |   18:14   |   Sent message   |   True.   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   14:15   |   18:15   |   Received message   |   How’s the phone?   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   14:16   |   18:16   |   Sent message   |   Still cracked. I still can’t believe I dropped it like that.   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   14:17   |   18:17   |   Received message   |   That was painful to watch.   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   14:20   |   18:20   |   Sent message   |   Here comes a picture   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   14:21   |   18:21   |   Sent picture   |   Emotet   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   14:22   |   18:22   |   Received message   |   Accurate! I’ll send one. Hang on.   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   14:23   |   18:23   |   Received picture   |   Ransomware operator   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:23   |   20:23   |   Sent message   |   DM’s?   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:24   |   20:24   |   Other party reacted   |   Reaction to message sent at 16:23 (thumbs up emoji)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   24/04/2023   |   16:25   |   20:25   |   Received message   |   You here?   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:27   |   20:27   |   Sent message   |   I am. Hang on and I’ll send a picture.   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:28   |   20:28   |   Sent picture   |   Cat   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:29   |   20:29   |   Received message   |   I’ll send one. Give me a minute.   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:31   |   20:31   |   Received picture   |   ChatGPT   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:33   |   20:33   |   Sent message   |   I will audio call you in just a minute.   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:34   |   20:34   |   Outgoing audio call   |   (~1:21)   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:36   |   20:36   |   Incoming audio call   |   (~1:31)   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:38   |   20:38   |   Received message   |   There was your audio call   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:39   |   20:39   |   Sent message   |   I will video call you in a moment   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:40   |   20:40   |   Outgoing video call   |   (~1:30)   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:42   |   20:42   |   Received message   |   I will video call you momentarily   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:43   |   20:43   |   Incoming video call   |   (~1:30)   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:45   |   20:45   |   Received message   |   Please delete this message when you get it.   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:46   |   20:46   |   Sent message   |   I can only delete messages I send   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   24/04/2023   |   16:47   |   20:47   |   Received message   |   Then delete that one.   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   24/04/2023   |   16:48   |   20:48   |   Deleted message   |   (Message sent at 16:46)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   24/04/2023   |   16:49   |   20:49   |   Sent message   |   Deleted.   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |

 The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Contacts   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Messages   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   Deleted message   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Calls   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   Servers   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Cache   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |

## Facebook Messenger

Relevant information for this application is stored in these files:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/Library/Application Support/lightspeed-userDatabases/<User\_ID>.db

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Preferences/com.facebook.Messenger.plist

**Direct parsing of this application is supported by all tested tools**. Some differences were found in parsing capabilities. **The deleted message was recovered from the SQLite database by most of the tools**.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **Type**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   25/04/2023   |   13:47   |   17:47   |   Sent message   |   You here?   |   Normal chat   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   13:48   |   17:48   |   Received message   |   You know it   |   Normal chat   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   25/04/2023   |   13:49   |   17:49   |   Sent message   |   Awesome. Let’s make this quick, shall we? We also have secure chat to do.   |   Normal chat   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   13:51   |   17:51   |   Received message   |   Sounds good to me. Do you even have any pictures to send? Lol.   |   Normal chat   |   YES   |   YES   |   NO   |   YES   |   YES   |   Notifications   |
|   25/04/2023   |   13:52   |   17:52   |   Sent message   |   Not much. I need to download a few.   |   Normal chat   |   YES   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   25/04/2023   |   13:53   |   17:53   |   Received message   |   I figured as much. Imgur is a good app.   |   Normal chat   |   YES   |   YES   |   NO   |   YES   |   YES   |   Notifications   |
|   25/04/2023   |   13:54   |   17:54   |   Sent message   |   I’ll look at it later. Here comes a picture.   |   Normal chat   |   YES   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   25/04/2023   |   13:56   |   17:56   |   Sent picture   |   YouTuber   |   Normal chat   |   NO   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   25/04/2023   |   13:57   |   17:57   |   Received message   |   Nice. I’ll send one. Give me a moment.   |   Normal chat   |   YES   |   YES   |   NO   |   YES   |   YES   |   Notifications   |
|   25/04/2023   |   13:59   |   17:59   |   Received picture   |   Free tacos   |   Normal chat   |   NO   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   25/04/2023   |   14:01   |   18:01   |   Sent message   |   Lol. I will call you in a minute.   |   Normal chat   |   YES   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   25/04/2023   |   14:01   |   18:01   |   Other party liked message   |   (Message sent 14:01 - heart emoji)   |   Normal chat   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   14:02   |   18:02   |   Outgoing audio call   |   Call failed   |   Normal chat   |   YES   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   25/04/2023   |   14:02   |   18:02   |   Incoming audio call   |   (~1:20)   |   Normal chat   |   YES   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   25/04/2023   |   14:04   |   18:04   |   Sent message   |   Ok. I’ll try to call you again. Lol.   |   Normal chat   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   25/04/2023   |   14:05   |   18:05   |   Outgoing audio call   |   (~1:30)   |   Normal chat   |   YES   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   25/04/2023   |   14:07   |   18:07   |   Received message   |   Here comes a video call.   |   Normal chat   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   25/04/2023   |   14:08   |   18:08   |   Incoming video call   |   Call failed   |   Normal chat   |   YES   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   25/04/2023   |   14:08   |   18:08   |   Outgoing video call   |   (~1:30)   |   Normal chat   |   YES   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   25/04/2023   |   14:10   |   18:10   |   Sent message   |   Want to try again?   |   Normal chat   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   25/04/2023   |   14:12   |   18:12   |   Incoming video call   |   (~1:30)   |   Normal chat   |   YES   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   25/04/2023   |   14:14   |   18:14   |   Received message   |   I’m over in secure chat now.   |   Secure chat   |   Notifications   |   YES   |   NO   |   Notifications   |   NO   |   Notifications   |
|   25/04/2023   |   14:15   |   18:15   |   Sent message   |   Same. Send me a picture please.   |   Secure chat   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   14:16   |   18:16   |   Received picture   |   A criminal   |   Secure chat   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   14:18   |   18:18   |   Sent message   |   Nice. I will send you one. Hang on.   |   Secure chat   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   14:19   |   18:19   |   Sent picture   |   Task failed successfully   |   Secure chat   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   14:20   |   18:20   |   Received message   |   I will audio call you   |   Secure chat   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   14:21   |   18:21   |   Liked message   |   (Message received at 14:20 – heart emoji)   |   Secure chat   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   14:22   |   18:22   |   Incoming audio call   |   (~1:40)   |   Secure chat   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   14:24   |   18:24   |   Sent message   |   I will audio call you now.   |   Secure chat   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   14:25   |   18:25   |   Outgoing audio call   |   (~1:35)   |   Secure chat   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   14:28   |   18:28   |   Received message   |   I will video call you after I get back.   |   Secure chat   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   17:04   |   21:04   |   Incoming video call   |   (~1:30)   |   Secure chat   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   17:06   |   21:06   |   Sent message   |   I will video call you in a minute.   |   Secure chat   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   17:07   |   21:07   |   Outgoing video call   |   (~1:30)   |   Secure chat   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   17:09   |   21:09   |   Sent message   |   I will delete this message momentarily.   |   Secure chat   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   17:10   |   21:10   |   Unsent message   |   (message sent at 17:09)   |   Secure chat   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   25/04/2023   |   17:11   |   21:11   |   Received message   |   Please delete this message when you get it.   |   Normal chat   |   YES   |   YES   |   NO   |   YES   |   YES   |   Notifications   |
|   25/04/2023   |   17:12   |   21:12   |   Removed message   |   (message received at 17:11)   |   Normal chat   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   25/04/2023   |   17:13   |   21:13   |   Sent message   |   I removed it.   |   Normal chat   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   25/04/2023   |   11:36   |   15:36   |   Sent static location   |    |   Normal chat   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   17/05/2023   |   11:43   |   15:43   |   Received static location   |    |   Normal chat   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   17/05/2023   |   11:48   |   15:48   |   Started sending dynamic location   |    |   Normal chat   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   17/05/2023   |   11:53   |   15:53   |   Stopped sending dynamic location   |    |   Normal chat   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   17/05/2023   |   11:56   |   15:56   |   Started receiving dynamic location   |    |   Normal chat   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   17/05/2023   |   12:01   |   16:01   |   Stopped sending dynamic location   |    |   Normal chat   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Contacts   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Chat   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Secure Chat   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   Deleted message   |   YES   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   Calls   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   Static Location   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Dynamic Location   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |

## Google Chat

Relevant information for this application is stored in an SQLite database:

- /private/var/mobile/Containers/Data/Application/<GUID>/Documents/user\_accounts/<USER\_ID>/dynamite.db

**Direct parsing of this application is supported by OXYGEN and BELKASOFT**. AXIOM, PA, XAMN, and iLEAPP were able to parse some incoming messages from iOS notifications.  

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   26/04/2023   |   12:28   |   16:28   |   Received message   |   In Google Chat. You here?   |   Notifications   |   Notifications   |   YES   |   Notifications   |   YES   |   Notifications   |
|   26/04/2023   |   12:30   |   16:30   |   Sent message   |   I am. My better is dying. This poor phone   |   NO   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   26/04/2023   |   12:31   |   16:31   |   Sent message   |   My battery is dying\*\*\*   |   NO   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   26/04/2023   |   12:32   |   16:32   |   Received message   |   Lol. I figured that is what you meant.   |   Notifications   |   Notifications   |   YES   |   Notifications   |   YES   |   Notifications   |
|   26/04/2023   |   12:33   |   16:33   |   Sent message   |   Let’s make this quick. Here comes a picture   |   NO   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   26/04/2023   |   12:37   |   16:37   |   Sent picture   |   Your PC   |   NO   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   26/04/2023   |   12:38   |   16:38   |   Other party reacted   |   (ROFL emoji)   |   NO   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   26/04/2023   |   12:39   |   16:39   |   Received message   |   Got it. I’ll send you one   |   Notifications   |   Notifications   |   YES   |   Notifications   |   YES   |   Notifications   |
|   26/04/2023   |   12:40   |   16:40   |   Received picture   |   Team of 1980   |   Notifications   |   Notifications   |   NO   |   Notifications   |   YES   |   Notifications   |
|   26/04/2023   |   12:42   |   16:42   |   Sent message   |   I’ll make a new group. Give me a few minutes   |   NO   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   26/04/2023   |   12:44   |   16:44   |   Created space   |   iOS 15 Test Space / added DFIR Two to space   |   NO   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   26/04/2023   |   12:45   |   16:45   |   Sent message   |   You here?   |   NO   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   26/04/2023   |   12:46   |   16:46   |   Received message   |   I am. How’s the battery doing?   |   Notifications   |   Notifications   |   YES   |   Notifications   |   YES   |   Notifications   |
|   26/04/2023   |   12:47   |   16:47   |   Sent message   |   Not great. (Smiley face emoji)   |   NO   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   26/04/2023   |   12:49   |   16:49   |   Received message   |   No calls on this app?   |   Notifications   |   Notifications   |   YES   |   Notifications   |   YES   |   Notifications   |
|   26/04/2023   |   12:50   |   16:50   |   Sent message   |   Doesn’t appear so. I’ll send a picture. Hang on.   |   NO   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   26/04/2023   |   12:52   |   16:52   |   Sent picture   |   Chickens   |   NO   |   NO   |   NO   |   NO   |   YES   |   NO   |
|   26/04/2023   |   12:53   |   16:53   |   Received message   |   Got it. I’ll send one now   |   Notifications   |   Notifications   |   YES   |   Notifications   |   YES   |   Notifications   |
|   26/04/2023   |   12:54   |   16:54   |   Received picture   |   The shower   |   Notifications   |   Notifications   |   NO   |   Notifications   |   YES   |   Notifications   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   Contacts   |   NO   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   Groups Chat Info   |   NO   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   Messages   |   Notifications   |   Notifications   |   YES   |   Notifications   |   YES   |   Notifications   |
|   Cache   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Message reactions   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |

## Google Meet

Relevant information for this application is stored in these files:

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/DataStore

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Preferences/com.google.Tachyon.plist

**Direct parsing of this application is supported by AXIOM, OXYGEN, XAMN, and iLEAPP**. PA parsed some calls from the native call history. 

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   26/04/2023   |   11:11   |   15:11   |   Outgoing video call   |   (1:20)   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   26/04/2023   |   11:14   |   15:14   |   Incoming video call   |   (1:31)   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   26/04/2023   |   11:17   |   15:17   |   Outgoing audio call   |   (1:30)   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   26/04/2023   |   11:20   |   15:20   |   Incoming audio call   |   (1:35)   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   26/04/2023   |   11:23   |   15:23   |   Received note   |   I am sending you this note.   |   YES   |   NO   |   YES   |   YES   |   NO   |   YES   |
|   26/04/2023   |   11:27   |   15:27   |   Sent note   |   Note received. Here is your note.   |   YES   |   NO   |   YES   |   YES   |   NO   |   YES   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   YES   |   NO   |   YES   |   YES   |   NO   |   NO   |
|   Contacts   |   YES   |   NO   |   YES   |   YES   |   NO   |   YES   |
|   Calls   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Notes   |   YES   |   NO   |   YES   |   YES   |   NO   |   YES   |

## Google Voice

Relevant information for this application is stored in these files:

- /private/var/mobile/Containers/Shared/AppGroup/<APP\_GUID>/voiceClientSettings

- /private/var/mobile/Containers/Shared/AppGroup/<APP\_GUID>/threadingStore.sqlite

**Direct parsing of this application is only supported by OXYGEN**. AXIOM, PA, XAMN, and iLEAPP extracted some incoming messages from iOS notifications, including the deleted one. 

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   05/05/2023   |   13:19   |   17:19   |   Obtained new number   |    |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   14:13   |   18:13   |   Sent message   |   Are you here?   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   05/05/2023   |   14:14   |   18:14   |   Received message   |   I am. What number is this?   |   Notifications   |   Notifications   |   YES   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   14:15   |   18:15   |   Sent message   |   Google Voice. It should show up as Wrightsville Beach.   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   05/05/2023   |   14:16   |   18:16   |   Received message   |   It’s an 839 number. Not sure if that’s WB. Things have changed.   |   Notifications   |   Notifications   |   YES   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   14:17   |   18:17   |   Sent message   |   Good point. Number portability is a pain   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   05/05/2023   |   14:19   |   18:19   |   Received message   |   Not much you can do. Oh well. (Man shrugging emoji)   |   Notifications   |   Notifications   |   YES   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   14:20   |   18:20   |   Sent message   |   You are correct. Want to send me a picture?   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   05/05/2023   |   14:21   |   18:21   |   Received message   |   Sure. Give me a minute to find one.   |   Notifications   |   Notifications   |   YES   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   14:22   |   18:22   |   Received picture   |   I is silent   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   14:24   |   18:24   |   Sent message   |   Lol. That’s sooo true. Hang on and I’ll send one.   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   05/05/2023   |   14:26   |   18:26   |   Sent picture   |   Missed it by that much   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   14:27   |   18:27   |   Received message   |   We can do calls when you get back.   |   Notifications   |   Notifications   |   YES   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   16:09   |   20:09   |   Sent message   |   I’m back. I’ll call you momentarily.   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   05/05/2023   |   16:11   |   20:11   |   Outgoing call   |   (call to 504-302-3074) (1:35)   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   05/05/2023   |   16:14   |   20:14   |   Received message   |   Great. Now I will call you.   |   Notifications   |   Notifications   |   YES   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   16:16   |   20:16   |   Incoming call   |   (0:20)   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   05/05/2023   |   16:18   |   20:18   |   Received message   |   Here is your bad message. Delete it when you get a chance   |   Notifications   |   Notifications   |   Notifications   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   16:19   |   20:19   |   Deleted message   |   (Message received at 16:18)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   16:21   |   20:21   |   Sent message   |   It’s gone. (Smiley face emoji)   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Contacts   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Messages   |   NO   |   NO   |   YES   |   Notifications   |   NO   |   Notifications   |
|   Deleted message   |   Notifications   |   Notifications   |   Notifications   |   Notifications   |   NO   |   Notifications   |
|   Attechments   |   NO   |   NO   |   Photos   |   NO   |   NO   |   NO   |
|   Deleted message   |   Notifications   |   Notifications   |   Notifications   |   Notifications   |   NO   |   Notifications   |
|   Cache   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

## GroupMe

Relevant information for this application is stored in these files:

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Preferences/com.groupme.iphone-app.plist

- /private/var/mobile/Containers/Data/Application/<GUID>/Documents/GroupMe.sqlite

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/com.groupme.iphone-app/Cache.db

**Direct parsing of this application is supported by AXIOM, PA, OXYGEN, and XAMN**. iLEAPP parsed some incoming messages from iOS notifications.  BELKASOFT officially supports this app, but it only parsed older messages and not messages sent/received in 2023. **The deleted message was not recovered by any of the tested tools**.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   26/04/2023   |   11:42   |   15:42   |   Sent message   |   You here?   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   26/04/2023   |   11:44   |   15:44   |   Received message   |   I totally am! What’s up?!   |   YES   |   YES   |   YES   |   YES   |   NO   |   Notifications   |
|   26/04/2023   |   11:48   |   15:48   |   Sent message   |   Nothing much. No new TV shows this week, unfortunately   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   26/04/2023   |   11:49   |   15:49   |   Received message   |   Yeah. It’s getting into the slow season. Strange New Worlds is coming, though.   |   YES   |   YES   |   YES   |   YES   |   NO   |   Notifications   |
|   26/04/2023   |   11:50   |   15:50   |   Sent message   |   In what, June?   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   26/04/2023   |   11:51   |   15:51   |   Received message   |   June 15th. It will be here before you know it   |   YES   |   YES   |   YES   |   YES   |   NO   |   Notifications   |
|   26/04/2023   |   11:52   |   15:52   |   Sent message   |   True. Here comes a picture.   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   26/04/2023   |   11:53   |   15:53   |   Sent picture   |   Storm Trooper selfie   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   26/04/2023   |   11:54   |   15:54   |   Other party liked picture   |   (Picture sent at 11:53)   |   NO   |   NO   |   YES   |   NO   |   NO   |   Notifications   |
|   26/04/2023   |   11:55   |   15:55   |   Received message   |   Nice. I’ll send one. Just a moment   |   YES   |   YES   |   YES   |   YES   |   NO   |   Notifications   |
|   26/04/2023   |   11:57   |   15:57   |   Received picture   |   Is this your card?   |   YES   |   YES   |   YES   |   YES   |   NO   |   Notifications   |
|   26/04/2023   |   11:58   |   15:58   |   Sent message   |   Move over to DM?   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   26/04/2023   |   11:59   |   15:59   |   Received message   |   Sure.   |   YES   |   YES   |   YES   |   YES   |   NO   |   Notifications   |
|   26/04/2023   |   12:00   |   16:00   |   Received message   |   Here. It looks like I can only delete messages I sent.   |   YES   |   YES   |   YES   |   YES   |   NO   |   Notifications   |
|   26/04/2023   |   12:01   |   16:01   |   Sent message   |   Same for me. I can hide your messages   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   26/04/2023   |   12:02   |   16:02   |   Received message   |   Hide this message.   |   YES   |   YES   |   YES   |   YES   |   NO   |   Notifications   |
|   26/04/2023   |   12:03   |   16:03   |   Hid message   |   (Message received at 12:02)   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |
|   26/04/2023   |   12:04   |   16:04   |   Sent message   |   Done   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   26/04/2023   |   12:05   |   16:05   |   Received message   |   I’m sending you a picture   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |
|   26/04/2023   |   12:06   |   16:06   |   Received picture   |   Soundcloud   |   YES   |   YES   |   YES   |   YES   |   NO   |   Notifications   |
|   26/04/2023   |   12:08   |   16:08   |   Liked picture   |   (Picture received at 12:06)   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   26/04/2023   |   12:09   |   16:09   |   Sent message   |   Nice. I’ll send one. Standby.   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   26/04/2023   |   12:12   |   16:12   |   Sent picture   |   New car   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   26/04/2023   |   12:16   |   16:16   |   Sent message   |   Got it. I am going to delete this message.   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   26/04/2023   |   12:17   |   16:17   |   Delted message   |   (Message sent at 12:16)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   26/04/2023   |   12:18   |   16:18   |   Sent message   |   I am going to delete this message   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   26/04/2023   |   12:19   |   16:19   |   Delted message   |   (Message sent at 12:18)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   17/05/2023   |   08:48   |   12:48   |   Sent static location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   17/05/2023   |   08:52   |   12:52   |   Received static location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Contacts   |   YES   |   YES   |   YES   |   NO   |   YES   |   YES   |
|   Groups   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Messages   |   YES   |   YES   |   YES   |   YES   |   NO   |   Notifications   |
|   Hidden Messages   |   YES   |   YES   |   YES   |   YES   |   NO   |   Notifications   |
|   Deleted Messages   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Message Likes   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Attachments   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Static location   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |

## imoHD

Relevant information for this application is stored in an SQLite database:

- /private/var/mobile/Containers/Data/Application/<GUID>/Documents/IMODb2.sqlite

**Direct parsing of this application is supported by AXIOM, BELKASOFT, and iLEAPP**. PA and XAMN parsed some incoming messages from iOS notifications.  PA supports imo but not imoHD, support for imoHD is coming. XAMN supports imoHD in version 7.8. **BELKASOFT was the only tool able to recover the deleted message from the database**. 

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiDAES9-1ldiA3ADDf8inODfh71LrGlCM1KEr_I6V51emq8Lwn_Oi1gTf82nA8vpG70OCxlQqvKoF1Wh5BPx35nUhkyLKfYxXgyOJuvv_bj9Ron6gDdhBTnGIcwJdc7bAYOdbg0JI8HwzO5a_AjzpPrT5j-xAQN6V2Dmy4UHU2P-wCNlaZLilrtkR1BHXg/w640-h307/imohd_belkasoft.jpg)

  

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   26/04/2023   |   14:11   |   18:11   |   Received message   |   Are you on imo?   |   YES   |   Notifications   |   NO   |   Notifications   |   YES   |   YES   |
|   26/04/2023   |   14:12   |   18:12   |   Sent message   |   I am. I cannot find my HAL avatar   |   YES   |   NO   |   NO   |   NO   |   YES   |   YES   |
|   26/04/2023   |   14:14   |   18:14   |   Received message   |   I have it. I can send it to you.   |   YES   |   Notifications   |   NO   |   Notifications   |   YES   |   YES   |
|   26/04/2023   |   14:15   |   18:15   |   Sent message   |   Please and thank you.   |   YES   |   NO   |   NO   |   NO   |   YES   |   YES   |
|   26/04/2023   |   14:16   |   18:16   |   Received message   |   HAL   |   YES   |   Notifications   |   NO   |   Notifications   |   NO   |   YES   |
|   26/04/2023   |   14:18   |   18:18   |   Changed avatar picture and added to story   |   HAL   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   26/04/2023   |   14:19   |   18:19   |   Sent message   |   How do I look?   |   YES   |   NO   |   NO   |   NO   |   YES   |   YES   |
|   26/04/2023   |   14:20   |   18:20   |   Received message   |   Like HAL   |   YES   |   NO   |   NO   |   NO   |   YES   |   YES   |
|   27/04/2023   |   13:02   |   17:02   |   Sent message   |   Can I audio call you?   |   YES   |   NO   |   NO   |   NO   |   YES   |   YES   |
|   27/04/2023   |   13:03   |   17:03   |   Received message   |   Sure. Whenever you’re ready.   |   YES   |   Notifications   |   NO   |   Notifications   |   YES   |   YES   |
|   27/04/2023   |   13:04   |   17:04   |   Outgoing audio call   |   (~0:20)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   27/04/2023   |   13:05   |   17:05   |   Received message   |   How about I call you?   |   YES   |   Notifications   |   NO   |   Notifications   |   YES   |   YES   |
|   27/04/2023   |   13:06   |   17:06   |   Sent message   |   Sounds good.   |   YES   |   NO   |   NO   |   NO   |   YES   |   YES   |
|   27/04/2023   |   13:07   |   17:07   |   Incoming audio call   |   (~1:20)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   27/04/2023   |   13:10   |   17:10   |   Outgoing audio call   |   (~1:29)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   27/04/2023   |   13:12   |   17:12   |   Received message   |   I will video call you.   |   YES   |   Notifications   |   NO   |   Notifications   |   YES   |   YES   |
|   27/04/2023   |   13:13   |   17:13   |   Incoming video call   |   (~1:30)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   27/04/2023   |   13:16   |   17:16   |   Sent message   |   My turn to video call   |   YES   |   NO   |   NO   |   NO   |   YES   |   YES   |
|   27/04/2023   |   13:17   |   17:17   |   Outgoing video call   |   (~1:40)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   27/04/2023   |   13:20   |   17:20   |   Received message   |   Please delete this message when you get it   |   Notifications   |   Notifications   |   NO   |   Notifications   |   YES   |   YES   |
|   27/04/2023   |   13:22   |   17:22   |   Deleted message   |   (Message received at 13:20)   |   YES   |   NO   |   NO   |   NO   |   NO   |   YES   |
|   27/04/2023   |   13:23   |   17:23   |   Sent message   |   I deleted it.   |   YES   |   NO   |   NO   |   NO   |   YES   |   YES   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Contacts   |   YES   |   NO   |   NO   |   NO   |   YES   |   YES   |
|   Messages   |   YES   |   Notifications   |   NO   |   Notifications   |   YES   |   YES   |
|   Deleted Message   |   Notifications   |   Notifications   |   NO   |   Notifications   |   YES   |   Notifications   |
|   Calls   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Cache   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

## Instagram

Relevant information for this application is stored in these files:

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/DirectSQLiteDatabase/<User-ID>.db

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/com.burbn.instagram.IGImageCache/

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/Library/Preferences/group.com.burbn.instagram.plist

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/<User-ID>/user\_bootstrap/shared\_bootstraps.plist

**Direct parsing of this application is supported by all tested tools**. Most of the tools were able to correctly parse most of the relevant information. **The deleted message was not recovered by any of the tested tools**.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   27/04/2023   |   13:33   |   17:33   |   Received message   |   You here?   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:34   |   17:34   |   Sent message   |   I am. Long time no see.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:35   |   17:35   |   Received message   |   Anything new?   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:36   |   17:36   |   Sent message   |   Nope. Same as before. Battery is still dying like crazy   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:37   |   17:37   |   Received message   |   Understood. We can use my phone for the next image   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:38   |   17:38   |   Sent message   |   Sounds good. I will send you a picture.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:39   |   17:39   |   Sent picture   |   One ticket   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:40   |   17:40   |   Received message   |   Got it. I’ll send you one now.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:41   |   17:41   |   Received picture   |   Unpatched system   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:43   |   17:43   |   Sent message   |   Received. I will audio call you now.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:44   |   17:44   |   Outgoing audio call   |   (1:39)   |   NO   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:46   |   17:46   |   Received message   |   I will audio call you now.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:47   |   17:47   |   Incoming audio call   |   (1:29)   |   NO   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:49   |   17:49   |   Sent message   |   I will video call you in a moment.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:50   |   17:50   |   Outgoing video call   |   (1:39)   |   NO   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:53   |   17:53   |   Received message   |   Now I will video call you.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:54   |   17:54   |   Incoming video call   |   (1:32)   |   NO   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   13:56   |   17:56   |   Sent message   |   We can only unsend messages here. I will unsend this one in a moment.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   27/04/2023   |   13:57   |   17:57   |   Other party liked message   |   (Message sent at 13:56)   |   Notifications   |   Notifications   |   Notifications   |   Notifications   |   YES   |   NO   |
|   27/04/2023   |   13:58   |   17:58   |   Unsent message   |   (Message sent at 13:56)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Posts   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Contacts   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Messages   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Deleted message   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Calls   |   NO   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Cache   |   YES   |   NO   |   YES   |   YES   |   YES   |   NO   |

## kikMessenger

Relevant information for this application is stored in these files:

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Preferences/com.kik.chat.plist

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/cores/private/<User-ID>/kik.sqlite

**Direct parsing of this application is supported by all tested tools**. Most of the tools were able to correctly parse most of the relevant information. **The deleted message was recovered from the SQLite database by AXIOM, PA, XAMN, and BELKASOFT**.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   27/04/2023   |   14:02   |   18:02   |   Set profile photo   |   HAL   |   YES   |   NO   |   YES   |   YES   |   NO   |   YES   |
|   27/04/2023   |   14:06   |   18:06   |   Sent message   |   Welcome to Kik. These ads are horrendous.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   14:08   |   18:08   |   Received message   |   That’s how they keep the app free. Just not like it used to be   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   14:09   |   18:09   |   Sent message   |   True. Just don’t go back to the chat list window.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   14:10   |   18:10   |   Received message   |   What happens if I do?   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   14:11   |   18:11   |   Sent message   |   More ads.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   14:12   |   18:12   |   Received message   |   Makes sense. How’s the battery?   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   14:13   |   18:13   |   Sent message   |   Not great. At all.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   14:14   |   18:14   |   Received message   |   Ok. We can make it quick. I’ll send you a picture   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   14:15   |   18:15   |   Received picture   |   I own you   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   14:17   |   18:17   |   Sent message   |   Got it. I’ll send you one. Let me look.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   14:18   |   18:18   |   Sent picture   |   Work every day   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   14:19   |   18:19   |   Received message   |   Got it.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   27/04/2023   |   14:20   |   18:20   |   Received message   |   Please delete this message when you get it   |   YES   |   YES   |   NO   |   YES   |   YES   |   Notifications   |
|   27/04/2023   |   14:21   |   18:21   |   Deleted message   |   (Message received at 14:20)   |   YES   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   27/04/2023   |   14:22   |   18:22   |   Sent message   |   It’s been deleted.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Contacts   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Messages   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Deleted message   |   YES   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   Attachments   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Cache   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |

## Line

Relevant information for this application is stored in these files:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/Application Support/PrivateStore/<User-ID>/Messages/Line.sqlite

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Preferences/jp.naver.line.plist

**Direct parsing of this application is supported by AXIOM, PA, OXYGEN, and BELKASOFT**. All the tools correctly parsed relevant information and **all of them recovered the deleted message from the SQLite database**. iLEAPP parsed some incoming messages from iOS notifications.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   27/04/2023   |   14:29   |   18:29   |   Changed profile picture   |   HAL   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   27/04/2023   |   16:11   |   20:11   |   Received message   |   I’m here. Just got back.   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   27/04/2023   |   16:12   |   20:12   |   Sent message   |   Same. Good thing, too. It started raining hard.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   27/04/2023   |   16:13   |   20:13   |   Received message   |   We pulled in just as it started. I didn’t think it would rain today.   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   27/04/2023   |   16:18   |   20:18   |   Sent message   |   What are you doing?   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   27/04/2023   |   16:23   |   20:23   |   Received message   |   Same thing as you. Generating data.   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   08:41   |   12:41   |   Sent message   |   Good morning! I will send a picture in a moment.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   08:43   |   12:43   |   Received message   |   (Reply to message sent at 08:41) (Thumbs up emoji)   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   08:44   |   12:44   |   Sent picture   |   Tax payers   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   08:45   |   12:45   |   Received message   |   Got it. Nice. Let me find one to send to you.   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   08:47   |   12:47   |   Received picture   |   Hot pocket   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   08:49   |   12:49   |   Received message   |   I will audio call you in just a moment   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   08:50   |   12:50   |   Incoming audio call   |   (1:40)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   08:52   |   12:52   |   Sent message   |   Now I’ll audio call you   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   08:53   |   12:53   |   Outgoing audio call   |   (1:30)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   08:55   |   12:55   |   Received message   |   Here comes a video call.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   08:57   |   12:57   |   Incoming video call   |   (1:47)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   08:59   |   12:59   |   Sent message   |   My turn.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   09:00   |   13:00   |   Outgoing video call   |   (1:23)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   09:02   |   13:02   |   Received message   |   Here is your bad message. Please delete it after you get it.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   09:04   |   13:04   |   Deleted message   |   (Message received at 09:02)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   09:05   |   13:05   |   Sent message   |   It has been deleted.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   17/05/2023   |   08:57   |   12:57   |   Sent static location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   17/05/2023   |   09:00   |   13:00   |   Received static location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Contacts   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Messages   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   Deleted message   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Calls   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Cache   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |

## Mastodon

Relevant information for this application is stored in an SQLite:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/Databases/shared.sqlite

Cache is stored in this folder:

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/org.joinmastodon.app

**Direct parsing of this app is only supported by XAMN**. AXIOM, PA, and iLEAPP parsed some incoming messages from iOS notifications. **The deleted message was not recovered in the SQLite database, but it was still available in iOS Notifications.**

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   05/05/2023   |   17:52   |   21:52   |   Received post   |   You here?   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   05/05/2023   |   17:55   |   21:55   |   Sent post   |   I am.  My keyboard got stuck on the emojis and could get out of it.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   05/05/2023   |   17:56   |   21:56   |   Received post   |   Do you mean you couldn’t get out of it?   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   17:57   |   21:57   |   Sent post   |   Lol.  Yeah, typo on my part (Smiley face emoji)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   05/05/2023   |   17:58   |   21:58   |   Received post   |   No worries.  I think this might be it for the day.  Want to send me a picture.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   18:00   |   22:00   |   Sent post with picture   |   Yes sir.  Here you go.  (Picture:  Jeffrey Dahmer)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   05/05/2023   |   18:01   |   22:01   |   Received post   |   Ha!!  I’ll send one.  Hang on.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   18:03   |   22:03   |   Received picture   |   Baraoque   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   18:05   |   22:05   |   Received post   |   Hey, here is your bad message.  Make sure you delete it.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   18:07   |   22:07   |   Sent post   |   I can’t.  I can only delete messages I send.  I will delete this one in a minute.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   18:09   |   22:09   |   Deleted message   |   (Post sent at 18:07)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   18:10   |   22:10   |   Sent post   |   It’s been deleted.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   Contacts   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Messages   |   NO   |   NO   |   NO   |   YES   |   NO   |   Notifications   |
|   Deleted message   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   Cache   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |

## MeWe

Relevant information for this application is stored in an SQLite database:

- /private/var/mobile/Containers/Data/Application/<GUID>/Documents/sgrouplesdb.sqlite

**Direct parsing of this app is only supported by XAMN**. AXIOM, PA, and iLEAPP parsed some incoming messages from iOS notifications. **The deleted message was not recovered in the SQLite database, but it was still available in iOS Notifications.**

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   28/04/2023   |   09:10   |   13:10   |   Changed profile picture   |   HAL   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   28/04/2023   |   09:13   |   13:13   |   Sent message   |   You here?   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   09:15   |   13:15   |   Received message   |   I am! This one is limited IIRC.   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   28/04/2023   |   09:16   |   13:16   |   Sent message   |   You are correct. You have to pay for extra features like calling.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   09:17   |   13:17   |   Received message   |   So pictures and locations only?   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   28/04/2023   |   09:18   |   13:18   |   Sent message   |   You are correct. Speaking of which, here comes a picture.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   09:21   |   13:21   |   Sent picture   |   Password reuse   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   09:23   |   13:23   |   Received message   |   Lol. Let me find one to send you.   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   28/04/2023   |   09:24   |   13:24   |   Received picture   |   Pooh missed Piglet   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   09:26   |   13:26   |   Sent message   |   Lol. Ouch!   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   09:27   |   13:27   |   Received message   |   I know I know. It’s funny, though.   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   28/04/2023   |   09:28   |   13:28   |   Sent message   |   It so is.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   09:29   |   13:29   |   Received message   |   Please delete this bad message after you get it.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   28/04/2023   |   09:30   |   13:30   |   Deleted message   |   (Message received at 09:29)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   28/04/2023   |   09:31   |   13:31   |   Sent message   |   All good. I deleted it.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   17/05/2023   |   09:04   |   13:04   |   Sent static location   |    |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   17/05/2023   |   09:08   |   13:08   |   Received static location   |    |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   Contacts   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   Social Groups   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   Polls   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   Status updates   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   Messages   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   Deleted message   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   Cache   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |

## Session

Relevant information for this app is stored in an SQLite database:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/database/Session.sqlite

Profile pictures and sent/received attachments are stored at:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/ProfileAvatars

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/Attachments

**The SQLite database is encrypted, and PA is the only tool with a decryption and parsing feature for this database.** 

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgeASljzrcAdsXxH7-caX-qXKnBa0DbrBT1F524ebFYfnUICDZFb8SDB2Qee0H3va301rCbRL6l4YooLU5nzfTTdAsk5c7jnF-7IDAe-DGRU6ws05TN1WXsjE85K0WcB5uSQPtnYWw_HVeTourXu3Lu8aHtofR5SKI_TcA9R1cJiDPWuakIe2ZStk5sm-Y/w640-h436/session_pa.jpg)

XAMN, AXIOM, and iLEAPP parsed some incoming messages from iOS notifications. **The deleted message was not recovered by any of the tested tools**.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   01/05/2023   |   19:58   |   23:58   |   Set profile picture   |   HAL   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:00   |   00:00   |   Scanned QR code   |    |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:01   |   00:01   |   Sent message   |   You here?   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:02   |   00:02   |   Received message   |   Yep. What are you up to?   |   Notifications   |   YES   |   NO   |   Notifications   |   NO   |   Notifications   |
|   01/05/2023   |   20:04   |   00:04   |   Sent message   |   Not much. Watching Below Deck. You?   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:05   |   00:05   |   Received message   |   Really? You’re still watching that?   |   Notifications   |   YES   |   NO   |   Notifications   |   NO   |   Notifications   |
|   01/05/2023   |   20:06   |   00:06   |   Sent message   |   Heck yeah! Good trashy TV. This is the one on the sailboat.   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:08   |   00:08   |   Received message   |   (Reply to message sent at 20:04) I’m not doing anything, btw. Just flipping channels.   |   Notifications   |   YES   |   NO   |   Notifications   |   NO   |   Notifications   |
|   01/05/2023   |   20:09   |   00:09   |   Sent message   |   (Eye rolling emoji) You’re so lame.   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:11   |   00:11   |   Received message   |   Whatever. I’ll send you a picture.   |   Notifications   |   YES   |   NO   |   Notifications   |   NO   |   Notifications   |
|   01/05/2023   |   20:16   |   00:16   |   Received picture   |   Acupuncture   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:18   |   00:18   |   Sent message   |   Lol. I’ll send one. Give me a minute.   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:19   |   00:19   |   Sent picture   |   Shell   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:21   |   00:21   |   Received system message   |   (Media saved by DFIR Two.)   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:24   |   00:24   |   Received message   |   Lol! That’s great. Please call me.   |   Notifications   |   YES   |   NO   |   Notifications   |   NO   |   Notifications   |
|   01/05/2023   |   20:27   |   00:27   |   Outgoing audio call   |   (1:35)   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:32   |   00:32   |   Sent message   |   Your turn.   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:33   |   00:33   |   Incoming audio call   |   (1:50)   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:36   |   00:36   |   Received message   |   See if you can video call me.   |   Notifications   |   YES   |   NO   |   Notifications   |   NO   |   Notifications   |
|   01/05/2023   |   20:40   |   00:40   |   Outgoing video call   |   (1:50)   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:47   |   00:47   |   Received message   |   I will video call you.   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:48   |   00:48   |   Incoming video call   |   (1:40)   |   Notifications   |   YES   |   NO   |   Notifications   |   NO   |   Notifications   |
|   01/05/2023   |   20:54   |   00:54   |   Received message   |   You want to send me a “bad” message and then delete it?   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:55   |   00:55   |   Sent message   |   Sure.   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:56   |   00:56   |   Sent message   |   Here is the bad message. I will delete it in a moment.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:58   |   00:58   |   Deleted message   |   (Message sent at 20:56)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   01/05/2023   |   20:59   |   00:59   |   Received message   |   I see you deleted it.   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   Contacts   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   Messages   |   Notifications   |   YES   |   NO   |   Notifications   |   NO   |   Notifications   |
|   Deleted Message   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Call   |   NO   |   YES   |   NO   |  |   NO   |   NO   |
|   Cache   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

## **Signal**

Relevant information for this application is stored in an SQLite database:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/grdb/signal.sqlite

Profile pictures and sent/received attachments are stored at:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/AvatarHistory

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/ProfileAvatars

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/Attachments

**The SQLite database is encrypted, and decryption is supported by AXIOM, PA, and XAMN**. iLEAPP parsed some incoming messages from iOS notifications. It’s important to mention that BELKSASOFT supports Signal decryption on iOS devices (https://belkasoft.com/signal-decryption-with-belkasoft-x), but in the tested case it was not able to read the keychain provided by the extraction. **The deleted message was not recovered from the SQLite database, but it was still available in iOS notifications. PA also recovered a deleted string from the SQLite database, without attribution (sender/recipient) but still really useful.**

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgRiIvnEVyzjqeWdkjZQ_CuzmYfxB1C26yry_Y0yfDC-BPWmM-S1jxFTk8ikFQhe9vDoZAwA18KakazERdR9ed8Iq1nS5Euix7GdpgjifXKAq59-Z9KTnYymPOx9ydW2cvhI5tTD7VMkl9G6r0NOXVTLJfrK_JlR2PNgrCs6ErjES-QkgmvI5KGooJkqTo/w640-h60/signal_pa.jpg)

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   28/04/2023   |   11:38   |   15:38   |   Set profile picture   |   HAL   |   YES   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   28/04/2023   |   11:39   |   15:39   |   Sent message   |   I’m here!   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   11:40   |   15:40   |   Sent message   |   Hello?   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   11:41   |   15:41   |   Received message   |   I think you sent that message before I had my phone setup.   |   YES   |   YES   |   NO   |   YES   |   NO   |   Notifications   |
|   28/04/2023   |   11:42   |   15:42   |   Sent message   |   My bad.   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   11:44   |   15:44   |   Received message   |   No worries. I just didn’t get that first message. No biggie.   |   YES   |   YES   |   NO   |   YES   |   NO   |   Notifications   |
|   28/04/2023   |   11:46   |   15:46   |   Sent message   |   Oh. Yeah, not that big of a deal. How’s the new switch?   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   11:47   |   15:47   |   Received message   |   It seems to be doing ok. Although, there isn’t a lot of headroom with wattage.   |   YES   |   YES   |   NO   |   YES   |   NO   |   Notifications   |
|   28/04/2023   |   11:48   |   15:48   |   Sent message   |   An excuse to get a bigger one. (Smiley emoji)   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   11:50   |   15:50   |   Received message   |   (Reply to message sent at 11:48) True. The new rack can handle it.   |   YES   |   YES   |   NO   |   YES   |   NO   |   Notifications   |
|   28/04/2023   |   11:51   |   15:51   |   Sent message   |   I’ll send over a picture.   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   11:52   |   15:52   |   Sent picture   |   Sorry I’m late   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   11:53   |   15:53   |   Received message   |   Lol. Hang on. I’ll send one.   |   YES   |   YES   |   NO   |   YES   |   NO   |   Notifications   |
|   28/04/2023   |   11:55   |   15:55   |   Received picture   |   Poop   |   YES   |   YES   |   NO   |   YES   |   NO   |   Notifications   |
|   28/04/2023   |   11:58   |   15:58   |   Sent message   |   That’s great! I saved it.   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   11:59   |   15:59   |   Received message   |   I’ll call you. Give me a second.   |   YES   |   YES   |   NO   |   YES   |   NO   |   Notifications   |
|   28/04/2023   |   12:01   |   16:01   |   Incoming audio call   |   (1:35)   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   12:03   |   16:03   |   Sent message   |   I’ll go next   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   12:04   |   16:04   |   Outgoing audio call   |   (1:40)   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   12:29   |   16:29   |   Received message   |   I’ll video call you.   |   YES   |   YES   |   NO   |   YES   |   NO   |   Notifications   |
|   28/04/2023   |   12:30   |   16:30   |   Incoming video call   |   (1:40)   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   12:33   |   16:33   |   Sent message   |   I’ll video call you now.   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   12:34   |   16:34   |   Outgoing video call   |   (1:30)   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   12:37   |   16:37   |   Received message   |   Here is your bad message. Please delete it when you get a chance.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   28/04/2023   |   12:39   |   16:39   |   Deleted message   |   (Message received at 12:37)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   28/04/2023   |   12:40   |   16:40   |   Sent message   |   Got you covered. It’s gone.   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   17/05/2023   |   09:32   |   13:32   |   Sent static location   |    |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   17/05/2023   |   09:34   |   13:34   |   Received static location   |    |   YES   |   YES   |   NO   |   YES   |   NO   |   Notifications   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   Contacts   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   Messages   |   YES   |   YES   |   NO   |   YES   |   NO   |   Notifications   |
|   Deleted message   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   Calls   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |

## Silent Phone

Relevant information for this application is stored in an SQLite database:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/com.silentcircle.SilentPhone/Chat/ChatMessages\_cipher.db

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/com.silentcircle.SilentPhone/Preferences/preferences.plist

User account information is stored in a plist file:

**The SQLite database is encrypted and XAMN is the only tool with a decryption and parsing feature for this database**. So, direct parsing of this app is only supported by XAMN.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi_PQEGxHqCozDZ2TLZkGcoiyKyMdh2qofj6Zwe8NZrhwTIXjWTrW4ZUJKseTl1SsyjQVOe2Wg4zfz7pFYaZem_02qNdZYFL0kJle-TlnxHYd1pQ9eIo-H0TSeqgTWwMPoDDymxGbJpmMI7nl-nO36IcA1tKFXu9c1w5_PGXhKxnvU7ATkSiaZH4xPyp5U/w638-h640/silentphone_xamn.jpg)

AXIOM, PA, and iLEAPP parsed some incoming messages from iOS notifications, but without text. **The deleted message was not recovered in the SQLite database and only the fact that it existed is available in iOS notification (no text).**

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   07/05/2023   |   10:25   |   14:25   |   Sent message   |   Another “private” app.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   07/05/2023   |   10:26   |   14:26   |   Received message   |   Yep. It’s getting old. How many more do we have?   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   07/05/2023   |   10:28   |   14:28   |   Sent message   |   We are close on the 3rd party apps. We still have native apps and the location stuff.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   07/05/2023   |   10:29   |   14:29   |   Received message   |   Man, these things just keeping getting longer.   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   07/05/2023   |   10:30   |   14:30   |   Sent message   |   The more we learn about these devices, the more test data that needs to be generated. That’s just how it is.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   07/05/2023   |   10:31   |   14:31   |   Received message   |   I suppose. Here comes a picture.   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   07/05/2023   |   10:32   |   14:32   |   Received picture   |   Goats butter   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   07/05/2023   |   10:34   |   14:34   |   Sent message   |   Nice. I’ll send one. Just a moment.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   07/05/2023   |   10:36   |   14:36   |   Sent picture   |   Tax payers   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   07/05/2023   |   10:38   |   14:38   |   Received message   |   Haha!! Very nice. I’ll audio call you in a minute.   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   07/05/2023   |   10:40   |   14:40   |   Incoming audio call   |   (1:30)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   07/05/2023   |   10:42   |   14:42   |   Sent message   |   Got it. I’ll audio call you now.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   07/05/2023   |   10:43   |   14:43   |   Outgoing audio call   |   (1:40)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   08/05/2023   |   11:42   |   15:42   |   Sent message   |   Here comes your video call.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   08/05/2023   |   11:43   |   15:43   |   Outgoing video call   |   (1:48)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   08/05/2023   |   11:45   |   15:45   |   Received message   |   Got it. I’ll video call you in a minute.   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   08/05/2023   |   11:46   |   15:46   |   Incoming video call   |   (1:41)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   08/05/2023   |   11:50   |   15:50   |   Received message   |   Here is your bad message. Please burn it and let me know when you do.   |   Notifications   |   Notifications   |   NO   |   NO   |   NO   |   Notifications   |
|   08/05/2023   |   11:51   |   15:51   |   Deleted message   |   (Message received at 11:50)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   08/05/2023   |   11:52   |   15:52   |   Sent message   |   You’re good. I burned it.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |
|   Contacts   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   Messages   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   Deleted message   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   Calls   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   Cache   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

## Skype

Relevant information for this application is stored in an SQLite:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/s4l-live\_.cid.<User-ID>.db

**Direct parsing of this application is supported by all tested tools**. Most of the tools were able to correctly parse most of the relevant information.  **The deleted message was recovered by AXIOM, PA, and BELKASOFT from the SQLite database**.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   28/04/2023   |   12:48   |   16:48   |   Received message   |   Did you make it over here?   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   12:49   |   16:49   |   Sent message   |   I did! I’m watching DS9. |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   12:52   |   16:52   |   Received message   |   Nice. I need to, as well, considering Picard.   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   12:54   |   16:54   |   Sent message   |   That season was crazy.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   12:56   |   16:56   |   Received message   |   Agreed. That is what Nemesis should have been.   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   13:04   |   17:04   |   Sent message   |   100%. Can you send me a picture?   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   13:05   |   17:05   |   Received message   |   Sure. Give me a moment.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   13:08   |   17:08   |   Received picture   |   4chan   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   13:10   |   17:10   |   Sent message   |   Ha! I’ll send you one. Hang on.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   13:11   |   17:11   |   Sent picture   |   Peter Cushing   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   13:13   |   17:13   |   Received message   |   Never thought about that, but that is accurate.   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   13:14   |   17:14   |   Sent message   |   I’ll audio call you momentarily.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   13:16   |   17:16   |   Outgoing audio call   |   (2:02)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   13:19   |   17:19   |   Received message   |   I’ll call you in a minute.   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   13:20   |   17:20   |   Incoming audio call   |   (1:36)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   13:24   |   17:24   |   Sent message   |   Here comes a video call.   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   13:25   |   17:25   |   Outgoing video call   |   (1:41)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   13:27   |   17:27   |   Received message   |   Here comes your video call.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   13:28   |   17:28   |   Incoming video call   |   (1:46)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   13:31   |   17:31   |   Sent message   |   I will remove this message in just a moment.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   13:32   |   17:32   |   Removed message   |   (Message sent at 13:31)   |   YES   |   YES   |   NO   |   NO   |   YES   |   NO   |
|   17/05/2023   |   09:37   |   13:37   |   Received static location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   17/05/2023   |   09:39   |   13:39   |   Sent static location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Contacts   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Messages   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   Delete message   |   YES   |   YES   |   NO   |   NO   |   YES   |   NO   |
|   Calls   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |

## Snapchat

Relevant information for this application is stored in these files and folders:

- /private/var/mobile/Containers/Data/Application/<GUID>/Documents/user.plist

- /private/var/mobile/Containers/Data/Application/<GUID>/Documents/user\_scoped/<User-ID>/DocObjects/primary.docobjects

- /private/var/mobile/Containers/Data/Application/<GUID>/Documents/user\_scoped/<User-ID>/arroyo/arroyo.db

- /private/var/mobile/Containers/Data/Application/<GUID>/Documents/user\_scoped/<User-ID>/databases/memories\_asset\_repository.sqlite

- /private/var/mobile/Containers/Data/Application/<GUID>/Documents/gallery\_data\_object/1/<User-Id>/scdb-27.sqlite3

**Direct parsing of this application is supported by AXIOM, PA, OXYGEN, XAMN, BELKASOFT, and ArtEx**. Most of the tools were able to correctly parse most of the relevant information. 

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   20/05/2023   |   14:20   |   18:20   |   Sent message   |   What’s up??   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   20/05/2023   |   14:23   |   18:23   |   Received message   |   Nothing. At the park with the kid. You?   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   20/05/2023   |   14:25   |   18:25   |   Sent message   |   About the same. No kid, though. Gettin close on this image.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   20/05/2023   |   14:30   |   18:30   |   Received message   |   Nice. Are you doing 16 after?   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   20/05/2023   |   14:31   |   18:31   |   Sent message   |   Yep. I’ll be switching to the 11, though. This 8 has had it.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   20/05/2023   |   14:34   |   18:34   |   Received message   |   Makes sense. Here comes a picture.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   20/05/2023   |   14:35   |   18:35   |   Unsaved video   |   (Video from 2021-12-15)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   20/05/2023   |   14:37   |   18:37   |   Received picture   |   Trash panda   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   20/05/2023   |   14:39   |   18:39   |   Sent message   |   Nice. I’ll send one.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   20/05/2023   |   14:40   |   18:40   |   Sent picture   |   Wilmington   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   20/05/2023   |   14:41   |   18:41   |   Other party saved picture to camera roll   |   Wilmington   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   20/05/2023   |   14:43   |   18:43   |   Took snap   |   iPhone 11 (caption: Test phone two!)   |   YES   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   20/05/2023   |   14:44   |   18:44   |   Added snap to story & sent to DFIR Two   |    |   YES   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   20/05/2023   |   14:47   |   18:47   |   Other party took a screenshot   |    |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   20/05/2023   |   14:48   |   18:48   |   Received snap   |   Notes   |   YES   |   YES   |   NO   |   Notifications   |   NO   |   NO   |   Notifications   |
|   20/05/2023   |   15:13   |   19:13   |   Saved snap in chat   |   Notes   |   YES   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Contacts   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Messages   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Memories   |   YES   |   YES   |   NO   |   NO   |    |   YES   |   NO   |

## Telegram

Relevant information for this application is stored in an SQLite:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/telegram-data/account-<Account\_ID>/postbox/db\_sqlite

Media are saved in the following folder:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/telegram-data/account-<Account\_ID>/postbox/media

**Direct parsing of this application is supported by all tested tools**. Most of the tools were able to correctly parse most of the relevant information.  **The deleted message was not recovered by any of the tested tools**.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   29/04/2023   |   15:58   |   19:58   |   Set profile picture   |   HAL   |   NO   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   30/04/2023   |   10:38   |   14:38   |   Received message   |   Good morning!   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   10:41   |   14:41   |   Sent message   |   Good morning! Battery is mostly charged. Should last about two hours. Lol.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   10:43   |   14:43   |   Received message   |   Ha! Sounds about right. What does the battery health status ay? (sic)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   10:44   |   14:44   |   Sent message   |   86%, so not “service.”   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   10:46   |   14:46   |   Received message   |   Well, something is up. I be that drop in the strawberry field didn’t help.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   10:47   |   14:47   |   Sent message   |   No it did not. I’ll send you a picture in a moment.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   10:56   |   14:56   |   Sent picture   |   I’m not dumb   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   10:57   |   14:57   |   Received message   |   I also feel that. Lol. I’ll send you a picture now.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   10:59   |   14:59   |   Received picture   |   Poop911   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   11:08   |   15:08   |   Sent message   |   I will audio call you in a minute.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   11:09   |   15:09   |   Other party reacted to message   |   (Message sent at 11:08) (Thumbs up emoji)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   30/04/2023   |   11:10   |   15:10   |   Received message   |   (Reply to message sent at 11:08) Sounds good. I’ll be here.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   11:11   |   15:11   |   Outgoing audio call   |   (1:35)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   11:14   |   15:14   |   Received message   |   Now I’ll audio call you.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   11:15   |   15:15   |   Incoming audio call   |   (1:40)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   11:17   |   15:17   |   Sent message   |   Video call incoming.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   11:18   |   15:18   |   Outgoing video call   |   (~1:30)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   11:20   |   15:20   |   Received message   |   Video call coming your way.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   11:21   |   15:21   |   Incoming video call   |   (~1:30)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   30/04/2023   |   11:23   |   15:23   |   Received message   |   Here is your bad message. Delete it when you can.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   30/04/2023   |   11:24   |   15:24   |   Deleted message   |   (Message received at 11:23)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   30/04/2023   |   11:25   |   15:25   |   Sent message   |   Got you fam. It’s been trashed.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   17/05/2023   |   09:42   |   13:42   |   Started receiving dynamic location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   17/05/2023   |   09:47   |   13:47   |   Stopped receiving dynamic location   |    |   YES   |   YES   |   YES   |   NO   |   YES   |   NO   |
|   17/05/2023   |   09:50   |   13:50   |   Received static location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   17/05/2023   |   09:54   |   13:54   |   Sent static location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   17/05/2023   |   10:00   |   14:00   |   Started sending dynamic location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   17/05/2023   |   10:05   |   14:05   |   Stopped sending dynanic location   |    |   YES   |   YES   |   YES   |   NO   |   YES   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Contacts   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Messages   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Deleted message   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Calls   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Locations   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |

## Threema

## 

Relevant information for this application is stored in an SQLite:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/ThreemaData.sqlite

**Direct parsing of this application is supported by PA, OXYGEN, and XAMN**. Most of the tools were able to correctly parse most of the relevant information.  **The deleted message was recovered by OXYGEN and XAMN from the SQLite database**.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   30/04/2023   |   15:42   |   19:42   |   Set profile picture   |   HAL   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   30/04/2023   |   15:43   |   19:43   |   Sent message   |   What’s up?!   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   15:44   |   19:44   |   Received message   |   Not much. Watching Bluey. Kid loves it.   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   15:46   |   19:46   |   Sent message   |   Awesome. Which episode?   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   15:47   |   19:47   |   Received message   |   The one where Chillie chases the kids and her dad in the woods.   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   15:49   |   19:49   |   Sent message   |   That’s a good one. My favorite is when Bandit takes the kids to get Chinese takeout.   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   15:50   |   19:50   |   Received message   |   That’s a good one. So is the date night episode.   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   15:51   |   19:51   |   Sent message   |   Agreed. Send me a picture?   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   15:52   |   19:52   |   Received message   |   (Reply to message sent at 15:51) Yep. Give me a minute to find one.   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   15:58   |   19:58   |   Received picture   |   Healthy Burgers   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   16:00   |   20:00   |   Sent message   |   That’s great. I’ll send one.   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   16:02   |   20:02   |   Sent picture   |   Food weapons   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   16:04   |   20:04   |   Received message   |   I’ll call you in a minute.   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   16:05   |   20:05   |   Reacted to message   |   (Message received at 16:04) (green thumbs up)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   30/04/2023   |   16:06   |   20:06   |   Incoming audio call   |   (1:45)   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   30/04/2023   |   16:09   |   20:09   |   Sent message   |   My turn. I’ll audio call you.   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   16:11   |   20:11   |   Outgoing audio call   |   (1:50)   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   30/04/2023   |   16:14   |   20:14   |   Received message   |   Let’s try video calls. I’ll go first.   |   Notifications   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   16:37   |   20:37   |   Incoming video call   |   (1:35)   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   30/04/2023   |   16:54   |   20:54   |   Sent message   |   My turn.   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   16:55   |   20:55   |   Outgoing video call   |   (1:40)   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   30/04/2023   |   16:59   |   20:59   |   Received message   |   Please delete this message when you get it. I don’t want anyone to see it.   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   17:00   |   21:00   |   Deleted message   |   (Message received at 16:59)   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |
|   30/04/2023   |   17:01   |   21:01   |   Sent message   |   It’s been handled (Smiley face emoji)   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   17/05/2023   |   10:09   |   14:09   |   Sent static location   |    |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   17/05/2023   |   10:14   |   14:14   |   Received static location   |    |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Contacts   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Messages   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Deleted message   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |
|   Calls   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   Locations   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Cache   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

## TikTok

Relevant information for this application is stored in these files:

- /private/var/mobile/Containers/Data/Application/<GUID>/Documents/AwemeIM.db

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/ChatFiles/<User-Id>/db.sqlite

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/AWEStorage/UnifyStorage.sqlite

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Documents/kAWEPublishLocalVideoStorageFolder/

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/TTPlayerCache/

**Direct parsing of this application is supported by all tested tools**. Some differences were found in parsing capabilities.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   17/05/2023   |   08:42   |   12:42   |   Added profile picture   |   HAL   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   20/05/2023   |   10:37   |   14:37   |   Sent message   |   Good morning. You around?   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   20/05/2023   |   10:38   |   14:38   |   Received message   |   (Reply to message sent at 10:37). I am. Good morning!   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   20/05/2023   |   10:39   |   14:39   |   Liked message   |   (Message received at 10:38)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   20/05/2023   |   10:44   |   14:44   |   Sent message   |   Awesome. Any plans for the day?   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   20/05/2023   |   10:45   |   14:45   |   Received message   |   Nope. You?   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   20/05/2023   |   10:46   |   14:46   |   Sent message   |   I am going to try to finish this image today. Have a few map-related things to do.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   20/05/2023   |   10:47   |   14:47   |   Received message   |   Did you finish Snapchat?   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   20/05/2023   |   10:48   |   14:48   |   Sent message   |   No. Also on the list.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   20/05/2023   |   11:02   |   15:02   |   Received message   |   Better get to work then!   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   20/05/2023   |   11:03   |   15:03   |   Sent message   |   I know. (upside down smiley face emoji)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   20/05/2023   |   14:03   |   18:03   |   Took picture   |   Coke & notes (with #work)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   20/05/2023   |   14:04   |   18:04   |   Posted picture   |   (Taken at 14:03)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   20/05/2023   |   14:06   |   18:06   |   Other party liked post   |   (Posted at 14:04)   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   20/05/2023   |   14:07   |   18:07   |   Other party commented on post   |   (Posted at 14:04) Shouldn’t you be off today?   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   20/05/2023   |   14:08   |   18:08   |   Replied to comment   |   (Comment received at 14:07) I should be. Lol.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   20/05/2023   |   14:12   |   18:12   |   Liked other paty's post   |    |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   20/05/2023   |   14:13   |   18:13   |   Commented on other paty's post   |   Don’t be lame. (ROFL emoji)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   YES   |   YES   |   NO   |   YES   |   YES   |   YES   |
|   Contacts   |   YES   |   YES   |   NO   |   NO   |   YES   |   YES   |
|   Groups   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   Messages   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Media   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |

## Truth Social

Relevant information for this application is stored in an SQLite:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/chat/v1/<User-ID>/ChatModel.sqlite

Attachments are stored in this folder:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/chat/v1/<User-ID>/Attachments

**None of the compared tools had a parser for this application**, so messages and contacts were not parsed by any tool. Some received messages were recovered by AXIOM, PA, XAMN, and iLEAPP in iOS notifications.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   05/05/2023   |   16:52   |   20:52   |   Login   |    |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   16:53   |   20:53   |   Accepted friend request   |   (From @lizdehner)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   16:54   |   20:54   |   Changed message retention to 90 days   |    |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   16:59   |   20:59   |   Sent message   |   You here yet?   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   17:00   |   21:00   |   Received message   |   I am! What are you doing?   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   17:21   |   21:21   |   Sent message   |   Not much. Winding down for the day. Also need to figure out dinner.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   17:22   |   21:22   |   Other party reacted to message   |   (Message sent at 17:21) (Thumb’s up emoji)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   17:23   |   21:23   |   Received message   |   I’m right there with you. I looked at the food truck rodeo but there wasn’t anything I wanted.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   17:24   |   21:24   |   Sent message   |   Same here. I was leaning towards the waffle truck but decided against it. Too crowded for waffles. Lol.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   17:26   |   21:26   |   Received message   |   Completely agree with the crowd. Too much of a hassle.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   17:28   |   21:28   |   Sent message   |   Give me a minute and I’ll send you a picture.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   17:30   |   21:30   |   Sent picture   |   Furniture at 4 am   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   17:31   |   21:31   |   Received message   |   I stayed in a hotel that had that type of chair. It was creepy. I’ll send you one. Hang on.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   17:32   |   21:32   |   Received picture   |   Updating the firmware   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   17:33   |   21:33   |   Saved picture   |   Updating the firmware   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   17:34   |   21:34   |   Sent message   |   Ha! That’s great!   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   17:35   |   21:35   |   Received message   |   Please delete this bad message when you get it. And let me know.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   05/05/2023   |   17:37   |   21:37   |   Deleted message   |   (Message received at 17:35)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   17:38   |   21:38   |   Sent message   |   It’s been deleted.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Contacts   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Messages   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   Deleted message   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   Cache   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

## Twitter

Relevant information for this application is stored in these files:

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/com.atebis.tweetie.direct-message.cache/<User-Id>

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/TSFModelCache.1/<User-Id>/modelCache.sqlite3

**Direct parsing of this application is supported by all tested tools**. Some differences were found in parsing capabilities. **The deleted message was not recovered from the SQLite database, but it was still available in iOS notifications.**

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   02/05/2023   |   10:47   |   14:47   |   Sent message   |   Hey, are you here?   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   02/05/2023   |   10:48   |   14:48   |   Received message   |   I am. About time you came over.   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   02/05/2023   |   10:49   |   14:49   |   Sent message   |   Sorry. I have been a bit slow all morning.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   02/05/2023   |   10:50   |   14:50   |   Received message   |   No worries. I was just kidding anyway. How’s the morning been?   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   02/05/2023   |   10:51   |   14:51   |   Sent message   |   Not too bad. One meeting, one canceled meeting, and one meeting that apparently wasn’t.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   02/05/2023   |   10:53   |   14:53   |   Received message   |   Same same. I’ll add that I’m waiting for an extraction to finish parsing.   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   02/05/2023   |   10:54   |   14:54   |   Sent message   |   Me, too. Mine is a do-over. PA got stuck the first time.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   02/05/2023   |   10:55   |   14:55   |   Other party reacted to message   |   (Message sent at 10:54) (Thumbs down emoji)   |   NO   |   Notifications   |   YES   |   Notifications   |   YES   |   Notifications   |
|   02/05/2023   |   10:56   |   14:56   |   Received message   |   Ugh. I hate it when that happens.   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   02/05/2023   |   10:57   |   14:57   |   Sent message   |   It happens. Here comes a picture.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   02/05/2023   |   10:59   |   14:59   |   Sent picture   |   Sorry I’m late   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   02/05/2023   |   11:00   |   15:00   |   Received message   |   Ha! Appropriate. I’ll send one now.   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   02/05/2023   |   11:01   |   15:01   |   Received picture   |   Wizard of Oz   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   02/05/2023   |   11:03   |   15:03   |   Sent message   |   Lol.   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   02/05/2023   |   11:04   |   15:04   |   Received message   |   Here is your bad message. Please let me know when you delete it.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   02/05/2023   |   11:05   |   15:05   |   Deleted message   |   (Message received at 11:04)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   02/05/2023   |   11:06   |   15:06   |   Sent message   |   Done. It’s been trashed (trash can emoji)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Users   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Messages   |   YES   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   Deleted message   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   Cache   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |

## Viber

Relevant information for this application is stored in two SQLite databases:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/com.viber/database/Contacts.data

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/com.viber/settings/Settings.data

Attachments are saved in this path:

- /private/var/mobile/Containers/Data/Application/<GUID>/Documents/Attachments

**Direct parsing of this application is supported by all tested tools**. Most of the tools were able to correctly parse most of the relevant information.  **The deleted message was not recovered from the SQLite database, but it was still available in iOS notifications**.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   02/05/2023   |   11:12   |   15:12   |   Sent message   |   You here?   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:13   |   15:13   |   Received message   |   I am. Long time no see. Lol.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:15   |   15:15   |   Sent message   |   How’s the new switch?   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:16   |   15:16   |   Received message   |   You mean the replacement for the new switch? Good. A lot more headroom for the wattage.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:17   |   15:17   |   Liked message   |   (Message received at 11:16)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:18   |   15:18   |   Replied to message   |   (Message received at 11:16) How bad was it?   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:19   |   15:19   |   Received message   |   I had one AP that would spike occasionally and push it over the threshold.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:21   |   15:21   |   Sent message   |   What would happen?   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:22   |   15:22   |   Received message   |   The AP would just reboot. Basically nothing would connect to it because it wouldn’t stay up long enough.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:24   |   15:24   |   Sent message   |   That sucks. Want to send me a picture?   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:25   |   15:25   |   Received message   |   Sure. Give me a minute to find one.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:26   |   15:26   |   Received picture   |   Try a reboot   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:28   |   15:28   |   Sent message   |   Got it. I’ll send one in a moment.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:31   |   15:31   |   Sent picture   |   Clippy   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:32   |   15:32   |   Received message   |   Nice, and possibly true. I’ll audio call you.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:33   |   15:33   |   Incoming audio call   |   (1:35)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:36   |   15:36   |   Sent message   |   My turn. I’ll audio call you.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:37   |   15:37   |   Outgoing audio call   |   (1:50)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:40   |   15:40   |   Received message   |   Here comes a video call.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:41   |   15:41   |   Incoming video call   |   (1:56)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:45   |   15:45   |   Sent message   |   Ok, here comes your video call.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:46   |   15:46   |   Outgoing video call   |   (1:35)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:49   |   15:49   |   Received message   |   Here is your bad message. Let me know when you delete it.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   02/05/2023   |   11:50   |   15:50   |   Deleted message   |   (Message received at 11:49)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   02/05/2023   |   11:51   |   15:51   |   Sent message   |   It’s gone. You’re covered.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   17/05/2023   |   10:17   |   14:17   |   Sent static location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   17/05/2023   |   10:21   |   14:21   |   Received static location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Contacts   |   NO   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Messages   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Deleted message   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   Calls   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Locations   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |

## WhatsApp

Relevant information for this application is stored in these files and folders:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/Library/Preferences/group.net.whatsapp.WhatsApp.shared.plist

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/ContactsV2.sqlite

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/ChatStorage.sqlite

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/CallHistory.sqlite

**Direct parsing of this application is supported by all tested tools**. Most of the tools were able to correctly parse most of the relevant information.  **The deleted message was not recovered from the SQLite database, but it was still available in iOS notifications**.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   02/05/2023   |   11:55   |   15:55   |   Added profile picture   |   HAL   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   02/05/2023   |   11:56   |   15:56   |   Sent message   |   I’m here!   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:57   |   15:57   |   Received message   |   Awesome. You had some chat history, eh?   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:58   |   15:58   |   Sent message   |   Yep. I don’t recall setting it to back up, but who knows.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   11:59   |   15:59   |   Received message   |   Makes sense with all the test data you generate.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   12:00   |   16:00   |   Reacted to message   |   (Message received at 11:59) (Thumbs up emoji)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   02/05/2023   |   12:01   |   16:01   |   Replied to message   |   (Message received at 11:59) I do generate quite a bit. I need to up my storage. A lot.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   12:02   |   16:02   |   Received message   |   QNAP is the way, my friend.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   12:03   |   16:03   |   Sent message   |   That’s what I keep hearing.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   12:04   |   16:04   |   Received message   |   You listen to good people then. Lol.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   12:05   |   16:05   |   Sent message   |   I do. Lol. I’ll send a picture. Just a moment.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   12:06   |   16:06   |   Sent picture   |   Mario Kart   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   12:07   |   16:07   |   Received message   |   Accurate. Alright, I’ll send you one now.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   12:09   |   16:09   |   Received picture   |   Assault Cow   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   12:11   |   16:11   |   Sent message   |   Nice. I’ll audio call you.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   12:12   |   16:12   |   Outgoing audio call   |   (1:35)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   02/05/2023   |   12:14   |   16:14   |   Received message   |   Now it’s my turn to audio call you.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   12:15   |   16:15   |   Incoming audio call   |   (1:41)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   02/05/2023   |   12:19   |   16:19   |   Sent message   |   Here comes a video call.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   12:20   |   16:20   |   Outgoing video call   |   (1:45)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   02/05/2023   |   12:23   |   16:23   |   Received message   |   My turn. Here comes a video call.   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   02/05/2023   |   12:24   |   16:24   |   Incoming video call   |   (1:44)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   02/05/2023   |   12:27   |   16:27   |   Received message   |   You can delete this message. Let me know when you do.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   02/05/2023   |   12:29   |   16:29   |   Deleted message   |   (Message received at 12:27)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   02/05/2023   |   12:31   |   16:31   |   Sent message   |   Done. It’s gone. (Smiley face emoji)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   17/05/2023   |   10:22   |   14:22   |   Started sending dynamic location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   17/05/2023   |   10:27   |   14:27   |   Stopped sending dynamic location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   17/05/2023   |   10:28   |   14:28   |   Started receiving dynamic location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   17/05/2023   |   10:33   |   14:33   |   Stopped receiving dynamic location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   17/05/2023   |   10:35   |   14:35   |   Sent static locaion   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   17/05/2023   |   10:36   |   14:36   |   Received static location   |    |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Contacts   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Groups   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Messages   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Deleted message   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   Calls   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Dynamic Locations   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Static Locations   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |

## WickrPro

Relevant information for this app is stored in an SQLite database:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/wickrLocal.sqlite

Other relevant files and folders are:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/Logs

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Preferences/com.wickr.pro.prod.plist

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Logs

**The SQLite database is encrypted, and PA is the only tool with a decryption and parsing feature for this database**. Oxygen parses the database and extracts messages and calls metadata, but without decryption of the content. AXIOM and iLEAPP parsed some incoming messages from iOS notifications but without text. **The deleted message was not recovered by any of the tested tools**.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   07/05/2023   |   08:55   |   12:55   |   Sent message   |   Good morning!   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   08:56   |   12:56   |   Received message   |   Good morning to you as well. What are you doing?   |   Notifications   |   YES   |   YES   |   NO   |   NO   |   Notifications   |
|   07/05/2023   |   08:58   |   12:58   |   Sent message   |   Not much. Got up and cooked breakfast this morning. Now everyone has run off.   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   08:59   |   12:59   |   Received message   |   Makes sense. They’re all full and you’re not useful anymore. Lol.   |   Notifications   |   YES   |   YES   |   NO   |   NO   |   Notifications   |
|   07/05/2023   |   09:03   |   13:03   |   Sent message   |   Ok, that’s fair. (Smiley face emoji)   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:04   |   13:04   |   Received message   |   (Reply to message sent at 09:03) You’re such a good sport.   |   Notifications   |   YES   |   YES   |   NO   |   NO   |   Notifications   |
|   07/05/2023   |   09:05   |   13:05   |   Sent message   |   I try. That’s all I can do.   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:07   |   13:07   |   Received message   |   I have a good meme. Let me find where I put it and I’ll send it to you.   |   Notifications   |   YES   |   YES   |   NO   |   NO   |   Notifications   |
|   07/05/2023   |   09:09   |   13:09   |   Reacted to message   |   (Message received at 09:07) (Thumbs up emoji)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:10   |   13:10   |   Received picture   |   East vs. West   |   Notifications   |   YES   |   YES   |   NO   |   NO   |   Notifications   |
|   07/05/2023   |   09:13   |   13:13   |   Sent message   |   Oh man, that’s awesome. Let me find one. Give me a moment.   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:14   |   13:14   |   Sent picture   |   Mediocretes   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:18   |   13:18   |   Received message   |   Ha! Nice. I’ll audio call you.   |   Notifications   |   YES   |   YES   |   NO   |   NO   |   Notifications   |
|   07/05/2023   |   09:20   |   13:20   |   Incoming audio call   |   (1:45)   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:23   |   13:23   |   Sent message   |   I’ll audio call you now.   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:24   |   13:24   |   Outgoing audio call   |   (1:40)   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:27   |   13:27   |   Received message   |   Here comes a video call.   |   Notifications   |   YES   |   YES   |   NO   |   NO   |   Notifications   |
|   07/05/2023   |   09:28   |   13:28   |   Incoming video call   |   (2:00)   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:32   |   13:32   |   Sent message   |   Here comes your video call.   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:33   |   13:33   |   Outgoing video call   |   (2:05)   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:37   |   13:37   |   Sent message   |   I’ll see about creating a room. Give me a few minutes.   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:38   |   13:38   |   Created room   |   (Room name: iOS 15 Room)   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:40   |   13:40   |   Sent message   |   You here?   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:42   |   13:42   |   Received message   |   I am. Let’s make this quick. I’ll send you a picture.   |   Notifications   |   YES   |   YES   |   NO   |   NO   |   Notifications   |
|   07/05/2023   |   09:44   |   13:44   |   Received picture   |   Eternal enemy   |   Notifications   |   YES   |   YES   |   NO   |   NO   |   Notifications   |
|   07/05/2023   |   09:46   |   13:46   |   Sent message   |   Got it. I’ll send you one now.   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:49   |   13:49   |   Sent picture   |   Clicked on the link   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:50   |   13:50   |   Received message   |   Nice. I’ll audio call you.   |   Notifications   |   YES   |   YES   |   NO   |   NO   |   Notifications   |
|   07/05/2023   |   09:51   |   13:51   |   Incoming meeting call   |   (1:43)   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:55   |   13:55   |   Sent message   |   These just look like meetings. I’ll call you and then we’ll call that part done.   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:56   |   13:56   |   Outgoing meeting call   |   (1:40)   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   09:59   |   13:59   |   Received message   |   Here is your bad message for the room. Please delete it.   |   Notifications   |   Notifications   |   YES   |   NO   |   NO   |   Notifications   |
|   07/05/2023   |   10:00   |   14:00   |   Deleted message   |   (Message received at 09:59)   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   07/05/2023   |   10:02   |   14:02   |   Received message   |   And now here is the bad message for the 1:1. Delete it, too.   |   Notifications   |   Notifications   |   NO   |   NO   |   NO   |   Notifications   |
|   07/05/2023   |   10:03   |   14:03   |   Deleted message   |   (Message received at 10:02)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   07/05/2023   |   10:04   |   14:04   |   Sent message   |   Both messages are gone.   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   17/05/2023   |   10:49   |   14:49   |   Started sending dynamic location   |    |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   17/05/2023   |   10:54   |   14:54   |   Stopped sending dynamic location   |    |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   17/05/2023   |   10:55   |   14:55   |   Started receiving dynamic location   |    |   Notifications   |   YES   |   YES   |   NO   |   NO   |   Notifications   |
|   17/05/2023   |   11:00   |   15:00   |   Stopped receiving dynamic location   |    |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   17/05/2023   |   11:02   |   15:02   |   Received static location   |    |   Notifications   |   YES   |   YES   |   NO   |   NO   |   Notifications   |
|   17/05/2023   |   11:05   |   15:05   |   Sent static location   |    |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   Contacts   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   Messages   |   Notifications   |   YES   |   YES   |   NO   |   NO   |   Notifications   |
|   Deleted message   |   Notifications   |   Notifications   |   NO   |   NO   |   NO   |   Notifications   |
|   Calls   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   Dynamic Location   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   Static location   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |

## Wire

Relevant information for this application is stored in an SQLite database:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/AccountData/052477D9-0C43-4BEE-A934-CED86153E9CF/store/store.wiredatabase

**Direct parsing of this application is supported by OXYGEN and XAMN, but only XAMN parsed messages**.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjyVP3I2hp6wEAEju38jIhO9-5I7gkmJ6sjKgenSWABz12DesXOQT4TCehR5tE1utPvNaoZy2Ac-lZe8L7X0JIRLpaf1L_2GAxVmCZ4QhEQaA-26jzrwwBKIIex-HB1Ws2lX3UJJEP3xpXFMewGwZjI6nAMmzyiZxSGx_2lH7bwqNSl9ZpRihuiIi-g3Xg/w506-h640/wire_xamn.jpg)

  

AXIOM, PA, and iLEAPP parsed some incoming messages from iOS notifications. **The deleted message was not recovered by any of the tested tools**.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   02/05/2023   |   12:54   |   16:54   |   Received message   |   I switched over whenever you’re ready.   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   02/05/2023   |   12:57   |   16:57   |   Sent message   |   Awesome. What are you doing?   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   02/05/2023   |   12:58   |   16:58   |   Received message   |   Not much. Just got some lunch.   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   02/05/2023   |   13:00   |   17:00   |   Sent message   |   Just grabbed a bite myself. Eat and go.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   02/05/2023   |   13:02   |   17:02   |   Received message   |   Same my dude.   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   02/05/2023   |   13:03   |   17:03   |   Sent message   |   How’s the testing going?   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   02/05/2023   |   13:05   |   17:05   |   Received message   |   (Reply to message sent at 13:03) Man, it’s one of those days when I want to toss the workstation out of the window.   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   02/05/2023   |   13:07   |   17:07   |   Sent message   |   That good, eh?   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   02/05/2023   |   13:08   |   17:08   |   Received message   |   Absolutely not. (Red-Angry face emoji)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   02/05/2023   |   13:47   |   17:47   |   Sent message   |   Sorry, got sidetracked. I’ll send you a picture.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   02/05/2023   |   13:50   |   17:50   |   Sent picture   |   My contact   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   02/05/2023   |   13:52   |   17:52   |   Received message   |   Nice. I’ll send one in a minute.   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   02/05/2023   |   13:53   |   17:53   |   Received picture   |   Last day of 2023   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   02/05/2023   |   13:54   |   17:54   |   Saved picture   |   Last day of 2023   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   02/05/2023   |   14:09   |   18:09   |   Sent message   |   Got it. I’ll audio call you.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   02/05/2023   |   14:10   |   18:10   |   Outgoing audio call   |   (1:40)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   02/05/2023   |   14:12   |   18:12   |   Received message   |   My turn with the audio call.   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   02/05/2023   |   14:13   |   18:13   |   Incoming audio call   |   (1:35)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   02/05/2023   |   14:15   |   18:15   |   Sent message   |   I’ll video call you now.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   02/05/2023   |   14:16   |   18:16   |   Outoging video call   |   (1:30)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   02/05/2023   |   14:18   |   18:18   |   Received message   |   I’ll video call you. Hang on.   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   02/05/2023   |   14:19   |   18:19   |   Incoming video call   |   (1:45)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   03/05/2023   |   12:07   |   16:07   |   Received message   |   Here is your bad message. Let me know when you delete it. I’ll feel better.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:08   |   16:08   |   Deleted message   |   (Message received at 12:07)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:10   |   16:10   |   Sent message   |   It’s gone. You can releax.   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   17/05/2023   |   10:40   |   14:40   |   Sent static location   |    |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   17/05/2023   |   10:44   |   14:44   |   Received static location   |    |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |
|   Contacts   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |
|   Messages   |   Notifications   |   Notifications   |   NO   |   YES   |   NO   |   Notifications   |
|   Deleted message   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Calls   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   Devices   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Static location   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |

## Final Comments

Some final comments after this complex post.

Most of the commonly used social networking and chat applications are supported by most if not all the tested tools.

The following table is an overview of which tools parsed which applications. Be careful in evaluating the quality of extracted data, as in some cases tools have a very limited parsing capability (i.e., extracting the user account information).

Some apps are correctly and completely supported only by a subset of tools. For example: Session and Wickr are only parsed by PA; MeWe, Mastodon, Silent Phone, and Wire are only parsed by XAMN; BeReal and Google Voice are only parsed by OXYGEN; and Burner is only parsed by AXIOM and OXYGEN. The only completely unsupported app in the dataset is Truth Social.

|   **App Name**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   BeReal   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Burner   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Clubhouse   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   Discord   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Facebook Messenger   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Google Chat   |   NO   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   Google Meet   |   YES   |   NO   |   YES   |   YES   |   NO   |   YES   |
|   Google Voice   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   GroupMe   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   imo HD   |   YES   |   NO   |   NO   |   NO   |   YES   |   YES   |
|   Instagram   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   kik   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Line   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Mastodon   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   MeWe   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   Session   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   Signal   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Silent Phone   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |
|   Skype   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Snapchat   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Telegram   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Threema   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   TikTok   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Truth Social   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Twitter   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Viber   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   WhatsApp   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Wickr Pro (AWS Wickr)   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   Wire   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |

 While doing the comparison, I noticed that tools have often issues when parsing calls (sometimes they are not able to differentiate between audio and video calls or fail in parsing the length of the call), sharing of static and dynamic locations, and message reactions. Also, most tools don’t show message threads in the correct way, and this could complicate the job of an analyst.

Deleted messages were recovered from the native SQLite database for 6 applications: Facebook Messenger, imoHD, kikMessenger, Line, Skype, Signal, and Threema. When the deleted message is an incoming message, you can still have luck by analyzing parsed notifications: in the tested scenario, deleted messages were found in notifications for Google Voice, MeWe, Signal, Silent Phone (only metadata), Twitter, Viber, and WhatsApp.

It’s important to highlight that is relevant, for an examiner, to know the application under investigation. Some applications have specific features that should be analyzed on a case-by-case basis: some examples are Instagram Posts and Stories, X/Twitter Tweets, Snapchat Memories, and so on.

That’s all for this episode! The next one will be about browsers, email clients, and productivity applications!

  

Go to Source
