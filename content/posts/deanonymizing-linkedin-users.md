---
title: "Deanonymizing LinkedIn Users"
date: 2025-01-22
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "forensics"
  - "linkedin"
  - "osint"
  - "privacy"
  - "security"
  - "security-awareness"
  - "security-awareness-3-0"
tags: 
  - "email"
  - "leak"
---

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjg7aLzYD2_lxNFbzk2uImf0Id_uOXb45d-z_mkCoZD3wcD3vMy52PtEow16K6BcXZshnhi_5fpJBmwQRxAc-suHOiIMf6ZsxtwpsjNdi0qCv93lGUoszq1uxTUFjr_SpvPcpxgfrGFRSJZ/s320/linkedin_card_v2%255B1%255D.png)

In this blog post, we will look at the privacy issues with some of LinkedIn’s external APIs. We will demonstrate how it is possible, with an email address, to find its associated LinkedIn profile. It is also possible from a LinkedIn profile to do the reverse path and find a person’s email address. To execute this deanonymization attack, documented features, like LinkedIn’s integration with Outlook and YahooMail, are used.

This short article is not exactly about a vulnerability. It is about documenting risks that LinkedIn is aware of. Our goal is to educate users about it. Meanwhile, we are going to go over technical details that are not explicitly described in LinkedIn’s online documentation and terms and conditions.

The impact in a nutshell: Your LinkedIn email and phone number can be found by users beyond your first-degree connections.

## The Feature

The initial feature that caught our eye is the LinkedIn tab presented on Outlook.com. When using the mail client, additional information is displayed about the contact on multiple tabs. One of those information tabs is the LinkedIn profile of the person. The information includes the company name, title, past experiences, location, etc. When seeing this feature, we wondered: how the information about the profile was being found.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj73URd5Yinc12gARRAa6bqc3eUVmlAdTKty76LwvnWYILz0HVYLvpvejfbHyPAqnH4br_0ld3FFc0G8EdCWJHmZaCUTqlC7tUiRxGErW7QCw50DMc3HBV6cy4ixZSqivP9EHbGzWzSzT-0/w640-h581/deanonymizing-linkedin-users_image-1%255B1%255D.png)

  

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjw9pE2P2MmEnUkENixNX9ljzGdxMAYJi8u-8OX174yz0Bf4P6O1x79-wXxuyyujAy2zUSiOu4wKastmaIPZN52M0JDQRJJGy5abA7MrprppBXRqgzYwyfoqmx2_3SxBTin6VRqfn5g-iUm/w592-h640/deanonymizing-linkedin-users_image-2%255B1%255D.png)

  

When looking at HTTP requests, we noticed that data was exchanged over WebSocket specifically when the Outlook detail card was displayed. We could not read the request or response message at first. The messages were encoded in a format that was not easily identifiable.

  

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhKi0cmroTx6bPt_S4e3eIkVbNpj3o_QRIZl5KhOGa12POj1-DWM3EYKs1mMNi140wWjtBfh3Ssn463UbIZ-FxZbAKVWTr90GotFjKbNJ390gOXEfHdukrrhR0AM02VMif4-9iLEqLTm2lP/w640-h184/deanonymizing-linkedin-users_image-3%255B1%255D.png) |
| --- |
| WebSocket request that appears to have been encoded/obfuscated |

  

  

The request is made to the URL https://sfnam.loki.delve.office.com over WebSocket. If we look at the JavaScript code initiating this communication, we can find the code responsible for the encoding format above. The Network Tab in Chrome displays the JavaScript file and source code that initiates the given WebSocket communication.

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg9OdVJFahQ-eOZ4pDQdgcBX4-03lzdO-TIuP2JoJA2cKtj4SNzmikAl7yyj5mYTWV3R_hP3PP-ax6pSnEZrIsFNGTqe5f_bhmCLuirlgwVCeLopOmzDhjky_73UDYwnTPJjEaiDxPR65HP/w640-h278/deanonymizing-linkedin-users_image-4%255B1%255D.png) |
| --- |
| Backtrace leading to the WebSocket Initialization |

  

