---
title: "iOS 15 Image Forensics Analysis and Tools Comparison  - Native Apps"
date: 2025-03-19
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "forensics"
  - "security"
  - "security-awareness"
  - "security-awareness-3-0"
---

I am finally back with the third blog post in the series!

Before I introduce this new post, I want to point out some updates to the previous blog post. 

I have corrected a couple of errors related to the Belkasoft tool, in particular the device UDID and the device phone number.

Also, after the blog post, Alexis Brignoni released some new iLEAPP parsers that cover most of the “red” boxes and MSAB confirmed to me that they have already fixed all the “red” boxes from the previous blog post. At the end of the series, I will re-process the dataset with the latest version of the various tools, and create a separate table with the updated comparison.

The goal of this third blog entry is to compare native application parsing, specifically the applications listed below that Josh Hickman documented while creating the test image:

- Contacts
- Call History (Phone and FaceTime)
- Messages (SMS/iMessage)
- Mail
- Safari
- Calendar
- Notes
- Maps
- Files
- Apple Music
- Wheater
- Alarms
- AppStore

There are other relevant apps, notably Photos and Health, both of which are covered in another blog post. Other apps such as podcasts, voice memos, and shortcuts are not compared in this review.

## Contacts

The native address book is stored in an SQLite database available at this path:

privatevarmobileLibraryAddressBookAddressBook.sqlitedb

Under the same path, the AddressBookImages.sqlitedb file contains the images associated with the contacts.

Seven of the compared tools had a parser for these databases, and all of them found 9 contacts. 

Basic information such as first name, last name and phone number was extracted by all tools, while others were parsed only by a subset.

By manually running the query available on Kacos2000's GitHub account, I verified that the database contains 9 contacts. The query provided coherent results with those provided by the tools.

It is worth noting that an address book entry can contain additional fields (e.g. organization, birthday, nickname, note, etc.). Since they were not filled in the test image, I could not verify which tools supported all available information. 

AXIOM has a public and detailed document explaining what information can be extracted for each artifact. (). Other tools provide similar documentation only to clients.

Three tools also extracted information about “Blocked Contacts”, which is available in a plist file that can be found at 

privatevarmobileLibraryPreferencescom.apple.cmfsyncagent.plist

The results for each value and tool are shown in the figure, where YES means that the value was parsed and parsed correctly, and NO, that the value was not parsed.

|   **Description**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **APOLLO**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   First Name   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Last Name   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Phone Number(s)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Email(s)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Picture   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   Thumbnail   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   Creation date   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   Last modified date   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   Address   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   Website   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Organization   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Blocked Contacts   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |   NO   |

## Call History

The native call history is stored in an SQLite database available at this path:

privatevarmobileLibraryCallHistoryDBCallHistory.storedata

All the tested tools have a parser for this database and all tools parsed most of the available information: other party phone number, direction (incoming/outgoing), call type (phone, face time audio, face time video), answered/unanswered, call start time and call duration. Most of the tools also extracted the “Service Provider Country Code”, when available. Only ArtEx parsed the “Disconnection Cause” and none of the tools included the “Face Time Data” column for Face Time Calls. 

By manually running the query available on Kacos2000 GitHub account, I verified that the database contains 22 contacts. The query provided coherent results with those provided by tools.

The results for each value and tool are shown in the figure, where YES means that the value was parsed and parsed correctly and NO means that the value was not parsed.

|  |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **APOLLO**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   Other party   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Direction (Incoming/Outgoing)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Call type (Phone/Face Time Audio/Face Time Video)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Answered/Unanswered   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Call Start Time   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Call Duration   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Service Provider Country Code   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   YES   |   YES   |
|   Disconnection reason   |   NO   |   NO   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   FaceTime Data   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

## Native Messages (SMS/iMessage)

The native SMS/iMessage database is stored in an SQLite database available at this path: 

privatevarmobileLibrarySMSsms.db

All the tested tools have a parser for this database and all tools parsed most of the available information: other party phone number, message timestamp, direction, and message text. Only AXIOM, PA, OXYGEN, and APOLLO extracted all the timestamps, when available: read (incoming messages only) and delivered (outgoing iMessage only). Most of the tools also parsed attachments, with different levels of detail and precision.

The results for each value and tool are shown in the figure, where YES means that the value was parsed and parsed correctly, and NO means that the value was not parsed.

