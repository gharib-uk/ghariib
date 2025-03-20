---
title: "Ashok - A OSINT Recon Tool AKA Swiss Army Knife"
date: Wed, 26 Jun 2024 08:30:00 -0400
draft: false
type: posts
categories: 
- Ashok,Banner Grabbing,Cmsdetecter,Githubrecon,Linkextractor,Nmap Scanning,Recon Tools,Subdomain Finder
---
# Ashok - A OSINT Recon Tool AKA Swiss Army Knife

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/a/AVvXsEhDSZHMyXiZrdj2WfsU7BKHbcVobLbdhFtl74UIPpj6EO_Q4PPtgoLGJmxK_ge5I4o8Ie9e_yRWuoXoMqbgjxOJmknIpLY1mSgsn8Pmv3pQo9oH_s3ttYU35pWf2KVQ_l-uJJ13UG-ORyj-JtootsUju7R9dAbG5-kWJD2ZaGKWF1sEvYYOcHk9ds198M4=w640-h478)](https://blogger.googleusercontent.com/img/a/AVvXsEhDSZHMyXiZrdj2WfsU7BKHbcVobLbdhFtl74UIPpj6EO_Q4PPtgoLGJmxK_ge5I4o8Ie9e_yRWuoXoMqbgjxOJmknIpLY1mSgsn8Pmv3pQo9oH_s3ttYU35pWf2KVQ_l-uJJ13UG-ORyj-JtootsUju7R9dAbG5-kWJD2ZaGKWF1sEvYYOcHk9ds198M4)

**Reconnaissance** is the first phase of penetration testing which means [gathering](https://www.kitploit.com/search/label/Gathering "gathering") information before any real attacks are planned So Ashok is an Incredible fast recon tool for penetration tester which is specially designed for [Recon](https://www.kitploit.com/search/label/<a href= "Recon")naissance" title="[Recon](https://www.kitploit.com/search/label/Recon "Recon")naissance">[Recon](https://www.kitploit.com/search/label/Recon "Recon")naissance phase. And in [Ashok-v1.1](https://github.com/ankitdobhal/Ashok/releases "Ashok-v1.1") you can find the [advanced google](https://github.com/ankitdobhal/Ashok/wiki/Usage#Google-dorking-using-number-of-results-as-dorknumber "advanced google") [dorker](https://www.kitploit.com/search/label/Dorker "dorker") and [wayback](https://github.com/ankitdobhal/Ashok/wiki/Usage#dump-internet-archive-machive-with-json-output-for-single-url "wayback") [crawling](https://www.kitploit.com/search/label/Crawling "crawling") machine.

  

[![](https://blogger.googleusercontent.com/img/a/AVvXsEjKiJOBbdxgV_G8UyDxfYHPzftvrZ4Fkemq0v_cBpmpfbSguBZlQZuGzvrTCKa1oXXDHvB_3ydHbbYi8mT69fCL3GmNOqF81g-oWeCpeJkBQ5_46Q7swkF-Z5fzTDq7o1x1hCkT8aVpQlSKikDowUySGfEFfIh5OgBqCVJuWyezLhF3S8x8-bARuDPDTLg=w640-h474)](https://blogger.googleusercontent.com/img/a/AVvXsEjKiJOBbdxgV_G8UyDxfYHPzftvrZ4Fkemq0v_cBpmpfbSguBZlQZuGzvrTCKa1oXXDHvB_3ydHbbYi8mT69fCL3GmNOqF81g-oWeCpeJkBQ5_46Q7swkF-Z5fzTDq7o1x1hCkT8aVpQlSKikDowUySGfEFfIh5OgBqCVJuWyezLhF3S8x8-bARuDPDTLg)

  

Main Features
-------------

```
- Wayback Crawler Machine- Google Dorking without limits- Github Information Grabbing- Subdomain Identifier - Cms/Technology Detector With Custom Headers
```

Installation
------------

```
~> git clone https://github.com/ankitdobhal/Ashok~> cd Ashok~> python3.7 -m pip3 install -r requirements.txt
```

How to use Ashok?
-----------------

A detailed usage guide is available on [Usage](https://github.com/ankitdobhal/Ashok/wiki/Usage "Usage") section of the [Wiki](https://github.com/ankitdobhal/Ashok/wiki "Wiki").

But Some index of options is given below:

-   [Extract Http Headers from single url](https://github.com/ankitdobhal/Ashok/wiki/Usage#Extract-Http-Headers-from-single-url "Extract Http Headers from single url")
-   [Dump internet-archive machine with json output for single url](https://github.com/ankitdobhal/Ashok/wiki/Usage#dump-internet-archive-machive-with-json-output-for-single-url "Dump internet-archive machine with json output for single url")
-   [Google dorking using number of results as dorknumber](https://github.com/ankitdobhal/Ashok/wiki/Usage#Google-dorking-using-number-of-results-as-dorknumber "Google dorking using number of results as dorknumber")
-   [Dns Lookup of single target domain](https://github.com/ankitdobhal/Ashok/wiki/Usage#Dns-Lookup-of-single-target-domain "Dns Lookup of single target domain")
-   [Subdomain Lookup of single target domain](https://github.com/ankitdobhl/Ashok/wiki/Usage#Subdomain-Lookup-of-single-target-domain "Subdomain Lookup of single target domain")
-   [Port Scan using nmap of single target domain](https://github.com/ankitdobhal/Ashok/wiki/Usage#Port-Scan-using-nmap-of-single-target-domain "Port Scan using nmap of single target domain")
-   [Extract data using Github username of target](https://github.com/ankitdobhal/Ashok/wiki/Usage#Extract-data-using-Github-username-of-target "Extract data using Github username of target")
-   [Detect Cms of target url](https://github.com/ankitdobhal/Ashok/wiki/Usage#Detect-Cms-of-target-url "Detect Cms of target url")

Docker
------

**Ashok** can be launched using a lightweight Python3.8-Alpine Docker image.

```
$ docker pull powerexploit/ashok-v1.2$ docker container run -it powerexploit/ashok-v1.2  --help
```

[![](https://blogger.googleusercontent.com/img/a/AVvXsEhDSZHMyXiZrdj2WfsU7BKHbcVobLbdhFtl74UIPpj6EO_Q4PPtgoLGJmxK_ge5I4o8Ie9e_yRWuoXoMqbgjxOJmknIpLY1mSgsn8Pmv3pQo9oH_s3ttYU35pWf2KVQ_l-uJJ13UG-ORyj-JtootsUju7R9dAbG5-kWJD2ZaGKWF1sEvYYOcHk9ds198M4=w640-h478)](https://blogger.googleusercontent.com/img/a/AVvXsEhDSZHMyXiZrdj2WfsU7BKHbcVobLbdhFtl74UIPpj6EO_Q4PPtgoLGJmxK_ge5I4o8Ie9e_yRWuoXoMqbgjxOJmknIpLY1mSgsn8Pmv3pQo9oH_s3ttYU35pWf2KVQ_l-uJJ13UG-ORyj-JtootsUju7R9dAbG5-kWJD2ZaGKWF1sEvYYOcHk9ds198M4)

  

Credits
-------

-   [hackertarget](https://hackertarget.com/ "hackertarget")

  
  

**[Download Ashok](https://github.com/powerexploit/Ashok "Download Ashok")**

[Source](http://www.kitploit.com/2024/06/ashok-osint-recon-tool-aka-swiss-army.html)
<br/>
---