Looking at the source code highlighted by Chrome, we found the code handling new requests and responses. We can see in the `onMessage` callback handler, presented in the code snippet below, that the message received is converted to a UInt8 array and decompressed using the `inflateRaw` method from the module Pako. Pako is a library that supports the LZip compression algorithm. The `InflateRaw()` function, compared to the `inflate()` function, is using LZip compression without a metadata header that contains filename and modification date.

```javascript
 }, e.prototype.onMessage = function (e) {
            var t = this;
            this.inflateData(e.data, function (n) {
                try {
                    t.setServerPingTimerIfEnabled();
                    var r = JSON.parse(n), o = k()(r.Key, 10);
                    if (-1 === o) return;
                    if (r.Headers = r.Headers && q(r.Headers), o in t.activeRequestsMap) {
                        var i = t.activeRequestsMap[o];
                        i && (clearTimeout(i.timeout), setTimeout(function () {
                            return i.onSuccess(r, e.timeStamp)
                        }, 0)), delete t.activeRequestsMap[o]
                    } else t.logError("WebSocket-onMessage-UnableToFindResponseKey", {Key: o.toString()})
                } catch (e) {
                    t.logError("WebSocket-onMessage-ReceiveFailure", {Exception: e.message})
                }
            })
        }, e.prototype.inflateData = function (e, t) {
            return G(this, void 0, void 0, function () {
                var n, r, o;
                return W(this, function (i) {
                    switch (i.label) {
                        case 0:
                            return this.compressionDisabled ? (t(e), [3, 4]) : [3, 1];
                        case 1:
                            return i.trys.push([1, 3, , 4]), [4, this.getPako()];
                        case 2:
                            return n = i.sent(), r = n.inflateRaw(new Uint8Array(e), {to: "string"}), t(r), [3, 4];
                        case 3:
                            return o = i.sent(), this.logError("WebSocket-inflateData-PakoInflateFailure", {Exception: o}), t(""), [3, 4];
                        case 4:
                            return [2]
                    }
                })
            }) 
 
 
```

Once we know the encoding, we can either hook the JavaScript method where the encoding/decoding occurs, but this can be difficult to maintain if the page refreshes or if the page is doing requests during page initialization. We opted for the creation of a simple ZAP plugin that would decode the messages, both requests and responses. The ZAP Auto-Decode detailed view (see screenshot below) decompresses a message if it matches the LZip or GZip format.

  

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh7N7m3X5K_peV-l1gKQzmyM6pBl6J0XolHTA73PPpkOAYN-FBfcnkjy9gah_Rb-gZwHpm6EMnw6Nb2zV_hWCJdKbp8tkXrmyezhA5dLvxC78d7fhqx0mk4T7Eivo6iqq3bpFuIWr_HnULT/w640-h348/deanonymizing-linkedin-users_image-5%255B1%255D.png) |
| --- |
| WebSocket request intercepted by ZAP and decoded with a custom plugin (early prototype) |

  

There are a few requests made for this LinkedIn endpoint. We can see, in the code snippet below, the request that is responsible for loading a LinkedIn profile in the JSON message with the path `/api/v1/linkedin/profiles/full`. Many requests to **sfnam.loki.delve.office.com** are HTTP requests wrapped in a JSON format. The JSON property Url is analog to the HTTP path, and all headers are under the property Headers. We tested the request directly in HTTP, without WebSocket connection, and the same features were accessible. JSON messages over WebSocket communication is not the only open channel.

```json
{
  "Key": "15",
  "Url": "https://sfnam.loki.delve.office.com/api/v1/linkedin/profiles/full?PersonaDisplayName=Peter%20Gibbons&ExternalPageInstance=332c2687-71f7-49df-8b7c-b2402ccbf473&UserLocale=en-US&OlsPersonaId=&AadObjectId=&Smtp=kmitnick%40mitnicksecurity.com&UserPrincipalName=&PersonaType=User&RootCorrelationId=2933990e-0cc2-4406-8103-0cba160e7047&CorrelationId=2933990e-0cc2-4406-8103-0cba160e7047&ClientCorrelationId=0d3bf626-18d0-46bf-8ce4-667b7bb485b4",
  "Verb": "GET",
  "Headers": {
    "Accept": "text/plain, application/json, text/json",
    "X-ClientType": "OwaMail",
    "X-ClientFeature": "LivePersonaCard",
    "X-LPCVersion": "1.20201124.2.1",
    "authorization": "Bearer EwAYA9[...]",
    "X-HostAppCapabilities": "{}"
  } 
```