|  |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **APOLLO**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   Other party   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Message Timestamp   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Service (SMS/iMessage)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Direction (Incoming/Outgoing)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Message Text   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Read (Yes/No) (incoming messages only)   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |   YES   |   YES   |
|   Read Timestamp (incoming messages only)   |   YES   |   YES   |   YES   |   NO   |   NO   |   YES   |   YES   |   YES   |
|   Sent (Yes/No) (outgoing messages only)   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |   YES   |   YES   |
|   Delivered (Yes/No) (incoming messages and outgoing iMessage only)   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |   YES   |   YES   |
|   Delivered Timestamp (only sent messages)   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |   YES   |   NO   |
|   Attachment name   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Attachment path   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |   YES   |   YES   |
|   Attachment size (bytes)   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   YES   |   YES   |
|   Message GUID   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   Reply   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

In the iMessage thread, Josh did some relevant and specific actions:

- Replied to a message
- Liked a message
- Deleted a received message
- Sent a dynamic location
- Received a dynamic location
- Sent a static location
- Received a static location.

Message reaction was parsed by all tools, and static locations were parsed by most commercial tools. Dynamic location parsing is really limited: tools found that a message was sent/received when the user started sharing/receiving dynamic location and another one was sent/received when the user stopped sharing/receiving dynamic location. Also looking at the raw database, rows for this type seem empty.

Message reply is a quite relevant aspect and, if ignored, could create confusion while reading a conversation. In the following picture, you can see how a reply is visible in Cellebrite PA conversation view, while it’s not with AXIOM, XAMN, OXYGEN, and BELKASOFT.

**Cellebrite PA**

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhyjoecFDCol_8g1CL_nDxuMzAEmGcQJckfVA1QJhsq220DveMSIerT7LInfJ4TgR2BTM3vIBbBBCLw21KNnQMXf6ocbcISeUCOutHcJJKPzaRmgnoeIBkgZliUs3CxUOdd5jr7AL0IPP6eeWcrGF-SE6eKE66nWsg8bRqdbKcVXzKPKPpnpeLFjn07p3I/w640-h326/iMessage_PA.jpg)

**MSAB XAMN**

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhOc5HEquzMkrkl3y_py3Hyv0Pa4eDXnJi3b07YQU66bW7-d3hllMfPIdpoFrGrbK1Vk8Xqs4u6jiaehA94bAeHKf7YncnsyQOLt_UxZMxv6e1GAT9BRCPXZYpeZGa0eZxzV7CrnArjFmI7nX1OJEWIzC5jWxeDy3yW16XOZbfoqkLcpfDWgsmxiPJ9IDY/w640-h406/iMessage_XAMN.jpg)

**Magnet AXIOM**

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjameDi2S-HBw1YmbUjq98LCsd1qxU2LyVMmRacW-TnAe1LjQDpGP53jy5gzYYCpnWzS3o9Rm_uQUf2Wl0MCd0CoGfWqeS8g6WSGXeUMJMM_YZd66Gd0zRg7bwmCLhj5s0GQyLPmOZwYLUQMmmckUluPuOPTenEQXLFKkGITgUvEeZ3jSScjb8oZy3W1js/w640-h364/iMessage_AXIOM.jpg)

**Oxygen Forensics**

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgn5GN7UalEmLCHbCnIy4Z_Io8AJT67JM1kUNf3eLVWaL-y5YZGcVuRJ7U_9mp5a-_1yVsgq9nDBhDAbm5WtOd8GDqhmdAI4kUkxNLANCFWzlICrAXHOb4Q3Mx9C8Ht_n6RQrAUzCipSVpjseoBBRCbUCurIUqH-D8e9lIQvMgQ9o88pie4UEJj-x1txF4/w640-h236/iMessage_Belkasoft.jpg)

**Belkasoft**

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhpAJuQYCEYLlaoBsfEAC11YspegIGEZe4rhNlGic9h_YabTEL_GcI0oFXVuaBXRnGl0iyhGjRdzY8hUnKyJUUNqezHGxBXJ9QfjemSEUhow89T34iy_DCPxmdXYlOyRZmRzbMwVUiQHAEyI18jQ3YTBt11lF4RJRT1nmgNLCHtTsD7Av3yzpi5wnvpWHI/w640-h200/iMessage_Belkasoft2.jpg)

