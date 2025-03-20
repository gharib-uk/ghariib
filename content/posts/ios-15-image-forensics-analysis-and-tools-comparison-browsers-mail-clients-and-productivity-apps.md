---
title: "iOS 15 Image Forensics Analysis and Tools Comparison  - Browsers, Mail Clients, and Productivity apps"
date: 2025-03-19
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "forensics"
  - "security"
  - "security-awareness"
  - "security-awareness-3-0"
---

The fifth episode is dedicated to three categories of third-party apps: browsers, mail clients, and productivity apps.

There are 6 browsers, 3 mail clients, and 3 productivity applications available in Josh Hickman’s acquisition. 

The 6 browsers are listed below, in alphabetical order.

1. Brave
2. DuckDuckGo
3. Firefox
4. Firefox Focus
5. Google Chrome
6. Microsoft Edge

The 3 mail clients are listed below, in alphabetical order.

1. Gmail
2. Proton Mail
3. Tutanota

The 3 productivity applications are listed below, in alphabetical order.

1. Microsoft Teams
2. Slack
3. Zoom

APOLLO has no parsers for third-party apps, while ArtEx only has a parser for Google Chrome.

Each section for each app is structured similarly: first, a list of the most important files and folders is provided, then a description of the tools parsing capabilities for that app, and finally two different tables. The first table provides the details for each action documented by Josh in the iOS 15 image documentation, and the second table provides a summary of the tools’ parsing capabilities based on the test results.

As usual, YES means that the value was parsed and parsed correctly, NO means that the value was not parsed, YELLOW means that it was partially parsed, and BLUE means that the value was found elsewhere (i.e., iOS Notification or iOS Cache).

The rest of the blog post is organized by categories: browsers, email clients, and last productivity applications.

## Brave Browser

Relevant information for this application is stored in these files and folders:

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Brave.sqlite

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application  Support/Chromium/Default/History

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application  Support/Chromium/Default/Login Data

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application  Support/Chromium/Default/Web Data

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application  Support/Chromium/Default/Favicons

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application  Support/Chromium/Default/Preferences

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/WebKit/NetworkCache/Version 16/Blobs

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/WebKit/NetworkCache/Version 16/Records

**Direct parsing of this application is supported by AXIOM, OXYGEN, and BELKASOFT**. Oxygen seems to cover this browser in a more complete way: it parses history, cache, cookies, and tabs, while AXIOM only parses tabs and BELKASOFT only parses history. All tools extracted some information about visited websites in normal mode, but not about visited websites in private mode.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Type**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   05/05/2023   |   10:50   |   14:50   |   Typed “nhl.com” in address bar. Visited NHL website.   |   Normal mode   |   YES   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   05/05/2023   |   10:53   |   14:53   |   Clicked on link “Panthers pleased after 2 wins in Toronto”   |   Normal mode   |   YES   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   05/05/2023   |   10:54   |   14:54   |   Opened second tab   |   Normal mode   |   YES   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   05/05/2023   |   10:56   |   14:56   |   Typed “digitalcorpora” in address bar (search)   |   Normal mode   |   YES   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   05/05/2023   |   10:57   |   14:57   |   Clicked result for “digitalcorpora.org” / visited site   |   Normal mode   |   YES   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   05/05/2023   |   10:58   |   14:58   |   Opened Private tab   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   11:00   |   15:00   |   Typed “sans.org” in the address bar. Visited SANS website   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   11:01   |   15:01   |   Clicked on the “Find Training” link   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   11:02   |   15:02   |   Opened second Private tab   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   11:03   |   15:03   |   Typed “ESPN” in address bar (search) (2nd Private tab)   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   11:04   |   15:04   |   Clicked on result for “espn.com” / visited site (2nd Private tab)   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   11:08   |   15:08   |   Switched from Private to Public tab space then back to Private   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   11:09   |   15:09   |   Opened new Private tab   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   History   |   NO   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   Cache   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Cookies   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Bookmarks   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Tabs   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |

