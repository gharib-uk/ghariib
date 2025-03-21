---
title: "Technology Deep Dive Building a Faster ORAM Layer for Enclaves"
date: Fri, 19 Aug 2022 00:00:00 +0000
draft: false
type: posts
---
# Technology Deep Dive Building a Faster ORAM Layer for Enclaves

<br/>

<br/>
 Private doesn’t mean lonely. Signal’s mission is to help you connect with people in a secure way. This means that you need to know which of your contacts are also on Signal. This is called “contact
<br/>
![Header Image](/blog/images/cdsi-header.png)

Private doesn’t mean lonely. Signal’s mission is to help you connect with people in a secure way. This means that you need to know which of your contacts are also on Signal. This is called “contact discovery” and it’s an essential part of any messaging app.

Most other communications apps enable contact discovery by maintaining their own internal social graph or by uploading and storing your full address book on their servers. But Signal enables you to find and connect with people in a privacy-preserving way. In short, we let you see which of your friends use Signal, but we don’t want to know who your friends are. Letting you know which of your contacts are using Signal while preserving the privacy of your social map requires some complex engineering choices on our end.

In this post, we’ll be diving into some new ways that improve our ability to handle contacts in a private way. That’s right folks. We’re getting technical. We’re talking enclaves and memory access patterns. \[If you’ve never heard of an enclave before, don’t worry. We’ll explain it soon.\]

Our new method is faster and more efficient. It lays the groundwork for the introduction of usernames and phone number privacy which will offer new privacy controls around your phone number’s visibility on Signal.

[_Read more..._](https://signal.org/blog/building-faster-oram/)

#### [Source](https://signal.org/blog/building-faster-oram/)

<br/>
---
