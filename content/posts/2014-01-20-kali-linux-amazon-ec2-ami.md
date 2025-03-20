---
title: "Kali Linux Amazon EC2 AMI"
date: Mon, 20 Jan 2014 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Kali Linux Amazon EC2 AMI
https://www.kali.org/blog/kali-linux-amazon-ec2-ami/images/kali-in-the-cloud.jpg
<br/>

<br/>
### Kali Linux in the Amazon EC2 Marketplace

**EDIT**: For updated Kali Rolling images in the Amazon AWS, check [this post](https://www.kali.org/blog/kali-linux-aws-cloud/).

After several weeks of “back and forth” with the Amazon EC2 team, Kali Linux has finally been approved into the Amazon EC2 marketplace. This means that our users can now activate and access Kali Linux instances in the Amazon cloud quickly and easily. We are “selling” these images on the marketplace for **free**, so other than the regular amazon charges, there no extras to pay. We have currently published a single 64 bit [minimal instance of Kali Linux](https://aws.amazon.com/marketplace/pp/B08LL91KKB), which can be found in the marketplace by searching for “Kali Linux” or accessed via its [direct link](https://aws.amazon.com/marketplace/pp/B08LL91KKB).

[![](https://www.kali.org/blog/kali-linux-amazon-ec2-ami/images/amazon-kali-marketplace.png)](https://www.kali.org/blog/kali-linux-amazon-ec2-ami/images/amazon-kali-marketplace.png)

Due to Amazon AMI image guidelines, the instance does not use the root account by default. Once you have obtained your SSH key from Amazon, you need to connect to your instance using the “kali” user, from which you can then sudo to root if needed. The image is a barebones Kali installation, allowing you to install any toolset you like, while still remaining small and lightweight. If you plan on using Kali Linux for any aggressive scanning on the Internet, make sure to update the Amazon security team through their [Penetration Testing Request](https://aws.amazon.com/security/penetration-testing/) form.

### Build Your Own Kali Images in the Amazon Cloud

If you would like to build your own Kali Linux Amazon Machine Images, you can use our [Kali cloud build-scripts](https://gitlab.com/kalilinux/build-scripts/kali-cloud) , which we published as part of our [Kali 1.0.6 release](https://www.kali.org/blog/kali-linux-1-0-6-release/). These scripts can be edited to create your own custom images with whatever toolset you wish.

### Ask Us Anything on Reddit

Tomorrow, Tuesday 21st Jan, 2014 from 1300 - 1500 EST, we are conducting a [Reddit AMA](https://www.reddit.com/r/netsec/comments/1vryus/we_are_offensive_security_we_do_kali_linux/) (Ask Me Anything), revolving around OffSec, Kali Linux, and our other projects. You are welcome to join and ask us anything!

#### [Source](https://www.kali.org/blog/kali-linux-amazon-ec2-ami/)

<br/>
---