## Duck Duck Go

Relevant information for this application is stored in these files and folders:

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Preferences/com.duckduckgo.mobile.ios.plist

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/com.duckduckgo.mobile.ios/Cache.db

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/WebKit/NetworkCache/Blobs/

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/WebKit/NetworkCache/Records/

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Cookies/Cookies.binarycookies

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/Bookmarks.sqlite

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/Favicons/

**Direct parsing of this application is supported by AXIOM and OXYGEN**. OXYGEN parses cache, cookies, and tabs, while AXIOM parses tabs and bookmarks. **Traces of visited webpages were found in tabs, cache, and cookies**.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   05/05/2023   |   12:37   |   16:37   |   Typed “nhl.com” in address bar. Visited NHL website.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   12:38   |   16:38   |   Clicked on link “Daily fantasy picks, projections”   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   12:39   |   16:39   |   Opened second tab   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   12:40   |   16:40   |   Typed “digitalcorpora” in address bar (search) (2nd tab)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   12:41   |   16:41   |   Clicked result for “digitalcorpora.org” / visited site (2nd tab)   |   Tabs   |   NO   |   Tabs   |   NO   |   NO   |   NO   |
|   05/05/2023   |   12:44   |   16:44   |   Bookmarked digitalcorpora.org (2nd tab)   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   12:45   |   16:45   |   Opened third tab   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   12:46   |   16:46   |   Typed “ESPN” in address bar (search) (3rd tab)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   12:47   |   16:47   |   Clicked on result for “espn.com” / visited site (3rd tab)   |   NO   |   Cookies   |   Cookies   |   NO   |   NO   |   NO   |
|   05/05/2023   |   12:48   |   16:48   |   “Clicked on link “Former Mets star Harvey ends ‘dream’ career” (3rd tab)   |   Tabs   |   NO   |   Tabs   |   NO   |   NO   |   NO   |
|   05/05/2023   |   12:49   |   16:49   |   Deleted first tab   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   History   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Cache   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Cookies   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   Tabs   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Bookmarks   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |

## Firefox

Relevant information for this application is stored in these files and folders:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/profile.profile/places.db

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/profile.profile/browser.db

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/org.mozilla.ios.Firefox/Cache.db

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/WebKit/NetworkCache/Version 16/Blobs/

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/WebKit/NetworkCache/Version 16/Records/

**Direct parsing of this application is supported by PA, OXYGEN, XAMN, and BELKASOFT**. PA, XAMN, and BELKASOFT parsed history and bookmarks, OXYGEN and XAMN parsed cache, and PA and OXYGEN parsed cookies. **Traces of websites visited in private mode were found in cache and cookies**.

