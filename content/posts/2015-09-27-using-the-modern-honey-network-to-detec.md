---
title: "Using the Modern Honey Network to Detect Malicious Activity"
date: 2015-09-27T13:42:00.000-04:00
draft: false
type: posts
---
# Using the Modern Honey Network to Detect Malicious Activity

<br/>

<br/>
### The Modern Honey Network

The [Modern Honey Network](http://threatstream.github.io/mhn/) (MHN) is an amazing honeypot framework created by the great team at [ThreatStream](https://www.threatstream.com/). MHN simplifies honeypot deployment and data collection into a central management system. From MHN you can send output to an [ELK](https://www.elastic.co/products/logstash) instance, Splunk or even an ArcSight digestible format. I personally output the data to Splunk because MHN has also made an elegant [Splunk application](https://splunkbase.splunk.com/app/2707/) that renders MHN data quite nicely.  
  
MHN comes pre built with deployment scripts for the following honeypots:  

-   Dionaea
-   Conpot
-   Kippo
-   Amun
-   Glastopf
-   Wordpot
-   ShockPot
-   Elastichoney

MHN also comes with scripts to install Snort and Suricata for IPS alerting as well as instructions to add additional honeypots to the framework. As mentioned earlier, the deployment scripts are designed to automatically feed their information back into MHN, which is then displayed within the MHN WebUI, ELK, ArcSight, or better yet, Splunk. Update: MHN also comes with p0F, which is not a honeypot but a passive fingerprint scanner.  
  

### Installation and Configuration

If you're interested in installing MHN on a server or VM you can follow the instructions by [n0where.net](https://n0where.net/modern-honeypot-network/). I installed MHN and the associated honeypots in Docker containers for convenience. This effectively isolates and compartmentalizes the services and allows multiple services that run locally on similar ports (such as 80 or 443) to use different "external" ports on the host machine.  
  
To install MHN on Docker start a container with the following command:  
  

```
docker run -p 10000:10000 -p 80:80 -p 3000:3000 -p 8089:8089 --name mhn  --hostname=mhndocker -t -i ubuntu:14.04.2 /bin/bash
```

\*Note: 8089 is specified if you are using the Splunk forwarder. You can chose between 80 and 443. You can also make the host OS' port separate from the docker container's port by using \[hostport\]:\[dockerport\], which is convenient for honeypots.

Next, create and run the following script:

```
#!/bin/bash

set -x

apt-get update 
apt-get upgrade -y 
apt-get install git wget gcc supervisor -y 
cd /opt/ 
git clone https://github.com/threatstream/mhn.git 
cd mhn

cat > /etc/supervisor/conf.d/mhntodocker.conf <<EOF
[program:mongod]
command=/usr/bin/mongod
stdout_logfile=/var/log/supervisor/%(program_name)s.log
stderr_logfile=/var/log/supervisor/%(program_name)s.log
autorestart=true
autostart=true

[program:nginx]
command=/usr/sbin/nginx
stdout_events_enabled=true
stderr_events_enabled=true
autostart=true
autorestart=true

EOF

mkdir -p /data/db /var/log/mhn /var/log/supervisor

supervisord &

#Starts the mongod service after installation
echo supervisorctl start mongod >> /opt/mhn/scripts/install_mongo.sh

./install.sh

supervisorctl restart all 
```

Don't forget to reference the host's IP address or hostname as the MHN server's IP during the ./install.sh script (not the docker container's IP address) unless you are using Docker's internal networking for Honeypot to MHN communication.

Unfortunately, due to the interactive nature of MHN's installation, supervisord is manually running in the background instead of as a started service. To restart the container later use:

```
docker start <containerID> 
docker exec <containerID> supervisord &
```

To deploy a honeypot go to the 'deploy' section in the MHN WebUI and select your honeypot from the drop-down list. Then, either copy the wget command or the script contents and run either in your honeypot system. 

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi5M7P9dE3yEBpzwnf31FgomxjMuYM-sLOl6pZSx7bQ6bqYBSw0BSJ5tkp615IpGWxundLfDp8hP7_JYYFfaBvI_iH7R9m8H5PPUImibGlMg_TzRJL0tnpTNpFlOg3LO1lY61cHUrDhghry/s320/Honeypot+Deployment.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi5M7P9dE3yEBpzwnf31FgomxjMuYM-sLOl6pZSx7bQ6bqYBSw0BSJ5tkp615IpGWxundLfDp8hP7_JYYFfaBvI_iH7R9m8H5PPUImibGlMg_TzRJL0tnpTNpFlOg3LO1lY61cHUrDhghry/s1600/Honeypot+Deployment.png)

