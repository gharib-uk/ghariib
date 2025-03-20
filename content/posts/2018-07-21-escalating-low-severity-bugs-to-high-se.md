---
title: "Escalating Low Severity Bugs To High Severity"
date: Sat, 21 Jul 2018 00:53:00 +0000
draft: false
type: posts
categories: 
- 
---
# Escalating Low Severity Bugs To High Severity

<br/>

<br/>
REDIRECTING TO THE NEW BLOG ...
===============================

setTimeout(()=> location="https://blog.noob.ninja/escalating-low-severity-bugs-to-high-severity/",3000);

  

This time I am gonna share about some ways that I have learned & applied while participating in bounty programs and was able to escalate Low severity issues to higher severity. Let's Go To the Technical details straight:  
  
**Note**:  
You might also be able to use Window Object instead of Iframe in the following Cases I mention but it's better to use "Iframe" instead of "Window" to be stealthier and have least User-Interaction though it requires Clickjacking to be present too.  
  
**Case #1. Self Stored-XSS and Login-Logout CSRF:**  
  
**Pre-Requisites:**  
1.) Victim must be loggedIn on the Application  
2.) Some kind of sensitive information of the currently authenticated user should be present on some page(via Web API etc.)  
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiFCJE64FTzMIGdE7w-qQbbHyvVCipHJS5FlvE-glU-Bd5kDefO_PKwh3mdsik7nbwypHKtbPazP-XrntJ3uOhR3V5JSyxvztf-8cOGkyhoqGAHHJZGp80oD5WVfZJ-OQjVRjI3FtTDhTbu/s1600/Screenshot_330.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiFCJE64FTzMIGdE7w-qQbbHyvVCipHJS5FlvE-glU-Bd5kDefO_PKwh3mdsik7nbwypHKtbPazP-XrntJ3uOhR3V5JSyxvztf-8cOGkyhoqGAHHJZGp80oD5WVfZJ-OQjVRjI3FtTDhTbu/s1600/Screenshot_330.png)  
**ATTACKER** Having Self-Stored XSS in Profile Description:  
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgaJx5XM6xlT9ewIKwjyW8loy_QN23pLeZrQ0iTp9pqn-gmsPv5-8o8HGBr7mhPYC4LQSEN99G5UVTnODS0Kk28tLTJI7htEzseIdbYEDSxJP2pamlEfWnrwHon6a8EIxW3-C8xwh16hpL9/s1600/Screenshot_329.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgaJx5XM6xlT9ewIKwjyW8loy_QN23pLeZrQ0iTp9pqn-gmsPv5-8o8HGBr7mhPYC4LQSEN99G5UVTnODS0Kk28tLTJI7htEzseIdbYEDSxJP2pamlEfWnrwHon6a8EIxW3-C8xwh16hpL9/s1600/Screenshot_329.png)  
**Attack Summary:-**  
1\. Victim Visits Attacker's Page  
2\. Create 2 Iframes  
   **Frame #1(VICTIM)** pointing to the sensitive info page (eg. CreditCards, API Keys, Secrets, password hashes, messages etc. which is only visible to the authenticated user)  
  
   **Frame #2(ATTACKER)** pointing to Self-Stored XSS page  
  
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgnVLjZeVez4bxBc00qhEuF89X_GfKGNFvZGjvGUoFG6O2DxqtUDQcR4mPfJweq573eOQhJljYNc-Xt8rUpPBZ8ZMJXcRN1XnzO3wfMIOYUeCEV0R7pq_ST7iJNhhcx6iliDJtBmVWZT4IN/s1600/Screenshot_331.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgnVLjZeVez4bxBc00qhEuF89X_GfKGNFvZGjvGUoFG6O2DxqtUDQcR4mPfJweq573eOQhJljYNc-Xt8rUpPBZ8ZMJXcRN1XnzO3wfMIOYUeCEV0R7pq_ST7iJNhhcx6iliDJtBmVWZT4IN/s1600/Screenshot_331.png)  
  
3\. Perform the following on the Attacker Page:  
Once the **Frame #1** is loaded completely  
     a) **Logout** from Victim's account  
     b) **Login to Attacker's**/your Account using the login CSRF  
  