It’s worth noting that **none of the tested tools parsed the folder /private/var/mobile/Containers/Shared/AppGroup/<GUID>/profile.profile/TabManagerScreenshots and the file /private/var/mobile/Containers/Data/Application/<GUID>/Documents/favicon-url-cache** that contain screenshots of tabs and favicons, **including those of webpages visited in private mode**.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Type**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   06/05/2023   |   20:28   |   00:28   |   Typed “nhl.com” in address bar. Visited NHL website.   |   Normal mode   |   NO   |   YES   |   Cookies   |   YES   |   YES   |   NO   |
|   06/05/2023   |   20:30   |   00:30   |   Clicked on link “Hurricanes succeeding in playoffs with goalie tandem”   |   Normal mode   |   NO   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   06/05/2023   |   20:32   |   00:32   |   Opened second tab   |   Normal mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   06/05/2023   |   20:33   |   00:33   |   Typed “digitalcorpora” in address bar (search) (2nd tab)   |   Normal mode   |   NO   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   06/05/2023   |   20:34   |   00:34   |   Clicked result for “digitalcorpora.org” / visited site (2nd tab)   |   Normal mode   |   NO   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   06/05/2023   |   20:35   |   00:35   |   Opened third tab   |   Normal mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   06/05/2023   |   20:37   |   00:37   |   Opened Private Browsing tab   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   06/05/2023   |   20:38   |   00:38   |   Typed “sans.org” in the address bar. Visited SANS website   |   Private mode   |   Cache   |   Cookies   |   Cache   |   Cache   |   NO   |   NO   |
|   06/05/2023   |   20:39   |   00:39   |   Opened second Private Browsing tab   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   06/05/2023   |   20:41   |   00:41   |   Typed “ESPN” in address bar (search) (2nd Private Browsing tab)   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   06/05/2023   |   20:43   |   00:43   |   Clicked on result for “espn.com” / visited site (2nd Private Browsing tab)   |   Private mode   |   NO   |   Cookies   |   Cookies   |   NO   |   NO   |   NO   |
|   06/05/2023   |   20:44   |   00:44   |   Clicked on the “Find Training” link (1st Private Browsing tab)   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   06/05/2023   |   20:45   |   00:45   |   Closed second Private Browsing tab   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   06/05/2023   |   20:46   |   00:46   |   Bookmarked “digitalcorpora.org” (2nd tab)   |   Normal mode   |   NO   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   06/05/2023   |   20:47   |   00:47   |   Closed 1st tab   |   Normal mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   History   |   NO   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   Cache   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Cookies   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   Bookmarks   |   NO   |   YES   |   NO   |   YES   |   YES   |   NO   |
|   Tabs   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

## Firefox Focus

This secure browser does not store relevant information on the device's internal memory. 

Some execution timestamps can be extracted from:

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/lo.sentry/<ID>/session.current

- /private/var/mobile/Containers/Data/Application/<GUID>/tmp/

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Preferences/org.mozilla.ios.Focus.plist

**None of the compared tools had a parser for this application**.

## Google Chrome

Relevant information for this application is stored in these files and folders:

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Google/Chrome/Default/History

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Google/Chrome/Default/Preferences

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Google/Chrome/Default/Web Data

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Google/Chrome/Default/Bookmarks

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Google/Chrome/Default/Favicons

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Google/Chrome/Default/Top Sites

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Google/Chrome/Default/Cache/

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Google/Chrome/Default/Session/{SyntheticIdentifier}/session.plist

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Google/Chrome/Default/OTR/Sessions/{SyntheticIdentifier}/session.plist

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/WebKit/NetworkCache/Blobs/

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/WebKit/NetworkCache/Records/

**Direct parsing of this application is supported by all tested tools**. Oxygen has the most complete parsing capability, while PA and AXIOM also have a good parsing capability, but PA didn’t parse tabs and AXIOM didn’t parse cookies. **Traces of websites visited in private mode were found in Tabs.**

It’s worth noting that none of the tested tools parsed the folders /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Google/Chrome/Default/Session/{SyntheticIdentifier}/Snapshots/ and /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Google/Chrome/Default/OTR/Sessions/{SyntheticIdentifier}/Snapshots/ that contain screenshots of tabs, **including those of webpages visited in private mode**

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Type**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   05/05/2023   |   11:19   |   15:19   |   Typed “nhl.com” in address bar. Visited NHL website.   |   Normal mode   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   05/05/2023   |   11:20   |   15:20   |   Clicked on link “Hurricanes’ Staal up to task against Hughes”   |   Normal mode   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   05/05/2023   |   11:22   |   15:22   |   Opened second tab   |   Normal mode   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   05/05/2023   |   11:23   |   15:23   |   Typed “digitalcorpora” in address bar (search)   |   Normal mode   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   05/05/2023   |   11:24   |   15:24   |   Clicked result for “digitalcorpora.org” / visited site   |   Normal mode   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   05/05/2023   |   11:26   |   15:26   |   Opened third tab   |   Normal mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   11:27   |   15:27   |   Opened Incognito tab   |   Private mode   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   11:28   |   15:28   |   Typed “sans.org” in the address bar. Visited SANS website   |   Private mode   |   Tabs   |   NO   |   Tabs   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   11:29   |   15:29   |   Opened second Incognito tab   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   11:30   |   15:30   |   Typed “ESPN” in address bar (search) (2nd Incognito tab)   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   11:31   |   15:31   |   Clicked on result for “espn.com” / visited site (2nd Incognito tab)   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   11:32   |   15:32   |   Clicked on the “Find Training” link (1st Incognito tab)   |   Private mode   |   Tabs   |   NO   |   Tabs   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   11:59   |   15:59   |   Closed second Incognito tab   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   05/05/2023   |   12:01   |   16:01   |   Bookmarked “digitalcorpora.org” in second tab   |   Normal mode   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   05/05/2023   |   12:02   |   16:02   |   Closed 1st tab   |   Normal mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- |
|   History   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Cache   |   YES   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   Cookies   |   NO   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   Tabs   |   YES   |   NO   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   Bookmarks   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Favicons   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   Autofill   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |

