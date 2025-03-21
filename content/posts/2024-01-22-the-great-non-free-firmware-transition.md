---
title: "The great non-free-firmware transition"
date: Mon, 22 Jan 2024 00:00:00 +0000
draft: false
type: posts
---
# The great non-free-firmware transition

https://www.kali.org/blog/non-free-firmware-transition/images/banner-non-free-firmware.jpg



TL;DR: Dear Kali user, when you have a moment, check your /etc/apt/sources.list, and add non-free-firmware if ever it&rsquo;s missing. Programmatically speaking: kali@kali:~$ sudo sed -i 's/non-free$/non-free non-free-firmware/' /etc/apt/sources.list Long story now. As you might know already, Kali Linux is a Debian-based Linux distribution. As such, it inherits a number of things from Debian, and

TL;DR: Dear Kali user, when you have a moment, check your `/etc/apt/sources.list`, and add `non-free-firmware` if ever it’s missing.

Programmatically speaking:

```console
kali@kali:~$ sudo sed -i 's/non-free$/non-free non-free-firmware/' /etc/apt/sources.list
```

* * *

Long story now.

As you might know already, Kali Linux is a [Debian-based Linux distribution](https://www.kali.org/docs/policy/kali-linux-relationship-with-debian/). As such, it inherits a number of things from Debian, and in particular, the structure of the package repository.

For anyone familiar with Kali, you already know that the package repository is split into different _archive areas_ (also called _components_). Historically, there’s always been 3 components: [`main`, `contrib` and `non-free`](https://http.kali.org/kali/dists/kali-rolling/). However, this changed last year, when [Debian introduced a new component called `non-free-firmware`](https://www.debian.org/releases/bookworm/amd64/release-notes/ch-whats-new.en.html#archive-areas).

Kali Linux followed suite, and introduced the `non-free-firmware` component back in version [2023.1](https://www.kali.org/blog/kali-linux-2023-1-release/). However, so far it’s been empty, and firmware were still part of the `non-free` component. This changed last week: firmware are now located in the `non-free-firmware` component. In practice, it means that _non-free-firmware must be enabled in your /etc/apt/sources.list_, otherwise firmware would not get updated when you run your favorite command `apt update && apt full-upgrade`.

For anyone who installed Kali post 2023.1, `non-free-firmware` is already enabled in your `sources.list`. But it does not hurt to check, so here’s how it should look like:

```console
kali@kali:~$ cat /etc/apt/sources.list
deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware
```

If ever `non-free-firmware` is missing, please edit the file `/etc/apt/sources.list` to add it. Or, just do it with this one-liner:

```console
kali@kali:~$ sudo sed -i 's/non-free$/non-free non-free-firmware/' /etc/apt/sources.list
```

Then complete the job with the traditional `sudo apt update`. No error? You’re done.

Thanks for your attention!

#### [Source](https://www.kali.org/blog/non-free-firmware-transition/)

<br/>
---
