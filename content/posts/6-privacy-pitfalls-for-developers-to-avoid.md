---
title: "6 Privacy Pitfalls for Developers to Avoid"
date: 2025-01-22
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "forensics"
  - "privacy"
  - "security"
  - "security-awareness"
  - "security-awareness-3-0"
tags: 
  - "anti-patterns"
  - "design"
---

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg1BM9Mqi1_2r34B27V2lS1KaekgvFgGVPuT_kNGRRdYMlOr3y0osyZ7Vbs6FcYmgbyJbb-RcQ3I-z6DZifKGhYE2b3NxS312wL-sjl0Nw-xVH81SYUx3rBCe_R2ydamcAEim2mToSHJ83otc7WPN1MlM3OYK6o_r0es7RbEqILrkqwFfrAMmcMvvs6IA/w640-h426/6privacy_front-1024x683-1.jpg)

While tech-savvy people are very concerned about privacy, knowing where to find metadata leaks can be nebulous even for developers. In this blog post, we will explore examples of unexpected user information leakage. We hope that the information shared in this blog will help developers assess and address potential privacy issues with their applications, as well as educate end-users about potential risks to their privacy that can result from information leaks.

We picked six examples based on design flaws that are often overlooked. We recognize that common vulnerabilities such as SQL injection and memory corruption often lead to confidentiality and privacy issues as well. We feel that these issues also pose a significant risk to privacy and can compromise personal data if not addressed properly.

### #1 – Gravatar: Weak Encoding of Private Information

The Gravatar service is a centralized avatar service where users upload a personal image, or avatar, that will be used by all services integrating with Gravatar. It removes the burden from the website to handle image upload and manage the user’s avatars. From a performance and security perspective, this can be highly beneficial but in return, the identity of users can be exposed.

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh8xV_OkUGpr3xy7bWS3NdI6dFA-89P_4ctUJAc4ZZqGz-0fSpQkVmvvZ9abEHJ2nxf9OQvneMsbALhKO82E91Zf_oiTQ0ktVphlQd7szICHd7ZVfSDqASRjKxTvRnL61sT8TgQ7kbUnTGYsuMCZinDepPHANx-oYLId43WMiB83PAW6EF0SZ_uNnJ6sQ/w518-h640/6privacy_gravatar.png) |
| --- |
| Gravatar integration on a blog |

  

To fetch those images, a URL needs to be built with the following format: a hash of the email address appended to Gravatar’s base URL along with some optional parameters.

`MD5("philippe@confoo.ca") => **06b856f7ee1266fbf86eaa018f5b0906**   https://secure.gravatar.com/avatar/**06b856f7ee1266fbf86eaa018f5b0906**?s=96&d=identicon&r=G`

Since 2007, Gravatar’s use has spread to hundreds of millions of websites. Being so widespread, it is easy to forget that it can negatively affect users. To demonstrate this, we sampled 91 million Gravatar hashes from the public WordPress.com API. From those, 60% were reversed easily into email form. To crack those hashes, we used some decent GPUs and hashcat. At this point, we disagree with Gravatar’s statement that no email addresses were exposed: A cryptographic hash function like MD5 applied to an email address does not provide sufficient protection to keep it private when it is exposed publicly as part of an API.

#### What is the risk for your users?

First, there are privacy concerns with the disclosure of user emails. A user might want to remain anonymous when commenting or using specific websites. Some organizations might need to protect their employees to avoid attacks targeting them.

Email disclosure of all users can also greatly improve brute force attack success rates. Since we live in an era of periodic password breaches, single-factor authentication is more vulnerable than ever to credential stuffing attacks. An attacker can correlate the list of a specific user’s email addresses against known passwords found in breaches.

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgpDdsxLU12TgzmadWkGMc7yvY92K7_Nyja4xznhefs7zbpbPVpy5UqrMz9ofqlxybLFKOYrrFIaR9588kqQ3qoNalm97IRdeSwTszKwKO7LaRlSw-7RjR8RFREBkkB6BF8-hF_CeDd_Ezw2KgraaQ9XNXiKYc5f9arADJcOV1i6tFQ81IdOaqTVXh09g/w640-h230/6privacy_gravatar_risk.png) |
| --- |
| Emails can serve as pivot points for effective credential stuffing |

  