## Microsoft Edge

Relevant information for this application is stored in these files and folders:

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Microsoft/Edge/Default/History

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/ Microsoft/Edge/Default/Preferences

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/ Microsoft/Edge/Default/Web Data

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/ Microsoft/Edge/Default/Bookmarks

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/ Microsoft/Edge/Default/Favicons

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/ Microsoft/Edge/Default/Top Sites

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/ Microsoft/Edge/Default/Cache/

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/ Microsoft/Edge/Default/Collections/collectionsSQLite

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Microsoft/Edge/Default/Session/{SyntheticIdentifier}/session.plist

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Microsoft/Edge/Default/OTR/Sessions/{SyntheticIdentifier}/session.plist

**Direct parsing of this application is supported by AXIOM, OXYGEN, and BELKASOFT**. Oxygen parses history, cache, cookies, and tabs, while AXIOM parses history, bookmarks, and favicons. **Traces of websites visited in private mode were found in tabs.**

It’s worth noting that none of the tested tools parsed the folders /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Microsoft/Edge/Default/Session/{SyntheticIdentifier}/Snapshots/ and /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Microsoft/Edge/Default/OTR/Sessions/{SyntheticIdentifier}/Snapshots/ that contain screenshots of tabs, **including those of webpages visited in private mode**

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Type**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   06/05/2023   |   10:36   |   14:36   |   Typed “nhl.com” in address bar. Visited NHL website.   |   Normal mode   |   YES   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   06/05/2023   |   10:38   |   14:38   |   Clicked on link “Hurricanes ease past Devils again in Game 2”   |   Normal mode   |   YES   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   06/05/2023   |   10:39   |   14:39   |   Opened second tab   |   Normal mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   06/05/2023   |   10:40   |   14:40   |   Typed “digitalcorpora” in address bar (search) (2nd tab)   |   Normal mode   |   YES   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   06/05/2023   |   10:41   |   14:41   |   Clicked result for “digitalcorpora.org” / visited site (2nd tab)   |   Normal mode   |   YES   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   06/05/2023   |   10:42   |   14:42   |   Opened third tab   |   Normal mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   06/05/2023   |   10:43   |   14:43   |   Opened InPrivate tab   |   Private mode   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   06/05/2023   |   10:44   |   14:44   |   Typed “sans.org” in the address bar (search)   |   Private mode   |   NO   |   NO   |   Tabs   |   NO   |   NO   |   NO   |
|   06/05/2023   |   10:45   |   14:45   |   Clicked on result for “sans.org” / visited site (InPrivate tab)   |   Private mode   |   NO   |   NO   |   Tabs   |   NO   |   NO   |   NO   |
|   06/05/2023   |   10:46   |   14:46   |   Opened second InPrivate tab   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   06/05/2023   |   10:47   |   14:47   |   Typed “espn.com” in address bar / visited ESPN website (2nd InPrivate tab)   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   06/05/2023   |   10:48   |   14:48   |   Clicked on the “Find Training” link (1st InPrivate tab)   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   06/05/2023   |   10:49   |   14:49   |   Closed second InPrivate tab   |   Private mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   06/05/2023   |   10:50   |   14:50   |   Added “digitalcorpora.org” to favorites (2nd tab)   |   Normal mode   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   06/05/2023   |   10:51   |   14:51   |   Closed 1st tab   |   Normal mode   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   History   |   YES   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   Cache   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Cookies   |   NO   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   Tabs   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Bookmarks   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Favicons   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Autofill   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

