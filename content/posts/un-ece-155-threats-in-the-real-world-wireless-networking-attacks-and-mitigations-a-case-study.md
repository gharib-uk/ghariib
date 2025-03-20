---
title: "UN ECE 155 Threats in the real world: Wireless Networking Attacks and Mitigations. A case study"
date: 2025-01-22
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "security"
  - "security-awareness"
tags: 
  - "automotive"
  - "dab"
  - "iso21434"
  - "rds"
  - "tara"
---

![](https://blogger.googleusercontent.com/img/a/AVvXsEiudD_Xp3lJCAzIrQrT-FSjXcxXuz72qLmrLHvFKhQhJzoSnmy-gWoWrCt-FNmbbFiylX3byUeWjZuJk7XE41wIFarc3fREroKYz7qMjXC4-l1BQJYbnvSpIUX-5LzdviiunBR1TrfDHRt-qKBvgpJNAHV8KOsUeDJM3M1Me3_ySBLstqadk8_JwyZk)

  
On March the 31st, I gave a quick talk on automotive security at VTM titled "**UN ECE 155 Threats in the real world: Wireless Networking Attacks and Mitigations. A case study**" (slides here).

The idea was to create some content about one of the _most hyped topics_ in the automotive cyber security world over the last year, without keeping it just theoretical;

UN/ECE 155 and ISO/SAE 21434 whose concerns are about the implementation of a CSMS (Cyber Security Management System) which consists in performing, for each vehicle, several high level security tasks, such as Threat Analysis and Risk Assessment (TARA), supply chain security issues tracking, implementation of the mitigations, update management and so on.

The following schema shows the product development lifecycle model, called V-Model, used in the automotive industry and the cybersecurity processes in each phase of the V-Model.

 ![](https://lh4.googleusercontent.com/1w47GbhM6KsZeduLejmcHjwimwwNm3cslMo_SDs8cpY3kdkBVpL-r4xQuYk3fTk84PKFWbNoTAig6OzboYlmaO1pPkmeiPekS7eqAA5bwWqluk0Y7bwmAR0VA8K_bqkeqiy6wINWAFjMSMOCy1qZIw=w400-h249)

The most interesting point that can help mitigating the risks and performing attack surface analysis is the TARA which can really help to minimize the risk in the earliest stage. In particular it will give its best, well when the technologies that are going to be implemented, _are well known from a security perspective_. 

The following figure describes the steps that must be covered to perform a TARA by the ISO 21434:

![](https://lh4.googleusercontent.com/gPxH7kj3KglNrhtMJPwjz7c-9ea3SLTvap2NZHGedi_1_8FDwnnskmdnxK7k6DAvVwz71BHcdGun8RwfQ4XDs3UkJivTRyDwtjSv0HIP012-dexgowspCOIz36rbCfwOs4gb_ZZDIzlRqBo0RxxZlQ)

  

Since the audience was expected to be mixed technical/non technical I decided to keep it in the middle as well, which, alas, sometimes means the hard way.

Also_,_ how to go practical without going vehicle specific? mmm, take something that is already on every vehicle and talk about attacks, risks and remediations in the context of UNECE R155 and ISO 21434 requirements.

### **Digital Radio Broadcasting!** 

Now, the problem is to research on those topics without being too obvious and condense all in a limited span of 30 minutes which is quite challenging.

With the goal of identifying some unexplored attack surface, I took a couple of weeks to go into RDS and DAB+ specifications and their previous research in the security context. 

As briefly described in the slides in IMQ Minded Security I created a lab testbed with:

- A **RDS transmitter** using Raspberry PI and this wonderful piece of software
- Several non automotive **RDS receivers** and their software and a Renault Scenic 2015 Head Unit with RDS support.
- A **DAB+ transmitter** using _HackRF One_, and this essential set of software together with this very useful tutorial. 
- A _RTL-SDR_ for local tests and a **DAB+ USB Dongle receiver** that is also used in the automotive world with the most used _Android Automotive OS_  software DAB-Z and several other applications that are mostly used in desktop environments. Alas, apart from DAB-Z we had no immediately available automotive head units supporting DAB+ :/.

The threats were identified after reading the whole RDS and DAB+ documentation and condensed for the talk.

The most interesting turned out to be DAB+ which has much more perimeter.

![](https://blogger.googleusercontent.com/img/a/AVvXsEi-n8WiaoeeYup2cAn9D5z1ZUwBwcli0dbC8qQaoDtXZPkoAwHMa3At73nEvaP6IP5d16Cl1tnxx7pkf4fQw12iOkP7k7scA9Eyiyqvegatq4gsCRAsLwXSavxeftoyVrx_Un5heQT9CyLUq2t60E8EDcof8ospFYCVTcFLYvrZ_FOVgaxn_GWSlpdK=w400-h217)

and has already at least one known real world issue. 

![](https://blogger.googleusercontent.com/img/a/AVvXsEjVkSxBWvH1pj-pmbH0XkIGw5nhS0BKHC2nIz9t44uDQMv7oS0V-OqtDWLEOwE3-v8LeDnVNuXBUA4BFpXfsIZ_P9wb6mpJpk2zs6zEqT41rI_smiQQkFFhstXueKUGofURZSgMrl1R4xm9jWxfIRuclqjgUIy9pKHO7zxP5qFi7Us7i1csvD-KeJ6S=w400-h169)

  
  

Next step was to identify a number of possible threats and attacks by studying the DAB+ specifications, a subset of tests was shown during the talk:

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEifiUjuR3a6anTy02t3Up95xBVMBt2yqnFAtrXjDsCyuyyCo6ES3-mvRKAvxyhWZA2iTeEUKJ_6Sv9omhQShjUP7OHncVjtP3LcoFsG6Yhyu7piMxmZaaEdoFK8YX9QLx6VTnHAmHMfo-eD0_N13gnXaCVSB9nRg1kdP0aYxDk1mZDmHA7GQn99wn06)

  

Apart from creating **filenames with no extension**, we identified several more possible attacks on parsers such as creating malformed unicode filenames, EPG, Journaline and other DAB+ defined formats.

The stumbling blocks when going practical was that some of those formats were not implemented by the receivers we tested, so we decided to keep the tests for future activities.

### Results

The most interesting issues were found on DAB+ desktop software, resulting in path traversal and HTML injection.

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEir56guXWYS-tzk8SnAHLpMriSsQqtiQVqgjFWVSZyT0_Oi9uqBaSpPqj6YXdiWH-X5LEZaO1xuILvecCQRc04L-5o7RLCtnq0idDPUy6GBeS0E8R7nz9QhUkn1cgeRDthw-Kciw0wDBm01NykOl7PsfJjYkQ2e3ZVOgKvEnBarZvbr_Zd_jDlyZEcj)

  
  

  

Unfortunately, the lack of head units or vehicles prevented us to perform more thorough tests to get some more juicy stuff..

Let's see what the next weeks will give back, since we are expecting new hardware to perform more tests!

  

PS. We were expecting to have a video of the talk to publish, but it's not clear when and if.. so here are the slides of the talk:

"**UN ECE 155 Threats in the real world: Wireless Networking Attacks and Mitigations. A case study**"

  

Feel free to comment or contact us for any question!

Go to Source
