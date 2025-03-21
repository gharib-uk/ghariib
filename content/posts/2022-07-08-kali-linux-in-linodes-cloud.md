---
title: "Kali Linux in Linodes Cloud"
date: Fri, 08 Jul 2022 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux in Linodes Cloud

https://www.kali.org/blog/kali-and-linode/images/linode-banner.jpg



A few months ago, Linode reached out to us asking &ldquo;What would be needed in order to get Kali added to Linode?&rdquo;. We explained to them how all the build-scripts that we used to create Kali are public, and what their different options and configurations mean. They went away and

A few months ago, [Linode](https://www.linode.com/) reached out to us asking “What would be needed in order to get Kali added to Linode?”. We explained to them how all the [build-scripts](https://gitlab.com/kalilinux/build-scripts) that we used to create Kali are public, and what their different options and configurations mean. They went away and came back shortly with an image for us to try out! After a bit of testing, we can now say “Kali is in Linode… (Twice)”!

Twice? You can get Kali two ways. Either:

-   Create a new Linode and select Kali as the Distribution. This gives you a bare install of Kali without any tools.
-   Alternatively, go to Linode’s marketplace, and select Kali, and scroll down…

Using Linode’s marketplace allows you to customize your Kali installation directly in the web browser! You will be asked a series of questions allowing you to personalize the installation, without having to SSH in, such as as “which [metapackages](https://www.kali.org/docs/general-use/metapackages/) to install _(none, default, or everything)_” or “do you want GUI access _(via VNC)_”? That’s pretty cool right?!

[![marketplace options](https://www.kali.org/blog/kali-and-linode/images/marketplace-02.png)](https://www.kali.org/blog/kali-and-linode/images/marketplace-02.png)

Which option is best? That depends on you. Some people like to have a [bare install](https://www.kali.org/docs/installation/barebone-kali/), without any tools, and as little packages as possible. You can install whichever package you want. This will reduce the running cost of the cloud instance and help you understand your system environment better. However, if you want to get going as quickly as possible, or be in a more familiar graphical environment (using Xfce), then the market place option may be better!

[![distribution](https://www.kali.org/blog/kali-and-linode/images/distribution-02.png)](https://www.kali.org/blog/kali-and-linode/images/distribution-02.png)

[![marketplace](https://www.kali.org/blog/kali-and-linode/images/marketplace-01.png)](https://www.kali.org/blog/kali-and-linode/images/marketplace-01.png)

Kali is **free** in Linode’s marketplace, the only cost is the running cost of a Cloud instance. How much is that? That depends on the system requirements you pick! There aren’t any extra changes for using Kali.

What’s the username/password to connect? That depends! The username will be `root` _(by default)_, and the password will be what you set during setup. You can also use SSH keys if you selected one during setup also. If you installed via the marketplace, you can also use VNC details you entered.

It goes without saying, also make sure you have permission. Make sure to read the [Linode’s small print](https://www.linode.com/legal/), read over their [security page](https://www.linode.com/security-solutions/) and [open up a Linode ticket](https://cloud.linode.com/support/tickets) if you need it in writing!

If you want more to read, check [Linode’s blog post](https://www.linode.com/blog/linux/kali-linux-available-on-linode/), [Linode’s documentation](https://www.linode.com/docs/products/tools/marketplace/guides/kali-linux/), as well as [Kali’s documentation](https://www.kali.org/docs/).

Happy Hacking!

#### [Source](https://www.kali.org/blog/kali-and-linode/)

<br/>
---