## Gmail

Relevant information for this application is stored in these files and folders:

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/data/<email\_adress>/sqlitedb

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/WebKit/NetworkCache/Version 16/Blobs

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/WebKit/NetworkCache/Version 16/Records

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/com.google.Gmail/Cache.db

- /private/var/mobile/Containers/Data/Application/<GUID>/Documents/drivekit/users/<Google-User-ID>/gdx-cello/cello.db

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Preferences/com.google.Gmail.plist

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/Library/Preferences/group.com.google.Gmail.plist

**Direct parsing of this application is supported by AXIOM, PA, OXYGEN, XAMN, and iLEAPP**.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Subject**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   20/05/2023   |   09:30   |   13:30   |   Sent message   |   iOS 15 Test   |   Here is your first test message.   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   20/05/2023   |   09:32   |   13:32   |   Received message   |   iOS 15 Test   |   Got it. Here is your reply.   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   20/05/2023   |   09:37   |   13:37   |   Sent message   |   iOS 15 Test Attachment   |   Test attachment message. (attached picture: year I was born)   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   20/05/2023   |   09:30   |   13:30   |   Received message   |   iOS 15 Test Attachment   |   Received. Picture attached. (attached picture: Assault Cow)   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Emails   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Attachments   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |

## Proton Mail

Relevant information for this application is stored in an SQLite database:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/ProtonMail.sqlite

**Messages are stored in an encrypted way in the database: PA and XAMN are the only tools with a decryption and parsing feature for this database**. PA parsed both email header and body, while XAMN only parsed headers. OXYGEN parsed contacts, that are stored unencrypted in the SQLite database.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Subject**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   08/05/2023   |   12:35   |   16:35   |   Sent email   |   iOS 15 Image   |   Here is your first test message.   |   NO   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   08/05/2023   |   12:37   |   16:37   |   Received email   |   Re: iOS 15 Image   |   Got it. Here is your reply.   |   Notifications   |   YES   |   NO   |   YES   |   NO   |   Notifications   |
|   08/05/2023   |   12:41   |   16:41   |   Received email   |   iOS 15 Pic   |   Picture attached. (eight hours of sleep)   |   Notifications   |   YES   |   NO   |   YES   |   NO   |   Notifications   |
|   08/05/2023   |   12:43   |   16:43   |   Saved picture   |    |   eight hours of sleep   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   08/05/2023   |   12:46   |   16:46   |   Sent email   |   Re: iOS 15 Pic   |   Got it. I attached one (Nothing)   |   NO   |   YES   |   NO   |   YES   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   Contacts   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Emails   |   Notifications   |   YES   |   NO   |   YES   |   NO   |   Notifications   |

## Tutanota

Relevant information is probably stored in this encrypted SQLite database:

- /private/var/mobile/Containers/Data/Application/<GUID>/Documents/offline\_NHk0zAV----9.sqlite

**None of the compared tools had a parser for this application** and I was not able to find relevant information in the application folders.

## Microsoft Teams

Relevant information for this application is stored in these files and folders:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/SkypeSpacesDogfood/<GUID>/SkypeSpacesDogood-<Microsoft-ID>.sqlite

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/SkypeSpacesDogfood/Downloads

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/Library/Preferences/group.com.microsoft.skype.teams.plist