#### Mitigations

- The first and most important mitigation would be to avoid using Gravatar if your users’ anonymity is required.
- If you want to make it much harder to deanonymize your users, you can download the image server-side rather than pointing to the Gravatar service. Image correlation might still be possible, but it will be much more time-consuming than targeting a specific hash. (See next section)
- As a complement to this, the implementation of a Multiple-Factor Authentication is your best ally to defend against credential stuffing.

### #2 – Facebook Login and Profile Picture: Correlation

Like Gravatar, using Facebook’s profile photo provided by the Facebook API can lead to the deanonymization of your users.

When integrating Facebook Login, it is tempting for developers to use available properties to complete the profile of a newly onboarded user. Once the user is authenticated, it is possible to obtain the public profile picture from this user by using a temporary or permanent graph URL.

`https://graph.facebook.com/**<USER_ID>**/picture?type=large`

We can’t query the graph API with this user id unless we have specially requested authorization from the user. We can, however, correlate the associated Facebook profile by comparing the picture content.

As an example, we pick one website using Facebook Login to authenticate its users: Strava. Strava – a fitness tracker application – is making great efforts to provide different levels of anonymity. Here is a user who created their account with Facebook Login. This can be proven by the presence of the graph URL:

`https://graph.facebook.com/**3096958013955938**/picture?height=256&width=256`

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgjipb_dZ-VeHUlUoAKk3RCBvBrHu5-ZClfp_4nB2adN8Gvr1elLRYap5LbJ2r47d5u529067Wg2YO1NlrcfrzoAh5Z2oJ3beu8sE5WfdBWozoF7TplA31IrS0eDiTkohJVAKhBTYhzAkzS17-2JWIdFvYEMOG0XOfeV-CCs7-KufGauN7Fl0PE0AL4yA/w640-h525/6privacy_facebook.png)

  

  

   
Here is the same profile picture when visiting their Facebook profile directly: a different URL is used to render the user photo:

`https://xxxx.fbcdn.net/v/t1.18169-1/11034273_1385063198478770_5760666877135868228_n.jpg`

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjBYk8mzMADZsCDp12KVkrX9NfyX9a9Hn6XFifz2QeUqeEr3mjLoIudxzOKM4rcMRaZZpLI9dX7QdUlI7qZ32fD8d_e98eEOsN_0dm84N2zNl5j2LhodhHLrqBT4Nt3Bifu7jWyjjAd7sCTh4fTIeQ9dKbt-8SS5Yx3WbjKxHvlYBbhCJOIcYkTFSBZxQ/w640-h482/6privacy_facebook2-1024x771-1.png)

  

  

   
Although a different URL is used to render the user photo, if we compare both images’ content, we can see that both images are identical:

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhyrHWroI7VgQpUwYWrbG2f6zft4cRiekYuuXw8nymend0YOKpd9fVZb_MO7ItXKybA4NTi_I1k5q5lXub2-HTnTvsJU8K-8uvj6Y5-TiAQV9YM7iosAnzw51se0EnEbZBNY5Fi0FfSXd080fEedb53IiMGXEGfHsP5mxYidK7XwG-Sr8vussr4ttDHBg/w640-h480/6privacy_facebook3-1024x767-1.png) |
| --- |
| Comparer view in Burp showing that the content is identical |

  

  

#### What does this mean for an attacker?

A motivated attacker could index the signature of the picture for the different profiles they target. From this database, a quick lookup could reveal the Facebook profile associated with any profile with a Graph picture reference. The Graph URL will have to be adjusted to return an image that matches the database. (For example, the query string can be changed from **?height=256&width=256** to **?height=200&width=200.**)

#### Potential defenses to reduce the risk

Re-encoding the image can increase the effort for a malicious actor trying to index profile images. Alternative methods could be used to match images. Exact pixel matching would, however, be prone to produce a false-negative due to compression artifacts and Convolutional Neural Network (CNN) would generate many false positives.

