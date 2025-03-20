---
title: "Demystifying Web Cache Threats"
date: 2025-01-22
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "security"
  - "security-awareness"
tags: 
  - "application-security"
  - "web-architecture-security"
  - "web-attacks"
  - "web-cache"
---

### 

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjiWL3jEeTLPmGyJjKXnLpPwr-d-BbCGNMHKmFBzDeq50KbLehNFFzo-VkEAfWlBu2NDbkTP-PSM4fT-KX5atRzl8fDjhWDGQaaNFwDIzwVO4UtjyTMhymBMAIhD-5NeWPFRJ-ySJvJBzUR/)

  
  

  
Authors

- Alessandro Brucato
- Giorgio Rando

  

### Introduction

Did you know the word “_Cache_” comes from French and means “Hidden”?  
If we transpose it to IT we can see why it has been named as such: It is because of its nature.  
A concealed and faster memory part of a CPU used to overcome the bottleneck of a slower RAM externally connected via BUS.  
  
In general, a caching algorithm has a predefined _set of primary keys_ to be matched so that it will be able to retrieve or store data according to them.  
The use of cache techniques has been spreading over time along with technologies, resulting in features such as:

- \- Database Caching
- \- DNS Caching
- \- Distributed Cache

  

Last but not least, when it comes to _Web Applications_ and _Web Servers_, caching has become a very important feature capable of managing thousands of different users and requests per second.  
In this case it is known as **Web Cache**.  
_Web Cache_ principles are:  

- \- Temporary storage of Web Application generated content (usually customized according to the different user’s data);
- \- Avoid unnecessary **server overload**, which would be otherwise needed to generate again the customized content;
- \- Define unique tuples (n-dimensional keys) per entry.

  
In case of web cache usage, contents generated from a web server are temporarily stored within the cache memory (_Request #1_ in the schema below). Then, the subsequent request (_Request #2_) will not be processed by the web server since it will be returned by the web cache.

### 

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhQ6wD_-vVk3OPMQq52DKd1FvA_NGLS7IUHXTpb13i4h6pTrpWXhcIrKXSPU3-Rf7nG9mdBn4TRTCJb_DKiXBEdDQpPcLj2z88tZy_w0P3FY75FVbhBQHsvSChjSDQUCIZ2oHGHNLFq0LV2/w640-h258/image.png)

  
  

### The Cache Flow

When client and server interact, there might be one or more middleware _caching_ components elaborating data in a more or less transparent way, during the actual client-server communication. The following components might involve data caching such as:

- _\- Internet Browsers_: static copies of the required web pages are stored within the device local disk, in order to provide a quicker response to a user if the requested web page has not be changed after the last request;
- _\- Internet Service Providers (ISPs_): due to the huge amount of requests that providers have to handle, often they rely on their own cache copies, in order to limit the bandwidth use;
- _\- Internet Gateways_: cached pages are stored within the web server reverse proxy in order to optimize the server memory use, avoiding useless overloads on the main one;
- _\- Web Server Proxies_: unlike other components which are detached from the internal network of the web server, often the “_Web Server Proxies_” are deployed on organisation network boundaries. The behaviour of this component is the same described above for _ISPs_ and _Internet Gateways_ components;
- _\- Content Delivery Networks (CDNs)_: users are redirected (through a _DNS_ service) to _CDNs_ when a website which relies on _CDNs_ servers is required. One of the _CDN_ objectives is to cache in an orderly fashion the different requested web pages from different regions;
- _\- Internal Servers:_ the _internal_ _servers_ cache has the same aim of _Internet Gateways_ one, reducing requests processing overload by temporarily saving required web pages.

  

The following diagram shows how the aforementioned components are usually distributed within a client-server communication.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj822WugVjSAOzZFcGMuj-SRonyFQvvz2UF0_UOCTOGYo5nkqjQtmR9GtCjyIpe9kCJWXzmZFRnO2KPM_GTe-NpwVcrc7zfklnkzy0V5WWGoIX6gHQSQDrIdtWf9svusecZbOcNPxjzZKE/w640-h323/components_diagram.png)

  
Each one of these components might handle its own cached copy, which is useful in a number of particular contexts. However, not every caching component can be controlled by the end user and that might pose a threat from a privacy point of view. 

  

For instance, **_internet browser web cache_** is highly manageable by the end-user itself, being able to delete at his sole discretion the web pages cached within. 

On the other hand, **_ISPs_** and **_CDNs_** (just to mention a few) work in a different manner and interact with the cached web pages without user interaction, according to particular HTTP directives.  
  

Usually, these directives are defined through _HTTP Headers_ which are parsed by the caching servers to understand if a web page has changed after its last request.  
  

