---
title: "2018-014- Container Security with Jay Beale"
date: Sun, 29 Apr 2018 23:58:09 +0000
draft: false
type: posts
---
# 2018-014- Container Security with Jay Beale

<br/>

<br/>
Container security

Jay Beale  @inguardians , @jaybeale

Containers

-   What the heck is a container?
-   -   Linux distribution with a kernel
    -   -   Containers run on top of that, sharing the kernel, but not the filesystem
    -   Namespaces
    -   -   Mount
        -   Network
        -   Hostname
        -   PID
        -   IPC
        -   Users
-   Somebody said we’ve had containers since before Docker
-   -   Containers started in 2005, with OpenVZ
    -   Docker was 2013, Kubernetes 2014
-   Image Security
-   -   CoreOS Clair for vuln scanning images
    -   Public repos vs private
    -   Don’t keep the image running for so long?
    -   Don’t run as root
-   More Containment stuff
-   -   Non-privileged containers
    -   Remap the users, so root in container isn’t root outside
    -   Drop root capabilities
    -   Seccomp for kernel syscalls
    -   AppArmor or SELinux
-   All of above is about Docker, what about Kubernetes
-   -   Get onto most recent version of K8S - 1.7 and 1.8 brought big security improvements
    -   Network policy (egress firewalls)
    -   RBAC (define what users and service accounts can do what)
    -   Use namespaces per tenant and think hard about multi-tenancy
    -   Use the CIS guides for lockdown of K8S and the host
    -   Kube-bench

Difference between containers and sandboxing

Roll your own -

    Containers

        Using public registries - leave you vulnerable

        Use your own private repos for deploying containers

Reduce attack surface

Reduce user access

Automation will allow more security to get baked in.

[https://www.infoworld.com/article/3104030/security/5-keys-to-docker-container-security.html](https://www.infoworld.com/article/3104030/security/5-keys-to-docker-container-security.html)

  
  

[https://blog.blackducksoftware.com/8-takeaways-nist-application-container-security-guide](https://blog.blackducksoftware.com/8-takeaways-nist-application-container-security-guide)

  
  
  
  

[https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html)

[https://www.vmware.com/products/thinapp.html](https://www.vmware.com/products/thinapp.html)

[https://www.meetup.com/SEASec-East/events/249983387/](https://www.meetup.com/SEASec-East/events/249983387/)

  
  
  
  

S3 buckets / Azure Blobs

[https://docs.microsoft.com/en-us/azure/architecture/aws-professional/services](https://docs.microsoft.com/en-us/azure/architecture/aws-professional/services)

[https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-policy.html](https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-policy.html)

Join our **#Slack** Channel! Email us at **bds.podcast@gmail.com**

or DM us on Twitter **@brakesec**

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

#### [Source](http://brakeingsecurity.com/2018-014-container-security-with-jay-beale)

<br/>
---
