---
title: "Reviewing the Attack Surface of the Autel MaxiCharger: Part Two"
date: 2025-01-17
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "cybersecurity"
  - "security"
  - "zero_day"
---

Previously, we covered the internals of the Autel MaxiCharger where we highlighted each of the main components. In this post, we aim to outline the attack surface of the MaxiCharger in the hopes of providing inspiration for vulnerability research.

All information has been obtained through reverse engineering, experimenting, and combing through the Autel MaxiCharger manual (PDF).

At the time of writing the following software versions were applicable:

·      Autel Charge app v3.0.7  
·      Autel Config app v2.1.0  
·      Autel MaxiCharger modules:  
·      Charge Control v1.36.00  
·      Power Control v1.21.00  
·      LCD Control v0.99.31  
·      LCD Information v0.99.08  
·      LCD Resources v0.99.08  
·      LCD Languages v0.04.04

**Mobile Applications**

Autel has published two mobile applications for both Android and iOS. The main app is called Autel Charge and contains functionality intended for end users. Some of the features include:

·      Defining charging schedules  
·      Load balancing  
·      Providing Wi-Fi credentials for the charger to use  
·      Forcing firmware updates  
·      OCPP server selection (including custom servers)  
·      Current limiting  
·      Finding other chargers on a map  
·      Checking charger version information

Upon loading the app on a rooted Android device a superuser request can be seen. This was unexpected and points towards the app employing anti-reversing measures. Denying the request loads the app normally.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/810bb29c-1cbf-4f6b-b568-70f05c2ffd48/Picture1.png?format=1000w)

<figcaption>

_Figure 1: Autel Charge superuser request_

</figcaption>

</figure>

After denying the superuser request a new Autel account can be created using an email address.

The second app is named Autel Config and allows installers / technicians to configure chargers and manage tickets. Unlike the Autel Charge app, there is no option to register for an account and providing Autel Charge account credentials doesn't work. This suggests that installers / technicians have some other way of obtaining valid credentials.

Further research into these apps could be valuable to better understand how the apps and charger communicate.

**Network Traffic Analysis**

Using the Autel Charge app the MaxiCharger was configured to connect to a researcher controlled Wi-Fi network in order to monitor the network traffic. The app and charger were then left idling whilst the traffic was captured.

A few DNS requests were sent out from the charger (`192.168.200.66`) for Autel related infrastructure.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/18e3ad11-5ddc-4f37-bfd4-609d360fa1dd/Picture2.png?format=1000w)

<figcaption>

_Figure 2: Charger DNS queries_

</figcaption>

</figure>

The first query was for `gateway-eneprodus.autel.com` which is an alias of `eneprodus-alb-internet-2014464356.us-west-2.elb.amazonaws.com`. This resolved to the following IP addresses (shown in the order received):

       • 54.185.127.160  
       • 52.36.153.97  
       • 44.240.206.177  
       • 34.215.58.124

Straight after the first DNS query response a TLS session was set up and encrypted data was sent by the charger on port 443 to 54.185.127.160. Data was sent back and forth between the charger and server a few times before another DNS query was sent. The charger issued another query for `gateway-eneprodus.autel.com` which, as before, is an alias and returned the same IP addresses but in a different order presumably due to load balancing. This time the DNS query returned the IP addresses:

       • 34.215.58.124  
       • 44.240.206.177  
       • 54.185.127.160  
       • 52.36.153.97

Like previously, the charger used the first IP address that was returned but this time no TLS session was set up. Plain HTTP was used.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/1889aec2-511a-4a6c-b968-ec288ad67f62/Picture3.png?format=1000w)

<figcaption>

_Figure 3: HTTP traffic_

</figcaption>

</figure>

Looking a bit closer showed the charger periodically sending log data to the Autel server. The server always responded with JSON that had a null `data` value, a 200 `code` value and a `message` value of `OK`.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/ce1fcfd8-e90b-4d3b-b6c6-d0f8d2c18ef6/Picture4.png?format=1000w)

<figcaption>

_Figure 4: HTTP POST traffic_

</figcaption>

</figure>

After a while the charger made another DNS request for `gateway-eneprodus.autel.com`, this time the `44.240.206.177` IP address was returned first. The charger then sent a HTTP POST to `/api/app-version-manager/version/upgrade/ota` with device related details such as the serial number and current firmware version. The server responded with JSON containing firmware update related information including a URL to download the latest version.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/45f0fbd9-b884-4c0a-bf93-165ddb38108f/Picture5.png?format=1000w)

<figcaption>

_Figure 5: HTTP firmware related traffic_

</figcaption>

</figure>

The charger then proceeded to send a DNS request for `s3.us-west-2.amazonaws.com` and directly downloaded the firmware update over HTTP.

