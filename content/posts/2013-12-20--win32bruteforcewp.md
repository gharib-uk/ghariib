---
title: "Win32BruteForceWP"
date: Fri, 20 Dec 2013 20:44:00 +0000
draft: false
type: posts
categories: 
- [object Object],[object Object],[object Object],[object Object],[object Object]
---
# Win32BruteForceWP

<br/>

<br/>
DrWeb released a [news](http://translate.google.com/translate?sl=ru&tl=en&u=http://news.drweb.com/?i=3811) about this malware in August, they know it as 'Trojan.WPCracker.1'  
And more recently ~ [1e8cd0f0f1702820c870302520bc0176](https://www.virustotal.com/en/file/a821b1c35c9c290b1759d6e8151bf95efdd13168146887f840d3d3f11d94411b/analysis/1386340092/).  
  
This executable communicate with a C&C at dorblu99.net  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhba5AHWCiGu093SXDO_TV8hw0FYYJkFfJONM-_oSZ0Yzdt6rtcUYef_fi-nwFcPKfCZdtWZrovMKWTSr_c56p_0nSP1vu55R-aPUjS8eofwT3w_VYV2LIvp7EAESJmYGYrZf8Dd_E_nO4/s400/06-12-2013+15-33-39.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhba5AHWCiGu093SXDO_TV8hw0FYYJkFfJONM-_oSZ0Yzdt6rtcUYef_fi-nwFcPKfCZdtWZrovMKWTSr_c56p_0nSP1vu55R-aPUjS8eofwT3w_VYV2LIvp7EAESJmYGYrZf8Dd_E_nO4/s1600/06-12-2013+15-33-39.png)

Let's have a closer look.  
  
Login:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjPJnB0Xqw7pPO-LRnfGxA2Ahe1WCWkFld1iS4HsyKOj_WWWNqQ2ou-j7JQ6sZJEZ-fFtCwVAq4qzVWLnXKeurvtyv1EXwXggxoAkM_5Kx2aUecyyTsnaYJQJjiE-Xms067mrxjZDiVPSk/s1600/29-11-2013+10-20-43.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjPJnB0Xqw7pPO-LRnfGxA2Ahe1WCWkFld1iS4HsyKOj_WWWNqQ2ou-j7JQ6sZJEZ-fFtCwVAq4qzVWLnXKeurvtyv1EXwXggxoAkM_5Kx2aUecyyTsnaYJQJjiE-Xms067mrxjZDiVPSk/s1600/29-11-2013+10-20-43.png)

  
Main:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj_lEdzFDmgzoz8pzbFPSTtGDNqHh82WE0mfKqEqKmJCbjF0Fy_JN8GqpVSSs4sZ8GmYIHgXNWlM3pudxaJbmzORhIxYx36YVG4y3s5J2SnhM6Yi52WWCFgsd4MRoiF_krLbG0Iav7ipCU/s400/29-11-2013+10-23-31.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj_lEdzFDmgzoz8pzbFPSTtGDNqHh82WE0mfKqEqKmJCbjF0Fy_JN8GqpVSSs4sZ8GmYIHgXNWlM3pudxaJbmzORhIxYx36YVG4y3s5J2SnhM6Yi52WWCFgsd4MRoiF_krLbG0Iav7ipCU/s1600/29-11-2013+10-23-31.png)

  
Bot info:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjxsZoKbtZ58S8gTFaVsicO0DZ1InXdP0So51gPbLQ96H28IsUs1hkEXZKO9zGh959tPJNaQtTIw7UA2Cfk0_jv8xV2mg9LiYiUgRDfXMd6yRSvzjGbzIlnpSSbhkRhE2UMu95X9rESJ-w/s400/29-11-2013+10-52-04.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjxsZoKbtZ58S8gTFaVsicO0DZ1InXdP0So51gPbLQ96H28IsUs1hkEXZKO9zGh959tPJNaQtTIw7UA2Cfk0_jv8xV2mg9LiYiUgRDfXMd6yRSvzjGbzIlnpSSbhkRhE2UMu95X9rESJ-w/s1600/29-11-2013+10-52-04.png)

  
Broken wordpress:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjvRdSzz81Iyp3hlBtRkTSOVOzeYEAQHfkN65a_WoMTAcMavsHmUOi-ANyDsB3ad_0Acn5QSD3jkodgxpzeVFwtONHUwrp_g7PpsDIjcCYpU6OWStwFKXyBkQ-cvm0LS_JAGXY69qXB5bk/s400/29-11-2013+10-46-23.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjvRdSzz81Iyp3hlBtRkTSOVOzeYEAQHfkN65a_WoMTAcMavsHmUOi-ANyDsB3ad_0Acn5QSD3jkodgxpzeVFwtONHUwrp_g7PpsDIjcCYpU6OWStwFKXyBkQ-cvm0LS_JAGXY69qXB5bk/s1600/29-11-2013+10-46-23.png)

  
Statistics:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgVidi5veGZJZeSVLVLh2CfSkqW4pBZbEqxsYYkJFcCtQ14__jcfaO9UesntYYq5HKbQlwiU_V0rXJxLc2pOG01ZVR2YVSRTJvNxBpZW0VEoY0k3t6T4KAGNnqciFWE4NV0P8x3Xo6u9I0/s400/29-11-2013+10-33-41.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgVidi5veGZJZeSVLVLh2CfSkqW4pBZbEqxsYYkJFcCtQ14__jcfaO9UesntYYq5HKbQlwiU_V0rXJxLc2pOG01ZVR2YVSRTJvNxBpZW0VEoY0k3t6T4KAGNnqciFWE4NV0P8x3Xo6u9I0/s1600/29-11-2013+10-33-41.png)

  
Add domains:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiEm13Udez7ORKpxEOa94uCUrByigjJCt7YX2d1DtLni1bESkjA9xe8sEjJtQ2qIslSqLT88rnK-zSH9-bGXZOR64hF3P9ogVqFPCuD5iK6i1yq9le_MHCmQxIMUt0UgqIEqqd7ZUYTbUY/s400/29-11-2013+10-29-50.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiEm13Udez7ORKpxEOa94uCUrByigjJCt7YX2d1DtLni1bESkjA9xe8sEjJtQ2qIslSqLT88rnK-zSH9-bGXZOR64hF3P9ogVqFPCuD5iK6i1yq9le_MHCmQxIMUt0UgqIEqqd7ZUYTbUY/s1600/29-11-2013+10-29-50.png)

  
Add admin panels:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgl1iqpkeH_WqNBbPdNmd4mU9y6fcZvgGZAIXyToZC9BD5TGzicwQaENMG5qlNExWKIZFat_v11X8GLuCGOH8ARMdty66pGIeKjpqXo0wZd39k_THMO3EV_vVy1eCHcRA1sN3b147ZT9gs/s400/29-11-2013+10-38-33.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgl1iqpkeH_WqNBbPdNmd4mU9y6fcZvgGZAIXyToZC9BD5TGzicwQaENMG5qlNExWKIZFat_v11X8GLuCGOH8ARMdty66pGIeKjpqXo0wZd39k_THMO3EV_vVy1eCHcRA1sN3b147ZT9gs/s1600/29-11-2013+10-38-33.png)

  
Add logins:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi0vaTJWUH5NEQkpYNOQ6YEpyWXSAGfZ8GBDPe9G9pFjPpVFKSSalbIjf4-17Fn2W-zgelOpFzNb6nwevojoiVF5Vhxz5dUrc8NzqW6W75qwvACiwQrRqkpG2_7c5WfqfII0BRgU3UWIio/s400/29-11-2013+10-39-46.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi0vaTJWUH5NEQkpYNOQ6YEpyWXSAGfZ8GBDPe9G9pFjPpVFKSSalbIjf4-17Fn2W-zgelOpFzNb6nwevojoiVF5Vhxz5dUrc8NzqW6W75qwvACiwQrRqkpG2_7c5WfqfII0BRgU3UWIio/s1600/29-11-2013+10-39-46.png)

  
Add passwords:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh88LiRBpS6M3cjg5VUF_8mQEqMxSeY1vjvicYUk7dyoGRe0eNGq7o4fbCMjt5ywzAFzb6cTRAMULflcNcB6OtFM1iACMN4niwa13FzCMQ7P6LEZWrUPMXrZMYP15KVndZUJOnCkLekBIc/s400/29-11-2013+10-40-30.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh88LiRBpS6M3cjg5VUF_8mQEqMxSeY1vjvicYUk7dyoGRe0eNGq7o4fbCMjt5ywzAFzb6cTRAMULflcNcB6OtFM1iACMN4niwa13FzCMQ7P6LEZWrUPMXrZMYP15KVndZUJOnCkLekBIc/s1600/29-11-2013+10-40-30.png)

  
Add module for jm(zip):  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjcg9M9AyScL1cRIRRrF3PkO46UvyXziYTMT0vaaPPsf-U_Wb6VUIHoNwN5NY5gFUWJqxWL2qi0f2LzhzqffDNW7p_OvcwUcD6jxTpA3zmWLhago_ks8UT6XwwiLnCKdVFFKXqXUAfTY0I/s400/29-11-2013+10-43-49.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjcg9M9AyScL1cRIRRrF3PkO46UvyXziYTMT0vaaPPsf-U_Wb6VUIHoNwN5NY5gFUWJqxWL2qi0f2LzhzqffDNW7p_OvcwUcD6jxTpA3zmWLhago_ks8UT6XwwiLnCKdVFFKXqXUAfTY0I/s1600/29-11-2013+10-43-49.png)

  
Add module for wp(zip):  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjc7MNzu7ZN6e7n8J3UQvGTWAlrWF_mMIstFyMJjva5vFeyJolgTMIbLcPnsMWWdmU77KQ2cnjc8cED632lodHBW01uOMakha5FavNx-ONCAnUao8tC5XhQ59hm-llp2rAvB9St4ZgZ7uw/s400/29-11-2013+10-44-35.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjc7MNzu7ZN6e7n8J3UQvGTWAlrWF_mMIstFyMJjva5vFeyJolgTMIbLcPnsMWWdmU77KQ2cnjc8cED632lodHBW01uOMakha5FavNx-ONCAnUao8tC5XhQ59hm-llp2rAvB9St4ZgZ7uw/s1600/29-11-2013+10-44-35.png)

  
Add shell jm(php):  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh0pkN8sbnAuz-C8JCH1_0GIwGaAasoPlnyUZ1DmsAiHazi9mRSqm0X9nlooe_VrXBewi2ubU0X2-rXNMOZaoq_plhqc3rNyJGYclO741iOilY7mq7tAulZysOg0cBS4JjrVS2BM2qry7A/s400/29-11-2013+10-45-21.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh0pkN8sbnAuz-C8JCH1_0GIwGaAasoPlnyUZ1DmsAiHazi9mRSqm0X9nlooe_VrXBewi2ubU0X2-rXNMOZaoq_plhqc3rNyJGYclO741iOilY7mq7tAulZysOg0cBS4JjrVS2BM2qry7A/s1600/29-11-2013+10-45-21.png)

  
Cron brute:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhD9O_7oT4XiBVNKUe2DpSXgseTqoSZASpN-MHpSKeZFMCdwoCcDKtgw1FNXparWBSqOIsiA2OkC4m4BOZQdOuG7Ut-DkYpq60QurbsucGUBd8Il8EhZQm_yivc4m0OD1BwOMYJXF8cZF0/s1600/29-11-2013+10-31-31.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhD9O_7oT4XiBVNKUe2DpSXgseTqoSZASpN-MHpSKeZFMCdwoCcDKtgw1FNXparWBSqOIsiA2OkC4m4BOZQdOuG7Ut-DkYpq60QurbsucGUBd8Il8EhZQm_yivc4m0OD1BwOMYJXF8cZF0/s1600/29-11-2013+10-31-31.png)

  
Ban list:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhheHjdF0PmMVqgD8wyboHt0XFFPXlC485cmpyclOMK7Cqzkygy7evsPZ7cFAVH4QEqOJFH8FNBNipX4lzBf0nyLbBSqTPKEGkvL3GCW-lKxFunFTEzT6l35cGJwVjIGIlWGARZ7FqxY8k/s1600/29-11-2013+10-59-22.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhheHjdF0PmMVqgD8wyboHt0XFFPXlC485cmpyclOMK7Cqzkygy7evsPZ7cFAVH4QEqOJFH8FNBNipX4lzBf0nyLbBSqTPKEGkvL3GCW-lKxFunFTEzT6l35cGJwVjIGIlWGARZ7FqxY8k/s1600/29-11-2013+10-59-22.png)

  
Logs:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEje9VrxYBXe6mA2RxLa2GALJkBegFaBnjLco-IzJL45AdkVm2OMLhrDxfijQvfGD1BKcmRMSnqY_EPEOtfMwYWV_0ve1zTmVjUSskHx3RNjKPCktPBQGfOVAwm4s0jm0QABhLnkfVWqqdU/s400/29-11-2013+10-59-14.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEje9VrxYBXe6mA2RxLa2GALJkBegFaBnjLco-IzJL45AdkVm2OMLhrDxfijQvfGD1BKcmRMSnqY_EPEOtfMwYWV_0ve1zTmVjUSskHx3RNjKPCktPBQGfOVAwm4s0jm0QABhLnkfVWqqdU/s1600/29-11-2013+10-59-14.png)

  
Domains list (downloaded by the malware to know wich wordpress he should brute force):  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg91z1azin6oHbP0r6LPmGW2Jkb2qWGhyCd7TPopjG450LsUGrTVX6PrzhEST2Gxz-QcicdZeOHwNfO88djFfUt-StfYBDVjlzs9oZOCm8_oAvRnXDJ5WEfUFVr-GLFu0xpfUrw356MvLE/s400/29-11-2013+12-06-51.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg91z1azin6oHbP0r6LPmGW2Jkb2qWGhyCd7TPopjG450LsUGrTVX6PrzhEST2Gxz-QcicdZeOHwNfO88djFfUt-StfYBDVjlzs9oZOCm8_oAvRnXDJ5WEfUFVr-GLFu0xpfUrw356MvLE/s1600/29-11-2013+12-06-51.png)

36k urls.  
  
Roman of abuse.ch have also wrote an [interesting post](http://www.abuse.ch/?p=5813) about this threat.

#### [Source](https://www.xylibox.com/2013/12/win32bruteforcewp.html)

<br/>
---