**Direct parsing of this application is supported by PA, OXYGEN, XAMN, and BELKASOFT**. All tested tools correctly parsed messages, including the deleted ones. Some differences were noted in the parsing capabilities of other artifacts (i.e., calls, file transfer, locations).

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   28/04/2023   |   09:44   |   13:44   |   Received message   |   You here yet?   |   Notifications   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   09:45   |   13:45   |   Reacted to message   |   (Message received at 09:44) (Thumbs up emoji)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   28/04/2023   |   09:47   |   13:47   |   Sent message   |   I am. Battery and all.   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   09:48   |   13:48   |   Received message   |   Yeah, how is it doing this morning?   |   Notifications   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   09:49   |   13:49   |   Sent message   |   Still horrible. It’s drained like 30 percent in the last hour or so.   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   09:50   |   13:50   |   Received message   |   Yikes! That is bad. When we do location stuff you’ll need to be plugged in.   |   Notifications   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   09:53   |   13:53   |   Sent message   |   This phone is retiring after this image.   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   09:54   |   13:54   |   Received message   |   Definitely. I’ll send you a picture. Hang on.   |   Notifications   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   09:57   |   13:57   |   Received image   |   50% chance   |   Notifications   |   Notifications   |   YES   |   YES   |   NO   |   Notifications   |
|   28/04/2023   |   10:18   |   14:18   |   Sent message   |   That picture is truth.   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   10:19   |   14:19   |   Received message   |   Thought you might like it.   |   Notifications   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   10:20   |   14:20   |   Sent message   |   I will audio call you now.   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   10:21   |   14:21   |   Outgoing audio call   |   (0:22) (Call was disconnected)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   10:23   |   14:23   |   Received message   |   What happened?   |   Notifications   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   10:24   |   14:24   |   Sent message   |   No clue. I’ll try it again.   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   10:25   |   14:25   |   Outgoing audio call   |   (0:20) (Call was disconnected)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   10:27   |   14:27   |   Sent message   |   Well, it happened again. You try audio calling and see what happens.   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   10:28   |   14:28   |   Incoming audio call   |   (1:30)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   10:34   |   14:34   |   Received message   |   Well, that worked. You can try video calling me now.   |   Notifications   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   10:37   |   14:37   |   Outgoing video call   |   (1:35)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   10:42   |   14:42   |   Received message   |   I’ll video call. Standby.   |   Notifications   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   28/04/2023   |   10:43   |   14:43   |   Incoming video call   |   (1:43)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   28/04/2023   |   10:47   |   14:47   |   Sent message   |   I am sending you this message. I will delete it in a minute.   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   28/04/2023   |   10:48   |   14:48   |   Deleted message   |   (Message sent at 10:47)   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   17/05/2023   |   09:10   |   13:10   |   Start sending dynamic location   |    |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   17/05/2023   |   09:15   |   13:15   |   End sending dynamic location   |    |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   17/05/2023   |   09:17   |   13:17   |   Start receiving dynamic location   |    |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   17/05/2023   |   09:22   |   13:22   |   End receiving dynamic location   |    |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   17/05/2023   |   09:27   |   13:27   |   Received static location   |    |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   17/05/2023   |   09:30   |   13:30   |   Sent static location   |    |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Contacts   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Messages   |   Notifications   |   YES   |   YES   |   YES   |   YES   |   Notifications   |
|   Deleted message   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Calls   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Static location   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Dynamic location   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Cache   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |

## Slack

Relevant information for this application is stored in these files and folders:

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Slack/<Slack-Channel-ID>/ModelDatabase/1/main\_db

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Application Support/Slack/<Slack-Channel-ID>/team-preferences

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/com.hackemist.SDImageCache/