Recovering deleted messages is one of our main dreams. We know that this has become more and more complicated, as both native and third-party chat apps improved the way in which they manage the database storing conversations. **None of the used tools was able to recover the iMessage that was deleted on this iPhone from the sms.db**. The interesting aspect is that **the deleted message was still available in iOS Notifications, including timestamp, sender, and text**. iOS Notifications are supported by Cellebrite PA, AXIOM, XAMN, and ILEAPP.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhNhEqaNM5c3FMR-GmvaM6zueEF-QfDlzWLR0oExgcp7o5v-E43RTO3yljttl1Nn2bTY5vqh6ucENDJRjPRvy2pooaZDOdlJMjNRB-48u8Yw6UYwbdS1Bb5prl3VkUynngcZSGtNZOb8idweGIBMtbCb7rbOmrKNumhKJWw5kO9iw678cwLFe3qFCUizKQ/w640-h334/deleted_imessage.jpg)

iLEAPP also has a plugin that checks the number of missing rows and can be used to spot when a deleted message was sent/received. A similar feature is available in ArtEx in Timeline view and when parsing the SQLite database.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjxrueGmK-kuqEpW487neAb8a_FH_UmmSZB2VjDQE8VVt7Nc-WDn-7wYZlkb5ThvV6dei7D07_qiPNr0WIVijjtScFJozHsivdI6-36ZcBypoxsX8aDpwsLNC1lKbZaViFLoDMgJ_CHMSnugfEdmovlhWgxkpbhmpdpkeffuXdV0iBJQWLnu2W1qkRIbTE/w640-h406/imessage_deleted_rows.jpg)

The KnowledgeC database stores the information that a notification was received, even after the message has been deleted, but only the timestamp is available (no sender, no text).

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi-isJLqhAZeSMfnl52trRT8PWEUve61xVTk1BuSkZfmJAQRt-Hd5GNKsotc4c__4DpYfSBTZH1Bnd0BP9Ar1xr0Jndfps3jyNUfZquT8FD-J6V7vJAKutt3Em3cssPqVgtcqFC-C4k7-jHi0QeU77lYUbKFzct_aA6JHEzkkUml_HjLc3UHDPzlp4nEbs/w640-h398/knwoeldgec_axiom.jpg)

## Apple Mail

Apple Mail is the default email client on iOS devices. All the used tools have more than one parser for this app, extracting data from various files.

Most of the email metadata is extracted from two SQLite databases:

- privatevarmobileLibraryMailEnvelope Index
- privatevarmobileLibraryMailProtected Index

Every single message is saved as an emlx file at the path privatevarmobileLibraryMailMessageData.

Most of the commercial tools parsed metadata and matched the content of the databases with the emlx file and related attachments. Belkasoft, ArtEx, and iLEAPP only parsed metadata.

The results for each value and tool are shown in the figure, where YES means that the value was parsed and parsed correctly, and NO means that the value was not parsed.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **APOLLO**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   Sent timestamp   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Received timestamp   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Subject   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   From   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   To   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   CC   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   BCC   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Body   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   YES   |
|   Folder   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Status (Sent, Read, Unread)   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   Header   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   Body   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   Summary   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Attachments   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |

## Safari

Safari is the default and most used browser on iOS devices. All the used tools have more than one parser for this app, extracting the data from various files. The most relevant files are:

- privatevarmobileLibrarySafariHistory.db
- privatevarmobileLibrarySafariSafariTabs.db 
- privatevarmobileLibrarySafariBookmarks.db
- privatevarmobileLibrarySafariCloudTabs.db
- privatevarmobileLibrarySafariBrowserState.db
- privatevarmobileContainersDataApplication<GUID>LibrarySafariDownloadsDownloads.plist
- privatevarmobileContainersDataApplication<GUID>LibraryPreferencescom.apple.mobilesafari.plist
- privatevarmobileContainersDataApplication<GUID>LibraryCachescom.apple.mobilesafariCache.db
- privatevarmobileContainersDataApplication<GUID>LibraryImage CacheFaviconsFavicons.db

During dataset creation, Josh took some actions, including visiting websites in normal and private mode, adding a website to bookmarks, downloading a PDF file, and closing various tabs.

The History.db database contains browsing history in normal mode. Websites visited in Private Mode are never saved to this database. All visits in normal mode were correctly parsed by tools.

For other files, the most detailed parsing is performed by PA and AXIOM, which parse all the available databases. 

