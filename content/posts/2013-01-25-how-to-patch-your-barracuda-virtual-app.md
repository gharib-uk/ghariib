---
title: "How to patch your Barracuda virtual appliance"
date: 2013-01-25T01:50:00.001-08:00
draft: false
type: posts
categories: 
- backdoor,barracuda,ikki,patch
---
# How to patch your Barracuda virtual appliance

<br/>

<br/>
It's today's "news" about backdoors found in multiple Barracuda gears. Basically, Barracuda appliances have multiple hardcoded system accounts and firewall rules specifically designed to allow remote assistance. If you want more gossip, you can read about it on [KrebsOnSecurity](http://krebsonsecurity.com/2013/01/backdoors-found-in-barracuda-networks-gear/), [The Register](http://www.theregister.co.uk/2013/01/24/barracuda_backdoor/) or [The H Online](http://www.h-online.com/security/news/item/Backdoors-in-many-Barracuda-appliances-1790947.html).  
  

### A new old story

According to the [original advisory,](https://www.sec-consult.com/fxdata/seccons/prod/temedia/advisories_txt/20130124-0_Barracuda_Appliances_Backdoor_wo_poc_v10.txt) the bug was discovered on 2012-11-20 by [Stefan Viehböck](https://twitter.com/sviehb). Although Stefan did pretty interesting research in the past (e.g. [WiFi WPS design bug](http://sviehb.wordpress.com/2011/12/27/wi-fi-protected-setup-pin-brute-force-vulnerability/)), the Barracuda backdoor is really not a new story. Not only this issue was known, but it was even disclosed and discussed several times_:_

-   [http://packetstormsecurity.com/files/35358/Barracuda\_Evil.txt.html](http://packetstormsecurity.com/files/35358/Barracuda_Evil.txt.html)  \- 2004
-   [http://archives.neohapsis.com/archives/fulldisclosure/2006-08/0110.html](http://archives.neohapsis.com/archives/fulldisclosure/2006-08/0110.html) - 2006
-   [http://blog.shiraj.com/2009/09/barracuda-spam-firewall-root-password/](http://blog.shiraj.com/2009/09/barracuda-spam-firewall-root-password/) \- 2009

Although it's natural to be surprised that such a critical issue has been underestimated for **nine** years, we should rather use this opportunity to stop these bad practices. Unfortunately, it's not just Barracuda - many vendors have adopted similar poorly-designed solutions for remote assistance. As customers, we should always evaluate products, pretend more accountability and transparency.  
  

[![](http://i.imgur.com/jlLJaEq.png)](http://i.imgur.com/jlLJaEq.png)

### Digital self-defense

In 2011, while helping a friend during the setup of his network, I came across the advisory from 2004 and I started investigating.  After having confirmed the issue, I decided to patch the virtual appliance on my own. If you think that the mitigation provided by Barracuda in the [security definition 2.0.5](https://www.barracudanetworks.com/support/techalerts)  is not adequate for your environment, keep reading. Hopefully, Barracuda will reconsider the situation and you won't need to manually patch your device.  
  
**Disclaimer:** _Use this information at your own risk!_   
_You may end up with a broken appliance and no more vendor warranty. Also, I am not a lawyer and I haven't reviewed the product EULA. Finally, n__ote that this method has been tested against the Barracuda WebApp Firewall 660vxl (v7.5.0.x)_ _virtual appliance only._   
  

### Patching your virtual appliance

Removing system accounts and changing _iptables_ configuration require privileged shell access. As the original techniques for rooting the device are now deprecated (at least in the device I had), I started looking for other ways to get a root shell. Soon, I realized that it's possible to abuse the recovery partition in order to include arbitrary resources. This technique requires "physical" access to the appliance and multiple reboots thus I consider it better than disclosing the root password and suggest you to abuse the backdoor in order to patch the device.  
  
Rooting the Barracuda WebApp Firewall requires a multi steps process:  
  
**1)** Boot the Barracuda virtual appliance with a standard Linux distribution (e.g. booting from the virtual CD) and mount the recovery partition (_/dev/sda9_) in order to copy the patcher script (_rootme.sh_).  
  
_rootme.sh_ can be downloaded [here](http://www.ikkisoft.com/stuff/rootme.sh)  
    
  $ mkdir /mnt/temp   
  $ mount /dev/sda9 /mnt/temp  
  $ cp rootme.sh /mnt/temp/  
  $ chmod 777 /mnt/temp/rootme.sh  
  $ /mtn/temp/rootme.sh  
  

[![](http://i.imgur.com/curiwiL.png)](http://i.imgur.com/curiwiL.png)

  
  $ umount /mnt/temp  
  $ reboot  
  
  
**2)** From the web console, revert the firmware to the factory installed version (_Advanced-->Firmware Update-->Firmware Revert_) and reboot again the appliance. If the factory _Firmware Revert_ button is not available (it's gray and cannot be selected), you need to update the device to the newest firmware and repeat the entire process.  
  
**3)** Visit _https://barracuda\_ip/cgi-mod/rootme.cgi_**.** After that, you can connect via SSH to the device using a temporary root password. Removing the hardcoded system accounts and changing iptables is left as exercise.  
  

[![](http://i.imgur.com/7v1hZEi.png)](http://i.imgur.com/7v1hZEi.png)

  
A few more technical details:  
  

-   _rootme.sh_ is simply used to copy _rootme.cgi_ to the web console webroot in order to facilitate the rooting process
-   _rootme.cgi_ is used to escalate privileges from the Apache user (nobody) to root, change the root password and the firewall rules in order to allow external access 
-   Privileges escalation is possible due to an insecure sudoers configuration. Again, nothing fancy. Please note that I have reported this misconfiguration to Barracuda on 09/12/2011.

   $ sudo mv /bin/ping /tmp/ping.old  

   $ sudo ln -s /bin/bash /bin/ping  
   $ sudo ping -c whoami

#### [Source](http://blog.nibblesec.org/feeds/3159367705128050447/comments/default)

<br/>
---
