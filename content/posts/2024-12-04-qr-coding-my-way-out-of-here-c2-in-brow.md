---
title: "QR Coding My Way Out of Here C2 in Browser Isolation Environments"
date: Wed, 04 Dec 2024 14:00:00 +0000
draft: false
type: posts
---
# QR Coding My Way Out of Here C2 in Browser Isolation Environments





Written by: Thibault Van Geluwe de Berlaere Executive Summary Browser isolation is a security technology where web browsing activity is separated from the user's local device by running the browser in a secure environment, such as a cloud server or a virtual machine, and then streaming the visual content to the user's

Written by: Thibault Van Geluwe de Berlaere

* * *

Executive Summary
-----------------

-   Browser isolation is a security technology where web browsing activity is separated from the user's local device by running the browser in a secure environment, such as a cloud server or a virtual machine, and then streaming the visual content to the user's device.
    
-   Browser isolation is often used by organizations to combat phishing threats, protect the device from browser-delivered attacks, and deter typical command-and-control (C2 or C&C) tactics used by attackers.
    
-   In this blog post, Mandiant demonstrates a novel technique that can be used to circumvent all three current types of browser isolation (remote, on-premises, and local) for the purpose of controlling a malicious implant via C2. Mandiant shows how attackers can use machine-readable QR codes to send commands from an attacker-controlled server to a victim device.
    

Background on Browser Isolation
-------------------------------