In the **Frame #2**  
     c) **Execute the Self-Stored XSS** in your(attacker's) and **Access the Frame #1 using** **top.frames\[0\].document.body.outerHTML** since the Same Origin and steal it and send that info to your server  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjAHH7ueoHJr-46uLlLWLxlsLE16IAAFNmEZuKA27ttUvM1g2cWSIvszAh-RbtCO31sWu8YqV0wMX9ajf1s2okS4pa7nXk43pIDS_mklYwV1K45vXNgozrw5L7uGJFaM8x5gm4OqEu9C4pN/s1600/Screenshot_333.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjAHH7ueoHJr-46uLlLWLxlsLE16IAAFNmEZuKA27ttUvM1g2cWSIvszAh-RbtCO31sWu8YqV0wMX9ajf1s2okS4pa7nXk43pIDS_mklYwV1K45vXNgozrw5L7uGJFaM8x5gm4OqEu9C4pN/s1600/Screenshot_333.png)

  
  
  
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj-0o3CeWBKPchqWFH08gSfNds3ZsdXDnAgaaq_WuU45PIBbuJuO765zBFikbQ5nTc9UQ0tq5jtSYDE4szcEfRlAHkDN5Hc-6bg2Blonydz6ai2NF8vt9sUcHeXe_T1FVnxI2mCVUVYfmEH/s1600/Screenshot_332.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj-0o3CeWBKPchqWFH08gSfNds3ZsdXDnAgaaq_WuU45PIBbuJuO765zBFikbQ5nTc9UQ0tq5jtSYDE4szcEfRlAHkDN5Hc-6bg2Blonydz6ai2NF8vt9sUcHeXe_T1FVnxI2mCVUVYfmEH/s1600/Screenshot_332.png)  
**Reference**:  
[https://www.youtube.com/watch?v=l3yThCIF7e4](https://www.youtube.com/watch?v=l3yThCIF7e4) by Mathias Karlsson  
  
**Case #2. Self-Stored XSS, Login, Logout CSRF + OAuth Social login CSRF**   
  
**Pre-Requisites:**  
1.) The victim must already be Logged In the OAuth Authorization Server(eg. Facebook..etc)  
2.) no CSRF on /oauth login endpoint(not 'state', Explained it Below)  
3.) Clickjacking  
  
**Some Basics**:  
Now Sometimes the Social Login endpoints would be like **/oauth/facebook** or **/oauth/twitter** etc. which on visiting redirects to the Authorization Server(Facebook/Twitter) along with state CSRF parameter(not necessary but say they are doing it right) and then the regular OAuth Flow goes on so:  
  
1\. A user has to be first authenticated on OAuth Authorization Server else they would be prompted to authenticate. Once authenticated,  
  
2\. If they don't have already connected the Client Application, then they have to Allow the application with the required scopes/permissions  
  
3\. otherwise if its already connected they wouldn't be prompted for anything and redirected back with **authorization code**/**access token** and get authenticated on the Application ie. no user interaction and password-less.  
  
so it itself becomes a login CSRF, **<img src=http://webapp.com/oauth/facebook/connect>** for an example, will make you logged in to the application if you are already logged into Facebook and has connected the application previously once.  
  
**Attack Summary:**  
1\. Victim visits Attacker Page:  
        a.) **Logout** from Victim's Account using Logout CSRF  
b.) **Login to Attacker's Account** using Login CSRF  

  

2\. On **Attacker's** Page create 2 iframes :  
  
**Frame #1:**  
a.) Navigate the Iframe to the **Self-Stored XSS** Page  
  
After **loading of Iframe #1 Create the Iframe #2**:  
a.) Logout from the **Attacker's Account**  
b.) Login back to Victim's account using Social Logins via OAuth endpoint  

  

For the **Stored-XSS** part, set a timeout before executing any javascript to extract data/steal cookies/do CSRF etc. till the **Frame #2** is completely loaded and then you could impersonate the victim's account using the javascript loaded in **Frame #1** since the Same Origin and we can perform CSRF by stealing CSRF Tokens, Steal other sensitive info or whatever you want in your XSS payload.  
  
