---
title: "The Time Zone Database Package (tzdata) review for 2024"
date: 2025-01-02
categories: 
  - "general"
---

The Time Zone Database Package (`tzdata`) provides Red Hat Enterprise Linux (RHEL) with the time zone information needed for all applications or runtimes in the operating system to print local time correctly. The GNU C Library (glibc) uses the `tzdata` package, so APIs like `strftime()` work correctly, while applications such as `/usr/bin/date` use this information to print the local date.

The `tzdata` package contains the data files describing current and historic transitions for various time zones worldwide. This data represents changes required by local governments, time zone boundary changes, UTC offsets, and daylight saving time (DST).

The start of 2024 included updates identified in the final days of 2023. Released early in January 2024, tzdata-2023d included changes for America/Scoresbysund and Antarctica/Vostok. Tzdata-2023d also updated the expiration date for the leapseconds file. Leap seconds were valid until June 28, 2024 but were marked to expire on December 28, 2023. No new leap seconds were added. A month later, tzdata-2024a shipped with changes for Asia/Almaty, Asia/Qostanay, Asia/Gaza, and Asia/Hebron.

Between February and September of 2024 there were several changes to both data and code. There were corrections to past time zone data but no new future time zone changes were introduced. The upstream project offered an optional update with these changes, tzdata-2024b. Most of these changes did not impact RHEL users as we continue to use the rearguard format on RHEL. The rearguard format provides better compatibility for downstream parsers.  

RHEL users may notice a change to move `TZ=‘EET’`, `TZ=‘WET’`, and `TZ=’MET’` to backward. These names were only present to provide compatibility with UNIX System V. Also notable, `$ TZ=’MET’` now behaves the same as `TZ=’CET’`.

There was also a change to use `%z` for main and vanguard formats. For example, America/Sao\_Paulo now contains the zone continuation line `-3:00 Brazil %z`, rather than the previous `-3:00 Brazil -03/-02`. This does not currently impact rearguard format.

As always, in order to prevent last minute timezone updates, it is recommended that local governing bodies plan changes ahead, preferably at least a year in advance. Change notification should be sent to the upstream project (tz@iana.org). Whenever possible, include links to official documents recording the change.

For more details on changes in `tzdata` during 2024, see the updated NEWS file included in each release. For the latest status on Red Hat `tzdata` updates, follow the Red Hat Enterprise Linux Timezone Data (tzdata) - Development Status Page.

The post The Time Zone Database Package (tzdata) review for 2024 appeared first on Red Hat Developer.  
  

Go to Source
