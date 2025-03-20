---
title: "How to deploy an anonymous website on the Dark Web"
date: 2025-03-19
---

Have you ever wondered what the Dark Web is? How to access it? Or even better, how to deploy your site to the dark web? Well I got you today. In this article we shall look at how you can deploy your site on the Tor network, which is a Dark web that allows clients and Servers to navigate and exist on the Internet Anonymously.

The Internet is not all you see, it's divided into 2 parts, The Clearnet and The Darknet. They are differentiated by Traceability. You see on the Clearnet, when a message is sent over the network, it's easy to trace the client that sent it and the destination server that it was sent to. That's not the case on the Darknet, on the Darknet the location/IP address of the server and the client are not traceable. The client sends a message to the server but doesn't know where it's located.

Take an example of youtube.com, If we do an NSLookup, we can identify the IP Address, Location and ISP of the server, in this case as of now its, 142.251.12.190 with location in Singapore and Google LLS ISP.

This traceability is due to the underlying design of the TCP/IP Protocol where communication, whether using TCP or UDP for communication, the IP address of the server and client is traceable and known.

Even in cases where we try to hide our identity over the internet, there is still some form of traceability. In large organisations, they usually try to make the client address private and all traffic is routed through a NAT. This keeps the address of the client hidden but the IP Address of the NAT is public to the Internet. Another way we attempt to navigate the internet anonymously is using VPN Systems, these work by routing the traffic from the client through a relay system or server and then this routes the traffic to the destination server. However much the IP Address of the client does not make it to the mainstream Internet, that of the VPN System is public to the Internet. Generally, there is always some form of traceability on the Clearnet

On the Darknet this traceability is not there, Darknets are designed with confidentiality, privacy, and freedom in mind, so maximum anonymity is ensured. Notice i just said "Darknets", this is because there are more than one form of Darknet. Actually the term "Darknet" refers to any overlay network that requires special software configurations and authorisation to access and is not indexed by search engines. Some forms of darknet technologies include I2P (Invisible Internet Project), Freenet, ZeroNet etc. But the most popular so far is Tor (The Onion Router) and its the one we shall be discussing and using in this article.

## The Onion Router (Tor)

The Onion Router, often known as Tor, is one of the forms of darknet technologies that is under operation and so far the most popular. It runs on a core technology known as Onion Routing that was developed in the mid 1990's by the United States Naval Research Laboratory to protect American intelligence communications online. In 2004, the Naval Research Laboratory released the code for Tor under a free license and now runs on free and open-source software and more than seven thousand volunteer-operated relays worldwide.

It is built with the principles of confidentiality, privacy, and freedom in mind. It enables anonymous communication over the internet between clients and servers. To accomplish this, strong encryption algorithms/technologies like SSL/TLS and relay servers are utilised to prevent message Man-in-the-Middle (MITM) attacks and Traceability respectively.

### How it works

Let's take a look at how Onion Routing works. In Onion Routing, a message is encapsulated with multiple layers of encryption, analogous to the layers of an onion, this message is then passed through a series of network nodes/relay servers known as "onion routers" that peel of a layer of encryption one by one at each node until the message arrives at the destination server. This mechanism provides confidentiality. THe diagram below shows how these encryption layers are peeled off at different nodes in the network

![Tor Circuit Diagram](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F90hcs8zr7wbc866af805.png)

When a client sends a network request over the Tor network through the Tor Browser (The web browser used to browse the Tor Network). The browser first sends the request to a gateway node known as the Entry Guard. This is the Entry Point into the Tor Network, however the Entry guard is not aware that it is the Entry point of the network and also whether the message came from another relay node or from a client. The Entry guard then connects to another relay called Middle Relay which also connects to another Middle Relay. The last relay in the chain is the Exist Relay which connects to the website. Each relay knows only the previous system and the next one, but never the whole chain. In the point of view of the website, it's being accessed through Tor and is aware of the address of the Exit Relay but cannot know anything else beyond that. This chaining of relay servers is what enables Tor to eliminate traceability.

![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fqio3om0kwcepqq1o5dp4.png)

One last thing we need to know when accessing a hidden service is the Onion Address. Unlike normal websites which we access using their URLs, hidden services are accessed using a special type of addresses called onion addresses. Each Onion Address contains an opaque non-mnemonic string of 16 characters followed by a special TLD (top level domain) address of .onion. The .onion addresses are not part of the typical DNS on the Internet hence are not accessible over traditional internet clients. THis is one of the reasons why Tor has its own special browser. Here is an example of an Onion Address: http://zsuwmakpr3gfryt6.onion

