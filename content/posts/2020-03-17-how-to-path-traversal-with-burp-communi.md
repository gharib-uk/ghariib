---
title: "How to Path Traversal with Burp Community Suite"
date: 2020-03-17T04:20:00.000-07:00
draft: false
type: posts
categories: 
- blackbox,burp,intruder,path traversal,testing
---
# How to Path Traversal with Burp Community Suite

<br/>

<br/>
  

### Introduction

  

A well-known, never out of fashion and highly impact vulnerability is the [Path Traversal](https://owasp.org/www-community/attacks/Path_Traversal). This technique is also known as dot-dot-slash attack (../) or as a directory traversal, and it consists in exploiting an insufficient security validation/sanitization of user input, which is used by the application to build pathnames to retrieve files or directories from the file system that is located underneath a restricted parent directory.  
By manipulating the values through special characters an attacker can cause the pathname to resolve to a location that is outside of the restricted directory.  
  
In OWASP terms, a path traversal attack falls under the category A5 of the top 10 (2017): Broken Access Control, so as one of top 10 issues of 2017 we should give it a special attention.  
  
In this blog post we will explore an example of web.config exfiltration via path traversal using **Burp Suite Intruder Tool**.  
  
Previous posts about path traversal:  
[How to prevent Path Traversal in .NET](https://blog.mindedsecurity.com/2018/10/how-to-prevent-path-traversal-in-net.html)  
[From Path Traversal to Source Code in Asp.NET MVC Applications](https://blog.mindedsecurity.com/2018/10/from-path-traversal-to-source-code-in.html)

### Testing Step-by-Step

First, get a copy of [Burp Suite Community Edition](https://portswigger.net/burp/communitydownload), a useful testing tool that provides many automated and semi-automated features to improve security testing performances.  
In particular, _[Burp Intruder](https://portswigger.net/burp/documentation/desktop/tools/intruder/using)_ feature can be very useful to exploit path traversal vulnerabilities.

  
Suppose there's a _DotNet web application_ vulnerable to path traversal. In order to exploit the issue the attacker can try to download the whole source code of the application by following [this tutorial](https://blog.mindedsecurity.com/2018/10/from-path-traversal-to-source-code-in.html).  
  

Once the attacker finds a server endpoint that might be vulnerable to Path Traversal, it's possible to send it to _Burp Intruder_ as shown in the following screenshot.

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjmsASlKPCYiIYcdX0-Gah5sNDQj1Fqhcb95wNqB30uN3hU0HbBnO-U_pH3M2o18O2Jbiajc2_gI1vuU2FUEo8pkt2opt8WImMBaicvkDWdF7Pa1sND98bYzZQ0FIdvUUnuUXBoPqkS85I/s640/SendToIntruder.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjmsASlKPCYiIYcdX0-Gah5sNDQj1Fqhcb95wNqB30uN3hU0HbBnO-U_pH3M2o18O2Jbiajc2_gI1vuU2FUEo8pkt2opt8WImMBaicvkDWdF7Pa1sND98bYzZQ0FIdvUUnuUXBoPqkS85I/s1600/SendToIntruder.PNG)

  

  
On the Intruder tab, the target has been set with the request that it will be used to manipulate in order to find the _web.config_ file.

  

Make sure that the payload is correctly injected in the right attribute position, if not, perform a "Clear §" action, then select the attribute to fuzz and click on "Add §" button. 

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEitDzejk8AqLSIl5JN5uRfnmUXUdDfcRX7mbsTevs6kv9eT0sn9Rda79Q79wUV7eSTb4cG6vNhCRAH4RA9trMSiZf4s5I-NaPdbD96xAuufrDoDRRCgOnzJZRX8UZpaw-1cWKDJ0ePOHeY/s640/IntruderPreview.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEitDzejk8AqLSIl5JN5uRfnmUXUdDfcRX7mbsTevs6kv9eT0sn9Rda79Q79wUV7eSTb4cG6vNhCRAH4RA9trMSiZf4s5I-NaPdbD96xAuufrDoDRRCgOnzJZRX8UZpaw-1cWKDJ0ePOHeY/s1600/IntruderPreview.PNG)

  

  

To set the payloads that Burp Intruder will use to perform the requests, download  file [traversals-8-deep-exotic-encoding.txt](https://github.com/fuzzdb-project/fuzzdb/blob/master/attack/path-traversal/traversals-8-deep-exotic-encoding.txt)  from [fuzzdb project](https://github.com/fuzzdb-project/fuzzdb)  and provide it to _Burp Intruder_ by executing the following actions:  

-   go to the "Payloads" sub-tab;
-   select from dropdown list "Payload type" the value "Simple List";
-   in the panel "Payload Options" click on "Load..." button and select the fuzzing path traversal file (as shown in following screenshot).

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgCX9Wh76nIEIybxAzj3IirS-CVi5N_2XYPwRHuZXHHU2f7QiUmJVEQvzsRdPj3-fkVaKUJ0bZ6jsw1pODi3lon0k_xW6MadF0LvDgzxjl2mAPwX9CtzeRkfAe2m57M-9HFHxLvP4APzK8/s640/PayloadConfiguration.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgCX9Wh76nIEIybxAzj3IirS-CVi5N_2XYPwRHuZXHHU2f7QiUmJVEQvzsRdPj3-fkVaKUJ0bZ6jsw1pODi3lon0k_xW6MadF0LvDgzxjl2mAPwX9CtzeRkfAe2m57M-9HFHxLvP4APzK8/s1600/PayloadConfiguration.PNG)

  

  

Next step is to add a Payload Processing rule in order to match and replace the placeholder "{FILE}" with the filename we want to exfiltrate (in our example "web.config"), so click on "Add button".

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiCzydd0_TkFx1GAM7_yboWzDMrIV2Jr5GUKQ3bUKgUlPE9Fkpg08S3n72giVzgdrk7dKNGYjK7avTCyoTIqqmrLBM7ADjbudTa9yKvHuXBpjdxTrggE1QCO-LBgpezAcVcrYEDPH-BLO0/s640/screenshot+Burp+1.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiCzydd0_TkFx1GAM7_yboWzDMrIV2Jr5GUKQ3bUKgUlPE9Fkpg08S3n72giVzgdrk7dKNGYjK7avTCyoTIqqmrLBM7ADjbudTa9yKvHuXBpjdxTrggE1QCO-LBgpezAcVcrYEDPH-BLO0/s1600/screenshot+Burp+1.PNG)

  
  
In the paylod processing rule modal, add the Match for string "{FILE}" and the Replace for string "web.config", as shown in following screenshot:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgJsQsM1Vc0Tljg52alKKtiaQyyz6E7R1DIqLLiYzqon3XZ48c_xT-qq28yZEUmpi7R9hq3aYJ8vN-EzC2CCRV7Y92MqEo0Uyartgn3FsyAicKP25j3x1z7m6MhctZQutbrlMFG1NyFMWA/s640/screenshot+Burp+2.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgJsQsM1Vc0Tljg52alKKtiaQyyz6E7R1DIqLLiYzqon3XZ48c_xT-qq28yZEUmpi7R9hq3aYJ8vN-EzC2CCRV7Y92MqEo0Uyartgn3FsyAicKP25j3x1z7m6MhctZQutbrlMFG1NyFMWA/s1600/screenshot+Burp+2.PNG)

  

  

In order to improve the probability of a successful attack, it is possible to add a Grep-Match value (if known), in order to easily identify a positive response.

  

Remove all already existing rules:

[  
](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgfJ2scT_vfa24m-Rb0kWO_yklJOrZ0pQLT5sqO7dnjIQpiGvlwZNhZ2fvwe3G8NDOz3sIHgFtF-ekI_fiGfNVbkc_y_mtdl9thIHtpyy67rjU80c8ZkJj3GWsPi5jQcmmFWWcVH5m0XJU/s1600/GrepMatchOptions1.PNG)[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgfJ2scT_vfa24m-Rb0kWO_yklJOrZ0pQLT5sqO7dnjIQpiGvlwZNhZ2fvwe3G8NDOz3sIHgFtF-ekI_fiGfNVbkc_y_mtdl9thIHtpyy67rjU80c8ZkJj3GWsPi5jQcmmFWWcVH5m0XJU/s640/GrepMatchOptions1.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgfJ2scT_vfa24m-Rb0kWO_yklJOrZ0pQLT5sqO7dnjIQpiGvlwZNhZ2fvwe3G8NDOz3sIHgFtF-ekI_fiGfNVbkc_y_mtdl9thIHtpyy67rjU80c8ZkJj3GWsPi5jQcmmFWWcVH5m0XJU/s1600/GrepMatchOptions1.PNG)

  

  

Then add a new Grep-Match rule for "<configuration>" string, that indicates web.config file has been found.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhu-vNtVBHcQ0dKQV9hJusqjjNXTD3V7hm2MSjZ2J5fQlm04Q4K5fWay-y1nqIWXf49KMr2OFLFWeG60DJwnrSxZ_I5As4gV0kEGufwUxQ1-vPiCtceSr67nY6PjIX0TvItRIeshrC10TI/s640/GrepMatchOptions2.PNG)

  

  

  
Finally, it's suggested to tune the Request Engine options basing on web server limitations (anti-throttling, firewall system, etc) in order to avoid false negative results, for example increasing retry delay.  

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiQe-wjFTFj_SQvnpahyphenhyphen-LBdzWgjqCZE-3UeWrdwyv1ePHA9Wfq8-OCbGiyYBP0LTqDQb9w6CVRgQgcEw2rj7e8GBxwaWYyx916wPM78Wex6IEwuISXiGLmA9expFYGgVL_UOJbAKwUXBk/s640/StartAttack.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiQe-wjFTFj_SQvnpahyphenhyphen-LBdzWgjqCZE-3UeWrdwyv1ePHA9Wfq8-OCbGiyYBP0LTqDQb9w6CVRgQgcEw2rj7e8GBxwaWYyx916wPM78Wex6IEwuISXiGLmA9expFYGgVL_UOJbAKwUXBk/s1600/StartAttack.PNG)

  
  

