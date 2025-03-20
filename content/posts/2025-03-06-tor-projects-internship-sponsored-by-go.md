---
title: "Tor Projects internship sponsored by Google Summer of Code"
date: 2025-03-06T00:00:00Z
draft: false
type: posts
---
# Tor Projects internship sponsored by Google Summer of Code

<br/>

<br/>
 The Tor Project is excited to once again participate in Google Summer of Code, an annual program that connects new contributors with open-source projects
<br/>
  ![](https://blog.torproject.org/tor-in-google-summer-of-code-mentorship/lead.png)

The Tor Project is excited to once again participate in [Google Summer of Code](https://summerofcode.withgoogle.com/programs/2025/organizations/the-tor-project), an annual program that connects new contributors with open-source projects through funded internships. [Sponsored by Google](https://summerofcode.withgoogle.com/how-it-works), GSoC provides an opportunity for students and newcomers to gain hands-on experience while helping maintain and improve critical open-source software like Tor.

For the Tor community, participating in GSoC is invaluable: it brings new contributors into our ecosystem, strengthens the broader open-source movement, and ensures mentorship opportunities for those interested in privacy and anonymity tools. The program removes financial barriers for participants by offering stipends, making it easier for people worldwide to contribute.

Between now and March 23rd, we welcome questions from prospective applicants about Tor and [the projects ideas we prepared](https://gitlab.torproject.org/tpo/team/-/wikis/GSoC). If you're interested, please reach out to gsoc at torproject dot org for any questions or comments. **Applications will be open from March 24th to April 8th 2025.**

Projects for GSoC 2025:
=======================

This year, we’re participating with [three projects](https://gitlab.torproject.org/tpo/team/-/wikis/GSoC) that focus on improving Tor metrics, expanding support for onion services, and optimizing network connectivity. These projects directly contribute to strengthening Tor’s security, reliability, and usability—key aspects of maintaining an open and censorship-resistant internet.

Rewrite metrics-lib in Rust
---------------------------

[Tor Metrics Library](https://gitlab.torproject.org/tpo/network-health/metrics/library.rs) is a Java library that fetches and parses Tor descriptors. It provides a Java API for processing Tor network data from the CollecTor service for statistical analysis and for building services and applications.

This project would involve a complete re-implementation of the Tor metrics library in Rust.

-   Mentors: [Hiro](https://gitlab.torproject.org/hiro) and [Sarthik](https://gitlab.torproject.org/sarthikg)
-   Project: [https://gitlab.torproject.org/tpo/team/-/wikis/GSoC#1-project-rewrite-metrics-lib-in-rust](https://gitlab.torproject.org/tpo/team/-/wikis/GSoC#1-project-rewrite-metrics-lib-in-rust)

Onion Service Support Tooling for Arti
--------------------------------------

Arti has two state management subcommands, `arti hss` and `arti hsc`, for managing the state of onion services and onion service clients, respectively. These commands are currently very limited in functionality, and do not support many of the features onion service clients and operators will require.

This project is about contributing to the tooling onion service clients and operators will need for managing the on-disk state and keys of their Arti onion services. It will involve extending the existing state management commands, as well as potentially adding new ones, and contributing to Arti's APIs and documentation.

-   Mentors: [Gabi](https://gitlab.torproject.org/gabi-250/) and [Wesley](https://gitlab.torproject.org/wesleyac/)
-   Project: [https://gitlab.torproject.org/tpo/team/-/wikis/GSoC#2-project-onion-service-support-tooling-for-arti](https://gitlab.torproject.org/tpo/team/-/wikis/GSoC#2-project-onion-service-support-tooling-for-arti)

Relay to relay connectivity in the Tor network
----------------------------------------------

This project would involve updating and optimizing [erpc](https://gitlab.torproject.org/tpo/network-health/erpc) to keep our dataset manageable. Additionally, it needs research into which algorithms are most suitable to find partitions in the Tor network. Since the network is currently stored as a directed graph, we can apply community detection and clustering algorithms. [Neo4j](https://neo4j.com/) already offers several clustering algorithms within its [Graph Data Science library](https://neo4j.com/docs/graph-data-science/current/installation/).

-   Mentors: [Juga](https://gitlab.torproject.org/juga) and [Georg](https://gitlab.torproject.org/gk)
-   Project: [https://gitlab.torproject.org/tpo/team/-/wikis/GSoC#3-relay-to-relay-connectivity-in-the-tor-network](https://gitlab.torproject.org/tpo/team/-/wikis/GSoC#3-relay-to-relay-connectivity-in-the-tor-network)

GSoC is an excellent opportunity to contribute to privacy-preserving technology while gaining real-world experience in open-source development. Whether you’re passionate about internet freedom, decentralized networks, or privacy tools, we encourage you to apply and help shape the future of Tor. We look forward to welcoming a new generation of contributors to our community!

-   [community](https://blog.torproject.org/category/community)
-   [announcements](https://blog.torproject.org/category/announcements)

#### [Source](https://blog.torproject.org/tor-in-google-summer-of-code-mentorship/)

<br/>
---
