---
title: "XMGoat - Composed of XM Cyber terraform templates that help you learn about common Azure security issues"
date: Sat, 22 Jun 2024 08:30:00 -0400
draft: false
type: posts
categories: 
- Terraform,Vulnerable,XMGoat
---
# XMGoat - Composed of XM Cyber terraform templates that help you learn about common Azure security issues

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/a/AVvXsEjotSIJejjnOiN1ePrfhx9cT-jH_g00-YoZpEC8beHD_5jIGpevCahyF8VZicbnTjnVAbz65lybKMYpSet228oICq_NP1tGaWXEbrkWGRiDkraesFMypltNi_HgrIxh9vGgtcu61sIcUsWUbwcCdTHhSh_2jNJz0zZ70IHQXENOb2nI52hvxH2ZrlYfPi0S=w400-h400)](https://blogger.googleusercontent.com/img/a/AVvXsEjotSIJejjnOiN1ePrfhx9cT-jH_g00-YoZpEC8beHD_5jIGpevCahyF8VZicbnTjnVAbz65lybKMYpSet228oICq_NP1tGaWXEbrkWGRiDkraesFMypltNi_HgrIxh9vGgtcu61sIcUsWUbwcCdTHhSh_2jNJz0zZ70IHQXENOb2nI52hvxH2ZrlYfPi0S)

  

XM [Goat](https://www.kitploit.com/search/label/Goat "Goat") is composed of XM [Cyber](https://www.kitploit.com/search/label/Cyber "Cyber") [terraform](https://www.kitploit.com/search/label/Terraform "terraform") templates that help you learn about common [Azure](https://www.kitploit.com/search/label/Azure "Azure") security issues. Each template is a [vulnerable](https://www.kitploit.com/search/label/Vulnerable "vulnerable") environment, with some significant misconfigurations. Your job is to attack and compromise the environments.

Here's what to do for each environment:

1.  Run installation and then get started.
    
2.  With the initial user and service principal credentials, attack the environment based on the scenario flow (for example, XMGoat/scenarios/scenario\_1/scenario1\_flow.png).
    
3.  If you need help with your attack, refer to the solution (for example, XMGoat/scenarios/scenario\_1/solution.md).
    
4.  When you're done learning the attack, clean up.
    

  

Requirements
------------

-   Azure tenant
-   Terafform version 1.0.9 or above
-   Azure CLI
-   Azure User with Owner permissions on Subscription and Global Admin privileges in AAD

Installation
------------

Run these commands:

```
$ az login$ git clone https://github.com/XMCyber/XMGoat.git$ cd XMGoat$ cd scenarios$ cd scenario_<\SCENARIO>
```

Where <\\SCENARIO> is the scenario number you want to complete

```
$ terraform init$ terraform plan -out <\FILENAME>$ terraform apply <\FILENAME>
```

Where <\\FILENAME> is the name of the output file

Get started
-----------

To get the initial user and service principal credentials, run the following query:

```
$ terraform output --json
```

For Service Principals, use application\_id.value and application\_secret.value.

For Users, use username.value and password.value.

Cleaning up
-----------

After completing the scenario, run the following command in order to clean all the resources created in your tenant

```
$ az login$ cd XMGoat$ cd scenarios$ cd scenario_<\SCENARIO>
```

Where <\\SCENARIO> is the scenario number you want to complete

```
$ terraform destroy
```

  
  

**[Download XMGoat](https://github.com/XMCyber/XMGoat "Download XMGoat")**

[Source](http://www.kitploit.com/2024/06/xmgoat-composed-of-xm-cyber-terraform.html)
<br/>
---
