---
title: "Kali Linux Repository HTTPS Support"
date: Mon, 24 Apr 2017 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux Repository HTTPS Support

https://www.kali.org/blog/kali-linux-repository-https-support/images/kali-repo-https.jpg



A couple of weeks back we added more HTTPS support to our Kali infrastructure, and wanted to give our users some guidance and point out what&rsquo;s new. While our Kali Linux download page (and shasums) has always been served via HTTPS, our mirror redirector has not. Now that we generate

A couple of weeks back we added more HTTPS support to our Kali infrastructure, and wanted to give our users some guidance and point out what’s new. While our [Kali Linux download page](https://www.kali.org/get-kali/) (and shasums) has always been served via HTTPS, our mirror redirector has not. Now that we generate weekly images, secure access to the mirror redirector has become crucial.

### [https://cdimage.kali.org](https://cdimage.kali.org)

This is our Kali Image Mirror Redirector. This server accepts your download requests from our [official download page](https://www.kali.org/get-kali/), and then serves your requested file from the geographically closest mirror. This is also the download point for our [Kali Weekly builds](https://cdimage.kali.org/kali-weekly/) - now with fresh and shiny HTTPS support. Hitting this redirector via HTTPS will redirect your request to an SSL enabled download server, while an unencrypted HTTP request will redirect to an HTTP enabled mirror. Where’s the catch? Not all donated mirrors support HTTPS, so choosing this transport may result in slower download speeds. Should downloading a Kali image over HTTP be a security concern? Not if you [GPG verify your downloaded image](https://www.kali.org/docs/introduction/download-official-kali-linux-images/).

### [https://http.kali.org](https://http.kali.org)

As a byproduct of enabling HTTPS on cdimage.kali.org, we now also support **apt** HTTPS transports. This means that our actual Kali package repositories can support HTTPS - resulting in encrypted Kali updates and upgrades. Surprisingly, this does not add much security to the update / upgrade process (read [here](https://askubuntu.com/questions/146108/how-to-use-https-with-apt-get) if you’re wondering why) - however it \*does\* add an extra layer of security, so we figured, “why not?”. To enable the **apt** HTTPS transport, first make sure the **apt-transport-https** package is installed (it’s installed by default in our weekly images and upcoming releases) and enable the HTTPS transport in your sources.list file as shown below:

```console
root@kali:~# apt install apt-transport-https
root@kali:~# cat /etc/apt/sources.list
deb https://http.kali.org/kali kali-rolling main contrib non-free
# deb-src https://http.kali.org/kali kali-rolling main contrib non-free
root@kali:~#
```

Now any update or upgrade operation preformed against our mirrors will be HTTPS enabled:

```console
root@kali:~# apt update
Hit:1 https://archive-3.kali.org/kali kali-rolling InRelease
Reading package lists... Done
root@kali:~#
```

As not all donated mirrors come with HTTPS support, shifting to the HTTPS transport may result in a less optimized mirror being selected for you, resulting in slower download speeds. As moving to an apt HTTPS transport [does not provide much extra security](https://askubuntu.com/questions/146108/how-to-use-https-with-apt-get), do so only if you feel you must!

#### [Source](https://www.kali.org/blog/kali-linux-repository-https-support/)