### #3 – Metadata Embedded in Various File Format

Documents and files can contain small bits of information that can be helpful to profile their author. Metadata in images can reveal additional information like the software version and local paths (some image generators will include those). More importantly, most photos taken with a cellphone will include GPS coordinates. It is the website’s duty to strip that information out of the images they expect to be shared publicly.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjtshDs2TKSF4UkAXTc81c3pk3iysDWYyTmKByxN7cSQmxI_X8KwX5TanOkt16saA49YXRNuzuTgOzCrv1zXIdqPtO9ovv1L0FWralJOuHLkVBc7YHVPmfUdEWh99RrA0kaQN4wdGwcEsRmKp3Xl2TshnzMRoqWGYHZJc8LePIZwwZA6ldoRKDKMmZ2IQ/w640-h358/6privacy_metadata-1024x572-1.png)

Additionally, images downloaded from Facebook can possibly be linked to their source because of a unique ID present in the metadata. This ID has the format **FBMD\_\_\_\_\_**.

#### More than just images

While images are the prime targets for metadata, it is easy to forget that many file formats (ie. Word documents, PDFs, etc.) can also contain metadata. Let’s look at email disclosure in a less common document type: PGP public keys.

Keybase is a website that aims to provide a profile aggregation with mechanisms to validate and connect the user’s identity to external profiles. It is also a platform where users can chat using end-to-end encrypted communication. It also encourages the use of PGP for email exchange. Therefore, a unique pair of keys is generated for all users during their onboarding.

Here is a profile example on Keybase.io:

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjOcKOpOBmugLZL98Dl-IScfm8iwAxjmN7k-YD-FwP1-qDGX_ycytr-fDbpUTHYM6zTLS4IToQFasrf6j0ilOPigSSkF7bMNYLd4oZavIXSqWP8aMe0cXPvIv5c9dyegpqnNBeL6uPABVMNc_2yKKRIMSI4l7QU2GSEP-anYfm19EDzVIFGsw4rJ6J7yA/w640-h522/6privacy_metadata_keybase.png) |
| --- |
| What the user sees on their profile |

It is possible to get a few more properties for each user through the REST API. In the capture below, we can see that the public PGP key is available.

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiQyPCmQPPEOuw0tMQ_CohO1Vd4gdpPbPa1oSCfoWZdYg7R63pceM3m1VCkp1ucOASR6uUlgOrgLDWB9wgViilJ2FvVIwwkOuF_kaACeN3sbXHI64FiR43SHhCDarhJT7xPy1UpPwOcaUvJOSrdVJZ05PziaX_fqGhD329OriBwU2SFnTgLjTqg5BH2FA/w640-h396/6privacy_metadata_keybase2-1024x635-1.png) |
| --- |
| Information available from the REST endpoint user/lookup |

  

The OpenPGP file format includes metadata such as software information (version), key material and a user id. The user id is generally the full name of the user and the email expected to be used for PGP.

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgt2cFJ6ik96AaWhx7hTQWQJLxtg2o51ihJD4CCGFPt9hg5WQQpxd7he3XK4Hw17ia5DM-mG_6wibWeIzPt5qXtaaJj4p1KLdyqVcas1u6wp65fOkniIR0ITIjp4tkktnUBw0VzgcQI2S9LZl62G41kELjuQx0aKIiWmu6m3sEEcmsfm8UQKqrWs-AW7g/w640-h512/6privacy_metadata_keybase3.png) |
| --- |
| User ID present in OpenPGP format |

  

#### Recommendations

To mitigate, you must first identify documents or files that are likely to contain metadata (eg. Docx, pptx, odt, ods, pgp, pdf). Then, a specific procedure should be applied to each document type to remove fields that could contain private information.

In the case of images, you can strip metadata by re-encoding the image. This process should obviously not be applied if your application is a storage hosting provider. In this case, you can potentially warn your users about the presence of specific metadata before they use sharing features.

### #4 – Search Features Matching on Private Fields

