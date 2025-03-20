---
title: "2019-032-kubernetes security audit dicussion with Jay Beale and Aaron Small"
date: Sat, 31 Aug 2019 19:00:57 +0000
draft: false
type: posts
---
# 2019-032-kubernetes security audit dicussion with Jay Beale and Aaron Small

<br/>

<br/>
Topics:  
  
Infosec Campout report

Derbycon Pizza Party (with podcast show!)  [https://www.eventbrite.com/e/brakesec-pizza-party-at-the-derbycon-mental-health-village-tickets-69219271705](https://www.eventbrite.com/e/brakesec-pizza-party-at-the-derbycon-mental-health-village-tickets-69219271705)  

Mental health village at Derbycon

Jay Beale (co-lead for audit) \*Bust-a-Kube\*  

Aaron Small (product mgr at GKE/Google)

  

Atreides Partners

Trail of Bits

What was the Audit? 

How did it come about? 

Who were the players?

    Kubernetes Working Group

        Aaron, Craig, Jay, Joel

    Outside vendors:

        Atredis: Josh, Nathan Keltner

        Trail of Bits: Stefan Edwards, Bobby Tonic , Dominik

    Kubernetes Project Leads/Devs

        Interviewed devs -- this was much of the info that went into the threat model

        Rapid Risk Assessments - let’s put the GitHub repository in the show notes

What did it produce?

    Vuln Report

    Threat Model - https://github.com/kubernetes/community/blob/master/wg-security-audit/findings/Kubernetes%20Threat%20Model.pdf

    White Papers

    [https://github.com/kubernetes/community/tree/master/wg-security-audit/findings](https://github.com/kubernetes/community/tree/master/wg-security-audit/findings)

    Discuss the results:

        Threat model findings

            Controls silently fail, leading to a false sense of security

                Pod Security Policies, Egress Network Rules

            Audit model isn’t strong enough for non-repudiation

                By default, API server doesn’t log user movements through system

            TLS Encryption weaknesses

                Most components accept cleartext HTTP

                Boot strapping to add Kubelets is particularly weak       

                Multiple components do not check certificates and/or use self-signed certs

                HTTPS isn’t enforced

                Certificates are long-lived, with no revocation capability

                Etcd doesn’t authenticate connections by default

            Controllers all Bundled together

                Confused Deputy: b/c lower priv controllers bundled in same binary as higher

            Secrets not encrypted at rest by default

            Etcd doesn’t have signatures on its write-ahead log

            DoS attack: you can set anti-affinity on your pods to get nothing else scheduled on their nodes

            Port 10255 has an unauthenticated HTTP server for status and health checking

  

        **Vulns / Findings** (not complete list, but interesting)

            Hostpath pod security policy bypass via persistent volumes

            TOCTOU when moving PID to manager’s group

            Improperly patched directory traversal in kubectl cp

            Bearer tokens revealed in logs

            Lots of MitM risk:

            SSH not checking fingerprints: InsecureIgnoreHostKey

            gRPC transport seems all set to WithInsecure()

HTTPS connections not checking certs 

            Some HTTPS connections are unauthenticated

            Output encoding on JSON construction

                **This might lead to further work, as JSON can get written to logs that may be consumed elsewhere.**

            Non-constant time check on passwords

Lack of re-use / library-ification of code

    Who will use these findings and how? Devs, google, bad guys? 

    Any new audit tools created from this? 

Brad geesaman “**Hacking and Hardening Kubernetes Clusters by Example \[I\] - Brad Geesaman, Symantec**   https://www.youtube.com/watch?v=vTgQLzeBfRU

Aaron Small: 

[https://cloud.google.com/blog/products/gcp/precious-cargo-securing-containers-with-kubernetes-engine-18](https://cloud.google.com/blog/products/gcp/precious-cargo-securing-containers-with-kubernetes-engine-18) 

[https://cloud.google.com/blog/products/gcp/exploring-container-security-running-a-tight-ship-with-kubernetes-engine-1-10](https://cloud.google.com/blog/products/gcp/exploring-container-security-running-a-tight-ship-with-kubernetes-engine-1-10)

[https://cloud.google.com/kubernetes-engine/docs/how-to/hardening-your-cluster](https://cloud.google.com/kubernetes-engine/docs/how-to/hardening-your-cluster) 

CNCF:  [https://www.youtube.com/watch?v=90kZRyPcRZw](https://www.youtube.com/watch?v=90kZRyPcRZw) 

  
  
  

Findings:

Scope for testing:

        **Source code review (what languages did they have to review?)**

            **Golang, shell, ...**

Networking (discuss the networking \*internal\* \*external\*

Cryptography (TLS, data stores)

AuthN/AuthZ 

RBAC (which roles were tested? Just admin/non-admin \*best practice is no admin/least priv\*)

Secrets

Namespace traversals

Namespace claims

Methodology:

  

Setup a bunch of environments?

    _Primarily set up a single environment IIRC_

    _Combination of code audit and active ?fuzzing?_

        _What does one fuzz on a K8s environment?_

Tested with latest alpha or production versions?

    _Version 1.13 or 1.14 - version locked at whatever was current - K8S releases a new version every 3 months, so this is a challenge and means we have to keep auditing._

Tested mulitple different types of k8s implementations?

    _Tested primarily against kubespray (https://github.com/kubernetes-sigs/kubespray)_

  

Bug Bounty program:

[https://github.com/kubernetes/community/blob/master/contributors/guide/bug-bounty.md](https://github.com/kubernetes/community/blob/master/contributors/guide/bug-bounty.md)

Check out our Store on Teepub! **https://brakesec.com/store**

**Join us on our #Slack Channel! Send a request to @brakesec on Twitter or email bds.podcast@gmail.com**

#**Brakesec** Store!:[https://www.teepublic.com/user/bdspodcast](https://www.teepublic.com/user/bdspodcast)

**#Spotify**: [https://brakesec.com/spotifyBDS](https://brakesec.com/spotifyBDS)

**#RSS**: [https://brakesec.com/BrakesecRSS](https://brakesec.com/BrakesecRSS)

**#Youtube Channel**:  [http://www.youtube.com/c/BDSPodcast](http://www.youtube.com/c/BDSPodcast)

**#iTunes** Store Link: [https://brakesec.com/BDSiTunes](https://brakesec.com/BDSiTunes)

**#Google Play** Store: [https://brakesec.com/BDS-GooglePlay](https://brakesec.com/BDS-GooglePlay)

Our main site:  [https://brakesec.com/bdswebsite](https://brakesec.com/bdswebsite)

**#iHeartRadio** App:  [https://brakesec.com/iHeartBrakesec](https://brakesec.com/iHeartBrakesec)

**#SoundCloud**: [https://brakesec.com/SoundcloudBrakesec](https://brakesec.com/SoundcloudBrakesec)

Comments, Questions, Feedback: **[bds.podcast@gmail.com](mailto:bds.podcast@gmail.com)**

Support Brakeing Down Security Podcast by using our **#Paypal**: [https://brakesec.com/PaypalBDS](https://brakesec.com/PaypalBDS) OR our **#Patreon**

[https://brakesec.com/BDSPatreon](https://brakesec.com/BDSPatreon)

**#Twitter**: [@brakesec](https://twitter.com/brakesec) [@boettcherpwned](https://twitter.com/boettcherpwned) [@bryanbrake](https://twitter.com/bryanbrake) [@infosystir](https://twitter.com/infosystir)

**#Player.FM** : [https://brakesec.com/BDS-PlayerFM](https://brakesec.com/BDS-PlayerFM)

**#Stitcher** Network: [https://brakesec.com/BrakeSecStitcher](https://brakesec.com/BrakeSecStitcher)

**#TuneIn** Radio App: [https://brakesec.com/TuneInBrakesec](https://brakesec.com/TuneInBrakesec)

#### [Source](http://brakeingsecurity.com/2019-032-kubernetes-security-audit-dicussion-with-jay-beale-and-aaron-small)

<br/>
---
