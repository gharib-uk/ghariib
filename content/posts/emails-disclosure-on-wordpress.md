---
title: "Emails Disclosure on WordPress"
date: 2025-01-22
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "forensics"
  - "privacy"
  - "security"
  - "security-awareness"
  - "security-awareness-3-0"
  - "wordpress"
tags: 
  - "email"
  - "leaks"
---

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgq2xqKk1ubaVqsHfi6sEA09BIrKp0tq5owY7LB4nlg8eNKmroRMqHEqFoTabFGi0ZLEQYie07RtSjDbve08MCwVdLisOINcEcnNfcPkHmWCRgAJXyMBOmDRhEc_o5G-ZRKVnxJbtBiMeaK/s320/blog-email-disclosure-wordpress-image-1%255B1%255D.jpg)

Password brute force is one of the common most attack on WordPress. Only a few hours after the deployment of a new blog, we can see login attempts to /xmlrpc.php or /wp-login.php endpoints. While not being sophisticated, they remain strong attacks as they put pressure on the limited complexity passwords and potential password reuse from users. In this article, we are going to explain how the public

wordpress.com REST API makes it easier for brute-force attacks on millions of WordPress instances managed by wordpress.com or private instances with the Jetpack plugin installed.

  

  

## WordPress Out of the Box

### User Enumeration

Out of the box, user accounts can be enumerated with unauthenticated REST API requests. This API will return all authors who have written at least one post. Additionally, the generated welcome post, automatically created on new installs, exposes the administrator user by default.

In the screen capture below, we can see 2 interesting pieces of metadata for the administrative user: its username (**superadmin**) and its email address hash in the form of a gravatar link (http://0.gravatar.com/avatar/6e2b22791df03c1290687b2807f52afd). Email MD5 hashes can be reversed (cracked) in most cases. For example, Dominique Bongard presented at PasswordCon 2013 that he manage to recover 45% from a sample of 2800 hashes taken on a french political website.

  

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj8SeEukR5XM2-szWIB0SfmgXas9y3nAcj5mj7jyDOuEC1iJDJ49x7Pp0wuroy3_1kW_qCaIbUSPqhbCfwf6W3uD7qFLCngLofSwUXOq9FxQVqN8F9G6j59kxG61HN9NXIejKDYo0U2sIzX/w640-h502/blog-email-disclosure-wordpress-image-2%255B1%255D.png) |
| --- |
| /wp/v2/users endpoints accessible by default |

  

  

### Potential Hardening

The simplest way to avoid this user enumeration is to disable the REST API. It can be done by installing a third-party plugin. There are also plenty of manual code edits that are being used to avoid installing third-party plugins. The iThemes Security plugin does filtering on the most sensitive endpoints by default.

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiNxxdPtnWCx8AsyLN3fid3CPBzMJEXvs16qXZBT-vqOTB0-dGKGYLmxFd0RFeBZ8PbR4_uWYCjM5xNysgJrR7FiEzZ6atvlIoS4rc966xSfnWpehwbRrWwb6XaWC36QcXj5WUjSMjfS4pg/w640-h560/blog-email-disclosure-wordpress-image-3%255B1%255D.png) |
| --- |
| Examples of errors messages |

  

## The Jetpack Plugin

### Jetpack and WordPress.com

Jetpack comes with the promise of improving security, performance, and management. Being developed by Automattic, the company supporting WordPress.com, the plugin is said to be installed on 5 million WordPress instances. To activate the plugin, the user must authenticate with a valid wordpress.com account. Most of the dashboard features and settings are hosted on wordpress.com. With the help of the Jetpack plugin, wordpress.com will obtain an authentication token to allow the synchronization of data including the list of users and posts.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhEY_da0NB3JG0GrOGLPo19bl-NVAMOHIEs4P2iEeOy0WmU811pTGqGhXucaNll2wPc1-VG-VmQpdlYWuzl0bVLTAlQpEjfj6gHMvmdSMIIzh-hvQJ6XqG5s7TvKz92X9Byrg_NGD4IYAMM/w640-h506/blog-email-disclosure-wordpress-image-4%255B1%255D.png)

  

WordPress is connecting through a privileged XML-RPC connection

The Jetpack plugin by itself is not causing any harm, however, part of the data that it synchronizes on WordPress.com servers is exposed publicly with a documented REST API.

Looking at the REST API for potential information leaks, we can see that the main endpoints returning user information are protected.

| **REST URL** | **Require Authentication Token** |
| --- | --- |
| /sites/$site/users | YES |
| /sites/$site/users/login:$user\_id | YES |
| /me | YES |