**Direct parsing of this application is supported by AXIOM, OXYGEN, XAMN, BELKASOFT, and iLEAPP**. All tested tools correctly parsed messages, including the deleted ones. Some differences were noted in the parsing capabilities of other artifacts (i.e., calls, file transfer, locations).

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   29/04/2023   |   15:04   |   19:04   |   Received message   |   Are you here?   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:05   |   19:05   |   Sent message   |   Present.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:06   |   19:06   |   Changed profile picture   |   HAL   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   29/04/2023   |   15:07   |   19:07   |   Received message   |   Awesome. I don’t think the third guy is coming.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:08   |   19:08   |   Sent message   |   You are correct. Lol.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:10   |   19:10   |   Received message   |   I suppose he’s already here to some degree.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:11   |   19:11   |   Sent message   |   True. Let’s hustle through this, shall we? I’ve got things to do.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:12   |   19:12   |   Received message   |   Works for me. Here comes a picture.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:15   |   19:15   |   Received picture   |   Perfect date   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:17   |   19:17   |   Sent message   |   Lol. ‘Murica. Hang on and I’ll send you one.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:21   |   19:21   |   Sent picture   |   Pulled pork sandwich   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:23   |   19:23   |   Received message   |   Nice! Want to huddle (I think that’s what they’re called here)?   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:25   |   19:25   |   Sent message   |   Sure.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:27   |   19:27   |   Received system message   |   (Huddle started by other party)   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:28   |   19:28   |   Joined huddle   |   (~1:30)   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |
|   29/04/2023   |   15:30   |   19:30   |   Sent message   |   Head over to DM’s.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:31   |   19:31   |   Other party reacted to message   |   (Message sent at 15:30)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   29/04/2023   |   15:32   |   19:32   |   Received message   |   I’m here. It looks like we can huddle here, too.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:33   |   19:33   |   Sent message   |   Yeah, I noticed that.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:34   |   19:34   |   Received message   |   I will send you a picture. Standby.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:35   |   19:35   |   Received picture   |   base64   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:37   |   19:37   |   Sent message   |   You’d be amazed how many people think that.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:38   |   19:38   |   Received message   |   No, no I would not.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:39   |   19:39   |   Sent message   |   Here comes a picture for you.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:40   |   19:40   |   Sent two pictures   |   Unpatched systems and Find out   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:41   |   19:41   |   Received message   |   Did you mean to send two?   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:42   |   19:42   |   Sent message   |   No. Total accident. I will start a huddle in just a moment.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:43   |   19:43   |   Started huddle   |   (~1:30)   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:46   |   19:46   |   Sent message   |   We can only delete messages we send. I will delete this one in a minute.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:47   |   19:47   |   Deleted message   |   (Message sent at 15:46)   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:48   |   19:48   |   Sent message   |   Deleted.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:49   |   19:49   |   Received message   |   (Reply to message sent at 15:48) Thank you.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:50   |   19:50   |   Sent message   |   I will also delete this message.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:51   |   19:51   |   Deleted message   |   (Message sent at 15:50)   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:52   |   19:52   |   Sent message   |   I deleted it.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   29/04/2023   |   15:53   |   19:53   |   Received message   |   (Reply to message sent at 15:52) Awesome! Thanks.   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   Contacts   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   Messages   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   Deleted messages   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   Cache   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |

## Zoom

Relevant information for this application is stored in these files and folders:

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/Library/Caches/contacts.db

- /private/var/mobile/Containers/Data/Application/<GUID>/Documents/data/<User-ID>@xmpp.zoom.us/

- /private/var/mobile/Containers/Data/Application/<GUID>/Library/Caches/us.zoom.videomeetings/Cache.db

**None of the compared tools had a parser for this application**. Some received messages were parsed by AXIOM, PA, OXYGEN, XAMN, and iLEAPP in iOS Notifications.

Following is the comparison table with the details of each action documented by Josh in the creation of the image.

