---
title: "<div>Tutorial: Security scanning in air-gapped environments</div>"
date: 2025-02-06
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Air-gapped environments are computer networks or systems that are physically isolated from unsecured networks, such as the public internet or unsecured local area networks. This isolation is implemented as a security measure to protect sensitive data and critical systems from external cyber threats by providing:

- Enhanced security: By physically isolating systems from external networks, air-gapped environments help prevent remote attacks, malware infections, and unauthorized data access. This is crucial for highly sensitive data and critical systems.
- Data protection: Air-gapping provides the strongest protection against data exfiltration since there's no direct connection that attackers could use to steal information.
- Critical infrastructure protection: For systems that control vital infrastructure (like power plants, water treatment facilities, or military systems), air-gapping helps prevent potentially catastrophic cyber attacks.
- Compliance requirements: Many regulatory frameworks require air-gapping for certain types of sensitive data or critical systems, particularly in government, healthcare, and financial sectors.
- Malware protection: Without network connectivity, systems are protected from network-based malware infections and ransomware attacks.

Even though air-gapped systems are isolated, they can still have vulnerabilities. Regular security scanning helps identify these weaknesses before they can be exploited. In this article, you will learn the different security scanners GitLab provides and how they can be added/updated in a limited-connectivity environment.

## GitLab security scanners in air-gapped environments

GitLab provides a variety of different security scanners for the complete application lifecycle. The scanners that support air-gapped environments include:

- Static Application Security Testing (SAST)
- Dynamic Application Security Testing (DAST)
- Secret Detection
- Container Scanning
- Dependency Scanning
- API Fuzzing
- License Scanning

By default, GitLab Self-Managed instances pull security scanner images from the public GitLab container registry (registry.gitlab.com) and store them within the built-in local GitLab container registry. I will demonstrate this flow below by running the following pipeline that scans for secrets on a sample project:

```yaml
include:
  - template: Jobs/Secret-Detection.gitlab-ci.yml
```

When running the job in an internet-connected GitLab instance the job passes:

