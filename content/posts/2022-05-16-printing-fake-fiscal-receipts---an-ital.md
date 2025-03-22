---
title: "Printing Fake Fiscal Receipts - An Italian Job p2"
date: Mon, 16 May 2022 10:00:30 +0000
draft: false
type: posts
---
# Printing Fake Fiscal Receipts - An Italian Job p2





TL;DR The ItalRetail RistorAndro app installed on the SpiceT fiscal printer is affected by a pre-authentication remote arbitrary file write and an arbitrary app installation. Moreover, the Android OS version installed is affected by two known vulnerabilities, namely CVE-2017-13156 (Janus), that allows to esclate the privileges to system, and CVE-2016-5195 (DirtyCOW) that

TL;DR
-----

The ItalRetail RistorAndro app installed on the SpiceT fiscal printer is affected by a **pre-authentication remote arbitrary file write** and an **arbitrary app installation**.

Moreover, the Android OS version installed is affected by two known vulnerabilities, namely [CVE-2017-13156 (Janus)](https://source.android.com/security/bulletin/2017-12-01), that allows to esclate the privileges to **system**, and [CVE-2016-5195 (DirtyCOW)](https://dirtycow.ninja/) that allows to escalate the privileges to **root** in the **vold** SELinux context.

Rewind ⏮
--------

In [the first post](https://www.shielder.com/blog/2022/04/printing-fake-fiscal-receipts-an-italian-job-p.1/) we analyzed the fiscal unit and its local attack surface. We discovered how it’s possible for any installed Android app to abuse the fiscal features.

#### [Source](https://www.shielder.com/blog/2022/05/printing-fake-fiscal-receipts-an-italian-job-p.2/)