MHN Deployment Page

  

  

Installing honeypots in containers uses a similar but less complex method where you: create an Ubuntu 14.04 container with your host and internal port mappings, install the required services (such as wget, sshd, python, supervisord, etc.) and run the install command or script from MHN deployment page above. There are consequences to installing honeypots on docker containers since some honeypots require direct interface access, which Docker supports, but reduces performance significantly. So decide how important packet capture is to your installation and choose appropriately. I'm not going to go through the installation instructions for each container, but if needed I can provide [guidance](https://twitter.com/Epicism1).

  

Run the following command to generate Splunk friendly output:

```
cd /opt/mhn/scripts/
sudo ./install_hpfeeds-logger-splunk.sh
```

This will log the events as key/value pairs to /var/log/mhn-splunk.log. This log should be monitored by the SplunkUniveralForwarder.  
  
To create an output for ArcSight run:  

```
cd /opt/mhn/scripts/
sudo ./install_hpfeeds-logger-arcsight.sh
```

This will log the events as CEF to /var/log/mhn-arcsight.log

  

Now you have MHN installed with Honeypots feeding information into it. 

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgc90h5YUHditlazQE2HKZEXj8is6nn0me5nTIaHIUZ5plMojUebCu7nTbgjQVTpMH9VHGZqs-b0KPfa8BgVdwWB_tpTuPVFwptyzO1sMMc-I7TtPSE_13QKG-bToUdXuajt0Yrpj6GFow9/s320/MHN+Main+page.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgc90h5YUHditlazQE2HKZEXj8is6nn0me5nTIaHIUZ5plMojUebCu7nTbgjQVTpMH9VHGZqs-b0KPfa8BgVdwWB_tpTuPVFwptyzO1sMMc-I7TtPSE_13QKG-bToUdXuajt0Yrpj6GFow9/s1600/MHN+Main+page.PNG)

MHN Main Page

### Configuring Honeypots

Once MHN is up and running an important question to ask is where to deploy honeypots and for what purpose. There are primarily three locations that a Honeypot can be installed:  
  
