---
title: "Lame scareware"
date: Sun, 27 Apr 2014 19:09:00 +0000
draft: false
type: posts
categories: 
- [object Object],[object Object],[object Object]
---
# Lame scareware

<br/>

<br/>
I've found a sample yesterday downloaded via this url: [skyways.co/play.exe](https://www.virustotal.com/en/url/680fdd7152a9019f62634c05c7dd197025c5c190f31198cca1c06fdb7cb46237/analysis/1398623762/), console application, and ugly code + scareware and third party FakeAV call center.  
All the following was so lame that i need to talk about this.  
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg4iJ4cXTxcuhWWBGDL9NLhtqP8OjEQ5dg-7sM6b4J-Qpk4_CM24XL8mkrlRkV8nBtQd0XQxMLMIj00wJ8gNlKNU4a3etJmLdRNRqpgOLudS3QAzyUSbEjMg9_kDUj22cxlpebk3rfpjYE/s1600/27-04-2014+20-53-52.png)  
  
 At first the malware will try to see if he's dropped into %SYSTEMROOT%/system/  
If it's not the case then he will create a file:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjuPWFoUx4UbXBqLLhdyCEkZawI-5vOSWabCocICn9D4cZ9eDK3J1rK_e9WbxO09siNbu7CK8c8FUEOebC9eQAZRSeO8hYl6qxZEb1Os_CVFMs2TFCl1M7QXoHjLRiZeKEB7dM0OJ3X6rk/s1600/27-04-2014+19-33-11.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjuPWFoUx4UbXBqLLhdyCEkZawI-5vOSWabCocICn9D4cZ9eDK3J1rK_e9WbxO09siNbu7CK8c8FUEOebC9eQAZRSeO8hYl6qxZEb1Os_CVFMs2TFCl1M7QXoHjLRiZeKEB7dM0OJ3X6rk/s1600/27-04-2014+19-33-11.png)

  
Then, you think he will write into the new file created but nope, he add a registry persistence, by using the api CreateProcess (oh god, why) instead of using RegCreateKey:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiAQ_ZcR52Dawy_hriWScuwdRH-mEnqkHS44TGum3T4gZVRlpfQBKirKMeEFG8XP_bkqER72cR2uiC_HIY9GtEq0CyCHWLepOlRS7xafCXax-rEUUwB0EuGAdmuHsNlVdT_xxc21n532xc/s1600/27-04-2014+19-45-52.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiAQ_ZcR52Dawy_hriWScuwdRH-mEnqkHS44TGum3T4gZVRlpfQBKirKMeEFG8XP_bkqER72cR2uiC_HIY9GtEq0CyCHWLepOlRS7xafCXax-rEUUwB0EuGAdmuHsNlVdT_xxc21n532xc/s1600/27-04-2014+19-45-52.png)

  
Wrote finally the file:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiCkY0fsgoIXdr6RNt7H9Win4tdBJSZZ8B0dPqardOeV2PCjmZusKfIeIaqV23g9hrLnFIWU9XbdQ52MXO23mDGjoqiim8vQ94dwb8iEp0wTrLlkwydHwarp4NpWcpCJ9I2els7FUQT4VM/s1600/27-04-2014+19-58-05.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiCkY0fsgoIXdr6RNt7H9Win4tdBJSZZ8B0dPqardOeV2PCjmZusKfIeIaqV23g9hrLnFIWU9XbdQ52MXO23mDGjoqiim8vQ94dwb8iEp0wTrLlkwydHwarp4NpWcpCJ9I2els7FUQT4VM/s1600/27-04-2014+19-58-05.png)

  
Wait 5 minutes then display a message box:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjvZ7OxYk4kYFark2nL7ap8-vWyD5M-h4ZakoHjdBCfIQpNkf2ZUZ-7JPppccUPdMI1EfaZDu0HiyMPC9ibe-wNy1drqQ1dmDCXot9cO1vZtUpZSDzaqb8rLgTXOinbT3v0V8XeBoTOEbw/s1600/27-04-2014+20-19-06.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjvZ7OxYk4kYFark2nL7ap8-vWyD5M-h4ZakoHjdBCfIQpNkf2ZUZ-7JPppccUPdMI1EfaZDu0HiyMPC9ibe-wNy1drqQ1dmDCXot9cO1vZtUpZSDzaqb8rLgTXOinbT3v0V8XeBoTOEbw/s1600/27-04-2014+20-19-06.png)

"Your computer's file system has encountered a serious error. Please restart the computer or call support at 1-866-286-6162"  
  
