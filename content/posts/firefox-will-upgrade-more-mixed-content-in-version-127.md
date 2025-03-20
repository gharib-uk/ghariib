---
title: "Firefox will upgrade more Mixed Content in Version 127"
date: 2025-01-19
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "firefox"
  - "security"
  - "vulnerabilities"
---

Most of the web already supports HTTPS: In fact, 93% of requests made by Firefox are already HTTPS. As a reminder, HTTP over TLS (HTTPS) fixes the security shortcoming of HTTP by creating a secure and encrypted connection. Oftentimes, when web applications enable encryption with HTTPS on their servers, legacy content may still contain references using HTTP, even though that content would also be available over a secure and encrypted connection. When such a document gets loaded over HTTPS but subresources like images, audio and video are loaded using HTTP, it is referred to as “mixed content”.

Starting with version 127, Firefox is going to automatically upgrade audio, video, and image subresources from HTTP to HTTPS.

## Background

When introducing the notion of “mixed content” a long while ago, browsers used to make a fairly sharp distinction between active and passive mixed content: Loading scripts or iframes over HTTP can be really detrimental to the whole document’s security and has long since been blocked as “active mixed content”. Images and other resources were otherwise called “passive” or “display” mixed content. If a network attacker could modify them, they would not gain full control over the document. So, in hope of supporting most existing content, passive content had been allowed to load insecurely, albeit with a warning in the address bar.

![](file:///Users/freddy/tmp/blog%20post%20-%20Shipping%20Mixed%20Content%20Level%202/images/image2.png)

![Previous behavior, without upgrading: Degraded lock icon, with a warning sign in the lower right corner.](https://blog.mozilla.org/security/files/2024/06/image2.png)

Previous behavior, without upgrading: Degraded lock icon, with a warning sign in the lower right corner.

With the web platform supporting many new and exciting forms of content (e.g., responsive images), that notion became a bit blurry: Responsive images are not active in a sense that a malicious responsive image can take over the whole web page. However, with an impetus toward a more secure web, since 2018, we require that new features are only available when using HTTPS.

## Upgradable and blockable mixed content

Given these blurry lines between active and passive mixed content, the latest revision of the Mixed content standard distinguishes between blockable and upgradable content, where scripts, iframes, responsive images and really all other features are considered blockable. The formerly-called passive content types (`<img>`, `<audio>` and `<video>` elements) are now being upgraded by the browser to use HTTPS and are not loaded if they are unavailable via HTTPS.

This also introduces a behavior change in our security indicators: Firefox will no longer make use of the tiny warning sign in the lower right corner of the lock icon:

![After our change. A fully secure lock icon. The image load was successfully upgraded or failed (e.g. Connection Reset)](https://blog.mozilla.org/security/files/2024/06/image1.png)

After our change. A fully secure lock icon. The image load was successfully upgraded or failed (e.g., Connection Reset).

With Firefox 127, all mixed content will either be blocked or upgraded. Making sure that documents transferred with HTTPS remain fully secure and encrypted.

## Enterprise Users

Enterprise users that do not want Firefox to perform an upgrade have the following options by changing the existing preferences:

- Set `security.mixed_content.upgrade_display_content` to `false`, such that Firefox will continue displaying mixed content insecurely (including the degraded lock icon from the first picture).
- Set `security.mixed_content.block_display_content` to `true`, such that Firefox will block all mixed content (including upgradable).

Reasons for changing these preferences might include legacy infrastructure that does not support a secure HTTPS experience. We want to note that neither of these options are recommended because with those, Firefox would deviate from an interoperable web platform. Furthermore, these preferences do not receive the amount of support, scrutiny and quality assurance as those available in our built-in settings page.

## Outlook

We will continue our mission where privacy and security is not optional, to bring yet more HTTPS to the web: Next up, we are going to default all addresses from the URL bar to prefer HTTPS, with a fallback to HTTP if the site does not load securely. This feature is already available in Firefox Nightly.

We are also working on another iteration that upgrades more page loads with a fallback called “HTTPS-First” that should be in Firefox Nightly soon. Lastly, security-conscious users with a higher desire to not expose any of their traffic to the network over HTTP can already make use of our strict HTTPS-Only Mode, which is available through Firefox settings. It requires all resource loads to happen over HTTPS or else be blocked.

The post Firefox will upgrade more Mixed Content in Version 127 appeared first on Mozilla Security Blog.

Go to Source
