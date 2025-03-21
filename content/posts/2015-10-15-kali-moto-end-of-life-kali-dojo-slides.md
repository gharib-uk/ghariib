---
title: "Kali Moto End of Life Kali Dojo Slides"
date: Thu, 15 Oct 2015 00:00:00 +0000
draft: false
type: posts
---
# Kali Moto End of Life Kali Dojo Slides
https://www.kali.org/blog/kali-moto-eol/images/kali-1.x.x-end-of-life-v3.jpg
<br/>

<br/>
Kali Sana Release Aftermath Kali Linux 2.0 has been out for a couple of months and the response has been great, with well over a million unique downloads of Kali 2.0 as a testament. Release day was somewhat hectic for us, as we did not anticipate the sheer volume of traffic
<br/>
Kali Sana Release Aftermath
---------------------------

Kali Linux 2.0 has been out for a couple of months and the response has been great, with well **over a million unique downloads of Kali 2.0** as a testament. Release day was somewhat hectic for us, as we did not anticipate the sheer volume of traffic … which we somehow always underestimate. In the first few days after the release of 2.0, we had **ten times** the download volume of 1.0 in a similar period, back in 2013.

Kali Moto Repository Purge
--------------------------

We’ve given Kali Moto (1.0) a good two months of grace time and will be purging the unsupported 1.0 distribution files from our repositories in the next few days. If you’re still using Kali 1.0 then it’s definitely time to either upgrade or update:

```sh
cat << EOF > /etc/apt/sources.list
deb http://http.kali.org/kali sana main contrib non-free
deb http://security.kali.org/kali-security/ sana/updates main contrib non-free
EOF
apt-get update
apt-get dist-upgrade # get a coffee, or 10.
reboot
```

If for some reason you can’t upgrade, we’ve set aside an archive mirror of Kali 1.0, which can be set as follows:

```sh
cat << EOF > /etc/apt/sources.list
deb http://old.kali.org/kali moto main contrib non-free
EOF
```

Please note, this repository will not be maintained or updated.

Kali Linux Dojo 2015 Materials
------------------------------

It occurred to us that after the BlackHat and DEF CON conferences, we never made a public announcement regarding the availability of our [Kali Linux Dojo 2015 slides](https://www.kali.org/docs/development/dojo-mastering-live-build/) and notes. The Dojo was **great fun** - we definitely enjoyed showing our brand spanking new Kali 2.0 to the the crowds, who built their own Kali 2.0 ISOs a day before release.

[![](https://www.kali.org/blog/kali-moto-eol/images/dojo-slide-e1444909261539.png)](https://www.kali.org/blog/kali-moto-eol/images/dojo-slide-e1444909261539.png)

#### [Source](https://www.kali.org/blog/kali-moto-eol/)

<br/>
---
