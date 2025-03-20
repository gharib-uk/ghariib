---
title: "2019-017-K8s Security Kamus interview with Omer Levi Hevroni"
date: Sun, 05 May 2019 23:58:42 +0000
draft: false
type: posts
---
# 2019-017-K8s Security Kamus interview with Omer Levi Hevroni

<br/>

<br/>
K8s security with Omer Levi Hevroni (@omerlh)

service tickets -

Super-Dev

Omer’s requirements for storing secrets:

Gitops enabled

Kubernetes Native

Secure

    “One-way encryption”

Omer’s slides and youtube video:

[https://www.slideshare.net/SolutoTLV/can-kubernetes-keep-a-secret](https://www.slideshare.net/SolutoTLV/can-kubernetes-keep-a-secret)

[https://www.youtube.com/watch?v=FoM3u8G99pc&&index=14&t=0s](https://www.youtube.com/watch?v=FoM3u8G99pc&&index=14&t=0s)

We’ve all experienced it: you’re working on a task, adding some code, and then you need to store some sensitive configuration value. It could be an API key, client secret or an encryption key ― something that’s highly sensitive and must be kept secret. And this is where things get messy. Usually, secret storage is highly coupled with how the code is deployed, and different platforms have different solutions. Kubernetes has a promise to simplify this process by using the native secret object, which, as the name implies, can be used to store secrets or sensitive configurations. Unfortunately, Kubernetes secrets are fundamentally broken, and a developer who tries to use them will definitely have some issues. But no need to worry ― there are solid alternatives for storing secrets securely on Kubernetes platform. One solution is to use Kamus, an open-source, git-ops solution, that created by Soluto, for managing secrets on Kubernetes. Kamus can encrypt a secret so it can be decrypted only by your app on runtime - and not by anyone else. The first part of this session will cover the challenges faced when using Kubernetes secrets (from a usability and security point of view). The second part will discuss some of the existing solutions (Sealed Secrets, Helm Secrets and others), their pros, and cons, and then feature Kamus: how it works, what problems it solves, how it differs from other solutions, and what threats it can help mitigate (and what threats it can’t). The talk will cover all that is required to know so you can run Kamus on your own cluster and use it for secret management. Join me for this session to learn how you can build a Kubernetes cluster than can keep a secret ― for real. Speakers Omer Levi Hevroni

Kubernetes Secrets

    Bad, because manifest files hold the user/password, and are encoded in Base64

        Could be uploaded to git = super bad

https://kubernetes.io/docs/concepts/configuration/secret/

[https://docs.travis-ci.com/user/encryption-keys/](https://docs.travis-ci.com/user/encryption-keys/)

Kamus threat model on Github: [https://kamus.soluto.io/docs/threatmodeling/threats\_controls/](https://kamus.soluto.io/docs/threatmodeling/threats_controls/)

[https://medium.com/@BoweiHan/an-introduction-to-serverless-and-faas-functions-as-a-service-fb5cec0417b2](https://medium.com/@BoweiHan/an-introduction-to-serverless-and-faas-functions-as-a-service-fb5cec0417b2)

    “**FaaS** is a relatively new concept that was first made available in 2014 by hook.io and is now implemented in services such as AWS Lambda, Google Cloud Functions, IBM OpenWhisk and Microsoft Azure Functions.”

Best practices: [https://kubernetes.io/docs/tasks/administer-cluster/securing-a-cluster/](https://kubernetes.io/docs/tasks/administer-cluster/securing-a-cluster/)

[https://github.com/owasp-cloud-security/owasp-cloud-security](https://github.com/owasp-cloud-security/owasp-cloud-security)

[https://www.omerlh.info/2019/01/19/threat-modeling-as-code/](https://www.omerlh.info/2019/01/19/threat-modeling-as-code/)

[https://telaviv.appsecglobal.org/](https://telaviv.appsecglobal.org/)

[https://github.com/Soluto/kamus](https://github.com/Soluto/kamus)

[https://kamus.soluto.io](https://kamus.soluto.io)

Infosec Campout = www.infoseccampout.com

#### [Source](http://brakeingsecurity.com/2019-017-k8s-security-kamus-interview-with-omer-levi-hevroni)

<br/>
---