**1\. Internally**  
Internal honeypots provide a low-noise, high value, alarm system that lets you know when someone is performing attacking your internal servers. In theory, nothing should ever hit your internal honeypots minus perhaps vulnerability scanners, which you can whitelist from any alarm. I would recommend deploying Kippo, Conpot  Dionaea, and Amun (although Amun is a new addition to MHN and I haven't had the opportunity to play around with it yet) across your environment, and especially in high-value networks. I would also consider Shockpot and any other honeypot that mimics services you run internally such as WordPot or Glastopf.  
  
**2\. Externally**  
Opening an IP or specific ports on your firewall to Honeypots can let you know who is scanning your perimeter environment looking for vulnerabilities. Although it is difficult to action external scans into an alert since you will have both legitimate and illegitimate scans against your external addresses.  
  
**3\. Globally**  
The third option is to rent a server in the cloud and place MHN Honeypots on random public IPs. You can then use this data to compare your external MHN data to try to determine who is randomly scanning the internet versus who is specifically targeting you. Although this is a very unscientific way of going about this, it cannot hurt to have more information for investigative purposes. This type of deployment is often used to gather generic threat data that is fed to IP/URL/Hash blacklist databases.  
  

### Feeding Your Data into Splunk

I won't go into much detail about what reports to create with MHN's external and global data because I think that MHN has done a great job with the MHN [Splunk application](https://www.threatstream.com/blog/mhn-splunk-announcing-the-modern-honey-network-splunk-application) that I mentioned earlier. The application displays summary data for each of the honeypots on a dashboard home page.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhG5xvYYdLqJXPyIirVymEaIlyo-CKMYVukNQc1UF_NXkdsjQzFCpSuLtIke8GFuO10zbKK5G22m4L1LoLvo659dGkatw8zuuZrCI4ktI_UilpVEeCFaj6BBRbfGfTlcXyBuHbhggjlMYuB/s400/Splunk+MHN+Oerview.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhG5xvYYdLqJXPyIirVymEaIlyo-CKMYVukNQc1UF_NXkdsjQzFCpSuLtIke8GFuO10zbKK5G22m4L1LoLvo659dGkatw8zuuZrCI4ktI_UilpVEeCFaj6BBRbfGfTlcXyBuHbhggjlMYuB/s1600/Splunk+MHN+Oerview.png)

Main MHN Splunk App page

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiewobiyLLbV_ydUKn0Z_k8AcaV61GgmnfSlUvnmCd5HR2CgLDJ2_0F3JXDuoAlWHwKG_dlMetoWp3PxbSwMb3J2So3_2jrDI03l6mvRJYhwmrAmcjM3miLxOWW1Sizw4szV64jyNnejNp4/s200/Splunk+MHN+conpot_analytics.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiewobiyLLbV_ydUKn0Z_k8AcaV61GgmnfSlUvnmCd5HR2CgLDJ2_0F3JXDuoAlWHwKG_dlMetoWp3PxbSwMb3J2So3_2jrDI03l6mvRJYhwmrAmcjM3miLxOWW1Sizw4szV64jyNnejNp4/s1600/Splunk+MHN+conpot_analytics.png)

Splunk Conpot Page

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgqC_3vHQQxvfcGERdHvoKejSnq5FjAQDSAR7aAufFlm-KN6CBnLpRy-KeiGS6PKMTUf63Ef_uO9UeeISoTCU-lAAqS3YoQ4u6CJs79qXcf6jNHODqdoRp6vfHrmax7HHpxtp2gU8VyRP91/s200/Splunk+MHN+dionaea_analytics.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgqC_3vHQQxvfcGERdHvoKejSnq5FjAQDSAR7aAufFlm-KN6CBnLpRy-KeiGS6PKMTUf63Ef_uO9UeeISoTCU-lAAqS3YoQ4u6CJs79qXcf6jNHODqdoRp6vfHrmax7HHpxtp2gU8VyRP91/s1600/Splunk+MHN+dionaea_analytics.png)

Splunk Dionea Page

For a free and open source product, I'm pretty impressed by the work ThreatStream has put into MHN. I hope that they continue this trend.

  

### What To Do With The Data

Honeypot data utilization relies heavily on the context of the data. Internal Honeypot hits are far more important to investigate than external or global hits. That said, some uses include:  

-   When something unexpected hits an internal honeypot start an investigation ASAP.
-   A report of your top external honeypot hits to understand who's making the most noise, what they're trying to hit and how frequent the connections are. This will give you an idea of where to tighten security and how you can tailor your patch management program. It may also provide data points that can be used to narrow threat hunting efforts on the network (such as confirming that targeted attacks on the perimeter we're all dropped and that no traffic from the malicious external IPs was allowed through).
-   An alert that takes the top 1000 global/external honeypot source IP addresses in the past month and compares them to your firewall traffic to see if any non-honeypot connection lasted longer than thirty seconds or contained more than 1MB of data.
-   Threshold vectors for when a particular connection makes an unusually high number of connections to an external honeypot such as over 50-5000 (depending on how popular you are).
-   Understand what countries your attackers are originating from to create rules looking for successful authentications/unusual connections from those geographic locations.
-   Identify what usernames attackers use to attempt automated authentication and ban them within your organization.

-   For my honeypot it's root, admin, test, user, MGR, oracle, postgres, guest, ubnt and ftpuser.

-   Identify what passwords that authentication spammers use to try to authenticate to ensure that your password complexity rules meet minimum requirements

-   For my honeypots it's 123456, password, admin, root, 1234, test, 12345, guest, default and oracle.

-   Collect packet samples of potentially malicious traffic for custom IPS signatures.

  
If you enjoy this post/project please be sure to thank the MHN project and volunteers and to support the [Honeynet Project](https://www.honeynet.org/).  
  
P.S. check out this great [introduction video](https://www.youtube.com/watch?v=Zd1Br8TW1mk) by Jason Tro.

#### [Source](https://www.redblue.team/feeds/8935876935861589891/comments/default)

<br/>
---