![GitLab Runner with internet access successfully pulling from external registry](https://images.ctfassets.net/r9o86ar0p03f/1xjIX1SEB9aG51s9Y71EiE/30da85dd73d7c426408bc3e38f4f6eac/pass-1.png)

<center><i>GitLab Runner with internet access successfully pulling from external registry</i></center>

<br></br> However, If I disable internet access to the VM running GitLab, the `secret-detection` job will fail to download the container image, causing the job to fail:

![GitLab Runner without internet access failing to pull from external registry](https://images.ctfassets.net/r9o86ar0p03f/25NWzvQA2RceTQlb2aLmzk/fa6e62e89a3835be5b9ff02a8fb245da/fail-1.png)

<center><i>GitLab Runner without internet access failing to pull from external registry</i></center> <br></br>

Alternatively, if I set my GitLab Runners’ pull image policy to `if-not-present` from `always`, I can load the cached version of the scanner if it was run before on the internet by using the image stored in our local docker:

![GitLab Runner without internet access successfully pulling from internal registry cache](https://images.ctfassets.net/r9o86ar0p03f/48sKSLIBwq6wuqo6jLCMg6/c286af66ec43c2aefd67d3735d175c57/pass-2.png)

<center><i>GitLab Runner without internet access successfully pulling from internal registry cache</i></center>

<br></br>

### Setting up offline scanning prerequisites

Running these security scanners in an air-gapped environment requires the following:

- GitLab Ultimate subscription
- Offline cloud license
- GitLab Self-Managed cluster

You can follow along with this tutorial in any GitLab Self-Managed EE instance (even those that are not air-gapped) to learn how to transfer and run images in an air-gapped environment. In this tutorial, I will demonstrate how to load scanner images onto a GitLab-EE instance running in a Google Compute VM where I cut off the `EGRESS` to everything by implementing firewall rules:

```bash
# egress firewall rule to block all outbound traffic to the internet
$ gcloud compute firewall-rules create deny-internet-egress 
    --direction=EGRESS 
    --priority=1000 
    --network=default 
    --action=DENY 
    --rules=all 
    --destination-ranges=0.0.0.0/0 
    --target-tags=no-internet

# Create an allow rule for internal traffic with higher priority
$ gcloud compute firewall-rules create allow-internal-egress 
    --direction=EGRESS 
    --priority=900 
    --network=default 
    --action=ALLOW 
    --rules=all 
    --destination-ranges=10.0.0.0/8,192.168.0.0/16,172.16.0.0/12 
    --target-tags=no-internet

# Apply tag to VM
$ gcloud compute instances add-tags YOUR_VM_NAME 
    --zone=YOUR_ZONE 
    --tags=no-internet
```

Then, once I SSH into my VM, you can see we cannot connect to registry.gitlab.com:

```bash
# showing I can’t access the gitlab container registry
$ ping registry.gitlab.com
PING registry.gitlab.com (35.227.35.254) 56(84) bytes of data.
^C
--- registry.gitlab.com ping statistics ---
3 packets transmitted, 0 received, 100% packet loss, time 2031ms
```

**Note:** I am still allowing ingress so I can copy files and SSH into the machine.

## Load security scanners in air-gapped environments

To use the various security scanners on air-gapped environments, the GitLab Runner must be able to fetch the scanner container images from GitLab’s built-in container registry. This means that the container images for the security scanners must be downloaded and packaged in a separate environment with access to the public internet. The process of loading security scanners onto an air-gapped environment includes the following:

1. Download and package container images from the public internet.
2. Transfer images to offline environment.
3. Load transferred images into offline container registry.

Now let’s go over how we can implement GitLab Secret Detection in an air-gapped environment.

### Download and package container images from public internet

Let’s download the container image for secret detection and store it within our local container registry. Other scanner images can be found in the offline deployments documentation. I will be using Podman desktop to download these images, but you can use Docker desktop or other alternatives.

1. Pull the GitLab Secret Detection image.

```bash
$ podman pull registry.gitlab.com/security-products/secrets:6
Trying to pull registry.gitlab.com/security-products/secrets:6...
Getting image source signatures
Copying blob sha256:999745130ac045f2b1c29ecce088b43fc4a95bbb82b7960fb7b8abe0e3801bf8
Copying blob sha256:a4f7c013bb259c146cd8455b7c3943df7ed84b157e42a2348eef16546d8179b1
Copying blob sha256:1f3e46996e2966e4faa5846e56e76e3748b7315e2ded61476c24403d592134f0
Copying blob sha256:400a41f248eb3c870bd2b07073632c49f1e164c8efad56ea3b24098a657ec625
Copying blob sha256:9090f17a5a1bb80bcc6f393b0715210568dd0a7749286e3334a1a08fb32d34e6
Copying blob sha256:c7569783959081164164780f6c1b0bbe1271ee8d291d3e07b2749ae741621ea3
Copying blob sha256:20c7ca6108f808ad5905f6db4f7e3c02b21b69abdea8b45abfa34c0a2ba8bdb5
Copying blob sha256:e8645a00be64d77c6ff301593ce34cd8c17ffb2b36252ca0f2588009a7918d2e
Copying config sha256:0235ed43fc7fb2852c76e2d6196601968ae0375c72a517bef714cd712600f894
Writing manifest to image destination
WARNING: image platform (linux/amd64) does not match the expected platform (linux/arm64)
0235ed43fc7fb2852c76e2d6196601968ae0375c72a517bef714cd712600f894

$ podman images
REPOSITORY                                                  TAG         IMAGE ID      CREATED      SIZE
registry.gitlab.com/security-products/secrets               6           0235ed43fc7f  4 hours ago  85.3 MB
```

2. Save the image as a tarball.

```bash
$ podman save -o secret-detection.tar registry.gitlab.com/security-products/secrets:6
$ chmod +r secret-detection.tar
$ ls -al secret-detection.tar
-rw-r--r--@ 1 fern  staff  85324800 Jan 10 10:25 secret-detection.tar
```

Alternatively, you can use the official GitLab template on an environment with internet access to download the container images needed for the security scanners and save them as job artifacts or push them to the container registry of the project where the pipeline is executed.

### Transfer images to offline environment

Next, let's transfer the tarball to our air-gapped environment. This can be done in several ways, depending on your needs, such as:

- Physical media transfer
- Data diodes
- Guard systems
- Cross-domain solutions (CDS)

I will SCP (Secure Copy Protocol) the tarball directly to my VM that does not have egress access, but does allow ingress. As this is just for demonstration purposes, make sure to consult your organization's security policies and transfer procedures for air-gapped environments.

#### Verify the image is not cached

Before transferring the file, I’ll delete the Docker images on my GitLab instance pertaining to secret detection to make sure they aren't cached:

```bash
$ docker images
REPOSITORY                                                          TAG              IMAGE ID       CREATED        SIZE
registry.gitlab.com/security-products/secrets                       6                0235ed43fc7f   9 hours ago    84.8MB
registry.gitlab.com/security-products/secrets                       <none>           16d88433af61   17 hours ago   74.9MB

$ docker image rmi 16d88433af61 -f
Untagged: registry.gitlab.com/security-products/secrets@sha256:f331da6631d791fcd58d3f23d868475a520f50b02d64000e2faf1def66c75d48
Deleted: sha256:16d88433af618f0b405945031de39fe40b3e8ef1bddb91ca036de0f5b32399d7
Deleted: sha256:1bb06f72f06810e95a70039e797481736e492201f51a03b02d27db055248ab6f
Deleted: sha256:a5ef2325ce4be9b39993ce301f8ed7aad1c854d7ee66f26a56a96967c6606510
Deleted: sha256:f7cdac818a36d6c023763b76a6589c0db7609ca883306af4f38b819e62f29471
Deleted: sha256:5eabf4d47287dee9887b9692d55c8b5f848b50b3b7248f67913036014e74a0e9
Deleted: sha256:51b7cb600604c0737356f17bc02c22bac3a63697f0bf95ba7bacb5b421fdb7da
Deleted: sha256:1546193b011d192aa769a15d3fdd55eb4e187f201f5ff7506243abb02525dc06
Deleted: sha256:1ea72408d0484c3059cc0008539e6f494dc829caa1a97d156795687d42d9cb57
Deleted: sha256:1313ee9da7716d85f63cfdd1129f715e9bbb6c9c0306e4708ee73672b3e40f26
Deleted: sha256:954ebfd83406f0dfed93eb5157ba841af5426aa95d4054174fff45095fd873a1

$ docker image rmi 0235ed43fc7f -f
Untagged: registry.gitlab.com/security-products/secrets:6
Deleted: sha256:0235ed43fc7fb2852c76e2d6196601968ae0375c72a517bef714cd712600f894
Deleted: sha256:f05f85850cf4fac79e279d93afb6645c026de0223d07b396fce86c2f76096c1f
Deleted: sha256:7432b0766b885144990edd3166fbabed081be71d28d186f4d525e52729f06b1f
Deleted: sha256:2c6e3361c2ee2f43bd75fb9c7c12d981ce06df2d51a134965fa47754760efff0
Deleted: sha256:7ad7f7245b45fbe758ebd5788e0ba268a56829715527a9a4bc51708c21af1c7f
Deleted: sha256:3b73a621115a59564979f41552181dce07f3baa17e27428f7fff2155042a1901
Deleted: sha256:78648c2606a7c4c76885806ed976b13e4d008940bd3d7a18b52948a6be71b60d
Deleted: sha256:383d4a6dc5be9914878700809b4a3925379c80ab792dfe9e79d14b0c1d6b5fad
```

Then I'll rerun the job to show the failure:

![GitLab Runner without internet access fails to pull an image from internal registry cache](https://images.ctfassets.net/r9o86ar0p03f/4Qv6C0DQfEWoWqGykW2lNU/be3d6cc478d4d86182f47bc2f8341ade/image2.png)

<center><i>GitLab Runner without internet access fails to pull an image from internal registry cache</i></center>

#### SCP file to GitLab instance

Now, from my local machine, I will SCP the file to my GitLab instance as follows:

```bash
$ gcloud compute scp secret-detection.tar INSTANCE:~ --zone=ZONE
secret-detection.tar                                                          100%   81MB  21.5MB/s   00:03
```

### Load transferred images into offline container registry

Next, I'll SSH into my VM and load the Docker image:

```bash
$ gcloud compute ssh INSTANCE --zone=ZONE

$ sudo docker load -i secret-detection.tar
c3c8e454c212: Loading layer [==================================================>]  2.521MB/2.521MB
51e93afaeedc: Loading layer [==================================================>]  32.55MB/32.55MB
e8a25e39bb30: Loading layer [==================================================>]  221.2kB/221.2kB
390704968493: Loading layer [==================================================>]  225.8kB/225.8kB
76cf57e75f63: Loading layer [==================================================>]  17.64MB/17.64MB
c4c7a681fd10: Loading layer [==================================================>]  4.608kB/4.608kB
f0690f406157: Loading layer [==================================================>]  24.01MB/24.01MB
Loaded image: registry.gitlab.com/security-products/secrets:6
```

### Run the scanners

I'll re-run the pipeline manually and the scanner will be pulled from the cache. Once the pipeline completes, we can see the secret detection job is successful:

![GitLab Runner without internet access successfully pulling from internal registry cache after image loaded](https://images.ctfassets.net/r9o86ar0p03f/2NwioKHVElZI8RWYLnpVGs/90b6a76f403eb46ff8171df844d1f47b/image7.png)

<center><i>GitLab Runner without internet access successfully pulling from internal registry cache after image loaded</center></i>

If you want to pull the image from a different location or you tag your images in a different way, you can edit the config as follows:

```yaml
include:
  - template: Jobs/Secret-Detection.gitlab-ci.yml

variables:
  SECURE_ANALYZERS_PREFIX: "localhost:5000/analyzers"
```

See the offline environments documentation for more information.

### View scanner results

Once the scanner completes on the default branch, a vulnerability report is populated with all the findings. The vulnerability report provides information about vulnerabilities from scans of the default branch.

You can access the vulnerability report by navigating to the side tab and selecting **Secure > Vulnerability Report**:

![GitLab Vulnerability Report with secret detection findings](https://images.ctfassets.net/r9o86ar0p03f/3Mf0eQrW7abnwowcEE1Fyq/96edfacb8008b7cf1bb74e391c218f27/vulnerability_report.png)

<center><i>GitLab Vulnerability Report with secret detection findings</i></center>

<br></br>

The project’s vulnerability report provides:

- totals of vulnerabilities per severity level
- filters for common vulnerability attributes
- details of each vulnerability, presented in tabular layout
- a timestamp showing when it was updated, including a link to the latest pipeline

We can see that two vulnerabilities were detected by the Secret Detection scanner. If we click on a vulnerability, we will be transported to its vulnerability page:

![GitLab Vulnerability Page showing detailed insights](https://images.ctfassets.net/r9o86ar0p03f/1xA76q6EapOqhyuG03eLXr/1aedd70afd87becbb099140facadbaf2/insights.png)

<center><i>GitLab Vulnerability Page showing detailed insights</center></i>

<br></br>

The vulnerability page provides details of the vulnerability, which can be used to triage and find a path to remediation. These vulnerability details include:

- description
- when it was detected
- current status
- available actions
- linked issues
- actions log
- filename and line number of the vulnerability (if available)
- severity

## Read more

To learn more about GitLab and running security scanners in air-gapped environments, check out the following resources:

- GitLab Ultimate
- GitLab Security and Compliance Solutions
- GitLab Offline Deployments Documentation
- GitLab Application Security Documentation

Air-gapped environments are computer networks or systems that are physically isolated from unsecured networks, such as the public internet or unsecured local area networks. This isolation is implemented as a security measure to protect sensitive data and critical systems from external cyber threats by providing:

- Enhanced security: By physically isolating systems from external networks, air-gapped environments help prevent remote attacks, malware infections, and unauthorized data access. This is crucial for highly sensitive data and critical systems.
- Data protection: Air-gapping provides the strongest protection against data exfiltration since there's no direct connection that attackers could use to steal information.
- Critical infrastructure protection: For systems that control vital infrastructure (like power plants, water treatment facilities, or military systems), air-gapping helps prevent potentially catastrophic cyber attacks.
- Compliance requirements: Many regulatory frameworks require air-gapping for certain types of sensitive data or critical systems, particularly in government, healthcare, and financial sectors.
- Malware protection: Without network connectivity, systems are protected from network-based malware infections and ransomware attacks.

Even though air-gapped systems are isolated, they can still have vulnerabilities. Regular security scanning helps identify these weaknesses before they can be exploited. In this article, you will learn the different security scanners GitLab provides and how they can be added/updated in a limited-connectivity environment.

## GitLab security scanners in air-gapped environments

GitLab provides a variety of different security scanners for the complete application lifecycle. The scanners that support air-gapped environments include:

- Static Application Security Testing (SAST)
- Dynamic Application Security Testing (DAST)
- Secret Detection
- Container Scanning
- Dependency Scanning
- API Fuzzing
- License Scanning

By default, GitLab Self-Managed instances pull security scanner images from the public GitLab container registry (registry.gitlab.com) and store them within the built-in local GitLab container registry. I will demonstrate this flow below by running the following pipeline that scans for secrets on a sample project:

```yaml
include:
  - template: Jobs/Secret-Detection.gitlab-ci.yml
```

When running the job in an internet-connected GitLab instance the job passes:

![GitLab Runner with internet access successfully pulling from external registry](https://images.ctfassets.net/r9o86ar0p03f/1xjIX1SEB9aG51s9Y71EiE/30da85dd73d7c426408bc3e38f4f6eac/pass-1.png)

<center><i>GitLab Runner with internet access successfully pulling from external registry</i></center>

<br></br> However, If I disable internet access to the VM running GitLab, the `secret-detection` job will fail to download the container image, causing the job to fail:

![GitLab Runner without internet access failing to pull from external registry](https://images.ctfassets.net/r9o86ar0p03f/25NWzvQA2RceTQlb2aLmzk/fa6e62e89a3835be5b9ff02a8fb245da/fail-1.png)

<center><i>GitLab Runner without internet access failing to pull from external registry</i></center> <br></br>

Alternatively, if I set my GitLab Runners’ pull image policy to `if-not-present` from `always`, I can load the cached version of the scanner if it was run before on the internet by using the image stored in our local docker:

![GitLab Runner without internet access successfully pulling from internal registry cache](https://images.ctfassets.net/r9o86ar0p03f/48sKSLIBwq6wuqo6jLCMg6/c286af66ec43c2aefd67d3735d175c57/pass-2.png)

<center><i>GitLab Runner without internet access successfully pulling from internal registry cache</i></center>

<br></br>

### Setting up offline scanning prerequisites

Running these security scanners in an air-gapped environment requires the following:

- GitLab Ultimate subscription
- Offline cloud license
- GitLab Self-Managed cluster

You can follow along with this tutorial in any GitLab Self-Managed EE instance (even those that are not air-gapped) to learn how to transfer and run images in an air-gapped environment. In this tutorial, I will demonstrate how to load scanner images onto a GitLab-EE instance running in a Google Compute VM where I cut off the `EGRESS` to everything by implementing firewall rules:

```bash
# egress firewall rule to block all outbound traffic to the internet
$ gcloud compute firewall-rules create deny-internet-egress 
    --direction=EGRESS 
    --priority=1000 
    --network=default 
    --action=DENY 
    --rules=all 
    --destination-ranges=0.0.0.0/0 
    --target-tags=no-internet

# Create an allow rule for internal traffic with higher priority
$ gcloud compute firewall-rules create allow-internal-egress 
    --direction=EGRESS 
    --priority=900 
    --network=default 
    --action=ALLOW 
    --rules=all 
    --destination-ranges=10.0.0.0/8,192.168.0.0/16,172.16.0.0/12 
    --target-tags=no-internet

# Apply tag to VM
$ gcloud compute instances add-tags YOUR_VM_NAME 
    --zone=YOUR_ZONE 
    --tags=no-internet
```

Then, once I SSH into my VM, you can see we cannot connect to registry.gitlab.com:

```bash
# showing I can’t access the gitlab container registry
$ ping registry.gitlab.com
PING registry.gitlab.com (35.227.35.254) 56(84) bytes of data.
^C
--- registry.gitlab.com ping statistics ---
3 packets transmitted, 0 received, 100% packet loss, time 2031ms
```

**Note:** I am still allowing ingress so I can copy files and SSH into the machine.

## Load security scanners in air-gapped environments

To use the various security scanners on air-gapped environments, the GitLab Runner must be able to fetch the scanner container images from GitLab’s built-in container registry. This means that the container images for the security scanners must be downloaded and packaged in a separate environment with access to the public internet. The process of loading security scanners onto an air-gapped environment includes the following:

1. Download and package container images from the public internet.
2. Transfer images to offline environment.
3. Load transferred images into offline container registry.

Now let’s go over how we can implement GitLab Secret Detection in an air-gapped environment.

### Download and package container images from public internet

Let’s download the container image for secret detection and store it within our local container registry. Other scanner images can be found in the offline deployments documentation. I will be using Podman desktop to download these images, but you can use Docker desktop or other alternatives.

1. Pull the GitLab Secret Detection image.

```bash
$ podman pull registry.gitlab.com/security-products/secrets:6
Trying to pull registry.gitlab.com/security-products/secrets:6...
Getting image source signatures
Copying blob sha256:999745130ac045f2b1c29ecce088b43fc4a95bbb82b7960fb7b8abe0e3801bf8
Copying blob sha256:a4f7c013bb259c146cd8455b7c3943df7ed84b157e42a2348eef16546d8179b1
Copying blob sha256:1f3e46996e2966e4faa5846e56e76e3748b7315e2ded61476c24403d592134f0
Copying blob sha256:400a41f248eb3c870bd2b07073632c49f1e164c8efad56ea3b24098a657ec625
Copying blob sha256:9090f17a5a1bb80bcc6f393b0715210568dd0a7749286e3334a1a08fb32d34e6
Copying blob sha256:c7569783959081164164780f6c1b0bbe1271ee8d291d3e07b2749ae741621ea3
Copying blob sha256:20c7ca6108f808ad5905f6db4f7e3c02b21b69abdea8b45abfa34c0a2ba8bdb5
Copying blob sha256:e8645a00be64d77c6ff301593ce34cd8c17ffb2b36252ca0f2588009a7918d2e
Copying config sha256:0235ed43fc7fb2852c76e2d6196601968ae0375c72a517bef714cd712600f894
Writing manifest to image destination
WARNING: image platform (linux/amd64) does not match the expected platform (linux/arm64)
0235ed43fc7fb2852c76e2d6196601968ae0375c72a517bef714cd712600f894

$ podman images
REPOSITORY                                                  TAG         IMAGE ID      CREATED      SIZE
registry.gitlab.com/security-products/secrets               6           0235ed43fc7f  4 hours ago  85.3 MB
```

2. Save the image as a tarball.

```bash
$ podman save -o secret-detection.tar registry.gitlab.com/security-products/secrets:6
$ chmod +r secret-detection.tar
$ ls -al secret-detection.tar
-rw-r--r--@ 1 fern  staff  85324800 Jan 10 10:25 secret-detection.tar
```

Alternatively, you can use the official GitLab template on an environment with internet access to download the container images needed for the security scanners and save them as job artifacts or push them to the container registry of the project where the pipeline is executed.

### Transfer images to offline environment

Next, let's transfer the tarball to our air-gapped environment. This can be done in several ways, depending on your needs, such as:

- Physical media transfer
- Data diodes
- Guard systems
- Cross-domain solutions (CDS)

I will SCP (Secure Copy Protocol) the tarball directly to my VM that does not have egress access, but does allow ingress. As this is just for demonstration purposes, make sure to consult your organization's security policies and transfer procedures for air-gapped environments.

#### Verify the image is not cached

Before transferring the file, I’ll delete the Docker images on my GitLab instance pertaining to secret detection to make sure they aren't cached:

```bash
$ docker images
REPOSITORY                                                          TAG              IMAGE ID       CREATED        SIZE
registry.gitlab.com/security-products/secrets                       6                0235ed43fc7f   9 hours ago    84.8MB
registry.gitlab.com/security-products/secrets                       <none>           16d88433af61   17 hours ago   74.9MB

$ docker image rmi 16d88433af61 -f
Untagged: registry.gitlab.com/security-products/secrets@sha256:f331da6631d791fcd58d3f23d868475a520f50b02d64000e2faf1def66c75d48
Deleted: sha256:16d88433af618f0b405945031de39fe40b3e8ef1bddb91ca036de0f5b32399d7
Deleted: sha256:1bb06f72f06810e95a70039e797481736e492201f51a03b02d27db055248ab6f
Deleted: sha256:a5ef2325ce4be9b39993ce301f8ed7aad1c854d7ee66f26a56a96967c6606510
Deleted: sha256:f7cdac818a36d6c023763b76a6589c0db7609ca883306af4f38b819e62f29471
Deleted: sha256:5eabf4d47287dee9887b9692d55c8b5f848b50b3b7248f67913036014e74a0e9
Deleted: sha256:51b7cb600604c0737356f17bc02c22bac3a63697f0bf95ba7bacb5b421fdb7da
Deleted: sha256:1546193b011d192aa769a15d3fdd55eb4e187f201f5ff7506243abb02525dc06
Deleted: sha256:1ea72408d0484c3059cc0008539e6f494dc829caa1a97d156795687d42d9cb57
Deleted: sha256:1313ee9da7716d85f63cfdd1129f715e9bbb6c9c0306e4708ee73672b3e40f26
Deleted: sha256:954ebfd83406f0dfed93eb5157ba841af5426aa95d4054174fff45095fd873a1

$ docker image rmi 0235ed43fc7f -f
Untagged: registry.gitlab.com/security-products/secrets:6
Deleted: sha256:0235ed43fc7fb2852c76e2d6196601968ae0375c72a517bef714cd712600f894
Deleted: sha256:f05f85850cf4fac79e279d93afb6645c026de0223d07b396fce86c2f76096c1f
Deleted: sha256:7432b0766b885144990edd3166fbabed081be71d28d186f4d525e52729f06b1f
Deleted: sha256:2c6e3361c2ee2f43bd75fb9c7c12d981ce06df2d51a134965fa47754760efff0
Deleted: sha256:7ad7f7245b45fbe758ebd5788e0ba268a56829715527a9a4bc51708c21af1c7f
Deleted: sha256:3b73a621115a59564979f41552181dce07f3baa17e27428f7fff2155042a1901
Deleted: sha256:78648c2606a7c4c76885806ed976b13e4d008940bd3d7a18b52948a6be71b60d
Deleted: sha256:383d4a6dc5be9914878700809b4a3925379c80ab792dfe9e79d14b0c1d6b5fad
```

Then I'll rerun the job to show the failure:

![GitLab Runner without internet access fails to pull an image from internal registry cache](https://images.ctfassets.net/r9o86ar0p03f/4Qv6C0DQfEWoWqGykW2lNU/be3d6cc478d4d86182f47bc2f8341ade/image2.png)

<center><i>GitLab Runner without internet access fails to pull an image from internal registry cache</i></center>

#### SCP file to GitLab instance

Now, from my local machine, I will SCP the file to my GitLab instance as follows:

```bash
$ gcloud compute scp secret-detection.tar INSTANCE:~ --zone=ZONE
secret-detection.tar                                                          100%   81MB  21.5MB/s   00:03
```

### Load transferred images into offline container registry

Next, I'll SSH into my VM and load the Docker image:

```bash
$ gcloud compute ssh INSTANCE --zone=ZONE

$ sudo docker load -i secret-detection.tar
c3c8e454c212: Loading layer [==================================================>]  2.521MB/2.521MB
51e93afaeedc: Loading layer [==================================================>]  32.55MB/32.55MB
e8a25e39bb30: Loading layer [==================================================>]  221.2kB/221.2kB
390704968493: Loading layer [==================================================>]  225.8kB/225.8kB
76cf57e75f63: Loading layer [==================================================>]  17.64MB/17.64MB
c4c7a681fd10: Loading layer [==================================================>]  4.608kB/4.608kB
f0690f406157: Loading layer [==================================================>]  24.01MB/24.01MB
Loaded image: registry.gitlab.com/security-products/secrets:6
```

### Run the scanners

I'll re-run the pipeline manually and the scanner will be pulled from the cache. Once the pipeline completes, we can see the secret detection job is successful:

![GitLab Runner without internet access successfully pulling from internal registry cache after image loaded](https://images.ctfassets.net/r9o86ar0p03f/2NwioKHVElZI8RWYLnpVGs/90b6a76f403eb46ff8171df844d1f47b/image7.png)

<center><i>GitLab Runner without internet access successfully pulling from internal registry cache after image loaded</center></i>

If you want to pull the image from a different location or you tag your images in a different way, you can edit the config as follows:

```yaml
include:
  - template: Jobs/Secret-Detection.gitlab-ci.yml

variables:
  SECURE_ANALYZERS_PREFIX: "localhost:5000/analyzers"
```

See the offline environments documentation for more information.

### View scanner results

Once the scanner completes on the default branch, a vulnerability report is populated with all the findings. The vulnerability report provides information about vulnerabilities from scans of the default branch.

You can access the vulnerability report by navigating to the side tab and selecting **Secure > Vulnerability Report**:

![GitLab Vulnerability Report with secret detection findings](https://images.ctfassets.net/r9o86ar0p03f/3Mf0eQrW7abnwowcEE1Fyq/96edfacb8008b7cf1bb74e391c218f27/vulnerability_report.png)

<center><i>GitLab Vulnerability Report with secret detection findings</i></center>

<br></br>

The project’s vulnerability report provides:

- totals of vulnerabilities per severity level
- filters for common vulnerability attributes
- details of each vulnerability, presented in tabular layout
- a timestamp showing when it was updated, including a link to the latest pipeline

We can see that two vulnerabilities were detected by the Secret Detection scanner. If we click on a vulnerability, we will be transported to its vulnerability page:

![GitLab Vulnerability Page showing detailed insights](https://images.ctfassets.net/r9o86ar0p03f/1xA76q6EapOqhyuG03eLXr/1aedd70afd87becbb099140facadbaf2/insights.png)

<center><i>GitLab Vulnerability Page showing detailed insights</center></i>

<br></br>

The vulnerability page provides details of the vulnerability, which can be used to triage and find a path to remediation. These vulnerability details include:

- description
- when it was detected
- current status
- available actions
- linked issues
- actions log
- filename and line number of the vulnerability (if available)
- severity

## Read more

To learn more about GitLab and running security scanners in air-gapped environments, check out the following resources:

- GitLab Ultimate
- GitLab Security and Compliance Solutions
- GitLab Offline Deployments Documentation
- GitLab Application Security Documentation

Go to Source