The feature is doing a lookup for a registered LinkedIn profile with the display name and the email address. Here the name “Peter Gibbons” and the email “peter.gibbons@aol.com” are passed as parameters.

We replaced the email address with another one from which we do not have any mail in our inbox. The server gave the JSON message shown below, which contains the complete LinkedIn profile of the user related to that address.

```json
{
  "Key": "4",
  "StatusCode": 200,
  "ReasonPhrase": "OK",
  "Headers": {
    "X-WebSocketCorrelationId": "9cb14592-5728-******",
    "Cache-Control": "no-store",
    "Server": "Microsoft-HTTPAPI/2.0",
    "X-BEServer": "_Loki_10716",
    "X-DataCenter": "PROD_NORTHCENTRALUS",
    "X-ServerVersion": "0.20201202.4.1",
    "X-Content-Type-Options": "nosniff",
    "X-TokenTtl": "86390",
    "X-InboundDuration": "14",
    "X-CorrelationId": "9cb14592-5728-47c5- ******,
    "Access-Control-Allow-Origin": "https://sfnam.loki.delve.office.com",
    "Access-Control-Allow-Credentials": "true",
    "Access-Control-Expose-Headers": "X-ServerVersion,X-InboundDuration,X-BEServer,X-TokenTtl,X-SocialDistance,X-CorrelationId,X-DataCenter,x-azure-ref,Retry-After",
    "Date": "Mon, 07 Dec 2020 16:26:01 GMT"
  },
  "Body": "<<JSON escaped string>>"
}
```

The response “Body” property includes a JSON object that is as follows. The data include the full name, the company, the location, the professional experience, the school attendance and the profile URL.

```json
{"resultTemplate":"ExactMatch","bound":true,"bindUrl":"https://login.live.com/accountbind.srf?provider=linkedin.com&redirect_uri=https://loki.delve.office.com/linkedInAuthRedirect.aspx&client_id=000000004C1E916B&dualbind=1&mkt=en-US&external_app=Owa&dualbindmobile=True",
"persons":[{"id":"urn:li:person:DgEN90FFXdpXh-OCiFTGl3l0pTo6d4ub6h19lWlc7mE","displayName":"Kevin Mitnick","headline":"The World's Most Famous Hacker | CEO | Author | Professional Speaker","companyName":"Mitnick Security Consulting","location":"Henderson, Nevada, United States",
"photoUrl":"https://media.licdn.com/dms/image/C4E03AQEKmI4XcvU8nQ/profile-displayphoto-shrink_800_800/0?e=1613001600&v=beta&t=o8EaQb4TeZn9JhUOTJJOS3PA9uzaewnHWO7n7nJNDfw","linkedInUrl":"https://www.linkedin.com/in/kevinmitnick", 

//... JSON messages also include schools, work experience, company details, 
 
} 
```

It is important to note that we do not need to be connected with the LinkedIn member to use this search feature. We can now get the LinkedIn profile from any email address.

## The Issue

### Privacy

Many malicious actors will find this information incredibly valuable. Such feature allows deanonymizing LinkedIn users in both small and large scales. For example, we could target an anonymous WordPress blogger and using the technique presented in the previous blog, we could easily find the email of that blogger. All an attacker has left to do is to transmit the same WebSocket request shown previously with the email to deanonymize the individual.

```json
{
"Url": "https://sfnam.loki.delve.office.com/api/v1/linkedin/profiles/full? [...] &Smtp=<<test.email@company.com>>&UserPrincipalName=&PersonaType=User [...]",
 [...]
}
```

WebSocket message that needs to be replayed

  

The PersonaDisplayName field is not mandatory making it easy to test arbitrary email addresses. Only the Smtp parameter is needed.

###   
Credential Stuffing

