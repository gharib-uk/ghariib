---
title: "UI Redressing Mayhem Identification Attacks and UI Redressing on Google Chrome"
date: 2012-12-31T13:06:00.000-08:00
draft: false
type: posts
categories: 
- 0day,amazon,chrome,daath,google,microsoft,ui redressing,yahoo
---
# UI Redressing Mayhem Identification Attacks and UI Redressing on Google Chrome

<br/>

<br/>
Today I'm going to disclose a series of UI Redressing issues that could be exploited in order to extract information that may help an attacker to _identify_ a victim-user whenever anonymity is a critical requirement. Moreover, a new extraction method, which has been successfully applied against Google Chrome, will be presented. Google's web browser disallows cross-origin drag&drop and what I'm introducing here is a completely different approach to achieve the extraction of potentially sensitive data.  
  

### Identification Attacks

  
I found that several world-renowned web applications lack protection of web resources from UI Redressing attacks, thus revealing data that can be abused to disclose a user's identity. An _identification attack_ could be successfully performed by exploiting a UI Redressing flaw affecting web resources that include, for example, the name or the e-mail address of the victim.  
  
A series of vulnerabilities are presented below in order to exemplify some of these attacks. The first issue affects a Google's web application: an authenticated Google user can be attacked by abusing a UI Redressing vulnerability related to the **support.google.com** domain. As shown in Figure 1, no X-Frame-Options header is adopted, thus allowing the cross-domain extraction of personal data such as:

-   Victim's e-mail address;
-   Victim's first and last name;
-   Victim's profile picture URL.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhc-maG9MQhQgsrrr1rkBcPk11fxkHsGq-Vl6oHUPS-KzAoqYaOU-FCLT15WrJxj1tnju1TfdyrXnd5kiWXs1YrjpdXEpCkevy99lns1KizUca8bk2prSXYTs-7ZmM1yB7nL4N5nUrg5Qrr/s1600/support_google.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhc-maG9MQhQgsrrr1rkBcPk11fxkHsGq-Vl6oHUPS-KzAoqYaOU-FCLT15WrJxj1tnju1TfdyrXnd5kiWXs1YrjpdXEpCkevy99lns1KizUca8bk2prSXYTs-7ZmM1yB7nL4N5nUrg5Qrr/s1600/support_google.png)

Figure 1 - Google Support vulnerable to UI redressing attacks.

  

A Proof of Concept exploit can be downloaded [here](https://github.com/daath1/nibblesec/tree/master/ui_redressing_mayhem/google_support). The following is a video demonstrating the attack:

  
  

  
Similar vulnerabilities have been found on other popular web applications. The following is a list of identified vulnerable web resources, exposing user's data:  
  
**Microsoft Profile** (First name, last name, e-mail address, etc - Figure 2)  

-   **https://profile.live.com/home/Services/?view=manage&productid=connectedtoprofile**

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjWuuo2PNz3tou3NjIdPSytGQmrsv3wgqD9RuaBQqul435R3k1OElh4oo35V1O8TbguSs2uipVKjUegK5QDKu8ChBrHoIfA0I1LvoTE9C5OcgKrRKrkRxn6n36ruKf3NBcDGqfsagMybNNk/s640/microsoft.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjWuuo2PNz3tou3NjIdPSytGQmrsv3wgqD9RuaBQqul435R3k1OElh4oo35V1O8TbguSs2uipVKjUegK5QDKu8ChBrHoIfA0I1LvoTE9C5OcgKrRKrkRxn6n36ruKf3NBcDGqfsagMybNNk/s1600/microsoft.png)

Figure 2 - Microsoft Profile web resource vulnerable to UI Redressing attacks.

  
**Yahoo!** (e-mail address, first name, birth date, sex, etc - Figure 3):  