Of particular interest is the database “SafariTabs.db”, which is used to store suspended state tabs: the interesting aspect is that both tabs in normal and private mode are saved here. Of particular interest, in our case, are two visits: on May 10th at 10:18 AM EDT to the SANS Website (sans.org) in a Private Tab left opened and at 10:23 AM EDT to the “espn.com” in a Private Tab that was closed after the visit. AXIOM, PA, and XAMN were able to find the tab still open, but they all parse the database partially or wrongly. AXIOM correctly parsed Title, URL, time stamp, and Tab ID, but states that the page was not visited in private mode. XAMS only extracts Title, URL, and Tab ID and Cellebrite PA only extracts Title and URL.

**Cellebrite PA**

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiOWjClQTPg-2wT1E39w8Bih5ppR63t4hULSXeBbFGfctTVd6RatdWEMRJhvXvbrsHjTUeVzBHB2fttXSxusBMyRmQcJGakVDtNOJA8jJVT-5Mlov6QOarXugIrUkE9Oz1_YirEuIURm057A_om-V3_gAllXQUihyphenhyphenZ-ajazRMo2rnHBzxRuZs8TjMpnOPg/w640-h512/pa_sfaritabs.png)

  

**AXIOM**

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgLtzV1u8O789025finexBuohQrJodpYEmHFYIB0UzgH3O5JWSqCPcQsJziOE4q0eFEQ4lkhCaA88GDrR86yjxfV8Bse9nCIU-fQ3Uuvza81PFEJrcuG-Mlwa7HiKpZBhtGV-VjaXLb_rqN9i9C5uzXNCPGz49ZB8RiPZHmBoxyop4vvcTGhQCCrUUtSMQ/w640-h488/axiom_safaritabs.jpg)

**XAMN**

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjL5udccWYXGY58a-6gSprsWPeDHvTvwIbk4lhH_uPnHhtfr8JgPO58licaKiDIVmWor2Y9gV4i8tk4M8QJPCbotRTQgEIYDIaU5AQ_GgPNQa8ivsjTF7U8dgwg-Ui-g5mCed03cMOMIxgfj2ifVwTRCaYzu-x3RxyXABrtdFfSziYGR_zoohOtcGarKjg/w640-h396/xamn_safaritabs.jpg)

The results for each value and tool are shown in the figure, where YES means that the value was parsed and parsed correctly, and NO means that the value was not parsed.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **APOLLO**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   Visit Timestamp   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   URL   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Redirect URL   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |   YES   |   YES   |
|   Title   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Visit Count   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |   YES   |   YES   |
|   Visit Source (Local/Cloud)   |   YES   |   YES   |   NO   |   NO   |   NO   |   NO   |   YES   |   NO   |
|   Search Terms /History.db / com.apple.mobilesafari.plist)   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |   YES   |
|   Bookmarks (Bookmarks.db)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   YES   |
|   iCloud Tabs (CloudTabs.db)   |   YES   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Last Session (Browser State.db)   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Downloads (Downloads.plist)   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Tabs (BrowserState.db)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   YES   |
|   Tabs (SafariTabs.db)   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   Favicons (Favicons.db)   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |   YES   |
|   Cache (Cache.db)   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   Cookies   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |   NO   |

## Calendar

The native calendar is stored in an SQLite database available at this path:  

privatevarmobileLibraryCalendarCalendar.sqlitedb

All the commercial tools and iLEAPP have a parser for this database. All the tools extracted information about events stored there including title (summary), event location, event organizer, start and end date, all-day event (yes/no), calendar (home/work), and notes. XAMN and iLEAPP also parsed the creation and modified timestamps for each entry. 

By manually executing the query available on Kacos2000 GitHub account, I verified that the database contains 133 rows. The query provided coherent results with the ones provided by tools.

Some tools show both the “Start Timezone” and the “End Timezone”: be careful in understanding that the raw value in the database is stored in UTC, so the output can be sometimes misleading. 

In the specific case of the analyzed acquisition, there is one event titled “My iOS 15 Test Event” that has a “Start Date” that was set by the user at 2 p.m. EDT on May 31st, 2023. Let’s take a look at how some tools show this information. 

