---
title: "New Release Rdsys 10"
date: 2025-02-24T00:00:00Z
draft: false
type: posts
---
# New Release Rdsys 10





 After years of development, we are officially releasing Rdsys 1.0. Although Rdsys has already been the sole mechanism for distributing bridges since it replaced

  ![](https://blog.torproject.org/new-release-rdsys-1-0/lead.png)

After years of development, we are officially releasing [Rdsys 1.0](https://gitlab.torproject.org/tpo/anti-censorship/rdsys/). Although Rdsys has already been the sole mechanism for distributing bridges [since it replaced BridgeDB last October](https://blog.torproject.org/making-connections-from-bridgedb-to-rdsys/), this version 1.0 milestone officially marks its new status.

We now consider Rdsys stable, but our work is far from finished. We are committed to improving Rdsys and fixing issues as they arise, and to adapting quickly to censors' evolving tactics.

Full changelog since 0.14.2 (when we fully replaced BridgeDB for Rdsys):

-   fix multiple race conditions ([#223](https://gitlab.torproject.org/tpo/anti-censorship/rdsys/-/issues/223))
-   core: propagate non working bridges to distributors ([#245](https://gitlab.torproject.org/tpo/anti-censorship/rdsys/-/issues/245))
-   moat: the type of the captcha bridge response is 'moat-bridges'
-   email: fix imap freeze ([#129](https://gitlab.torproject.org/tpo/anti-censorship/rdsys/-/issues/129))
-   email: attach a qr of the bridgelines ([#244](https://gitlab.torproject.org/tpo/anti-censorship/rdsys/-/issues/244))
-   telegram: send a qr of the bridgelines ([#243](https://gitlab.torproject.org/tpo/anti-censorship/rdsys/-/issues/243))
-   https: distribute vanilla bridges ([#239](https://gitlab.torproject.org/tpo/anti-censorship/rdsys/-/issues/239))
-   https: reduce logs for common failures
-   bridgedb-metrics: use country if available ([#235](https://gitlab.torproject.org/tpo/anti-censorship/rdsys/-/issues/235))

-   [circumvention](https://blog.torproject.org/category/circumvention)
-   [community](https://blog.torproject.org/category/community)
-   [releases](https://blog.torproject.org/category/releases)
-   [human rights](https://blog.torproject.org/category/human-rights)
-   [announcements](https://blog.torproject.org/category/announcements)

#### [Source](https://blog.torproject.org/new-release-rdsys-1-0/)