An example what I did was, [Steal CSRF Token and then use it to update the Victim's emai](http://bugdisclose.blogspot.com/2016/09/rahul-maini-xss-csrf-bhoom.html)l/password. This case provides much more exploitation than 1st Case.  
  
**References:**  
**[https://whitton.io/articles/uber-turning-self-xss-into-good-xss/](https://whitton.io/articles/uber-turning-self-xss-into-good-xss/)**  
**[https://www.geekboy.ninja/blog/airbnb-bug-bounty-turning-self-xss-into-good-xss-2/](https://www.geekboy.ninja/blog/airbnb-bug-bounty-turning-self-xss-into-good-xss-2/)**  
  
**Case #3 Login, Logout CSRF +  OAuth Login CSRF (Application Specific)**  
  
**Pre-Requistes:**  
1\. Victim must be logged into his Social Account  
2\. Victim must have connected/logged with his Social Account into the Web App. once  
3\. Oauth Social Login CSRF  
  
I found this sometime back. It was a special case as It solely depended on the Web Application that I came across. There was a Oauth connect endpoint which was vulnerable to CSRF.  
  
**Attack Summary:**  
1\. Victim Visits Attacker's Page  
2\. We make a request stealthily to log out Victim from his account using Logout CSRF <img src>  
3\. Next, Login Victim to Our Attacker Account (Using Login CSRF) by using xhr/form.  
4\. Make a get request to Oauth login endpoint https://api.webapp/network/facebook/oauth, so that will connect **Victim's Social Account to My(Attacker's) account on that Web App as we are logged in as Attacker from Step 3**  
**Note:** Even that Social Account was already connected with Victim's Account I was still able to link it. This was something that shouldn't be allowed but was possible on this Application.  
  
4\. Now when I login to my(Attacker's) account I had my account linked with Victim's Social Account and According to the Application's API docs I found that we could write posts/share/see friends/followers etc on Social Media(Facebook etc) on Victim's behalf etc. without requiring to login to Victim's Facebook account again. The Catch was that I guess the Access Token was stored Offline and I was able to perform actions on behalf of Victim's Social Account via Application's API utilizing the stored access token.  
**Case #4 Social Login Connect CSRF due to Leaked state parameter**  
This was an another scenario, I got in a recent program. I found there was a JSONP endpoint where the callback function was containing "state" parameter in a Javascript Object. so all I had to do is to steal the "state" parameter by creating a function to steal "state" and loading that JSONP Callback via script src.  
  
JSONP Endpoint:  
https://www2.site.com/api/webstore?callback=jQuery123  
  
Response:  
**jQuery123**({"**state**":"xxxx",....})  
  
Now, All I had to do is:  
1\. Login to Attacker's Account in the Application  
2\. Connect it to my(attacker's)Social Login and Intercept Request  
3\. Now Copy the "Authorization Code" you get in the Last Step of Oauth Social Connect  
4\. Drop that request and use that **unused** authorization code in the following similar request  
  
**Exploit POC:**  
  

<script\>
/\*
A Function jQuery123 that takes the Javascript Object as input "x" and extracts the "state" property's value from that object and further use that along with the unused Authorization Code from Step #4 to make request connecting the Attacker's Social Account to Victim's
\*/
function jQuery123(x){

document.write('<img src="https://connect.site.com/connect/social/auth/logged?code=','{{ATTACKER AUTHORIZATION CODE HERE}}','&state=',encodeURIComponent(x.state),'">');
}

</script>

<!--
Now when the following request is made inside script src it tries to execute the response of this JSONP endpoint as javascript and thus calling jQuery123({"state":"xxxx",....}) and thus our own created function gets called and that's how exploit works
\-->
<script src\=https://www2.site.com/api/webstore?callback=jQuery123></script>

  
  
5\. Now, the Attacker's Social Account is linked with Victim's Application Account.  
  
That's all for this post :D hoping to write more on this if I get good response from the community on this.  
  
\- Rahul Maini

#### [Source](https://www.noob.ninja/2018/07/escalating-low-severity-bugs-to-high.html)

<br/>
---