Cellebrite PA, which doesn’t show the “Start Timezone”, if set to show the date and time in UTC, states that the event is set for 6 p.m. UTC on May 31, 2023, as shown in the picture. The provided information is correct, but we don’t have visibility on the “Timezone” of the event.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgv0JZKQJC46v7VGJfn7k4T4qnDcYiS5kyoe-v9smBnZoLRNbXQ9TgwUJCGGYg4lK700-lLNCwWdW4paNx7ypVsE0Y9sgW2hyNGrMJ4LpNjg4ZiPw0y38GodTjwbXvt2d5-MRfMnwyJolFZaRUsgBkKAa1qRA3XXY1CHgPonfkwYv9GbtY-RxX_FIebflE/w474-h640/pa_calendar.jpg)

  

  

MAGNET Axiom, which shows the “Start Timezone” as “America/New\_York”, if set to show the date and time in UTC, states that the event is set for 6 p.m. UTC on May 31st, 2023, as shown in the picture. The provided information is correct, and you also have visibility on the “Timezone” of the event. But you have to be careful in not to state that the event is set for 6 p.m. on the America/New\_York timezone!

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhWeDMh_isE-aTrksswA8_BIu81ghv_fj-VSo12ECnFGWYICcPFLkfa3H07gIBusbxpCo31glNOw3PVn59yvyv8ixLbP1xLTKRcJePkrP0TXVfkPQxwojnuehv6s7Z7qgOitYLNQQurg0VYDH2qUFXaLa_MRA7UmuBQwhk7kVgQnDf5hs6bCNCvx7-GOSE/w410-h640/axiom_calendar.jpg)

  

The same applies for iLEAPP, that shows the timestamp in UTC and also mentions the America/New\_York timezone. Be careful in understanding the output!

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjtdL5pK_VcwY7qUS7F9xr9DqYRc2vK8UyJnIle0pslRsMfB74BwRaVWC8h0EuhOcnah71gRt0RUlLApeH4cZYtRuQF_ohBs2v7dTrsKSK6Zg5RlbDrFyjpdnAIMAe6dsNQBgZ6RyMRjQn74VV3tpoIfRhaJCMCudABFwN7bLb_mVb4b1FkZN6DvxxfF_8/w640-h48/ileapp_calendar.jpg)

The results for each value and tool are shown in the figure, where YES means that the value was parsed and parsed correctly, and NO means that the value was not parsed.

|   **Description**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **APOLLO**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   Subject/Summary   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   YES   |
|   Folder   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   Related Account   |   NO   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   Location   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   Start date   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   YES   |
|   End date   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   YES   |
|   Timezone   |   YES   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |   YES   |
|   Entry creation time   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |   YES   |
|   Entry modified time   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |   YES   |

## Notes

The Notes database is stored in an SQLite database available at this path:  

privatevarmobileContainersSharedAppGroup<GUID>NoteStore.sqlite

All the tools, apart from APOLLO, have a parser for this database and can extract, in the case of unencrypted notes, the modified time, the title, the body, and the summary. Some tools can only identify encrypted notes, while others also have a decryption feature. Cellebrite, ArtEx and Oxygen can decrypt notes when a correct password is provided or by cracking it (Cellebrite and ArtEx only with a keyword list, while Oxygen also with brute force attack), while **AXIOM was able to decrypt the encrypted notes without the need of providing or cracking the password**. AXIOM, PA, and XAMN also extract information about another party who shared the note with the user. Creation timestamp is provided only by Cellebrite and AXIOM.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **APOLLO**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   Creation time   |   YES   |   YES   |   NO   |   NO   |   YES   |   YES   |   NO   |   YES   |
|   Modified time   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Title   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Body   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Parties   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   Summary   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Password protected (yes/no)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Password hint   |   YES   |   YES   |   YES   |   NO   |   NO   |   YES   |   NO   |   YES   |
|   Decrypt encrypted notes   |   YES   |   YES   |   YES   |   NO   |   NO   |   YES   |   NO   |   NO   |

## Apple Maps

Apple Maps is the default navigation app on iOS devices. All the used tools have parsers for this app, extracting data from various files.

The most relevant information can be extracted from an SQLite database and three plist files:

- privatevarmobileContainersSharedAppGroup<GUID>MapsMapsSync\_0.0.1
- privatevarmobileContainersSharedAppGroup<GUID>LibraryPreferencesgroup.com.apple.Maps.plist
- privatevarmobileContainersDataApplication<GUID>LibraryMapsPins.metadata
- privatevarmobileContainersDataApplication<GUID>DocumentslocationQueryGeococdeCacheFolder

An interesting and overlooked artifact by commercial tools is Map Cache Files, actually parsed by AXIOM and ilEAPP. This folder contains three relevant SQLite databases:

