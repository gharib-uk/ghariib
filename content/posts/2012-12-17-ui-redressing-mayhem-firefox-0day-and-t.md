---
title: "UI Redressing Mayhem Firefox 0day and the LeakedIn affair"
date: 2012-12-17T03:49:00.001-08:00
draft: false
type: posts
categories: 
- 0day,daath,firefox,linkedin,ui redressing
---
# UI Redressing Mayhem Firefox 0day and the LeakedIn affair

<br/>

<br/>
In the past weeks I worked on [UI Redressing](http://en.wikipedia.org/wiki/Clickjacking) exploitation methods. The **UI Redressing Mayhem** series is going to illustrate the results of my research, presenting 0day exploiting techniques and several vulnerabilities that involve high-profile web applications. Each post of the series will also provide detailed information about the vulnerabilities and techniques, together with working Proof-of-Concept exploits.  
  
The following article will detail a previously unknown Mozilla Firefox  [vulnerability](https://bugzilla.mozilla.org/show_bug.cgi?id=822215) that affects the latest version (v.17.0.1) of the Mozilla web browser and allows malicious users to perform cross-domain extraction of sensitive data via UI Redressing vectors.  
  

### It was a dark and stormy night...

  
My security research on UI Redressing exploitation techniques grounds its roots in a web application penetration test where I was asked to exploit a UI Redressing bug with the explicit constraints to target Mozilla Firefox users. My objective was to achieve the cross-domain [content extraction](http://html5sec.org/#119) of an anti-CSRF token, in order to trigger the update of the victim's profile e-mail address: the powerful double drag&drop method was found to be appropriate in that context. To the best of my knowledge, the method was first introduced by [Ahamed Nafeez](http://blog.skepticfx.com/2011/09/facebook-graph-api-access-token.html) and is based on the possibility to perform a drag&drop action between a _framed_ web page, which displays the "sensitive" contents and is not protected by the [X-Frame-Options](https://developer.mozilla.org/en-US/docs/The_X-FRAME-OPTIONS_response_header) header, and the _framing_ page (the "dropper" page), which receives and stores the extracted content. The [view-source](http://en.wikipedia.org/wiki/View-source_URI_scheme) handler is used here to bypass any [framebusting](http://en.wikipedia.org/wiki/Framekiller) code.  
  
The main problem with my exploit development, during the penetration test, was that the drag&drop method was recently [killed](https://bugzilla.mozilla.org/show_bug.cgi?id=605991) by Mozilla. An interesting solution to the Mozilla fix is the [fake CAPTCHA](http://blog.kotowicz.net/2011/07/cross-domain-content-extraction-with.html) method that was introduced by [Krzysztof Kotowicz](http://blog.kotowicz.net/) — and demonstrated to be effective against [Facebook](http://blog.kotowicz.net/2012/08/how-facebook-lacked-x-frame-options-and.html) and [Google eBookstore](http://blog.kotowicz.net/2011/11/google-ebookstore-content-extraction.html) — but I chose the hard way and tried to bring the drag&drop method back to the masses: so please welcome the _iframe-to-iframe cross-domain extraction method_.  
  

### The iframe-to-iframe extraction method

  
The extraction method is extremely simple: instead of performing a drag&drop action of sensitive data, from a framed vulnerable web page to the framing one (attacker-controlled), the victim is tricked to visit a malicious html page that includes _two_ iframes: the vulnerable page - where the sensitive content resides - and _another_ attacker's page that is used to drop the extracted content (Figure 1). Firefox is not able to block this kind of attack because no check on cross-domain drag&drop between iframes is performed. As mentioned before, the method was tested against Mozilla Firefox version 17.0.1 - the latest stable release at the time of writing. The iframe-to-iframe technique was also tested against Google Chrome but the browser has been proved robust to the proposed attack.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgi5k2d4Hm4N_x1gRtvIYsu3DZMea4RMyQugdH9lJO-trBk52bzcydJ3Sh9qmfkgUTyHjtdOfnGyVUQcRzhSZtsb3w42aOygrkxXvZgc3q_AYIQxa971VXPmesbgcDmBeEZk7xK55u2IkoM/s640/dnd.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgi5k2d4Hm4N_x1gRtvIYsu3DZMea4RMyQugdH9lJO-trBk52bzcydJ3Sh9qmfkgUTyHjtdOfnGyVUQcRzhSZtsb3w42aOygrkxXvZgc3q_AYIQxa971VXPmesbgcDmBeEZk7xK55u2IkoM/s1600/dnd.png)

Figure 1 - iframe-to-iframe d&d extraction method.

The iframe-to-iframe method re-introduces the possibility to abuse the Firefox drag&drop mechanism to perform a cross-domain data extraction. Let me now introduce an high-profile vulnerability and attack that targets the **LinkedIn** application implementing the proposed method.  
  

### All your LinkedIn accounts are belong to us

  
LinkedIn implements a _stateless_ anti-[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_%28CSRF%29) mechanism that associates tokens to the HTTP requests that result in a change of the remote application state, such as the update of a user's profile information (e.g. job title or the login e-mail address). A stateless anti-CSRF method is generally based on a secret token, delivered as a cookie parameter, and a token which is included in every state-changing HTTP request: the remote web application considers as _genuine_ exclusively the HTTP requests that have the same token value for both the cookie and HTTP parameter. Otherwise, a request is considered untrusted and it is not computed. The LinkedIn's anti-CSRF mechanism involves a cookie parameter called **JSESSIONID** and an HTTP parameter named **csrfToken** in order to store the secret tokens (Figure 2). A stateless mechanism can be easily bypassed with well known web hacking techniques.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhRI4AnUdsNBDBGIkn1pWKyaymCRiTcVwJknnMACW-7dGIBlqSFKaXTTycTqD0AJLVuS4CQn4D5ArVSnPqDUEFoycVfwWW-5M2irJKwx-zQHsLTutqDbCPjTSkWzEVaZ3EYA0Uc-U6sgimx/s320/AJAX0.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhRI4AnUdsNBDBGIkn1pWKyaymCRiTcVwJknnMACW-7dGIBlqSFKaXTTycTqD0AJLVuS4CQn4D5ArVSnPqDUEFoycVfwWW-5M2irJKwx-zQHsLTutqDbCPjTSkWzEVaZ3EYA0Uc-U6sgimx/s1600/AJAX0.png)

Figure 2 - anti-CSRF tokens.

For example, the attacker could abuse a [Cross-Site Scripting](https://www.owasp.org/index.php/Cross-site_Scripting_%28XSS%29) issue on both www.linkedin.com or any LinkedIn's subdomains to _poison_ the cookie parameter JSESSIONID and bypass the mechanism — this attack is also known as [Cookie Tossing](http://media.blackhat.com/bh-ad-11/Lundeen/bh-ad-11-Lundeen-New_Ways_Hack_WebApp-WP.pdf). During my security research I found a vulnerable LinkedIn's page that includes the anti-CSRF token within the HTML code, despite not being protected by the X-Frame-Options header. Under these circumstances, the iframe-to-iframe method can be used to attack authenticated LinkedIn users and steal their secret token in order to perform different kind of malicious actions on the victim's profile. The following URL refers to the LinkedIn vulnerable web resource as detailed in Figure 3:  

-   **http://www.linkedin.com/companies?trk=hb\_tab\_compy** 

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi_5due4GG0wBspdfos5ZXQtCrMUcUF3fp36uKCdsQp7OeD6Vlm3bppKy4dYImTBjeXOHqg3tFgbe2quNlaW2rAVHR6kWIqvro0xHpATlKzA7ipqO96kQxqO3nHpR80jba2HEWNu0llpPkH/s400/ajax.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi_5due4GG0wBspdfos5ZXQtCrMUcUF3fp36uKCdsQp7OeD6Vlm3bppKy4dYImTBjeXOHqg3tFgbe2quNlaW2rAVHR6kWIqvro0xHpATlKzA7ipqO96kQxqO3nHpR80jba2HEWNu0llpPkH/s1600/ajax.png)

Figure 3 - Vulnerable LinkedIn web resource.

  
The vulnerability can be easily abused to craft a UI Redressing exploit that triggers the victim to drag&drop the anti-CSRF token. The token can then be abused to edit any information on the victim's profile and even to _reset the account password_. In order to demonstrate the effectiveness of the attack I developed a fully working Proof of Concept exploit that adds the attacker's e-mail as a trusted address to the victim's profile and verifies the e-mail itself. At that point, the attacker can easily reset the victim's password using LinkedIn password reset mechanism.  
  
The following are the logical steps implemented by the Proof of Concept exploit:

1.  The malicious page frames both the LinkedIn vulnerable page and the attacker-controlled "dropper" page;
3.  The malicious page allows the victim to play the d&d game, which extracts the anti-CSRF token;
4.  The malicious page can now bypass the anti-CSRF protection and adds a new e-mail address to the victim's profile. The action involves the forwarding of a confirmation e-mail from LinkedIn system to the attacker box: an activation URL is included;
5.  The exploit interacts with an attacker's script — **/linkedin/linkedin.php** — which accesses the attacker's mail box via IMAP and waits for the Linkedin activation e-mail. Once obtained the e-mail, the URL is returned back to the malicious page, which is still loaded by victim's web browser;
6.  The script can now simulate the navigation of the fetched URL in order to _confirm_ the new address.

The attacker can now reset the victim's account password abusing the password reset functionality, where he will type the e-mail address previously added to the targeted profile. Figure 4 highlights the different HTTP requests exchanged between the attacked web browser, the attacker's servers and the LinkedIn web application, in order to achieve the password resetting.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh7naknCEmJGbuka804vZA6NGltrXRjSnMm8VGedoh5y9UhQj3IbhS30YoO1pgYBFaxvvI16NqjNIMB7M_fOBrlRRn-l4iBRF5Pa8K3r0gxBgGhBdu9LSTb7pl4vWMlo9UPT4jhMITLZ8kU/s640/1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh7naknCEmJGbuka804vZA6NGltrXRjSnMm8VGedoh5y9UhQj3IbhS30YoO1pgYBFaxvvI16NqjNIMB7M_fOBrlRRn-l4iBRF5Pa8K3r0gxBgGhBdu9LSTb7pl4vWMlo9UPT4jhMITLZ8kU/s1600/1.png)

Figure 4 - Sequence diagram detailing the attack.

A working PoC has been developed and can be downloaded [here](https://github.com/daath1/nibblesec/tree/master/ui_redressing_mayhem/linkedin). The following is a video of the attack:  
  
  

### Beyond the Mayhem

  
LinkedIn Team was informed about this attack scenario. The following are a series of suggestions that should prevent this kind of attacks:  

-   Protect every web resource that includes anti-CSRF tokens with the _X-Frame-Options_ header. Nowadays, this mechanism is available in all major browsers;
-   Consider to adopt a stateful anti-CSRF mechanism that should not perform the validation on the basis of potentially attacker-controlled inputs.

#### [Source](http://blog.nibblesec.org/feeds/4661828001464122932/comments/default)

<br/>
---