-   **http://profile.yahoo.com/y/edit/**

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiTpUQGYEfdCJEgA0ThfHQ7LyOTPbVSLYbTa7Z65HKsg9J_k-SxcnPGzf1-YWrBSmZ13_HGHMN6jBy26P_Q37dO_EWVx5Nt6t2tuV7eCpc_0II6Ae-eR7usQWOwn0h-CXQOYaSdZpfltG8q/s1600/yahoo.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiTpUQGYEfdCJEgA0ThfHQ7LyOTPbVSLYbTa7Z65HKsg9J_k-SxcnPGzf1-YWrBSmZ13_HGHMN6jBy26P_Q37dO_EWVx5Nt6t2tuV7eCpc_0II6Ae-eR7usQWOwn0h-CXQOYaSdZpfltG8q/s1600/yahoo.png)

Figure 3 - Yahoo! web resource vulnerable to UI Redressing attacks.

### Beyond the iframe-to-iframe extraction method

  
The Google Chrome web browser seems to have defeated any extraction methods, denying the use of the view-source handler and disallowing _cross-origin_ drag&drop. Despite these adverse conditions, I identified some attack scenarios where a UI Redressing issue could be still performed in order to extract sensitive data. Once again, the method is extremely simple. Instead of a cross-origin drag&drop, the victim is tricked to perform a _same-origin_ action, where the dragged content belongs to a vulnerable web page of the targeted application and the "dropper" is a form (text area, input text field, etc.) located on the same domain. Using a site's functionality that _allow_s publishing externally-facing content, it is still possible to extract information. Under these circumstances, Chrome will not reasonably deny the same-origin drag&drop, thus inducing the victim to involuntary publish sensitive data. As a matter of fact, the attacker is exploiting a subsequent clickjacking vulnerability on the same domain, which causes the publication of the personal information. I refer to this kind of attack chain as a "bridge" that allows the attacker to _move_ sensitive data _from being private to public, while remaining on the same domain_. Then, the attacker can simply access the (now) public information to obtain the extracted data. It should be outlined that the technique requires two vulnerabilities: a web resources that is not protected by the X-Frame-Options (or uses a weak frame-busting code) and a site's functionality that is affected by clickjacking.  
  
The following list summarizes a series of functionalities that could be abused to extract the sensitive data:  

-   Forum's post mechanism;
-   "comment this item" functionalities;
-   Public profile information updating function (or any "update function" that involves public available data - e.g. administrative functions that cause the updating of the web site's content);
-   Messaging functionalities (e.g. from the victim to the attacker);

The proposed method has been successfully applied against Google Chrome version 23.0.1271.97, targeting the Amazon web application. Amazon exposes a series of web resources that include user's data - such as the name, e-mail address, mobile number and "address book" details - that are not protected with both X-Frame-Options header or any frame-busting mechanism. As an example, the following vulnerable URL includes Amazon's user first name, last name and e-mail address:  

-   **https://www.amazon.com/gp/css/account/info/view.html?ie=UTF8&ref\_=ya\_20**

A second issue on the comment function - our "bridge" - can be abused to publish the user's information as a comment for an Amazon item (e.g. a book), previously known by the attacker, and whose comments are "monitored". The following steps summarize the exploitation phases:  

1.  The exploit frames both the vulnerable URL and the comment form of a attacker-chosen Amazon's book;
2.  The victim is triggered to drag his data and drop the information to the framed comment form;
3.  A clickjacking attack is then performed against the "Post" mechanism, in order to publish the dropped data;
4.  At that point the attacker can access all personal details by simply visualizing the submitted comment of the Amazon's item.

The exploit code can be download [here](https://github.com/daath1/nibblesec/tree/master/ui_redressing_mayhem/amazon), while the following is a video of the described attack:

  
  
  
**Update - media coverage:** [http://threatpost.com/chrome-clickjacking-vulnerability-could-expose-user-information-google-amazon-010213/77358](http://threatpost.com/chrome-clickjacking-vulnerability-could-expose-user-information-google-amazon-010213/77358)

#### [Source](http://blog.nibblesec.org/feeds/5723939286487735656/comments/default)

<br/>
---