**Let's launch the attack.** 

  

If the endpoint will result vulnerable to path traversal, the column "configuration" will be checked.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjDXUlRQpmsaljyh_z7j-QdoeBF_4hZxwt7XTuYnb6utZIxPbIei7U01xHl4usJnOFWDvUYakToXG6qtXsTtm2JjtBTGUtxVSvu09bx108vWhyImmprq1INYHSieBsj7xZJ3v4mzmqLmW0/s640/AttackResultOrdered.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjDXUlRQpmsaljyh_z7j-QdoeBF_4hZxwt7XTuYnb6utZIxPbIei7U01xHl4usJnOFWDvUYakToXG6qtXsTtm2JjtBTGUtxVSvu09bx108vWhyImmprq1INYHSieBsj7xZJ3v4mzmqLmW0/s1600/AttackResultOrdered.PNG)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgWao7_MpXEfrKlofdlG8Mphrh1IAaI8NuVrsqErRtt8xeMwKbQxwiWWktcxuRp-Nk6UPV4vAzGgitz1FcIrhmZLbwhe4RBG7E9mYG40p90Xl7WTebnPZExszwa_xD7PxP28RsOUxfAYw4/s640/WebConfigExtracted.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgWao7_MpXEfrKlofdlG8Mphrh1IAaI8NuVrsqErRtt8xeMwKbQxwiWWktcxuRp-Nk6UPV4vAzGgitz1FcIrhmZLbwhe4RBG7E9mYG40p90Xl7WTebnPZExszwa_xD7PxP28RsOUxfAYw4/s1600/WebConfigExtracted.PNG)

#### [Source](https://blog.mindedsecurity.com/feeds/1510663604572076596/comments/default)

<br/>
---
