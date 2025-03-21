---
title: "BitM Up Session Stealing in Seconds Using the Browser-in-the-Middle Technique"
date: Mon, 17 Mar 2025 05:00:00 +0000
draft: false
type: posts
---
# BitM Up Session Stealing in Seconds Using the Browser-in-the-Middle Technique





Written by: Truman Brown, Emily Astranova, Steven Karschnia, Jacob Paullus, Nick McClendon, Chris Higgins Executive Summary The Rise of Browser in the Middle (BitM): BitM attacks offer a streamlined approach, allowing attackers to quickly compromise sessions across various web applications. MFA Remains Crucial, But Not Invulnerable: Multi-factor authentication (MFA) is a vital security

Written by: Truman Brown, Emily Astranova, Steven Karschnia, Jacob Paullus, Nick McClendon, Chris Higgins

* * *

Executive Summary
-----------------

-   **The Rise of Browser in the Middle (BitM):** BitM attacks offer a streamlined approach, allowing attackers to quickly compromise sessions across various web applications.
    
-   **MFA Remains Crucial, But Not Invulnerable:** Multi-factor authentication (MFA) is a vital security measure, yet sophisticated social engineering tactics now effectively bypass it by targeting session tokens.
    
-   **Strong Defenses Are Imperative:** To counter these threats, organizations must implement robust defenses, including hardware-based MFA, client certificates, and FIDO2.
    

Social Engineering and Multi-Factor Authentication
--------------------------------------------------

Social engineering campaigns pose a significant threat to organizations and businesses as they capitalize on human vulnerabilities by exploiting cognitive biases and weaknesses in security awareness. During a social engineering campaign, a red team operator typically targets a victim's username and password. A common mitigation used to address these threats are security measures like multi-factor authentication (MFA). 

MFA is a security measure that requires users to provide two or more methods of authentication when logging in to an account or accessing a protected resource. This makes it more difficult for unauthorized users to gain access to sensitive information even if they have obtained one of the factors, such as a password.

Red team operators have long targeted various methods of obtaining user session tokens with a high degree of success. Once a user has completed MFA and is successfully authenticated, the application typically stores a session token in the user's browser to maintain their authenticated state. Stealing this session token is the equivalent of stealing the authenticated session, meaning an adversary would no longer need to perform the MFA challenge. This makes session tokens a valuable target for adversaries and red team operators alike.

Techniques for Targeting Tokens
-------------------------------