The same pattern was observed multiple times as the device downloaded firmware updates for each of its modules. A list of these modules and their versions can be viewed in the Autel Charge app by navigating to the Charger Info page.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/8b3d880e-1c58-4d25-802b-85f0f777a1a6/Picture6.png?format=1000w)

<figcaption>

_Figure 6: MaxiCharger module versions_

</figcaption>

</figure>

After the firmware was updated and the charger rebooted no further HTTP traffic was observed to the logging or firmware update endpoint, instead only HTTPS was used.

Port scanning the charger over Wi-Fi showed no open TCP or UDP ports however UDP ports 6000 and 6666 appear to be listening over the Ethernet interface. The Ethernet interface is a valid target for the competition so these 2 listening services may be worth researching further.

**Bluetooth Low Energy**

By default the MaxiCharger uses the device serial number as the device name when advertising over Bluetooth. Once connected there are 4 available services that offer a total of 14 characteristics. Autel Charge uses these endpoints to communicate with the charger. A dump of each service and associated characteristics is shown below.

Further research into Autel Charge and Autel Config will likely assist in understanding the bluetooth services better.

**Firmware**

As mentioned in the previous blog the main microcontroller has readout protection enabled however this can be bypassed using techniques covered in Jonathan Andersson's and Thanos Kaliyanakis' Blackhat EU talk. Keep an eye out for future blog posts that will cover these techniques. One of which doesn't require glitching!

The main firmware can also be acquired by sniffing the charger update process (as described in the Network Traffic Analysis section) or by reversing the app to figure out the download URLs.

The firmware of ESP32 WROOM 32D module can be dumped using the standard `esptool.py` from Espressif. During research it was noted that the `esptool.py` would sometimes fail to dump the full firmware image. To mitigate this the firmware can be dumped in smaller chunks and then stitched back together into a single blob.

**Other Potential Attack Surfaces**

There are a few other attack surfaces that are considered in scope and are worth mentioning. One of these is the undocumented USB C port that can be found behind a small panel on the side of the unit. There is no publicly available information about what this USB port is used for.

Also, next to the USB port is the SIM card tray. Attacks that utilize a SIM card are also considered to be in scope.

And finally, there is the RFID (NFC) reader.

**Summary**

Hopefully this blog post provides enough information to kickstart vulnerability research against the Autel MaxiCharger.

We are looking forward to Pwn2Own Automotive again in Tokyo in January 2025 at Automotive World, and we will see if IVI vendors have improved their product security. Don’t wait until the last minute to ask questions and register! We hope to see you there.

You can find me on Twitter at @ByteInsight, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

Previously, we covered the internals of the Autel MaxiCharger where we highlighted each of the main components. In this post, we aim to outline the attack surface of the MaxiCharger in the hopes of providing inspiration for vulnerability research.

All information has been obtained through reverse engineering, experimenting, and combing through the Autel MaxiCharger manual (PDF).

At the time of writing the following software versions were applicable:

·      Autel Charge app v3.0.7  
·      Autel Config app v2.1.0  
·      Autel MaxiCharger modules:  
·      Charge Control v1.36.00  
·      Power Control v1.21.00  
·      LCD Control v0.99.31  
·      LCD Information v0.99.08  
·      LCD Resources v0.99.08  
·      LCD Languages v0.04.04

**Mobile Applications**

Autel has published two mobile applications for both Android and iOS. The main app is called Autel Charge and contains functionality intended for end users. Some of the features include:

·      Defining charging schedules  
·      Load balancing  
·      Providing Wi-Fi credentials for the charger to use  
·      Forcing firmware updates  
·      OCPP server selection (including custom servers)  
·      Current limiting  
·      Finding other chargers on a map  
·      Checking charger version information

Upon loading the app on a rooted Android device a superuser request can be seen. This was unexpected and points towards the app employing anti-reversing measures. Denying the request loads the app normally.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/810bb29c-1cbf-4f6b-b568-70f05c2ffd48/Picture1.png?format=1000w)

<figcaption>

_Figure 1: Autel Charge superuser request_

</figcaption>

</figure>

After denying the superuser request a new Autel account can be created using an email address.

The second app is named Autel Config and allows installers / technicians to configure chargers and manage tickets. Unlike the Autel Charge app, there is no option to register for an account and providing Autel Charge account credentials doesn't work. This suggests that installers / technicians have some other way of obtaining valid credentials.

Further research into these apps could be valuable to better understand how the apps and charger communicate.

**Network Traffic Analysis**

Using the Autel Charge app the MaxiCharger was configured to connect to a researcher controlled Wi-Fi network in order to monitor the network traffic. The app and charger were then left idling whilst the traffic was captured.

A few DNS requests were sent out from the charger (`192.168.200.66`) for Autel related infrastructure.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/18e3ad11-5ddc-4f37-bfd4-609d360fa1dd/Picture2.png?format=1000w)

<figcaption>