The great folks at SpecterOps released [a blog post](https://posts.specterops.io/calling-home-get-your-callbacks-through-rbi-50633a233999) earlier this year on browser isolation and how penetration testers and red team operators may work around browser isolation scenarios for ingress tool transfer, egress data transfer, and general bypass techniques. In summary, browser isolation protects users from web-based attacks by sandboxing the web browser in a secure environment (either local or remote) and streaming the visual content back to the user’s local browser. The experience is (ideally) fully transparent to the end user. According to most [documentation](https://www.proofpoint.com/au/threat-reference/browser-isolation), three types of browser isolation exist:

1.  Remote browser isolation (RBI), the most secure and the most common variant, sandboxes the browser in a cloud-based environment.
    
2.  On-premises browser isolation is similar to RBI but runs the sandboxed browser on-premises. The advantage of this approach is that on-premises web-based applications can be accessed without requiring complex cloud-to-on-premises connectivity.
    
3.  Local browser isolation, or client-side browser isolation, runs the sandboxed browser in a local containerized or virtual machine environment ( e.g., Docker or [Windows Sandbox](https://learn.microsoft.com/en-us/windows/security/application-security/application-isolation/windows-sandbox/windows-sandbox-overview)).
    

The remote browser handles everything from page rendering to executing JavaScript. Only the visual appearance of the web page is sent back to the user’s local browser (a stream of pixels). Keypresses and clicks in the local browser are forwarded to the remote browser, allowing the user to interact with the web application. Organizations often use proxies to ensure all web traffic is served through the browser isolation technology, thereby limiting egress network traffic and restricting an attacker’s ability to bypass the browser isolation.

SpecterOps detailed some of the challenges that offensive security professionals face when operating in browser isolation environments. They document possible approaches on how to circumvent browser isolation by abusing misconfigurations, such as using HTTP headers, cookies, or authentication parameters to bypass the isolation features.

Browser Isolation Prevents Typical Command-and-Control
------------------------------------------------------

Command and control (C2 or C&C) refers to an attacker’s ability to remotely control compromised systems via malicious implants. The most common channel to send commands to and from a victim device is through HTTP requests: 

1.  The implant requests a command from the attacker-controlled C2 server through an HTTP request (e.g., in the HTTP parameters, headers, or request body).
    
2.  The C2 server returns the command to execute in the HTTP response (e.g., in headers or response body).
    
3.  The implant decodes the HTTP response and executes the command.
    
4.  The implant submits the command output back to the C2 server with another HTTP request.
    
5.  The implant “sleeps” for a while, then repeats the cycle.
    

However, this approach presents challenges when browser isolation is in use—when making HTTP requests through a browser isolation system, the HTTP response returned to the local browser only contains the streaming engine to render the remote browser’s visual page contents. The original HTTP response (from the web server) is only available in the remote browser. The HTTP response is rendered in the remote browser, and only a stream of pixels is sent to the local browser to visually render the web page. This prevents typical HTTP-based C2 because the local device cannot decode the HTTP response (step 3).

![Sequence diagram of browser isolation HTTP request lifecycle](https://storage.googleapis.com/gweb-cloudblog-publish/original_images/qr-browser-isolation-fig1_OXJgN0w.png)

Figure 1: Sequence diagram of browser isolation HTTP request lifecycle

In this blog post, we will explore a different approach to achieving C2 with compromised systems in browser isolation environments, working entirely within the browser isolation context. 

Sending C2 Data Through Pixels
------------------------------

Mandiant’s Red Team developed a novel solution to this problem. Instead of returning the C2 data in the HTTP request headers or body, the C2 server returns a valid web page that visually shows a QR code. The implant then uses a local headless browser (e.g., using [Selenium](https://www.selenium.dev/)) to render the page, grabs a screenshot, and reads the QR code to retrieve the embedded data. By taking advantage of machine-readable QR codes, an attacker can send data from the attacker-controlled server to a malicious implant even when the web page is rendered in a remote browser.

![Sequence diagram of C2 via QR codes](https://storage.googleapis.com/gweb-cloudblog-publish/images/qr-browser-isolation-fig2.max-1000x1000.png)

Figure 2: Sequence diagram of C2 via QR codes

Instead of decoding the HTTP response for the command to execute; the implant visually renders the web page (from the browser isolation’s pixel streaming engine) and decodes the command from the QR code displayed on the page. The new C2 loop is as follows:

1.  The implant controls a local headless browser via the [DevTools protocol](https://chromedevtools.github.io/devtools-protocol/).
    
2.  The implant retrieves the web page from the C2 server via the headless browser. This request is forwarded to the remote (isolated) browser and ultimately lands on the C2 server.
    
3.  The C2 server returns a valid HTML web page with the command data encoded in a QR code (visually shown on the page).
    
4.  The remote browser returns the pixel streaming engine back to the local browser, starting a visual stream showing the rendered web page obtained from the C2 server.
    
5.  The implant waits for the page to fully render, then grabs a screenshot of the local browser. This screenshot contains the QR code.
    
6.  The implant uses an embedded QR scanning library to read the QR code data from the screenshot, thereby obtaining the embedded data.
    
7.  The implant executes the command on the compromised device.
    
8.  The implant (again through the local browser) navigates to a new URL that includes the command output encoded in a URL parameter. This parameter is passed through to the remote browser and ultimately to the C2 server (after all, in legitimate cases, the URL parameters may be required to return the correct web page).The C2 server can decode the command output as in traditional HTTP-based C2.
    
9.  The implant “sleeps” for a while, then repeats the cycle.
    

Mandiant developed a proof-of-concept (PoC) implant using [Puppeteer](https://pptr.dev/) and the Google Chrome browser in headless mode (though any modern browser could be used). We even went a step further and integrated the implant with [Cobalt Strike’s External C2 feature](https://hstechdocs.helpsystems.com/manuals/cobaltstrike/current/userguide/content/topics/listener-infrastructue_external-c2.htm), allowing the use of Cobalt Strike’s BEACON implant while communicating over HTTP requests and QR code responses.

![Demo of C2 through QR codes in browser isolation scenarios](https://storage.googleapis.com/gweb-cloudblog-publish/original_images/qr-browser-isolation-fig3.gif)

Figure 3: Demo of C2 through QR codes in browser isolation scenarios (Chrome browser window would be hidden in real-world applications)

Because this technique relies on the visual content of the web page, it works in all three browser isolation types (remote, on-premises, and local). 

While the PoC demonstrated the feasibility of this technique, there are some considerations and drawbacks:

-   During Mandiant’s testing, using QR codes with the maximum data size (2,953 bytes, 177x177 grid, Error Correction Level "L") was infeasible as the visual stream of the web page rendered in the local browser was of insufficient quality to reliably read the QR code contents. Mandiant was forced to fall back to QR codes containing a maximum of 2,189 bytes of content. Note: QR codes can store [up to 2953 bytes](https://stackoverflow.com/a/11065499) per instance, depending on the Error Correction Level (ECL). Higher ECL settings make the QR code more easily readable, but reduce the maximum data size.
    
-   Due to the overhead of using Chrome in headless mode, the remote browser startup time, the page rendering requirements, and the stream of visual content from the remote browser back to the local browser, each request takes ~5s to reliably show and scan the QR code. This introduces significant latency in the C2 channel. For example, at the time of writing, a BEACON payload is ~323 KiB. At 2,189 bytes per QR code and 5s per request, a full BEACON payload is transferred in approximately 12m20s (~438 bytes/s, assuming every QR code can be successfully scanned and every network request goes through seamlessly).While this throughput is certainly sufficient for typical C2 operations, some techniques (e.g., SOCKS proxying) become infeasible.
    
-   Other security features of browser isolation, such as domain reputation, URL scanning, data loss prevention, and request heuristics, are not considered in this blog post. Offensive security professionals will have to overcome these protection measures as well when operating in browser isolation environments.
    

Conclusion and Recommendations
------------------------------

In this blog post, Mandiant demonstrated a novel technique to establish C2 when faced with browser isolation. While this technique proves that browser isolation technologies have weaknesses, Mandiant still recommends browser isolation as a strong protection measure against other types of attacks (e.g., client-side browser exploitation, phishing, etc). Organizations should not solely rely on browser isolation to protect themselves from web-based threats, but rather embrace the "defense in depth" strategy and establish a well-rounded cyber defense posture. Mandiant recommends the following controls:

1.  **Monitor for anomalous network traffic:** Even when using browser isolation, organizations should inspect network traffic and monitor for anomalous usage. The C2 method described in this post is low-bandwidth, hence transferring even small datasets will require many HTTP requests.
2.  **Monitor for browsers in automation mode:** Organizations can monitor when browsers are used in automation mode (as shown in the video above) by inspecting the process command line. Chromium-based browsers use flags such as `--enable-automation` and `--remote-debugging-port` to enable other processes to control the browser through the DevTools protocol. Organizations can monitor for these flags during process creation.
    

Through numerous adversarial emulation engagements and [Red Team](https://cloud.google.com/security/consulting/mandiant-red-team) and [Purple Team](https://cloud.google.com/security/resources/datasheets/consulting-services-purple-team-assessment) assessments, Mandiant has gained an in-depth understanding of the unique paths attackers may take in compromising their targets. Review our [Technical Assurance](https://cloud.google.com/security/consulting/mandiant-technical-assurance) services and [contact us](https://www.mandiant.com/contact-us) for more information.

#### [Source](https://cloud.google.com/blog/topics/threat-intelligence/c2-browser-isolation-environments/)