After a reboot, a shutdown procedure is initialized:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEge_pPWXkC8XYooxKjvzvaH_7MV3HLOVrQJvUTAdyF7Kac2pDWQbchyz7zBkQ8aCPu-MfPhUH93_dxedAn7Clgni2e65VUy3o2JVcHdYbk6yXz73uqqNhFLegCYUQwdV81EiPTl8JfU8fA/s1600/27-04-2014+20-32-20.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEge_pPWXkC8XYooxKjvzvaH_7MV3HLOVrQJvUTAdyF7Kac2pDWQbchyz7zBkQ8aCPu-MfPhUH93_dxedAn7Clgni2e65VUy3o2JVcHdYbk6yXz73uqqNhFLegCYUQwdV81EiPTl8JfU8fA/s1600/27-04-2014+20-32-20.png)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiGFjphwJHnXtqeEPjcIlJfYtIGq8qBcjcE82IZ7E4JZTtYWfGd9jXsfDCNp-Pji_gIvTCJI3ihEGJjUmgV6GMl8z-P1f8t63EFDIiYoIu-QyP8JlpgfEmQZCZJWhqVCbm46EXBUfC8by8/s1600/27-04-2014+20-33-33.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiGFjphwJHnXtqeEPjcIlJfYtIGq8qBcjcE82IZ7E4JZTtYWfGd9jXsfDCNp-Pji_gIvTCJI3ihEGJjUmgV6GMl8z-P1f8t63EFDIiYoIu-QyP8JlpgfEmQZCZJWhqVCbm46EXBUfC8by8/s1600/27-04-2014+20-33-33.png)

  
And 5 minutes after, once again the messagebox:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhJIZY7kALRkN9awpuUw-nki-9XRZTrNfeE8mD09iCW1YpgPFiEZ8QBkkH_wcyLPkcLO_BIhgLhEs_IWmBfEqZtxBxsYH6XS3f9jXC5nXE9sk7VII_HX9zeE4JGPC8voFmdW4gPimaQr8A/s1600/27-04-2014+20-38-31.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhJIZY7kALRkN9awpuUw-nki-9XRZTrNfeE8mD09iCW1YpgPFiEZ8QBkkH_wcyLPkcLO_BIhgLhEs_IWmBfEqZtxBxsYH6XS3f9jXC5nXE9sk7VII_HX9zeE4JGPC8voFmdW4gPimaQr8A/s1600/27-04-2014+20-38-31.png)

  
  
I searched the phone number on google and found this:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEguuSuPqK5JDW6kUBfqkQIhHYj8j4ftwprRYtJFqVLDEz0PPeN6XKE2Xihi7FphWMG0Mdn_xctGyI5BgGezjHfvowWKgvGyGOAR3XSwIFabLEHZqkmvrSW42rfRJqH_UKXQewOvOgRayB4/s1600/27-04-2014+20-42-08.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEguuSuPqK5JDW6kUBfqkQIhHYj8j4ftwprRYtJFqVLDEz0PPeN6XKE2Xihi7FphWMG0Mdn_xctGyI5BgGezjHfvowWKgvGyGOAR3XSwIFabLEHZqkmvrSW42rfRJqH_UKXQewOvOgRayB4/s1600/27-04-2014+20-42-08.png)

"Technicion is an independent provider of on-demand tech support and not affiliated with any third party"  
  
ok, what's about the payement page:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgXxMa59UKDJS2h-6fWwCM9UW4CgQdh8I8uvdqgbbE0vM0UHR3twKHaLdCsTyCZu_SbkfwXIdFWq_xgaTD5mj4XC70IS0V6WTlgr7TFzDnX6XM_3P2QLran8lCCjo0MpLzpJ9btMuo5f40/s1600/27-04-2014+20-46-09.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgXxMa59UKDJS2h-6fWwCM9UW4CgQdh8I8uvdqgbbE0vM0UHR3twKHaLdCsTyCZu_SbkfwXIdFWq_xgaTD5mj4XC70IS0V6WTlgr7TFzDnX6XM_3P2QLran8lCCjo0MpLzpJ9btMuo5f40/s1600/27-04-2014+20-46-09.png)

Just 99.99 without any explanation, even the currency symbol is unknown, what a serious site.  
  
And for the story i tried to call 1-866-286-6162 to insult them and tell them how much i hate their ugly code etc.. but there was no available representatives..

#### [Source](https://www.xylibox.com/2014/04/lame-scareware.html)

<br/>
---