- privatevarmobileLibraryCachescom.apple.geodAP.db
- privatevarmobileLibraryCachescom.apple.geodMapTiles.sqlitedb
- privatevarmobileLibraryCachescom.apple.geodPDPlaceCache.db

A great reading about this artifact is the master thesis written by Jesse Spangenberger (https://drive.google.com/file/d/1QqvRoMS-R1WB58EiD6gNmvsuU7GpP\_SJ/view?ref=digitalforensics.io) and the related blog post (https://digitalforensics.io/ios-forensics-ileapp-updates/).

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **APOLLO**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   Origin Address   |   YES   |   NO   |   NO   |   NO   |   NO   |   YES   |   NO   |   YES   |
|   Origin Latitude   |   YES   |   NO   |   NO   |   NO   |   NO   |   YES   |   NO   |   YES   |
|   Origin Longitude   |   YES   |   NO   |   NO   |   NO   |   NO   |   YES   |   NO   |   YES   |
|   Destination Address   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   Destination Latitude   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   Destination Longitude   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   Creation date   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   Search terms   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |   NO   |   YES   |
|   Map Tiles Cache   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |   YES   |

## Files

The Files application stores its data in three SQLite databases:  

- privatevarmobileLibraryApplication SupportCloudDocssessiondbserver.db
- privatevarmobileLibraryApplication SupportCloudDocssessiondbclient.db
- privatevarmobileContainersSharedAppGroup<GUID>smartfolders.db

**iLEAPP is the only tool parsing this application**. While creating the test image, Josh documented that on May 20th at 10:51 AM EDT, he downloaded a file named “f7390d55-8690-4b52-a794-bc54c2a58fbd.pdf “ and then at 10.54 AM EDT he sent this file via iMessage. Also, at 10.58 AM EDT a file named “SANS\_DFPS\_FOR585\_v3.5\_2211.pdf” was saved on May 8th at 2.23 PM EDT 

The only issue I found in the iLEAPP parser, is that the shown date and time are the local time of the machine where it was run. In my case, I run it on a computer set to UTC+2 so the shown date time is not in UTC. Information about the downloaded file (with Safari) and the file sent by iMessage is parsed by all the tools in the specific section (Messages and Safari Browser).

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjtkYbLSZ3LpnP1PsChue7Na9xlqeQHU8sU579wGMiHOdPxolaG180S9LCHCWrKGsWJHfzCWLTcPFp8Rfa4l4DjOSgwwKEX-kFojnI_EjU4Dx22RaF-wsqfYb8tWTU6lFqUnNGh-gSzrh1v186fLh_VRFGQLNW1KgoIiQ6FNj_L_BxcK71sQ-QdtztwCNE/w640-h338/Files_iLEAPP.jpg)

## Apple Music

The Apple Music app is not directly supported by any of the tested tools. By analyzing the KnowledgeC database and, specifically, the “NowPlaying” (Media History) entries, it is possible to find information about which application was playing a media, the start and end date, the song title, the artist, the album, and so on. This information was extracted from the KnowledgeC by AXIOM, Cellebrite, Belkasoft, ArtEx, and APOLLO: the last two provide the most detailed view.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **APOLLO**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   Start playing time   |   KnC   |   NO   |   KnC   |   KnC   |   KnC   |   KnC   |   KnC   |   NO   |
|   End playing time   |   KnC   |   NO   |   KnC   |   KnC   |   KnC   |   KnC   |   KnC   |   NO   |
|   Title   |   KnC   |   NO   |   KnC   |   NO   |   KnC   |   KnC   |   KnC   |   NO   |
|   Artist   |   KnC   |   NO   |   KnC   |   NO   |   KnC   |   KnC   |   KnC   |   NO   |
|   Album   |   KnC   |   NO   |   KnC   |   KnC   |   KnC   |   KnC   |   KnC   |   NO   |
|   Duration   |   KnC   |   NO   |   NO   |   KnC   |   NO   |   KnC   |   KnC   |   NO   |
|   State   |   NO   |   NO   |   NO   |   NO   |   NO   |   KnC   |   KnC   |   NO   |
|   State Duration   |   NO   |   NO   |   NO   |   NO   |   NO   |   KnC   |   KnC   |   NO   |

## Weather

The Apple Weather app is only supported by XAMN and iLEAPP, which parsed the associated plist file available at

privatevarmobileContainersSharedAppGroup<GUID>LibraryPreferencesgroup.com.apple.weather.plist