Password leaks are rarely including employee emails. More than 85 % of leaked passwords are associated with personal email addresses using public providers. This conclusion is based on the analysis of 2.6 billion unique email addresses with known information leaks provided by Flare Systems. The top domains of such providers are gmail.com, hotmail.com, yahoo.com and outlook.com. However, because LinkedIn profiles are tied to companies, it is possible to link these personal email addresses (peter@hotmail.com) to corporate emails (pgibbons@initech.com) based on company email formats.

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEglX5_8GOKjqadsQfYMJDxXSaCHoUmp5flrNw4dQPE9mA4ZzoKxj7Z_eR9fhLmUym1u-dLtRW9lhQ8e3FrqD4LWJlQpq67dRYphf1s9aQOl-mbk99XGrEOAtkxWT7Jk738bOvkGcROr4NBL/w640-h396/deanonymizing-linkedin-users_image-6%255B1%255D.png) |
| --- |
| Schema that illustrates emails and passwords correlation    |

  

Being able to enumerate all personal emails can make several attacks easier, such as user enumeration and credential stuffing. Credential stuffing is a brute force attack that reuses a known email address and password to compromise an account. Using common passwords alone can be highly inefficient if a solid password policy is in place. However, being able to correlate personal emails to corporate emails allows an attacker to do authentication attempts with more personalized passwords, as shown in the diagram above.

  

### Phishing

Finally, tricking employees to click on malicious links using deceptive emails (known as phishing) can be done via platforms outside of corporate environments, such as personal inboxes. Employees could receive fishing links on their personal email clients, a channel that cannot be monitored by corporate organizations to identify and mitigate initial device compromises.

## Potential Defense

It is worth mentioning that the LinkedIn APIs are throttling the number of queries to the service sfnam.loki.delve.office.com. Users can make a thousand requests every 2 days. This is likely meant to slow down data harvesters.

In general, information lookups using email addresses should be avoided. Some marketers and recruiters are getting more aggressive when it comes to collecting personal information.

From the user’s perspective, three settings can be changed.

**1.** Profile visibility off LinkedIn: Manage Your Profile’s Visibility On and Off LinkedIns and Off-LinkedIn Visibility

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgqKjX_UjX1y4DBs_4u38PBFWFEoiTE74HBinyo79wgovEMwdG0ncTyOnesRKV3MFfSIxsmyVfkYeVHJ70pOpc5pzH7pRMynHdqjXuiZmZI7iADzzXVOaAK00LcI6m9ncOqAfpKGWgI4JhR/w640-h152/deanonymizing-linkedin-users_image-7%255B1%255D.png)

  

  

**2.** Manage who can discover your profile from your email address

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjEAnthAQnjvld2E4_onb5UtPi72V8-Bkp0yqyB6HCVGF8WhsScmbrSB3HGAq3utyRAoZVRZO41Ds6aYyB7eKbX45sqrVwqol9dUq44dBXpAzJPvkJHRjgv90NsAKk4ZNEhqOHr9XQ9jPsB/w640-h252/deanonymizing-linkedin-users_image-8%255B1%255D.png)

  

This setting will prevent the email de-anonymization shown before.

**3.** Refuse connections with users you do not know or hide contact information from everybody

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjY1CgELij1ugeFhgH35AMpg2kiogu2Pe9Y7WyjT6SjihkZbG8onmq1b5bkhMCWnz1cdvdmvhB_6B6_tSnocnk2uVeb0KcnbtkI-vLVRbU1y6cOoc23ZJy3fWMsnh-_SUAtR7xGgWxnGE9I/w640-h330/deanonymizing-linkedin-users_image-9%255B1%255D.png)

  

  

By changing these settings, you are limiting the exposure of your email(s).

  

## Final Thoughts

If you are privacy conscious, you should revisit your LinkedIn settings, as mentioned above, to avoid being targeted by malicious actors, email marketers or recruiters. It is also important to recognize that pieces of information published online with the same email address are likely to be linked. Even if the information is published on multiple websites, emails can be leaked from public or undocumented APIs, making it easy to correlate different websites on which you have created accounts.

  

## References

- Similar issue on Facebook  
    https://web.archive.org/web/20161226011136/dawgyg.com/2016/12/21/disclosing-the-primary-email-address-for-each-facebook-user/
- Announcement about the integration to YahooMail  
    https://blog.linkedin.com/2015/04/16/yahoo-mail
- LinkedIn Help: Profile visibility off LinkedIn  
    https://www.linkedin.com/help/linkedin/answer/83634
- LinkedIn Help: Off-LinkedIn Visibility  
    https://www.linkedin.com/help/linkedin/answer/79854

  

_This blog was originally posted on GoSecure blog._

Go to Source