|   **Date**   |   **Time (EDT)**   |   **Time (UTC)**   |   **Action**   |   **Message**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   03/05/2023   |   12:14   |   16:14   |   Login   |    |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:15   |   16:15   |   Set profile picture   |   HAL   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:20   |   16:20   |   Sent message   |   You here yet?   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:21   |   16:21   |   Received message   |   I am. Looks like our chat history populated after all.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   03/05/2023   |   12:22   |   16:22   |   Sent message   |   I know. Odd since it didn’t show up initially.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:24   |   16:24   |   Received message   |   (Shrugging man emoji) Who knows. Anything exciting happening today?   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   03/05/2023   |   12:25   |   16:25   |   Sent message   |   (Reply to message received at 12:24) Nope. Just general stuff.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:26   |   16:26   |   Received message   |   That’s good. You can work on this image.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   03/05/2023   |   12:27   |   16:27   |   Sent message   |   I know. It’s long overdue.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:28   |   16:28   |   Received message   |   Are you doing to do 16 after?   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   03/05/2023   |   12:29   |   16:29   |   Sent message   |   Probably. It needs to be done. I won’t use this phone, though.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:30   |   16:30   |   Received message   |   Makes sense. I am going to send you a picture.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   03/05/2023   |   12:31   |   16:31   |   Reacted to message   |   (Message received at 12:30) (Thumbs up emoji)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:32   |   16:32   |   Received picture   |   Last day of 2023   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   03/05/2023   |   12:34   |   16:34   |   Sent message   |   Got it. I’ll send you one.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:35   |   16:35   |   Sent picture   |   Close the lid   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:37   |   16:37   |   Received message   |   I’ll audio call you now.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   03/05/2023   |   12:38   |   16:38   |   Incoming audio call   |   (~2:00)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:42   |   16:42   |   Sent message   |   My turn to call you.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:43   |   16:43   |   Outgoing audio call   |   (~1:30)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:47   |   16:47   |   Received message   |   I will video call you now.   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   03/05/2023   |   12:48   |   16:48   |   Incoming video call   |   (   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:53   |   16:53   |   Sent message   |   Ok. I’ll video call you.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:54   |   16:54   |   Outgoing video call   |   (~1:30)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   12:58   |   16:58   |   Sent message   |   We can only delete our own messages. I will delete this one in a minute.   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   03/05/2023   |   13:01   |   17:01   |   Deleted message   |   (Message sent at 12:58)   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

The overall results for each category and tool are shown in the following table.

|    |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Account Information   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Contacts   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Messages   |   Notifications   |   Notifications   |   NO   |   Notifications   |   NO   |   Notifications   |
|   Deleted message   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Calls   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Cache   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |

## Final comments

Some final comments after this complex post.

The most commonly used browsers in the iOS world (Safari, Google Chrome, and Mozilla Firefox) are supported by most of the tested tools, but there are still some unexplored areas, like tabs screenshots. Information about websites visited in Private Browsing were recovered for Google Chrome (Tabs), Mozilla Firefox (Tabs, Cookies, Cache, and Favicons), and Microsoft Edge (Tabs). Support for Gmail email client is wide, but encrypted email clients are only supported by some tools (Proton Mail is supported by PA and XAMN) or not supported at all (Tutanota). Common productivity applications like Microsoft Teams and Slack are supported by most of the tested tools and for both of them all tools were also able to recover the deleted message. Zoom databases seem encrypted, so messages were recovered only from iOS notifications.

The following tables contain an overview of which tools parsed which applications. Be careful in evaluating the quality of extracted date, as in some cases tools have a very limited parsing capability (i.e., extracting the user account information).

|   **App Name**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Brave   |   YES   |   NO   |   YES   |   NO   |   YES   |   NO   |
|   Chrome   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   DuckDuckGo   |   YES   |   NO   |   YES   |   NO   |   NO   |   NO   |
|   Firefox   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Firefox Focus   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Microsoft Edge   |   YES   |   NO   |   YES   |   NO   |   YES   |   NO   |

  

|   **App Name**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- |
|   Gmail   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Microsoft Teams   |   NO   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Proton Mail   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |
|   Slack   |   YES   |   NO   |   YES   |   YES   |   YES   |   YES   |
|   Tutanota   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Zoom   |   NO   |   NO   |   YES   |   NO   |   NO   |   NO   |

That’s all for this episode! The next one will be about cloud storage, photos and videos, navigation, news, and music applications!

  

Go to Source