Red team operators can target these session tokens using a variety of tools and techniques. The most common tool is [Evilginx2](https://github.com/kgretzky/evilginx2), a transparent proxy where a red team operator's server acts as an intermediary between the victim and the targeted service. Any HTTP requests made by the victim are captured by the phishing server and then forwarded directly to the intended website. However, before returning the responses to the victim, the server subtly modifies them by replacing any references to the legitimate domain with the phishing domain. This manipulation allows operators to not only capture the victim's login credentials from POST requests but also to extract session cookies (tokens) from the server's response headers after the victim has completed authentication and MFA prompts.

During a red team engagement, a consultant working within a constrained time frame is tasked with achieving a series of objectives that cover a broad spectrum, such as retrieving sensitive employee data (e.g., personally identifiable information \[PII\]) or even a complete takeover of the target's Active Directory infrastructure. The red team's mission is to simulate a real-world attack and evaluate the effectiveness of the client's security measures by exploiting vulnerabilities and employing various techniques to gain unauthorized access. It is rare for a consultant to deploy a transparent proxy targeting a custom application unique to a client due to the high degree of customization that can be involved during setup.

Transparent proxies require significant customization and configuration to work against a targeted application. This process can be time-consuming, complex, and error-prone, especially for a red team operator targeting multiple applications. Often the operator will have to fully understand the way the application handles sessions and authentication before being able to successfully target the application. Once an application has been fully reduced to a template that is usable with the chosen transparent proxy, red team operators will still need to keep the templates up to date introducing a large amount of overhead.

Dynamic Targeting with Browser in the Middle 
---------------------------------------------

According to MITRE, [Browser in the Middle (BitM)](https://capec.mitre.org/data/definitions/701.html) "uses the inherent functionalities of a web browser to convince the victim they are browsing normally under the assumption that the connection is secure. All the actions performed by the victim in the open window are actually performed on the machine of the adversary." In short, this attack is similar to a victim sitting in front of an attacker's computer and signing in for them; all of the data required to authenticate to the application is now under the attacker's control. 

BitM offers a number of advantages for red team operators when compared to traditional methods of stealing authenticated session tokens. A pivotal benefit of employing a BitM framework lies in its rapid targeting capability, allowing it to reach any website on the web in a matter of seconds and with minimal configuration. Once an application is targeted through a BitM tool or framework, the legitimate site is served through an attacker-controlled browser. This makes the distinction between a legitimate and a fake site exceptionally challenging for a victim. From the perspective of an adversary, BitM allows for a simple yet effective means of stealing sessions protected by MFA.

BitM Overview
-------------

Mandiant has developed an internal tool (Delusion) for performing BitM attacks, enabling an operator to target a specific application without possessing prior knowledge about the authentication protocols employed by the application.

![delusion traffic flow](https://storage.googleapis.com/gweb-cloudblog-publish/images/bitm-traffic-flow.max-1000x1000.png)

Delusion includes a number of unique features that enable session-stealing attacks at scale:

-   Support for storing and downloading Firefox browser profiles, making session stealing trivial, no cookie import required
    
-   A monitor page where an operator can interact with a victim's session in real time
    
-   The ability to scale containers and automatically add them to a load balancer for large-scale phishing campaigns
    
-   Two modes of operation designed for either vishing or phishing (Manual and Automatic)
    
-   Bookmarks to simplifying deploying against multiple websites
    
-   Tagging for campaign management
    
-   Session recording for reporting purposes
    

Delusion was inspired by the following blog posts and research papers. Development would not have been possible without their commitment to publishing and releasing research.

-   [https://mrd0x.com/bypass-2fa-using-novnc/](https://mrd0x.com/bypass-2fa-using-novnc/)
    
-   [https://link.springer.com/article/10.1007/s10207-021-00548-5](https://link.springer.com/article/10.1007/s10207-021-00548-5)
    
-   [https://fhlipzero.io/blogs/6\_noVNC/noVNC.html](https://fhlipzero.io/blogs/6_noVNC/noVNC.html) 
    

Mandiant has chosen not to publish Delusion due to weaponization concerns. If you are interested in how your application or portal performs against BitM and other session-stealing threats, check out these open source projects:

-   [https://github.com/JoelGMSec/EvilnoVNC](https://github.com/JoelGMSec/EvilnoVNC)
    
-   [https://github.com/fkasler/cuddlephish](https://github.com/fkasler/cuddlephish)
    
-   [https://github.com/kgretzky/evilginx2](https://github.com/kgretzky/evilginx2) 
    

BitM Session Stealing in Action
-------------------------------

BitM is well suited for targeting applications that allow for initial access to privileged networks or environments through Virtual Desktop Infrastructure (VDI). BitM makes deploying session-stealing infrastructure against any publicly exposed infrastructure very easy. Targeting a login portal is as simple as specifying the portal information and clicking "Deploy", as shown in Figure 1. Figure 2 shows the view of an operator during an engagement. An operator can view any actions taken on the phishing site in real time. Figure 3 shows the view of a victim authenticating through the phishing site. Figure 4 shows an example of a captured session; despite there being no cookies, the browser profile will still contain everything used by the application to maintain the authenticated state. Figure 5 shows the downloaded browser profile being opened and the session being resumed.

![Deploying the victim container](https://storage.googleapis.com/gweb-cloudblog-publish/images/bitm-fig1.max-1000x1000.png)

Figure 1: Deploying the victim container

![Monitoring the victim container](https://storage.googleapis.com/gweb-cloudblog-publish/images/bitm-fig2.max-1000x1000.png)

Figure 2: Monitoring the victim container

![Victim authenticating to app](https://storage.googleapis.com/gweb-cloudblog-publish/original_images/bitm-fig3.gif)

Figure 3: Victim authenticating to app

![Captured session and keylogger output](https://storage.googleapis.com/gweb-cloudblog-publish/images/bitm-fig4.max-1000x1000.png)

Figure 4: Captured session and keylogger output

![Using the captured Firefox session](https://storage.googleapis.com/gweb-cloudblog-publish/original_images/bitm-fig5.gif)

Figure 5: Using the captured Firefox session

![Browser-in-the-Middle attack flow](https://storage.googleapis.com/gweb-cloudblog-publish/images/bitm-fig6.max-1000x1000.png)

Figure 6: Browser-in-the-Middle attack flow

Figure 6 shows a typical attack scenario where a victim is lured to a malicious website through a phone call, text message or email **(1)**. Upon visiting the site given by the operator, the victim's connection is routed through a load balancer to an available proxied browser **(2)**. The victim unknowingly interacts with the proxied browser, entering their credentials, including any MFA tokens **(3)**. Once the attacker observes a successful login, the victim is disconnected from the proxied browser, and their session is compromised **(4)**.

Defense Considerations
----------------------

To defend against such attacks, organizations can adopt the following strategies. Requiring client certificates for authentication can deter BitM attacks, as these certificates are typically bound to specific devices and cannot be easily manipulated by attackers. Similarly, hardware-based MFA solutions like FIDO2 compatible security keys offer strong protection against BitM. Figure 7 depicts a typical FIDO2 authentication flow.

![FIDO2 authentication flow](https://storage.googleapis.com/gweb-cloudblog-publish/images/bitm-fig7.max-1000x1000.png)

Figure 7: FIDO2 authentication flow

FIDO2 and/or certificate-based authentication halts an attack scenario, as shown in Figure 8. The attacker's browser is attempting to steal a session from the legitimate site. The attacker's site requests a page from the website it is trying to steal a session from. The website requires a FIDO2 key or certificate to authenticate, and the attacker's site does not have one. Although the attacker's site mirrors the legitimate site's screen so it can trick a possible victim into authenticating for them, the attacker does not have a key or certificate, thus they cannot proceed and the authentication fails. Furthermore, the FIDO2 protocol ensures the BitM cannot successfully replay the FIDO2 response from the real user, as the browser on the user's machine would ensure the responses are immutably tied to the request's origin (i.e., the attacker site cannot request a FIDO2 response to a different target website).

![FIDO2 and certificate-based authentication with BitM](https://storage.googleapis.com/gweb-cloudblog-publish/images/bitm-fig8.max-1000x1000.png)

Figure 8: FIDO2 and certificate-based authentication with BitM

There are some caveats with the aforementioned scenario that are important to point out. Certificate-based authentication and FIDO2 security keys only protect sessions when the device they are hosted on is not compromised. It is possible to compromise sessions, and even phish sessions, that are protected with FIDO2 security keys and certificates if you are able to compromise the device they are connected to. This should underscore the importance of a layered security approach with all applications that host sensitive data or provide access to restricted networks.

Conclusion
----------

The threat of BitM attacks emphasizes the importance of robust authentication and access-control mechanisms. By adopting a multi-layered defense strategy incorporating client certificates, hardware-based MFA solutions such as FIDO2-compatible security keys, and compensating controls, organizations can significantly enhance their resilience against these sophisticated threats. The integration of security keys into this defense strategy provides a particularly effective safeguard against session stealing, offering users a tangible and reliable way to protect their online identities and sensitive data.

Acknowledgements
----------------

A very special thanks to everyone who contributed to this project's early and continued development. This blog post was made possible by Chris King, Evan Peña, Jerry McClurg, and Jeff Hoffmann.

#### [Source](https://cloud.google.com/blog/topics/threat-intelligence/session-stealing-browser-in-the-middle/)

<br/>
---
