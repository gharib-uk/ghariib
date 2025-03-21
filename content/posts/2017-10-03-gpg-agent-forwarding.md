---
title: "GPG Agent Forwarding"
date: Tue, 03 Oct 2017 06:27:06 GMT
draft: false
type: posts
---
# GPG Agent Forwarding





I recently discovered that after upgrading my various Debian based devices, I’m now using a version of OpenSSH (&gt;=6.7) which supports GPG Agent Forwarding - https://wiki.gnupg.org/AgentForwarding. In Debian Stretch, systemd is involved with running gpg-agent and sockets are automatically created in /run/user/$UID/gnupg/ where $UID is the uid of the user you’re

I recently discovered that after upgrading my various Debian based devices, I’m now using a version of OpenSSH (>=6.7) which supports GPG Agent Forwarding - [https://wiki.gnupg.org/AgentForwarding](https://wiki.gnupg.org/AgentForwarding).

In Debian Stretch, systemd is involved with running gpg-agent and sockets are automatically created in /run/user/$UID/gnupg/ where $UID is the uid of the user you’re using. So when I want to forward my gpg-agent to “server.example.com”, I just add the following to my ~/.ssh/config file:

```
Host server server.example.com
    RemoteForward /run/user/$REMOTE_UID/gnupg/S.gpg-agent /run/user/$LOCAL_UID/gnupg/S.gpg-agent.extra
```

Replacing $REMOTE\_UID with the uid of my user on the remote system, and $LOCAL\_UID with the uid of the user on my local system. If you want to figure out your current uid, just run “id -u”.

The locations of your sockets on the local and remote system will differ according to the distribution you’re using, and as per the wiki if you’re not using Debian Stretch on both sides, you may need to explicitly turn this functionality on in your gpg-agent config.

You may also need to add “StreamLocalBindUnlink yes” to the /etc/ssh/sshd\_config file on the server side as mentioned in [the wiki](https://wiki.gnupg.org/AgentForwarding)

Once done, I can ssh in to “server.example.com” and use the gpg command on that box, without my private keys being installed on that box. In fact, my private keys aren’t installed on my local box either as I use a smart card, but that’s [a different blog post](https://www.grepular.com/blog/)

[![](https://www.grepular.com/images/amazon/pgp_and_gpg.jpg)](https://www.grepular.com/redir?key=amazon_pgp_and_gpg "PGP & GPG: Email for the Practical Paranoid") [![](https://www.grepular.com/images/amazon/web_application_hackers_handbook.jpg)](https://www.grepular.com/redir?key=amazon_web_application_hackers_handbook "The Web Application Hackers Handbook") [![](https://www.grepular.com/images/amazon/schneier.jpg)](https://www.grepular.com/redir?key=amazon_schneier "Schneier on Security")

#### [Source](https://www.grepular.com/GPG_Agent_Forwarding)