## Deploying on the Tor Network

Now that we have a general understanding of how the Tor network works, let's move on to deploying a simple website to the Tor Network. We shall do this by following a series of steps. Before continuing ensure you have a Linux server or Virtual Machine running.

### Installing Tor Services

To use the Tor network, you are required to have two main components, The Tor Browser and the Tor Server. The Tor browser can be installed on almost all platforms, head over to the Tor Download page to download it for your platform. On the other hand, as far as I know The Tor server is a linux program so you are required to have a linux Instance running. If you're using a debian system run the following commands to install it.  

```
apt-get update
apt-get install tor
apt-get install torbrowser-launcher
```

### Setting up your Web Application/Server

Depending on the tech stack of your application or website, you will be required to start your website on the local host address (127.0.0.1) listening at a specific port, for example port `3000`. Ensure that the port you're listening to is not exposed outside the server as this will make your site accessible over the Clearnet hindering the anonymity we are aiming for.

### Add the Website to Tor

Now that we have our web server listening on localhost, we can configure our Tor program to designate this server as a Tor hidden service. We do this by editing Tor configuration file `/etc/tor/torrc` as follows:  

```
############### This section is just for location-hidden services ###

## Once you have configured a hidden service, you can look at the
## contents of the file ".../hidden_service/hostname" for the address
## to tell people.
##
## HiddenServicePort x y:z says to redirect requests on port x to the
## address y:z.

HiddenServiceDir /var/lib/tor/hidden_service/
HiddenServicePort 80 127.0.0.1:3000

```

When you open the `torrc` file, you will find the lines `HiddenServiceDir` and `HiddenServicePort` commented, so you will have to uncomment them and also modify the `HiddenServicePort` to make sure that it points to the port at which you application is listening.

### Running the Tor Service

After setting up the required configuration, we can then run the Tor Service to perform all necessary actions and register our site to the Tor network. It will also create private and public keys for encryption and also create an onion address for your site. To run the Tor Service, simply run the command `tor`  

```
$ tor

Nov 13 17:18:09.832 [notice] Tor 0.3.4.8 (git-5da0e95e4871a0a1) running on Linux with Libevent 2.1.8-stable, OpenSSL 1.1.0h, Zlib 1.2.8, Liblzma 5.2.2, and Libzstd 1.3.5.
Nov 13 17:18:09.851 [notice] Tor can’t help you if you use it wrong! Learn how to be safe at https://www.torproject.org/download/download#warning
Nov 13 17:18:09.947 [notice] Read configuration file “/etc/tor/torrc”.
…
…
…
Nov 13 17:18:36.000 [notice] Bootstrapped 90%: Establishing a Tor circuit
Nov 13 17:18:37.000 [notice] Tor has successfully opened a circuit. Looks like client functionality is working.
Nov 13 17:18:37.000 [notice] Bootstrapped 100%: Done

```

You can also use a service like `nohup` inorder to run it as a background process  

```
nohup tor &
```

The Tor program will create two files under the directory `/var/lib/tor/hidden_services/` which include the Private Key and a `hostname` file containing the onion address of your site. Ensure to keep the Private Key secure and confidential.

You can now get your onion address by reading the contents of the `hostname` file  

```
cd /var/lib/tor/hidden_service/

cat hostname

zjsytjmah6hiyw1h.onion
```

Lastly, open the Tor Browser and head to the onion address provided and you will be able to access your anonymous site.

The Dark Web is often known for the illicit Activities that are carried out over it. This has portrayed the Dark Web as a dangerous place on the Internet. However, the Dark Web is used for more than that. Law enforcement agencies and cybersecurity professionals use Tor for undercover operations, intelligence gathering, and security research to combat cybercrime, terrorism, and fraud. Researchers, companies, and individuals dealing with sensitive topics (such as cybersecurity, medical research, or financial transactions) use Tor to protect trade secrets, proprietary data, and confidential communications from cyber threats and corporate espionage. Human rights activists and political dissidents use Tor to evade surveillance and protect themselves from oppressive governments that seek to suppress dissent. This fosters democracy and civil liberties worldwide.

Go to Source
