---
title: "Hunting for bugs in Telegrams animated stickers remote attack surface"
date: Tue, 16 Feb 2021 09:00:00 +0100
draft: false
type: posts
---
# Hunting for bugs in Telegrams animated stickers remote attack surface





Introduction At the end of October &lsquo;19 I was skimming the Telegram&rsquo;s android app code, learning about the technologies in use and looking for potentially interesting features. Just a few months earlier, Telegram had introduced the animated stickers; after reading the blogpost I wondered how they worked under-the-hood and if they

Introduction
------------

At the end of October ‘19 I was skimming the [Telegram’s android app code](https://github.com/drklo/telegram), learning about the technologies in use and looking for potentially interesting features. Just a few months earlier, Telegram had introduced the [animated stickers](https://telegram.org/blog/animated-stickers); after reading the blogpost I wondered how they worked _under-the-hood_ and if they created a new image format for it, then forgot about it. Back to the skimming, I stumbled upon the [**rlottie** folder](https://github.com/DrKLO/Telegram/tree/master/TMessagesProj/jni/rlottie) and started googling. It turned out to be the [Samsung native library](https://github.com/samsung/rlottie) for playing Lottie animations, originally created by [Airbnb](http://airbnb.io/lottie/#/). I don’t know about you but the combination of **Telegram**, **Samsung**, **native** and **animations** instantly triggered my interest in learning more 👀.

#### [Source](https://www.shielder.com/blog/2021/02/hunting-for-bugs-in-telegrams-animated-stickers-remote-attack-surface/)