Search features can be a source of information leakage. Confidential information might not be shown on your application but could be queried through search features. In the example below, the application is never going to show the address and phone number of the user. The application is, however, allowing those fields to be queried.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj6IztwqjKyvChbve8-1rfrTBrTh7KNIkaTLJ7i-8Av9QZac7IGcRQugt_a9rQuXUJtOtld2eTzaR-mHRGqnlSie6TmWMB6FJ0jgYvq97pJxyhVwcG-HoeOt3_VfNmcwzlfhgUPm5tTrgTJMb0PJkcU6FmKFlXq0si3M-KSGGYg3jE6h_JjkmmJz-DEGg/w640-h280/6privacy_search.png)

Here is a simple example of information disclosure through search. In SonarSource, a popular code analysis software, there is a public API to search users. The API’s documentation describes the email and other fields as private (see image below).

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhL-QE5FC_sBHVa1CV3fgFadhZfrSyZj6POIEtimE_k7qnMhmGYOvYzN06JVVj2sVf7BaC3eOQ870qEFInKdFmDIOlphOaViDPQo_DeOa7TO29fR1B-w-NWUxq050IzORxXoHKfAD2T_0eu3WAwsT94cOIc0-k-OtRlgkg-rt3KMC60jw6TpQTX7dPWqA/w640-h226/6privacy_search2-1024x363-1.png)

However, it is possible to brute force the email address of all users because it matches all fields from the entity, not just those being displayed.

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEirchT8TfqNBaHLsfwv1M1LQJuB8nfRzAsIMSdTvvhOBaTrFfD7c_qbsSnRALHDrUD-NATCaAeBVk5AjMldxVYxcXiupYfcqIuQQrs59vmO54Ow-lfFvlLjwjoHnmF22pituDTgingrUQK0NTRIjK9Pi29YL4_cV3oupzmh8ZTDD3EJ5nJboaNPJy4JfQ/w640-h342/6privacy_search3.png) |
| --- |
| The search with an email returns the associated user. |

#### Doubt the documentation

