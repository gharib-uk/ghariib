---
title: "Kalis stable Docker image is now named kali-last-release"
date: Wed, 12 Jan 2022 00:00:00 +0000
draft: false
type: posts
---
# Kalis stable Docker image is now named kali-last-release

https://www.kali.org/blog/renaming-kali-stable-docker-image/images/banner.jpg



Here is a very quick announcement for users of the Kali Linux Docker Images. Until recently we used to have a Docker image named simply kali, and it was built from the last versioned release of Kali (e.g. 2019.4, 2020.1, etc.) matching our &ldquo;kali-last-snapshot&rdquo; network repositories branch. In a way, this

Here is a very quick announcement for users of the [Kali Linux Docker Images](https://www.kali.org/docs/containers/official-kalilinux-docker-images/).

Until recently we used to have a Docker image named simply `kali`, and it was built from the last [versioned release](https://www.kali.org/releases/) of Kali (e.g. 2019.4, 2020.1, etc.) matching our [“kali-last-snapshot” network repositories branch](https://www.kali.org/docs/general-use/kali-branches/). In a way, this is our “stable” release, as it will only get updates quarterly as it is in synchronisation with our release cycle.

We still provide this **Docker image**, but now it has been **renamed from `kali` to [`kali-last-release`](https://hub.docker.com/r/kalilinux/kali-last-release)** for clarity.

The old `kali` Docker image is still around for the time being, but if you use it you will see a deprecation warning to indicate that it is in the **process of being phased out**. The rest of the deprecation plan is as follow:

-   Upon Kali `2022.1` release, we will update the image to make it fail, meaning that the container will error out with a helpful message when you try to run it.
-   Upon Kali `2022.2` release, we will completely remove it from [our Docker Hub](https://hub.docker.com/r/kalilinux/).

So if you still use this image in your scripts or Dockerfiles, **please update** those and use `kali-last-release` instead.

An additional reminder: if you are after the **most up-to-date version of Kali, you probably want to use the [kali-rolling](https://hub.docker.com/r/kalilinux/kali-last-release) Docker image**. This is the “main” image _(which matches our other platforms)_, and it is updated every week _(the network packages get updated daily also like all other Kali platforms)_.

For lots more details, please refer to our [official Kali Docker documentation](https://www.kali.org/docs/containers/official-kalilinux-docker-images/).

Thank you and happy 11111100110!

PS – On a related topic: the Kali images were recently added to the [containers shortnames list](https://github.com/containers/shortnames). It means that **Podman users** can run a Kali container simply by typing `podman run -it kali-rolling`, rather than using the full image name `docker.io/kalilinux/kali-rolling`. This works if the host system provides a very up-to-date shortnames list in `/etc/containers/registries.conf.d/shortnames.conf`. _If your host system is Kali Rolling, that’s already the case!_

#### [Source](https://www.kali.org/blog/renaming-kali-stable-docker-image/)

