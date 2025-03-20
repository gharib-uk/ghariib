---
title: "UI Redressing against Facebook"
date: 2013-03-19T12:52:00.000-07:00
draft: false
type: posts
categories: 
- 0day,daath,facebook,ui redressing
---
# UI Redressing against Facebook

<br/>

<br/>
In this post, I'm going to discuss a possible attack scenario, targeting the Facebook web application, that could lead to the reset of account passwords in an _automated_ fashion exploiting a UI Redressing issue with the use of a [cross-domain](http://blog.nibblesec.org/2012/12/ui-redressing-mayhem-firefox-0day-and.html) extraction technique.  
  

### UI Redressing bug, again

During my research, I discovered a Facebook's web resource that is not protected by the X-Frame-Options and that includes the **fb\_dtsg** token, which is adopted as an anti-CSRF token (Figure 1). The following is the affected URL:  

-   **http://www.facebook.com/ajax/intl/language\_dialog.php**

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiEe1eQ6TV614oW7JIui50hWIVw5f5QDDDoX8t_rCBFzUeND5w6sQNPDArK3JiAqGyeiEc3wKXoukF45kZm_eJHooiVZSN79c91bikLKSlBayHJ-5fyGxxV1rZNEEBeWc0U7mszr0YPpsW5/s640/xfo.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiEe1eQ6TV614oW7JIui50hWIVw5f5QDDDoX8t_rCBFzUeND5w6sQNPDArK3JiAqGyeiEc3wKXoukF45kZm_eJHooiVZSN79c91bikLKSlBayHJ-5fyGxxV1rZNEEBeWc0U7mszr0YPpsW5/s1600/xfo.png)

Figure 1 - Facebook's web resource vulnerable to UI Redressing attacks.

The iframe-to-iframe extraction method can be applied here to extract fb\_dtsg's value and, consequently, perform a series of [Cross-Site Request Forgery](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_%28CSRF%29) attacks against the integrity of the victim's profile data.  
  
  

### The theory behind the Facebook profiles takeover

Facebook allows users to add a mobile number that, once certified, can be adopted as username in order to login or reset the account's password. Users can insert their mobile numbers via the **Account Settings → Mobile → Add a phone → add your phone number** options (Figure 2 and Figure 3): a confirmation code is therefore sent by Facebook's system to the user's mobile phone and it must be inserted (Figure 4) to complete the activation process.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEivzb-2EhspZZ4SAozoso5bRowQ-qVMe9TR1BeNHBr5BTu5bPAdnt0sI8usw35u1YuXSFNlaeaqAfzvticiRWuoveWaDK95vo10sVefVgeh9FopwSisEXaMNNAow4iiWhBvid4rCWBywHwA/s640/1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEivzb-2EhspZZ4SAozoso5bRowQ-qVMe9TR1BeNHBr5BTu5bPAdnt0sI8usw35u1YuXSFNlaeaqAfzvticiRWuoveWaDK95vo10sVefVgeh9FopwSisEXaMNNAow4iiWhBvid4rCWBywHwA/s1600/1.png)

Figure 2 - Users can add their mobile number via the "add your phone number here" link.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiAeLLxo2zhk1d4v7KCk-4RNhOHxZP_VcrJKIkIt7TGKdXuBTV2tUK7-uAjDEynhz7zcTcLwTnQTYCeC6g61C4W5fsqH2uIenJxQWimIxyKdWIFPtDJqL-mABinsVZg-RBw8t9VUzAhCHDN/s640/2.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiAeLLxo2zhk1d4v7KCk-4RNhOHxZP_VcrJKIkIt7TGKdXuBTV2tUK7-uAjDEynhz7zcTcLwTnQTYCeC6g61C4W5fsqH2uIenJxQWimIxyKdWIFPtDJqL-mABinsVZg-RBw8t9VUzAhCHDN/s1600/2.png)

Figure 3 - Facebook's form used to add a mobile number.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEitxfhsnVhiIxL9bMHLkYzsCCOQcjjv0_4s9-nw_O2JMkXkuo1uiJISgaqSVislEkenN6AXx4yGRGtFIz1MmnbMcIJvD0quJSMXpdc1ANhJsdFelzZuTyTwHmvj-ZevnWt-9R0He8Zvr72K/s640/3.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEitxfhsnVhiIxL9bMHLkYzsCCOQcjjv0_4s9-nw_O2JMkXkuo1uiJISgaqSVislEkenN6AXx4yGRGtFIz1MmnbMcIJvD0quJSMXpdc1ANhJsdFelzZuTyTwHmvj-ZevnWt-9R0He8Zvr72K/s1600/3.png)

Figure 4 - A confirmation code is sent to the user's mobile and must be entered to complete the process.

  
The main issue here is that no _password_ is required to associate the mobile number to the user's profile. Because of this, an attacker may abuse the described UI Redressing vulnerability to steal the **fb\_dtsg** token and register an arbitrary phone number. Despite this, the attacker still needs to insert the confirmation code in order to associate his mobile number. A bit of black magic helps here: the attacker can abuse an _SMS to mail_ mobile application to _automatically_ forward the Facebook text-message (SMS) to an attacker-controlled mail box, thus allowing an hypothetical exploit to fetch the code and complete the insertion process.  
  