_Figure 2: Charger DNS queries_

</figcaption>

</figure>

The first query was for `gateway-eneprodus.autel.com` which is an alias of `eneprodus-alb-internet-2014464356.us-west-2.elb.amazonaws.com`. This resolved to the following IP addresses (shown in the order received):

       • 54.185.127.160  
       • 52.36.153.97  
       • 44.240.206.177  
       • 34.215.58.124

Straight after the first DNS query response a TLS session was set up and encrypted data was sent by the charger on port 443 to 54.185.127.160. Data was sent back and forth between the charger and server a few times before another DNS query was sent. The charger issued another query for `gateway-eneprodus.autel.com` which, as before, is an alias and returned the same IP addresses but in a different order presumably due to load balancing. This time the DNS query returned the IP addresses:

       • 34.215.58.124  
       • 44.240.206.177  
       • 54.185.127.160  
       • 52.36.153.97

Like previously, the charger used the first IP address that was returned but this time no TLS session was set up. Plain HTTP was used.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/1889aec2-511a-4a6c-b968-ec288ad67f62/Picture3.png?format=1000w)

<figcaption>

_Figure 3: HTTP traffic_

</figcaption>

</figure>

Looking a bit closer showed the charger periodically sending log data to the Autel server. The server always responded with JSON that had a null `data` value, a 200 `code` value and a `message` value of `OK`.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/ce1fcfd8-e90b-4d3b-b6c6-d0f8d2c18ef6/Picture4.png?format=1000w)

<figcaption>

_Figure 4: HTTP POST traffic_

</figcaption>

</figure>

After a while the charger made another DNS request for `gateway-eneprodus.autel.com`, this time the `44.240.206.177` IP address was returned first. The charger then sent a HTTP POST to `/api/app-version-manager/version/upgrade/ota` with device related details such as the serial number and current firmware version. The server responded with JSON containing firmware update related information including a URL to download the latest version.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/45f0fbd9-b884-4c0a-bf93-165ddb38108f/Picture5.png?format=1000w)

<figcaption>

_Figure 5: HTTP firmware related traffic_

</figcaption>

</figure>

The charger then proceeded to send a DNS request for `s3.us-west-2.amazonaws.com` and directly downloaded the firmware update over HTTP.

The same pattern was observed multiple times as the device downloaded firmware updates for each of its modules. A list of these modules and their versions can be viewed in the Autel Charge app by navigating to the Charger Info page.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/8b3d880e-1c58-4d25-802b-85f0f777a1a6/Picture6.png?format=1000w)

<figcaption>

_Figure 6: MaxiCharger module versions_

</figcaption>

</figure>

After the firmware was updated and the charger rebooted no further HTTP traffic was observed to the logging or firmware update endpoint, instead only HTTPS was used.

Port scanning the charger over Wi-Fi showed no open TCP or UDP ports however UDP ports 6000 and 6666 appear to be listening over the Ethernet interface. The Ethernet interface is a valid target for the competition so these 2 listening services may be worth researching further.

**Bluetooth Low Energy**

By default the MaxiCharger uses the device serial number as the device name when advertising over Bluetooth. Once connected there are 4 available services that offer a total of 14 characteristics. Autel Charge uses these endpoints to communicate with the charger. A dump of each service and associated characteristics is shown below.

Further research into Autel Charge and Autel Config will likely assist in understanding the bluetooth services better.

**Firmware**

As mentioned in the previous blog the main microcontroller has readout protection enabled however this can be bypassed using techniques covered in Jonathan Andersson's and Thanos Kaliyanakis' Blackhat EU talk. Keep an eye out for future blog posts that will cover these techniques. One of which doesn't require glitching!

The main firmware can also be acquired by sniffing the charger update process (as described in the Network Traffic Analysis section) or by reversing the app to figure out the download URLs.

The firmware of ESP32 WROOM 32D module can be dumped using the standard `esptool.py` from Espressif. During research it was noted that the `esptool.py` would sometimes fail to dump the full firmware image. To mitigate this the firmware can be dumped in smaller chunks and then stitched back together into a single blob.

**Other Potential Attack Surfaces**

There are a few other attack surfaces that are considered in scope and are worth mentioning. One of these is the undocumented USB C port that can be found behind a small panel on the side of the unit. There is no publicly available information about what this USB port is used for.

Also, next to the USB port is the SIM card tray. Attacks that utilize a SIM card are also considered to be in scope.

And finally, there is the RFID (NFC) reader.

**Summary**

Hopefully this blog post provides enough information to kickstart vulnerability research against the Autel MaxiCharger.

We are looking forward to Pwn2Own Automotive again in Tokyo in January 2025 at Automotive World, and we will see if IVI vendors have improved their product security. Don’t wait until the last minute to ask questions and register! We hope to see you there.

You can find me on Twitter at @ByteInsight, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

Go to Source