We have also stumbled upon a few API endpoints where the same principle applies. In one case, the application supports an endpoint with the URL pattern “api/users/**<****USER\_ID****\>**“. While the documentation gave examples with a user ID described as GUID, the field also supported username and email as identifiers. With some APIs, users can even be retrieved from a partial email address search. That said, even if it requires a full email match, an attacker could still feed in a large list of email addresses to reveal many user accounts registered on the system with their associated emails.

### #5 – Verbose REST or GraphQL Endpoint

REST API and GraphQL are widely popular in dynamic applications making use of AJAX. It is important for developers to remember to show **only** what is needed by the front end. The code below demonstrates a simple API that reuses the same class for persistence and its public API. It might have been fine at the API inception, but over time new fields might be added to the _UserEntity_ class. If that happens, fields such as physical address and IP address are now visible on the client-side.

```
@Entity 
public class UserEntity { 
    @Id 
    private Long id; 
    private String username; 
    private String fullName; 
    private String address; 
    private String ip;

[…] 

@Controller 
public class UsersController { 

    @RequestMapping("/users") 
    public UserEntity getU(@RequestParam(“id") String id) { 
        return … 
    } 

[…] 

```

To avoid such problems, it is important to keep a separation between the public API class – often called Data Transfer Object (DTO) – and the persistence model. While it seems like an unnecessary conceptual duplication, this same design choice can also help stabilize the API if the model is having frequent evolution.

### #6 – Side-Channels

A side-channel attack is based on information gained from the implementation. Data is sent to trigger a specific behavior and another channel can provide an extra source of information. Here, we are referring to channels as different systems or protocols.

> _These attacks usually refer to physical attacks on hardware. It might be stretch to call the following scenarios as such._

Here is an example of side-channels observed on most Git hosting providers (eg. GitHub, Gitlab and BitBucket).

#### GitHub: Enriching Git commits

If an attacker wants to deanonymize multiple corporate emails, they will be interested in obtaining the full name of the owners of those emails and their personal email, if possible.

The deanonymization process can be performed in two simple steps:

1. The attacker imports a git repository with commits containing spoofed emails (emails to deanonymize).
2. Once imported, the attacker views those same commits through the web interface to see if the identities were enriched with additional information. (username, full name, organization)

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjaHCXyBYqQrXhBOWMwqYuQ24aVjekM3znEhDvUr-LrRlzNokOLNxg8OxqHllgJjDkZ_VZ488K_zOIowekAGs60Jk4peVvuxSk-nR53caW1ENoX9A8nmLSbA5As-nE3kNwNmb89pQ6ChrQsiA-6Pb3eXvhOZy5A1xj3VHqlm36rISamtPYjzBPabdAC_Q/w640-h364/6privacy_side.png)

Commit imported seen via the Git logs:

```
$ git log 
commit a152c6fc3ac19ebf0afe4b2da3e9414ef2c303fa (HEAD -> master, origin/master) 
Author: unknown <h@cked.in> 
Date:   Wed Mar 2 02:05:57 2022 -0500 

Test commit 

```

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEizbWwhLDZqqbk_TtcqkLmHvx0Zsc67kuZjUC_YLQtehTo47ybZ6lbeEIu8BtfZMN29LGRD_8PdW-OJU2Pj3QkaqX0pmYRS1F_LHT6aZmw0uEK96SPRZIR7L3kKLtllGHOl105z3E7OFS0gJjFTkJkosiRbJ5ZNAvkD3AePk1H3okZ3GJ6pWjb38-FYpA/w640-h346/6privacy_side2.png) |
| --- |
| Account information associated with the email used in the commit |

  

This technique is only useful to discover emails that are not already public in commit logs or to correlate email associated to the same individual.

#### SMTP: Enriching Emails Sender

We have seen multiple webmail services subject to this same design issue. You could import a series of crafted emails with email addresses to deanonymize as sources (ie. _From:_ attribute) and view the author of the mail through the web interface. However, most of those email providers have simpler ways to deanonymize emails.

For Gmail or Google accounts, a tool named GHunt returns additional metadata based on any Google account email found. For Outlook/LinkedIn, rather than using the SMTP channel, we can use LinkedIn contacts import feature and the WebSocket API for outlook card as it provides a direct way to get the information.

#### Threat Modeling

Evaluating the risk of small information leakage can often be subtle. For this reason, a dedicated threat modeling exercise might be needed to properly evaluate the privacy implications of your APIs.

Here are three things you can identify in your next threat modeling exercise:

- APIs that could return **sensitive information**
- APIs that could be **queried** with sensitive information
- Third **party integrations** (ie: Facebook Login, Google Account, Gravatar, etc.) and their privacy implications

### Conclusion

With the information provided, we are hopeful that developers can address some key challenges to privacy through improvements to their API designs. In addition, threat modeling exercises can help with the ongoing evaluation of the risks associated with applications.

This blog can also be beneficial to end-users. ‘_Forewarned is forearmed’ – as the saying goes._ We hope that a warning like this will result in end-users taking more precautions. As a user, you can reconsider how you share information publicly, such as your location, affiliations or even full name knowing that your Facebook account will be linked to multiple websites. Keep in mind that what you publish is likely to be linked with additional information on other websites integrating with third parties such as Facebook Login.

And be sure to check this blog regularly for research, threat intelligence and security updates from the team at GoSecure Titan Labs. You can also follow GoSecure on Twitter and LinkedIn. 

### References

- Gravatar public response to email disclosure: https://en.gravatar.com/support/data-privacy
- Email disclosure on WordPress.com and its impact: https://www.gosecure.net/blog/2021/03/02/emails-disclosure-on-wordpress/
- Graph API documentation: https://developers.facebook.com/docs/graph-api/overview/
- A presentation at Confoo about Privacy Pitfalls for Developers: https://gosecure.github.io/presentations/2022-02-25-confoo-privacy/Privacy\_pitfalls\_for\_your\_web\_application.pdf

  

### More references on related topics

- Privacy Patterns
- NIST Privacy Framework
- W3C: Privacy Anti-Patterns in Standards

  

_This blog was originally posted on GoSecure blog._

Go to Source
