---
title: "Using Dharma to rediscover Nodejs out-of-band write in UTF8 decoder"
date: 2015-08-09T23:43:00.001-07:00
draft: false
type: posts
categories: 
- 1day,fuzzing,ikki,nodejs
---
# Using Dharma to rediscover Nodejs out-of-band write in UTF8 decoder

<br/>

<br/>
A month ago, Node.js released a [security update](http://blog.nodejs.org/2015/07/04/node-v0-12-6-stable/) for a bug in V8's utf-8 decoder affecting Buffer to String conversions. Since numerous native functions for networking and I/O are affected, a malicious user could deliver a crafted input to crash a remote Node.js process. A truncated four-bytes sequence can be used to create a misalignment in the _WriteUtf16Slow_ function, resulting in a segmentation fault. For more details on the actual vulnerability, have a look at the [V8 patch](https://chromium.googlesource.com/v8/v8.git/+/b199bcdd47ae97ec116b430e34ab42001c8f04c0%5E%21/#F2) and the original [bug report](https://github.com/joyent/node/issues/25583).

  
Just after the release of the patch, I started experimenting with this vulnerability to create a proof-of-concept:  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjjiOBm3lLTWmlHEkRHE-xdJcUsLWxVe-Mg_NtTgmE2fWCa4OUX2GbdjNTZ36SfQAwoir5dea4BjGiwJ3q6UfFJuiPGMTBmje9cpNFxfJT7VZUsxPaCozeNvH5WoNNmueNBWSCBax641P4/s400/Screen+Shot+2015-07-26+at+12.06.53+AM.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjjiOBm3lLTWmlHEkRHE-xdJcUsLWxVe-Mg_NtTgmE2fWCa4OUX2GbdjNTZ36SfQAwoir5dea4BjGiwJ3q6UfFJuiPGMTBmje9cpNFxfJT7VZUsxPaCozeNvH5WoNNmueNBWSCBax641P4/s1600/Screen+Shot+2015-07-26+at+12.06.53+AM.png)

  

Almost around the same time, I noticed that [Christoph Diehl](https://twitter.com/posidron) from Mozilla published a grammar-based fuzzer named [Dharma](https://blog.mozilla.org/security/2015/06/29/dharma/). The tool parses formal grammar definitions and generates test cases. Although the concept is not new, Mozilla released a neat implementation with great efficiency.  
  
  

### Can we rediscover the same bug using Dharma? 

As an excuse to play with Dharma, I decided to try to replicate the same Buffer vulnerability. In this post, I will guide you through the setup and execution.

  

First, we need to create a grammar to define Node's Buffer functions. From the [official API doc](https://nodejs.org/api/buffer.html), I started classifying all APIs in three categories: _definitions_, _permutations_ (from Buffer to Buffer) and _operations_ (from Buffer to other types).

  

Based on this model, all test cases will resemble the following template:

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgrBUdEdVPRP4a98W0AgFRuXtnR339Dq-dKG1PGag6LVLvx8ziNTzfkxsvdQLOy7AcrOYeQYPzY3PcxlKVFh0q2e6vYWCAmF_nkKKb-VDNu0FFOCjaUm7zZjyD-vSXDJQ8lhvoJqVLG_p8/s320/Screen+Shot+2015-07-27+at+10.55.04+PM.png)

  

The resulting _buffer.dg_ grammar has been merged in the [official Github repository](https://github.com/MozillaSecurity/dharma/blob/master/dharma/grammars/nodejs/buffer.dg).

  

With Dharma, we can now generate test cases with a simple command:

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh_nOSUD9FSbTfAv6k0x-SPGkLWQ6y4ki7TjoLKJL2B6sPQ2nrEuq0H7Ypn9BqAZ8bFU-W452o4Vo3FwPqgQnn9Vk9x6_i5g8ym2bkSae7IbvvRM-FGyrG349IfBTaeXnTSGtijr8-BRug/s640/Screen+Shot+2015-07-27+at+11.01.34+PM.png)

  

At this point, we just need to execute our test cases and wait for the results. After trying a few different solutions, I ended up using a very simple bash script:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEguRh4DkLV7lLhXViHSqXZmIQ7DSRt2iDOLYC3l4YLZ7OYCErDbfcs7aDOH5qCuuRU6suCmnRsw7BUpDxdG2d4SQjiPvOUe0VyJWsTj7XCaf_hp8AdpOsck7hWSuMT9ecNJzIPQ30UWZOU/s400/Screen+Shot+2015-07-27+at+11.08.07+PM.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEguRh4DkLV7lLhXViHSqXZmIQ7DSRt2iDOLYC3l4YLZ7OYCErDbfcs7aDOH5qCuuRU6suCmnRsw7BUpDxdG2d4SQjiPvOUe0VyJWsTj7XCaf_hp8AdpOsck7hWSuMT9ecNJzIPQ30UWZOU/s1600/Screen+Shot+2015-07-27+at+11.08.07+PM.png)

  

After leaving the fuzzer alone for the night, I came back in the morning to discover a multitude of core dumps. Hidden among thousands of _V8::FatalProcessOutOfMemory_ and _SIGILL Illegal instruction_ errors, I finally discovered a sample that was triggering something interesting.

  

Looking at the backtrace,  we can confirm that we're triggering the same vulnerability. If you're interested, I've uploaded the [auto-generated test case](http://nibblesec.org/files/17360.test).

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgLOLTKjb_ErLiVL41ML8GmDbq8rb3YE2Z-zehqfcoSgm9QgdoxLG4Qs5r8zBZkMwKH1-6mQLcDfsjkPxtXvH1TmfTNwmWzBsSM0_YBIofBmzMig3fhvQqsmHu6GZPWr8b_x6-EO_adeLc/s640/Screen+Shot+2015-07-27+at+11.20.30+PM.png)

###   

### Now what?!

Node.js Buffer provides a very powerful API with raw memory allocation capabilities. Ilja van Sprundel outlined some of the risks during a [recent webcast](https://www.youtube.com/watch?v=XXqP-jzJBWs), and the latest vulnerability was a clear demonstration of the possible outcomes. Having already spent a few hours on building the grammar, I expanded this little fuzzing exercise with the goal of discovering similar vulnerabilities. After a few days of generation/execution and over 400,000 test cases, I have yet to triggered another segmentation fault in Node.js' Buffer. Although this exercise doesn't give us a definitive assurance, it is probably a good sign of the maturity of the API. Nonetheless, grammar-based fuzzing is fun and can lead to interesting results.

#### [Source](http://blog.nibblesec.org/feeds/2446573427485052685/comments/default)

<br/>
---
