---
title: "OpenCTF Ellingson Mineral 2"
date: Sun, 12 Aug 2018 09:37:59 +0000
draft: false
type: posts
---
# OpenCTF Ellingson Mineral 2

<br/>

<br/>
**Category:** OSINT

**Points:** 100

**Description:**

```
We heard word that The Plague escaped prison three weeks ago. We've been notified that he was recently seen on soundcloud liking a song by ytcracker called "hacker music." Let us know what you find.
```

First perform a search on soundcloud.com for “ytcracker hacker music” then go to likes. `https://soundcloud.com/ytcracker/ytcracker-hacker-music/likes`

Go to ThePlague2018x’s profile

`https://soundcloud.com/user-843651506`

The next clue seemed to be to go to the website on ThePlague2018x’s website which is the following

```
https://exit.sc/?url=https%3A%2F%2FNjY2YzYxNjc3Yjc0Njg0NTUyNjU1ZjY5NzM1ZjRlMzA1ZjcyMzE0NzY4NzQ1ZjYxNGU0NDVmNTc1MjMwNmU0NzVmNzQ2ODMzNTI2NTVmMzE3MzVmNGY0ZTZjNzk1ZjQ2NzU0ZTVmNDE2ZTY0NWY0MjMwNzI2OTRlNDc3ZAo.com%2Fhome
```

Extract the string after the hex values “%3A%2F%2F” to “.com” the value between this is the following

```
666c61677b74684552655f69735f4e305f72314768745f614e445f5752306e475f74683352655f31735f4f4e6c795f46754e5f416e645f423072694e477d
```

Looking at the string it seems to be hex so encode to hex to get the flag.

```
>>> '666c61677b74684552655f69735f4e305f72314768745f614e445f5752306e475f74683352655f31735f4f4e6c795f46754e5f416e645f423072694e477d'.decode('hex')
'flag{thERe_is_N0_r1Ght_aND_WR0nG_th3Re_1s_ONly_FuN_And_B0riNG}'
```

Flag
====

**flag{thERe\_is\_N0\_r1Ght\_aND\_WR0nG\_th3Re\_1s\_ONly\_FuN\_And\_B0riNG}**

#### [Source](http://b0tchsec.com/2018/openctf/ellingsonmineral2)

<br/>
---