The full set of standard headers used for caching is defined in the RFC7234, some of them follows:

|   Header   |   Definition   |
| --- | --- |
|   Age   |   delta-seconds   |
|   Cache-Control (request)   |   max-age  max-stale  min-fresh  only-if-cached  no-cache  no-store  no-transform   |
|   Cache-Control (response)   |   max-age  s-maxage  must-revalidate  no-cache  no-store  no-transform  public  private  proxy-revalidate   |
|   Expires   |   HTTP-date timestamp   |
|   Pragma   (allows backwards compatibility with HTTP/1.0 caches)   |   no-cache, token   |

  

In addition, more conditional headers can be implemented in the request and response, in order to ensure some precondition while a web page cache-copy has to be provided to users. These headers are described in the RFC7232, some of them follows:

|   Header   |   Definition   |
| --- | --- |
|   If-Match   |   entity-tag   |
|   If-None-Match   |   entity-tag   |
|   If-Modified-Since   |   HTTP-date timestamp   |
|   If-Unmodified-Since   |   HTTP-date timestamp   |
|   If-Range   |   entity-tag / HTTP-date timestamp  (https://tools.ietf.org/html/rfc7233#section-3.2)   |
|   ETag   |   entity-tag  weak  opaque-tag  etagc   |

  

In some specific cases, the following **custom caching headers** can be implemented in the response when a communication relies on a _CDN_ or a _reverse proxy server_:

|   Header   |   Definition   |
| --- | --- |
|   CF-Cache-Status   |   used by CloudFlare CDN in order to provide user the web page caching-state   |
|   X-Drupal-Cache   |   used by Drupal CMS in order to provide user the web page caching-state   |
|   X-Cache-Status   |   used by NGINX reverse proxy in order to provide user the web page caching-state      |
|   X-Proxy-Cache   |   enables the web-page caching in NGINX reverse proxy   |
|   X-Cache   |   general custom header used by CDNs in order to provide user the web page caching-state   |
|   X-Served-By  X-Cache-Hits   |   “X-Served-By” header lists nodes which provide a web page cache copy  (i.e. “node1-cacheserver,  node2-cacheserver, external-node-cacheserver”);  “X-Cache-Hits” specifies which of the listed hosts provides the actual cache  copy (i.e. in case of “node2-cacheserver”,  the header would be “0, 1, 0”)   |
|   X-Cacheable   |   general custom caching header   |
|   X-Cache-Enabled   |   general custom status caching header   |

  

These headers play an important role in the web cache exploitability context, since they can be used by a malicious user in order to obtain additional information related to the aimed cache/web server and framework in use.

  

### A Suitable WebServer Configuration

As described in the _RFC7234_ section 2 and section 3, within a web server configuration, the **logical control** of the web page “_changed-state_” is assigned to specific headers inside the request. These headers are also known as “_**cache keys**_”.  
  
For instance, a “_Nginx_” web server configured as “Content Caching” uses by default the following cache keys:

- **\- headers** (i.e. _Accept-Language, Host, User-Agent, Accept-Encoding_);
- **\- cookies**;
- **\- resources path**;
- **\- URL query**.

Once the cache server finds a different _cache key_ value _within the header_, it will process it as a brand new web page, until its expiration.  
  
Beyond _cache keys_, the basis of a web cache configuration is a suitable **web server** setup which provides the following **basic rules** in order to avoid to cache **custom responses with sensitive information** or _dynamic documents_:  
  

|   \# If you are using the HTTP 1.0 protocol (you are a bad person in this case, you should   \# disable it since it has been deprecated!) the following instruction enables the   \# “Expires: 0” header limits the detention of the cache response to 0 seconds:      httpResponse.setDateHeader("Expires", 0);      \# If you are using the HTTP 1.0 protocol, the following instruction disables the web   \# response caching:      httpResponse.setHeader("Pragma", "no-cache");      \# For further HTTP protocols (v1.1, v1.2 and v1.3), please refer to the following rule  \# in order to disable the web resource caching:      httpResponse.setHeader("Cache-Control", "no-cache,must-revalidate");      \# For further HTTP protocols (v1.1, v1.2 and v1.3), if you are using a proxy or a   \# reverse-proxy, do not cache the required resource within but relies exclusively on the   \# web server version:      httpResponse.addHeader("Cache-Control", "proxy-revalidate");   |
| --- |

  
In order to provide a fine-tuned configuration, for a more in-depth analysis as regards the custom caching headers, you can refer to the following official providers guides:

- \- NGINX Caching guide;
- \- Apache Caching Guide;
- \- Akamai Caching Guide: Part 1, Part 2, Part 3;
- \- Cloudflare Caching Guide;
- \- Fastly Caching Guide.

  

### Web Cache Exploitability

The web-cache headers might sometimes be part of the problem related to web cache security _issues_.  
For example, an attacker might abuse a requested path to create a **type confusion flaw** within a misconfigured server-side web cache storage leading, for instance, to **user data exfiltration** and that might happen even if the response headers related to the cache are properly configured.  
  

As shown in the following table, there can be multiple gateways layers composing a web infrastructure. Each of these layers handles the web cache server according to its own rules.  
  
These usually are: 

|   TOP   Architecture Gateways                     LOW   Architecture Gateways   |   DNS   |
| --- | --- |
|   Load Balancer (AWS)   CDN (Cloudflare, Akamai, Amazon CloudFront)   Reverse Proxy (Haproxy, AWS, Caddy)    |
|   CMS (OPENCMS, AEM, DRUPAL)   |
|   Application Server (JBOSS, JETTY)   |
|   Web Server (NGINX, TOMCAT, APACHE)   |

   
When different gateways enable **conflicting caching rules**, the web cache server may prioritize one of the gateways for certain cases. Specifically, this happens when the same web page would be cached by one gateway but not by another one.  
In these cases, **a secure configuration of the gateways that are not prioritized is completely useless for the purpose of preventing web caching attacks**.  
  

To prevent them, it is necessary to ensure that all the active caching mechanisms are properly configured, considering also edge cases in which different mechanisms result in conflicts about what to cache.  
  
For instance, while performing a security assessment for one of our clients, we faced a web infrastructure in which the _Content Management System (CMS)_ and the web cache server itself had different rules for detecting and caching static files.  
The _CMS_ identified static files based on the URL only, thus it could be fooled to treat an API response as an image.  
The _web cache_ server did not identify that kind of response as images and so it returned the correct caching headers, aiming not to cache them.  
  

In summary, the flaw was generated by the _CMS_ rules with a higher priority than the web cache server’s ones, leading to a **Web Cache Deception Vulnerability** (deeply described following).  
  
The previous scenario is called **_path confusing web cache_**, in that case, it should be paid attention to the “**_vertical order_**” of the components and layers involved in the communication between the two endpoints.  
  
The purpose and the behaviour of the different cache copies related to the components mentioned in the table, as discussed in the beginning of this article, are mostly the same. Differences lie in the **physical and logical position** of these copies within the space and the architecture of the internet communication itself. Moreover, in the **internal configuration** related to each component.

  

### Web Cache Attacks

Beyond the issue of different overlapped web cache configurations, the main impact of a web cache attack is to _store a web page with unexpected content_. In other words, caching ordinary web pages containing **malicious content** or **users’ sensitive information**.  
In summary, an insecure cache management could allow the following attacks:

- **\- Web Cache Deception**, where the attacker forces the victim to cache their sensitive data;
- **\- Web Cache Poisoning**, where the attacker poisons a web page and induces the victim to visit it.

  
Overall, these attacks might result in the possibility of exploiting the following scenarios:

- **_\- Phishing_**
- **_\- HTML Injection_**
- **_\- Cross-Site Scripting_**
- **_\- Open Redirect_**
- **_\- Secrets Disclosure_**

  

### Web Cache Deception Attack

So far so good, but how to perform a “**Web Cache Deception Attack**“ (_WCD_) in order to verify if your web application is vulnerable?  
  
A WCD attack consists in forcing the web server to store victim’s data in a **public cache** and then access it. The fun part is that this is one of those rare cases in which it is easier done than said.  
This attack was firstly documented by Omer Gil in 2017 and presented at Black Hat in the same year.  
  
Let’s consider a web application that provides an API to retrieve the user personal data at the endpoint ‘_/my-account/getData.json_’.  
In a standard configuration, the server would cache web pages based on the response headers. Instead, the application implements a CMS, whose configuration caches web pages based on file extension, disregarding the headers. For this reason, since the response of this API call is JSON data, it does not get cached.  
  
What if the user calls ‘_/my-account/getData.json/**test.css**_’? The application processes the request to ‘/getData.json’ and then the CMS caches the response, since it detected a static file extension.  
  
Request:

|   GET /my-account/getData.json/test.css HTTP/1.1  Host: www.vulndomain.com   |
| --- |

  
 Response: 

|   HTTP/1.1 200 OK  Cache-Control: max-age=0, no-cache, no-store      {"companyName":"Acme Corp.","userName":"admin","firstName":"Mario",  "lastName":"Rossi","email":"mario@acme.com","telephone":"+39 3333333333"}   |
| --- |

  
    It can be noted that the “Cache-Control” header in the response should prevent the server from caching the page, but it still gets stored in a public cache. This is due to the CMS treating the response as a public static file.  
Thus, anyone who has access to the same web cache of that user, will be able to access the same content by requesting ‘_/my-account/.getData.json/test.css_’.  
That happens because the subsequent request does not reach the central server (that processes API calls), but stops at a closer edge server, which returns the cached response.  
  
In other words, to exploit this vulnerability an attacker sends the victim a custom URL which forces the CMS to publicly cache sensitive data once the victim user visits the malicious URL. The malicious user then, accessing the URL he knows, will receive within the response body the victim user’s sensitive information wrongly stored due to the cache flaw.  
  
Therefore, if the web server is configured to not cache any requests under the directory "_/getData.json_" and in a second place an admin adds a rule within the reverse-proxy in order to cache JPEG files, an attacker could exploit a **_Web Cache Deception_** attack by confusing the required path (i.e. requiring the resource "_/getData.json/..;/a.jpg_").  
  
You should also verify the following payloads (which may fool one of the caching components of your infrastructure) to ensure that your endpoints are properly secured against Web Cache Deception Attacks:

|   \# Encoded Newline (n)  example.com/account.php%0Anonexistent.css  \# Encoded Semicolon (;)  example.com/account.php%3Bnonexistent.css      \# Encoded Pound (#)  example.com/account.php%23nonexistent.css      \# Encoded Question Mark (?)  example.com/account.php%3Fname=valnonexistent.css   |
| --- |

  
 Reference: “Cached and Confused: Web Cache Deception in the Wild”.

  

### Web Cache Poisoning Attack

On the other side, the “**Web Cache Poisoning Attack**” objective is to **add malicious code** inside a vulnerable web page and **aim the attack** at a specific victim since the attacker would know the exact cache keys used to **store** the malicious web page.  
  
This attack was firstly documented by James Kettle in 2018 and presented at Black Hat in the same year.  
  
Within this scenario, the targeted web page is identified with a specific “_cache key_” which could be, for instance, a **nonexistent URL parameter value** on a specific path. Indeed, the attacker-controlled web page will be cached on this specific nonexistent parameter value.  
Moreover, while in the “_Web Cache Deception_” scenario it is necessary to detect a vulnerable file extension, this attack also requires the interaction of the web page with a header parameter that is not used as key by the caching server.  
  

For instance, suppose that a malicious user discovers through the BurpSuite extension “Param Miner” the hidden “**X-Origin-URL**” header which is **reflected** in the response web page and it is **not used as cache-key**. This header represents an entry point able to exploit a **Cross-Site Scripting** vulnerability, as shown in the following request.  
  
Request:

|   GET /home/private/projections?insurance=not\_existing HTTP/1.1  Host: vulnerable-host.com  User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86\_64; rv:81.0) Gecko/20100101 Firefox/81.0  Accept-Language: it-IT,it;q=0.8,en-US;q=0.5,en;q=0.3  Accept-Encoding: gzip, deflate  X-Origin-URL: “><img src=x onerror=alert(document.cookie)><!-- |
| --- |

  

 Response:   

|   HTTP/1.1 200 OK  Content-Type: text/html  Connection: close  X-Cache: MISS  Content-Length: 3114      \[ . . . \]  <img src=””><img src=x onerror=alert(document.cookie)><!--/img/bank\_logo.png”><br>  \[ . . . \]   |
| --- |

  

Once the victim user visits the resource with the arbitrary URL parameter value controlled by the malicious user:  

- _“vulnerable-host.com/home/private/projections?insurance=not\_existing”_

The **cached response** with the **XSS payload** is returned to the victim user, exploiting successfully the Cache Poisoning attack.  
  
Request:   

|   GET /home/private/projections?insurance=not\_existing HTTP/1.1  Host: vulnerable-host.com  User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86\_64; rv:81.0) Gecko/20100101 Firefox/81.0  Accept-Language: it-IT,it;q=0.8,en-US;q=0.5,en;q=0.3  Accept-Encoding: gzip, deflate   |
| --- |

   
Response:   

|   HTTP/1.1 200 OK  Content-Type: text/html  Connection: close  X-Cache: HIT  Content-Length: 3114      \[ . . . \]  <img src=””><img src=x onerror=alert(document.cookie)><!--/img/bank\_logo.png”><br>  \[ . . . \]   |
| --- |

  
A different scenario regards the “**_Edge Side Includes_**” (_ESI_) _**tag injection attack**_ when reverse proxies, load balancers, caching servers or proxy servers (for instance, _“Squid Proxy”, “Varnish”, “Oracle WebLogic”, “F5” and “Fastly”_) are involved in a communication.  
  
This attack, combined with the cache poisoning one, could lead to potential **stored SSRF** and **XSS attacks**. Since the ESI parser is not able to distinguish between legitimate ESI tags provided by the upstream server and malicious ones injected in the HTTP response, an attacker could abuse a Cache Poisoning flaw in order to **store a malicious <esi>** **tag inside** a dynamic-generated response page.  
  
Let’s consider, for instance, the _SSRF_ scenario:  
The following request injects an _<esi>_  tag inside the response through a URL parameter usafely reflected in the response page and the “client\_id” header used as cache-key. 

Request: 

|   GET /home/contact?client\_id=143&country\_id=<esi:include%20src="http://10.0.0.7/transfer?recipient=12345&amount=54321"/> HTTP/1.1  Host: vulnerable-host.com  User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86\_64; rv:81.0) Gecko/20100101 Firefox/81.0  Accept-Language: it-IT,it;q=0.8,en-US;q=0.5,en;q=0.3  Accept-Encoding: gzip, deflate   |
| --- |

  
Response:   

|   HTTP/1.1 200 OK  Content-Type: text/html  Connection: close  X-Cache: MISS  Content-Length: 3114      \[ . . . \]  <esi:include src="http://10.0.0.7/transfer?recipient=12345&amount=54321"/>  \[ . . . \]   |
| --- |

  
If a malicious user tricks a victim user to visit the URL with the attacker’s cache-key:

- _“vulnerable-host.com/home/contact?client\_id=143”_

the application sends the cached response page with the injected _<esi>_ tag to the surrogate. It detects the tag and the “_Stored Server-Side Request Forgery_” attack is successfully exploited!  
The diagram below shows how the parts interact each other:

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgKq_4iN5O_TZW0DeVTjbm7RSATWqxyswNBjiXHFu2jVZdKeVk9u0CKbaSVWBdtsQB6dgFPsa8h_z_98itLZZ20vfmCStKCHq-vaRYoA9i8JqXya2fQDJH20KkJOfIpE9yTvkxQwEuhyphenhyphenpU/w640-h384/ssrf_blog.jpg)

  

### Conclusions

It is recommended to pay close attention when **new caching rules** are added, since extending pre-existing configurations is a typical **main source** for cache flaws.  
  
For this reason, it is strongly discouraged the use of _pre-made_ _scripts_ for an homogeneous and immediate cache-control setting over all endpoints.  
  
Regarding **Web Cache Deception** mitigation, all the gateway layers should have a consistent configuration in order to prevent a misconfiguration among them and should cache web pages based on their content type.  
  
On the other hand, in order to decrease the possibility of being vulnerable against _Web Cache Poisoning_ attacks, it is recommended to **validate** and **escape** all the users-controlled inputs, including the request headers.  
  
In particular, it is important to check all the inputs which are **reflected** in cacheable responses. Indeed, it is strongly recommended to **avoid caching web responses** which include user-provided inputs.  
  
Last but not least, as a best-practice, it is important to **stay up to date** with the last **security bulletin** related to your own _web components_ in-use. In order to become aware of new **security patches** or guides for a correct and secure infrastructure configuration!

  

## References

- \- Cloudflare CDN cache behaviour: https://support.cloudflare.com/hc/en-us/articles/200172516
- \- Reverse proxies related attacks: https://www.acunetix.com/blog/articles/a-fresh-look-on-reverse-proxy-related-attacks/
- \- Paper “Cached And Confused”: https://arxiv.org/pdf/1912.10190.pdf 
- \- CloudFlare - How to avoid cache poisoning: https://support.cloudflare.com/hc/en-us/articles/360014881471-Avoiding-Web-Cache-Poisoning-Attacks
- \- Header Caching by Amazon:  
    https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/header-caching.html
- \- Cache Poisoning Attack:  
    https://samcurry.net/abusing-http-path-normalization-and-cache-poisoning-to-steal-rocket-league-accounts/
- \- General HTTP header list: https://hackertarget.com/http-header-check/
- \- Introduction to cache: http://home.eng.iastate.edu/~zzhang/courses/cpre581-f05/reading/smith-csur82-cache.pdf  
    https://appcheck-ng.com/web-cache-poisoning-explained/
- \- Using cache memory to reduce processor-memory traffic: https://dl.acm.org/doi/abs/10.1145/800046.801647
- \- Beyond XSS: Edge Side Include Injection:  
    https://www.gosecure.net/blog/2018/04/03/beyond-xss-edge-side-include-injection/

Go to Source