While creating the test image, Josh documented that he added Austin, TX to the location list on May 9th, 2023 at 21:08 EDT.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgXLTTQOdwmoRpeO8PFV9e9E9NYoUUnscsUWCrdsmpqnf1e3WF1OuEEQOtTDN44g-yzUt-WGs6ERI2RbcYVVtpRe-Rdv_pqcwCAGo6JvPeLMCHwXe-edK9vkj-pgzYLkwoWSSi_q6FbptjhKAHzImQiLTaNGWoKQhHKoNehKwhuxR47OjLdw7beKOR51d8/w640-h332/weather_ileapp.png)

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj8Ngv7IE6XeHG4jeJuQCgul-OKvUU0gRpheCflzpDRsakqFl4jvYBdRfjFrZnPCvwQvuDbtIh_0dL5WoVb1Lwoh61GXGR_sFAILipIk8eKWWitXSvAQTRts6fL1yg4VfnnsR5dv4UEaa1UHrzhQvm4GcQMp9rEu0RiwjoaJeR0jucoNJPUtXwaWVymkgc/w640-h268/weather_xamn.png)

Additional information about the app usage can be found in the Cache.db stored at 

privatevarmobileContainersDataApplication<GUID>LibraryCachescom.apple.weatherCache.db

This artifact is parsed by AXIOM.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhEDU0Aofkwu-h_c5yd2QPCDdoAIyGupCb59Jix2m7gvf3uvycODrvt90M_noXPMEZDFs5HZ1ZeP8BTgp39VjRpc5WQeMwpIZPd5oOrxAdNtnCwSPe1ft3aZH0UHdcMweGem0MlViNiBLrWwxEoiEUk79GuCwIyBCE34mChVdqNBT4zZD8-B6_9wKhhz40/w640-h458/weather_axiom.png)

  

## AppStore

As mentioned in Josh’s documentation, some searches were performed in the Apple AppStore application. This artifact is not explicitly supported by any of the tested tools. By knowing which keywords and when they were searched, traces can be found in the AppStore Cache, stored in

privatevarmobileContainersDataApplication<GUID>LibraryCachescom.apple.AppStoreCache.db.

An example of this is the URL that could be extracted from the AppStore Cache.db related to the search of “Firefox”:

https://search.itunes.apple.com/WebObjects/MZSearchHints.woa/wa/hints?caller=com.apple.AppStore&clientApplication=Software&term=Firefox&v=4&with=appEvents

The analysis of the iOS Cache file format is supported by all the commercial tools, but only two of them parsed the Cache.db file for the AppStore: AXIOM and XAMN. 

Here follows a comparison table for app store activities, among the various tools.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **Belkasoft**   |   **ARTEX**   |   **APOLLO**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   05/05/2023   |   12:14   |   16:14   |   Searched for “Clubhouse”   |   iOS App Cache   |   NO   |   NO   |   iOS App Cache   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   13:14   |   17:14   |   Searched for “CoverMe”   |   iOS App Cache   |   NO   |   NO   |   iOS App Cache   |   NO   |   NO   |   NO   |   NO   |
|   06/05/2023   |   10:55   |   14:55   |   Searched for “Firefox”   |   iOS App Cache   |   NO   |   NO   |   iOS App Cache   |   NO   |   NO   |   NO   |   NO   |
|   20/05/2023   |   09:23   |   13:23   |   Searched for “Gmail”   |   iOS App Cache   |   NO   |   NO   |   iOS App Cache   |   NO   |   NO   |   NO   |   NO   |

## Alarms

The Apple Alarms app is only supported by iLEAPP, which parses the associated plist file available at

privatevarmobileLibraryPreferencescom.apple.mobiletimerd.plist

While creating the test image, Josh documented that on May 8th at 2.23 PM EDT, he set a timer labelled “My Daily Alarm” at 11.30 EDT from Monday to Friday.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjWb7tfIkvX3mEoNictIJzjoYgvwQS8VawrC_f3fFLtKPtMN_c4sEYs7o7MFT7oW9TLccCh7Cqj1IwMYnyD03vFVAuG36pfhrhCZas92CvF7gt2Btyn1oR-EwqgJhxd_v-v90H19k36CZ5fiErCTSBjNNO_brVfmRiDxbbAsc0tb7MrRMRDUgqR6MyF3Nk/w640-h272/alarm_ileapp.png)

Go to Source
