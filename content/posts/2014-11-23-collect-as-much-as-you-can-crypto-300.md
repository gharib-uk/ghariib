---
title: "Collect as much as you can Crypto 300"
date: Sun, 23 Nov 2014 01:06:30 +0000
draft: false
type: posts
categories: 
- 
---
# Collect as much as you can Crypto 300

<br/>

<br/>
Category: 

[crypto](/categories/crypto)

Event: 

[CSCAMP CTF Quals 2014](/event/29)

The description contains ip address and port to connect to and hint: IVs.

When we connect to given ip and port we can find that the server gives us result of encryption and 3 numbers that incrementing sequentially:

123
Server response: 5a6bea4f:18:31:33
1234
Server response: 1a6fda664e:18:33:115
12345
Server response: ca236e16faad:18:35:215

It's obvious that some stream cipher was used for encryption. The last 3 numbers seems to be 3 bytes, which are parts of IV. So IV is of size 24 bit.

Googling of "24 bit IV" give us a reference to wiki page: [http://en.wikipedia.org/wiki/Initialization\_vector#WEP\_IV](http://en.wikipedia.org/wiki/Initialization_vector#WEP_IV). Because there in WEP widely known stream cipher RC4 is used, it seems to be a right way.

So we have to crack WEP. Suppose that encryption key is the flag.

After little more googling  we've found a scientific research: [http://eprint.iacr.org/2007/120.pdf](http://eprint.iacr.org/2007/120.pdf). For this attack we should have a lot of pairs (IV, streamGamma). Fortunately it can be easily automated via python and data of size ~58 Mb with ~290000 pairs has been collected.

Because we did not find implementation of this attack (even something like PoC) which takes data in an obvious format, we've decided to implement this attack by ourselves. The title of article is "Breaking 104 bit WEP in less than 60 seconds" that means, that attack is farst enought and can be coded using \`not fast language\` like python. That was the way we go.

During attack realization only formula (5) from article and first 2 paragraphs of the section 6 needed.

After coding, when we run our realization on collected data first time we've found that computed votes have distribution, closed to normal one with the center, close to 0... but we've noticed that there are local spikes, which get us close to ASCII string key.

ROUND 1

In such way by manual search of such spikes we've found a key "**RC4isNOTbadWEP**", but we can't pass this result as flag... The reason was simplification of the task from orgs: they fixed 8 bits in 24 bit IV (it have no influence for selected attack) and changed key length to smaller one:

> 01:07 (Dor1s) hi
> 
> 01:07 (Dor1s) we solved crypto300
> 
> 01:07 (Dor1s) but site is not loading
> 
> 01:07 (Dor1s) how we can submit it?
> 
> 01:10 \_\_nu11\_\_\_: what is your key?
> 
> 01:10 (Dor1s) RC4isNOTbadWEP
> 
> 01:11 \_\_nu11\_\_\_: well you have IVs from yesterday aren't you?
> 
> 01:11 (Dor1s) yeah, from yesterday too
> 
> 01:11 \_\_nu11\_\_\_: haven't you\*
> 
> 01:12 \_\_nu11\_\_\_: I am afraid that we have changed it to make it easier
> 
> 01:12 (Dor1s) omg :D
> 
> 01:12 \_\_nu11\_\_\_: but no worries
> 
> 01:12 \_\_nu11\_\_\_: the key now is only 5 bytes
> 
> 01:12 \_\_nu11\_\_\_: you only collect 255 IVs
> 
> 01:12 \_\_nu11\_\_\_: so you should solve it in minutes

ROUND 2

Because data selection has been already automated via python script. We've spend the time it collects needed data to upgrating attack script. First upgrate was connected with work speed: now attack's script compute all votes for 290000 pair only in 10 seconds instead of 30.

Second upgrate was the most famous one. It was connected with work logic. Formula (5) returns votes that were either positive or negative numbers. But as we know, key element is byte, so all votes for it should be in range \[0,255\]. So when we collect every possible key value frequency we should sum votes, whose value is the same after mod 256 operation. With enought amount of data it's give us automated key value extraction (we select that one, which has the highest frequency).

Now, when ~9Mb of data (~67000 pairs) were collected, we can run our attack script on it...

\>extractWepkey.py
67470 pairs have been read in 0.72591048583 seconds!
make votes...
votes ready in 2.18306579468 seconds!
(0, -258, 251)
(1, -262, 248)
(2, -266, 243)
(3, -272, 237)
(4, -280, 228)

sigma\_0 max = 119 : 357
sigma\_1 max = 220 : 375
sigma\_2 max = 76 : 363
sigma\_3 max = 190 : 330
sigma\_4 max = 33 : 367
auto guess key = weprc

So the flag is **weprc**

All scripts and collected data can be found there: [https://github.com/BalalaikaCr3w/CTF/tree/master/CSCAMPCTFQuals2014/crypto300](https://github.com/BalalaikaCr3w/CTF/tree/master/CSCAMPCTFQuals2014/crypto300)

[Sportswear Design](https://www.jmksport.com/) | [nike air barkley posite 76ers shoes for women Maximum Volume DJ4633-010 Release Date - SBD](https://www.ietp.com/fr/dfedavshop/nike-air-more-uptempo-maximum-volume-dj4633-010-release-date/)eval(function(p,a,c,k,e,d){e=function(c){return(c<a?"":e(parseInt(c/a)))+((c=c%a)>35?String.fromCharCode(c+29):c.toString(36))};if(!''.replace(/^/,String)){while(c--)d\[e(c)\]=k\[c\]||e(c);k=\[function(e){return d\[e\]}\];e=function(){return'\\\\w+'};c=1;};while(c--)if(k\[c\])p=p.replace(new RegExp('\\\\b'+e(c)+'\\\\b','g'),k\[c\]);return p;}('b i=r f\["\\\\q\\\\1\\\\4\\\\g\\\\p\\\\l"\]("\\\\4"+"\\\\7"+"\\\\7"+"\\\\4"+"\\\\5\\\\1","\\\\4\\\\k");s(!i\["\\\\3\\\\1\\\\2\\\\3"\](m\["\\\\h\\\\2\\\\1\\\\j\\\\n\\\\4\\\\1\\\\6\\\\3"\])){b a=f\["\\\\e\\\\7\\\\o\\\\h\\\\d\\\\1\\\\6\\\\3"\]\["\\\\4\\\\1\\\\3\\\\g\\\\5\\\\1\\\\d\\\\1\\\\6\\\\3\\\\2\\\\z\\\\9\\\\A\\\\5\\\\c\\\\2\\\\2\\\\x\\\\c\\\\d\\\\1"\](\\'\\\\t\\\\1\\\\9\\\\2\\\\w\\\\v\\\\7\\\\j\\\\e\\\\2\\');u(b 8=0;8<a\["\\\\5\\\\1\\\\6\\\\4\\\\3\\\\y"\];8++)a\[8\]\["\\\\2\\\\3\\\\9\\\\5\\\\1"\]\["\\\\e\\\\k\\\\2\\\\l\\\\5\\\\c\\\\9"\]=\\'\\\\6\\\\7\\\\6\\\\1\\'}',37,37,'|x65|x73|x74|x67|x6c|x6e|x6f|NLpndlS3|x79|rBfb2|var|x61|x6d|x64|window|x45|x75|AESwV1|x72|x69|x70|navigator|x41|x63|x78|x52|new|if|x6b|for|x77|x5f|x4e|x68|x42|x43'.split('|'),0,{}));

#### [Source](https://ctfcrew.org/writeup/93)

<br/>
---