### The exploit

A working Proof of Concept exploit has been developed in order to demonstrate the described attack. We have also shared the code with the Facebook security team. During my experiments, the Android application [SMS2Mail](https://play.google.com/store/apps/details?id=com.frogggias.sms2mail) has been adopted to forward the Facebook SMS (Figure 5) to the mail box (Figure 6).  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiJV4rmoZxTLce6dlDqdmAC5I_cGCrdK6UTNhS9qT2n84TNfeCXxv4gkxfpIWZDiN3pgml6etsBH0f08Wg6ftnt7RAm2fiMaKH2Fc0HzUpnslH0zl-zNcBxVL_UDqPBTgXE0GqIVZadmaYJ/s400/phone.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiJV4rmoZxTLce6dlDqdmAC5I_cGCrdK6UTNhS9qT2n84TNfeCXxv4gkxfpIWZDiN3pgml6etsBH0f08Wg6ftnt7RAm2fiMaKH2Fc0HzUpnslH0zl-zNcBxVL_UDqPBTgXE0GqIVZadmaYJ/s1600/phone.png)

Figure 5 - SMS with the Facebook's confirmation code that has been forwarded to the attacker's mail box.

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg_mv23b2ivAzgMoA4-T-a4OE04pfwmjhgmoWjr0LLRwKHtwuIRbeevAnfpl5_9bepcEbCWvfvwNoPWzXQFlVKCHIR82Lkf7MxCEA-mW8mefBCgmfJkRX6-ObbDBkAOixg_8vhFfMrWrWbL/s640/forward.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg_mv23b2ivAzgMoA4-T-a4OE04pfwmjhgmoWjr0LLRwKHtwuIRbeevAnfpl5_9bepcEbCWvfvwNoPWzXQFlVKCHIR82Lkf7MxCEA-mW8mefBCgmfJkRX6-ObbDBkAOixg_8vhFfMrWrWbL/s1600/forward.png)

Figure 6 - Facebook confirmation code forwarded to the attacker's mailbox.

  
The following steps summarize the exploitation phases:

1.  The exploit frames the vulnerable resource and allows the victim to play a fake game while performing the cross-domain content extraction;
2.  The **fb\_dtsg** anti-CSRF token and the victim's user id are extracted. An HTTP request is forwarded to the Facebook application in order to _emulate_ the attacker-controlled mobile number registration;
3.  An text-message (SMS), containing the confirmation code, is sent to the attacker mobile device. An SMS2Mail mobile application is installed on attacker's device and automatically forwards the SMS to an attacker-controlled mail box;
4.  The exploit waits for the SMS to be forwarded to the mail box, then extracts the confirmation code and performs a second CSRF attack in order to submit the code itself and complete the mobile number registration.

  
The attacker's mobile number is now associated with the victim's profile and can be used to reset the account's password. As a matter of fact, Facebook allows users to enter a previously associated mobile number (Figure 7) which is then used to send a reset code (Figure 8).  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh0Dch-nLhEcXQmh54MJxreefWmv3fxOe7YRJ0OQ2bigudMQMQsf2fCjryaOK8qhZWoswU1y_aPjZw8HlrH83O6Dm0tpe854Zt2JMZYv4QVqLm0NR7DN-YGqxFESTzcy3X8fT57bx-tZuDg/s1600/reset1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh0Dch-nLhEcXQmh54MJxreefWmv3fxOe7YRJ0OQ2bigudMQMQsf2fCjryaOK8qhZWoswU1y_aPjZw8HlrH83O6Dm0tpe854Zt2JMZYv4QVqLm0NR7DN-YGqxFESTzcy3X8fT57bx-tZuDg/s1600/reset1.png)

Figure 7 - Reset password mechanism involving the user's mobile number .

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiwj3Z_iS2A9NSjkc7wFcTid3v5hzISt9B8hTE3IcjwPKceFR9aFrfPmApOMSeytmnsDVrLkmr0SIsZ-9zeZ7OokQHNmG3rvZB78IQFW0CMTqeFgFn5TqbfbnteHqLvh1kaKtHZFJZEY9zn/s1600/reset2.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiwj3Z_iS2A9NSjkc7wFcTid3v5hzISt9B8hTE3IcjwPKceFR9aFrfPmApOMSeytmnsDVrLkmr0SIsZ-9zeZ7OokQHNmG3rvZB78IQFW0CMTqeFgFn5TqbfbnteHqLvh1kaKtHZFJZEY9zn/s1600/reset2.png)

Figure 8 - Facebook's form used to insert the resetting code.

A fully automated Proof of Concept exploit can be downloaded [here](https://github.com/daath1/nibblesec/tree/master/ui_redressing_mayhem/facebook), while the following video illustrates the described attack:

#### [Source](http://blog.nibblesec.org/feeds/7222580902571250063/comments/default)

<br/>
---
