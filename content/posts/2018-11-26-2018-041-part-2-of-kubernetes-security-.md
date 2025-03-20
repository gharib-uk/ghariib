---
title: "2018-041 part 2 of Kubernetes security insights w ian Coldwater"
date: Mon, 26 Nov 2018 05:42:50 +0000
draft: false
type: posts
---
# 2018-041 part 2 of Kubernetes security insights w ian Coldwater

<br/>

<br/>
@IanColdwater  [https://www.redteamsecure.com/](https://www.redteamsecure.com/) \*new gig\*

So many different moving parts

Plugins

Code

Hardware

She’s working on speaking schedule for 2019

How would I use these at home?

    [https://kubernetes.io/docs/setup/minikube/](https://kubernetes.io/docs/setup/minikube/)

Kubernetes - up and running

    [https://www.amazon.com/Kubernetes-Running-Dive-Future-Infrastructure/dp/1491935677](https://www.amazon.com/Kubernetes-Running-Dive-Future-Infrastructure/dp/1491935677)

General wikipedia article (with architecture diagram): [https://en.wikipedia.org/wiki/Kubernetes](https://en.wikipedia.org/wiki/Kubernetes)

[https://twitter.com/alicegoldfuss](https://twitter.com/alicegoldfuss) \- Alice Goldfuss

Derbycon Talk: [http://www.irongeek.com/i.php?page=videos/derbycon8/track-3-10-perfect-storm-taking-the-helm-of-kubernetes-ian-coldwater](http://www.irongeek.com/i.php?page=videos/derbycon8/track-3-10-perfect-storm-taking-the-helm-of-kubernetes-ian-coldwater)

Tesla mis-configured Kubes env:

From the talk: [https://arstechnica.com/information-technology/2018/02/tesla-cloud-resources-are-hacked-to-run-cryptocurrency-mining-malware/](https://arstechnica.com/information-technology/2018/02/tesla-cloud-resources-are-hacked-to-run-cryptocurrency-mining-malware/)

Redlock report mentioned in Ars article:  [https://redlock.io/blog/cryptojacking-tesla](https://redlock.io/blog/cryptojacking-tesla)

Setup your own K8s environment: [https://kubernetes.io/docs/setup/pick-right-solution/#local-machine-solutions](https://kubernetes.io/docs/setup/pick-right-solution/#local-machine-solutions) (many options to choose from)

Securing K8s implementations: [https://kubernetes.io/docs/tasks/administer-cluster/securing-a-cluster/](https://kubernetes.io/docs/tasks/administer-cluster/securing-a-cluster/)

[https://github.com/aquasecurity/kube-hunter](https://github.com/aquasecurity/kube-hunter) \-

  

Threat Model  
    What R U protecting?

    Who R U protecting from?

    What R your Adversary’s capabilities?

    What R your capabilities?

Defenders think in Lists

Attackers think in Graphs

What are some of the visible ports used in K8S?

    44134/tcp - Helmtiller, weave, calico

    10250/tcp - kubelet (kublet exploit)

        No authN, completely open

    10255/tcp - kublet port (read-only)

    4194/tcp - cAdvisor

    2379/tcp - etcd

        Etcd holds all the configs

        Config storage

Engineering workflow:

    Ephemeral -  

CVE for K8S subpath - https://kubernetes.io/blog/2018/04/04/fixing-subpath-volume-vulnerability/

Final points:

    Advice securing K8S is standard security advice

    Use Defense in Depth, and least Privilege

    Be aware of your attack surface

    Keep your threat model in mind

David Cybuck (questions from Slack channel)

My questions are: 1. Talk telemetry?  What is the best first step for having my containers or kubernetes report information?  (my overlords want metrics dashboards which lead to useful metrics).

2.  How do you threat model your containers?  Has she ever or how would she begin to run a table-top exercise, a cross between a threat model and a disaster recovery walk through, for the container infrastructure?

3.  Mitre Att&ck framework, there is a spin off for mobile.  Do we need one for Kube, swarm, or DC/OS?

#### [Source](http://brakeingsecurity.com/2018-041-part-2-of-kubernetes-security-insights-w-ian-coldwater)

<br/>
---