![blog-email-disclosure-wordpress-image-7](https://3akfc19rcxr3p4ohv3z7zzp6-wpengine.netdna-ssl.com/wp-content/uploads/blog-email-disclosure-wordpress-image-7.png "blog-email-disclosure-wordpress-image-7")

User information returned

However, most of the user metadata is also publicly available on the posts endpoints. The following endpoints are returning post objects which include author metadata. The author object includes the username and the hashed e-mail via the Gravatar URL.

| **REST URL** | **Require Authentication Token** |
| --- | --- |
| /sites/$site/posts/$post\_ID | NO |
| /sites/$site/posts | NO |

  

  

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgQJO2vgSewfmud7D8c91hqxpdbpvvIZ23NAPP4XCr6eAHVAABOYLkP7kegd25tTTGY1sw3dyqJ4GebJjrHvvswQHKFMzZPV6EbqD7ClpvphM6aqQdNxy90FMsoKDkyHijG8IpYnmh4ZCUi/w472-h640/blog-email-disclosure-wordpress-image-5%255B1%255D.png) |
| --- |
|   Information available publicly on post related endpoints   |

  

### Why are emails so important?

First, there are privacy concerns with the disclosure of user emails. A user might want to remain anonymous when publishing their personal blog. Some organizations might need to protect their employees to avoid attacks targeting them. Associated metadata, such the organization and website, could also be used to personalize phishing attacks.

While we could extend on the topic of privacy and phishing, the present article will focus on the application security aspects. Email disclosure of all users can greatly improve brute force attack success rates. As we live in an era of periodic password breaches, single factor authentication is more vulnerable than ever to brute force attacks. An attacker can correlate the list of a specific user’s email addresses against known passwords that they have used.

**Potential abuse scenario**

An automated bot could do the following steps:

1. Enumerate the user’s emails for a given blog
2. Attempt to log in with every password known for the specific emails
3. For a successful login, verify the permission of the user.
4. Install a malicious plugin / Edit template page (PHP) / Insert malicious HTML in article post / etc…

### Quick data analysis

Since email addresses are in a relatively straightforward format, obfuscating them as MD5 in order to anonymize them is no longer sufficient. Using modern GPUs and simple rulesets we were able to deanonymize over 53 million emails out of the 91 million hashes we extracted with the WordPress.com API. 32 million users had one password leak in the past 10 years. This is at least 36% percent accounts that are more susceptible to credential stuffing attacks. The correlation was made using a collection of 2.6 billion leaked emails and passwords provided by Flare Systems.

When we analyzed the dataset, we verified the number of illegitimate accounts. Less than 1% of the accounts were either of invalid email format, temporary email inboxes or known spam emails. This is a sign that Automattic is actively closing spam accounts. The remaining emails are likely to be legitimate WordPress users.

| **Category** |  | **Numbers** |
| --- | --- | --- |
| Emails that could not be cracked (40%) |  | 36,381,990 |
| Cracked Emails (60%) | With at least one leaked password | 32,735,798 |
| With no leaked password found | 20,644,054 |
| Invalid | 480,879 |

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjD3tppWIOOQuqkx0cLd1VFKfS-M7XzbxQ4d4_tYWpMjHGsWLnQjKXRGFYd9rOUj-cy69ZsTBEenXJMlDWqdNCcyqG3tw4L33w2XhKUFH9PtLFdHD7GbZrSPj3UgxVX6IX6mRk2eVsoQR7-/w640-h440/blog-email-disclosure-wordpress-image-6%255B1%255D.png)

  

Here are some of the notable organizations using WordPress with user emails being exposed:

| **Organization** | **Number of Users** | **Alexa Rank** |
| --- | --- | --- |
| newsfeed.time.com | 186 | 1524th |
| vimeo.com/blog | 177 | 170th |
| blog.ted.com | 120 | 1291st |
| blog.etsy.com | 2570 | 76th |
| techcrunch.com | 57 | 1754th |
| wired.com | 1052 | 1525th |
| devblogs.microsoft.com | 1000+ | 21st |
| gblogs.cisco.com | 123 | 824th |
| godaddy.com/garage | 1430 | 172nd |
| books.disney.com | 14 | 4436th |

## Moving Forward

### Response from WordPress.com

Regarding the e-mail disclosure through the Gravatar URL, the Automattic team responded with the following message:

> “Thanks for the report.
> 
> I have just confirmed with the team that Gravatar is working as it should be. The information that is exposed by Gravatar are intended for the public.”

Our interpretation is that Automattic is not willing to remove the Gravatar integration from their APIs.

## Conclusion

Easy user enumeration is not a fatal flaw. However, it is something to keep in mind if you are deploying a blog for commercial or personal purposes. Here are a few ideas to mitigate this risk:

- Enforce multi-factor authentication for all your users with the JetPack plugin
- Force users to use generated passwords
- Disable the Jetpack plugin if you are not using any of its features

  

_This blog was originally posted on GoSecure blog._

Go to Source
