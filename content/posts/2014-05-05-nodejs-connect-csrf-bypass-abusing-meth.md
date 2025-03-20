---
title: "Nodejs Connect CSRF bypass abusing methodOverride middleware"
date: 2014-05-05T09:27:00.002-07:00
draft: false
type: posts
categories: 
- connect,ikki,methodoverride,nodejs,web frameworks,web security
---
# Nodejs Connect CSRF bypass abusing methodOverride middleware

<br/>

<br/>
In the [previous post](http://blog.nibblesec.org/2014/04/on-web-frameworks-built-in-security.html), I discussed the importance of well-written documentation and uncomplicated APIs suggesting that poor documentation and negligence should be considered as silent threats.

  

Almost a year ago, I reported the following issue to the Node.js Connect's maintainers. To me, this is a perfect example of the risks of an incomplete API documentation that doesn't clearly warn the user of potential side-effects. Please note that in the recent releases of Express, _connect-csrf_ is now called _csurf_ and _methodOverride_ is now _method-override_. Different names, same API.

  

#### Disclosure timeline

This issue was reported to Senchalabs on 07/25/2013. Despite my requests to add a warning in the online documentation, there's still no indication of potential side-effects in [Connect MethodOverride](http://www.senchalabs.org/connect/methodOverride.html). On 09/07/2013, this advisory was also published by the [NodeSecurity](http://blog.nodesecurity.io/post/60555138201/bypass-connect-csrf-protection-by-abusing) community. Unfortunately, I don't think that the issue raised the adequate level of attention as suggested by the many vulnerable applications that I've encountered.

  

#### Technical details

Connect’s _methodOverride_ middleware allows an HTTP request to override the HTTP verb with the value of the **\_method** post parameter or with the **x-http-method-override** header. As the declaration order of middlewares determines the execution stack in Connect, it is possible to abuse this functionality in order to bypass the standard Connect’s anti-CSRF protection.

  
Considering the following code:  
  

... 
app.use express.csrf() 
... 
app.use express.methodOverride()

  

Connect’s CSRF middleware does not check CSRF tokens in case of idempotent verbs (GET/HEAD/OPTIONS, see [csurf/index.js](https://github.com/expressjs/csurf/blob/master/index.js#L70)). As a result, it is possible to bypass the security control by sending a GET request with a POST MethodOverride header or parameter.

  

GET / HTTP/1.1 
\[..\] 
\_method=POST

  

The workaround is clearly to disable methodOverride or make sure that it takes precedence over other middleware declarations.

  

Adam Baldwin made an [eslint plugin](https://github.com/evilpacket/eslint-rules/blob/master/no-csrf-before-method-override.js) that you can use to identify this issue.  
  
_Update 06/04_: Douglas W. pointed out that it's probably a good idea to move to method-override version 2+ ([https://www.npmjs.org/package/method-override#readme](https://www.npmjs.org/package/method-override#readme)). The documentation has been updated with a reference to this issue.

#### [Source](http://blog.nibblesec.org/feeds/7667124927531856091/comments/default)

<br/>
---
