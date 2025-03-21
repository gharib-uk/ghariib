---
title: "Improving first impressions on Signal"
date: Mon, 01 Nov 2021 00:00:00 +0000
draft: false
type: posts
---
# Improving first impressions on Signal

<br/>

<br/>
 The phenomenon of unsolicited and unwanted messaging dates back to the earliest communication platforms: from prehistoric cave graffiti and postal chain letters to popup ads and robocalls. To prevent spam, most online services rely
<br/>
![An unsolicited Signal message appears, offering extended car warranty deals.](/blog/images/spam-car-small.jpg)

The phenomenon of unsolicited and unwanted messaging dates back to the earliest communication platforms: from [prehistoric cave graffiti](https://www.livescience.com/7028-ancient-cave-art-full-teenage-graffiti.html) and postal chain letters to popup ads and robocalls. To prevent spam, most online services rely on large-scale inspection of plaintext conversation content or detailed social-graph analysis to determine who is saying what to whom and whether or not that should be allowed. At Signal, we build on a foundation of privacy and do not have access to that type of data or metadata.

We build Signal in the open, with [publicly available](https://github.com/signalapp) source code for our applications and servers. To keep Signal a free global communication service without spam, we must depart from our totally-open posture and develop _one_ piece of the server in private: a system for detecting and disrupting spam campaigns. Unlike encryption protocols, which are designed to be provably secure even if everyone knows how they work, spam detection is an ongoing chore for which there is no concrete resolution and for which transparency is a major disadvantage. If we put this code on the Internet alongside everything else, spammers would just read it and adjust their tactics to gain an advantage in the cat-and-mouse game of keeping spam off the network. The Signal protocols, cryptography, and source code are peer reviewed, shared for independent inspection, and provably private by design. We are bound by these security guarantees, so that your conversations and contacts remain as private and protected as ever, even if we keep spam-fighting tools out of sight.

[_Read more..._](https://signal.org/blog/keeping-spam-off-signal/)

#### [Source](https://signal.org/blog/keeping-spam-off-signal/)

<br/>
---
