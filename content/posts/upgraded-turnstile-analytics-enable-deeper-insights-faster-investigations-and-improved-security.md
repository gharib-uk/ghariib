---
title: "Upgraded Turnstile Analytics enable deeper insights, faster investigations, and improved security"
date: 2025-03-19
---

Attackers are increasingly using more sophisticated methods to not just brute force their way into your sites but also simulate real user behavior for targeted harmful activity like account takeovers, credential stuffing, fake account creation, content scraping, and fraudulent transactions. They are no longer trying to simply take your website down or gain access to it, but rather cause actual business harm. There is also the increasing complexity added by attackers rotating IP addresses, routing through proxies, and using VPNs. In this evolving security landscape, meaningful analytics matter. Many traditional CAPTCHA solutions provide simplistic pass or fail trends on challenges without insights into traffic patterns or behavior. Cloudflare Turnstile aims to equip you with more than just basic trends, so you can make informed decisions and stay ahead of the attackers. 

We are excited to introduce a major upgrade to Turnstile Analytics. With these upgraded analytics, you can identify harder-to-detect bots faster, and fine-tune your bot security posture with less manual log analysis than before. Turnstile, our privacy-first CAPTCHA alternative, has been helping you protect your applications from automated abuse while ensuring a seamless experience for legitimate users. Now, using enhanced analytics, you can gain deeper insights into your visitor traffic, challenge effectiveness, and potential security threats. 

Previously, Turnstile users had limited visibility into what types of bots were being blocked, what specific characteristics were exhibited by bots that were attacking your website, and what identifiable behavior they had. Customers had to manually sift through limited analytics, correlate Siteverify API responses, and cross-reference multiple sources to identify trends. The previous Turnstile analytics dashboard made it difficult to get a bird's eye view of Turnstile efficacy, identify any patterns of abuse, and drill down on the specifics of an attack to create additional rules and safeguards. 

The new Turnstile Analytics surfaces all of this information in one place, making it easier than before to assess your visitor traffic patterns through Turnstile and take immediate action against suspicious activity.

### What’s new with Turnstile Analytics?

The main motivation behind this release is to provide actionable insights that further strengthen the layers of protection and to give customers the ability to dissect visitor traffic by the most relevant attributes, so that identifying bot behavior patterns becomes easier. New features of Turnstile Analytics include: 

#### Top statistics 

When you click into widget analytics under Turnstile in the Cloudflare Dashboard, you now have enhanced visibility of TopN statistics, and granular views of your traffic. The new TopN section is where you can view the top statistics of attributes such as hostname, autonomous system (ASN), user agent, browser, source IP address, country, and OS. This allows customers to analyze traffic at a more granular level and detect potential anomalies or patterns. You can analyze which browsers, user agents, ASNs, and locations generated the most failed challenges, making it easier to detect bot behavior patterns and anomalies in your visitor traffic. Suspicious IP addresses that have a high challenge failure rate can be proactively mitigated through additional security measures. For instance, if you have WAF custom rules in place based on suspicious IP addresses, you can in turn adjust your WAF custom rules based on the trends you see in Turnstile, strengthening your other layers of security even further.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/51u7UF1W6ud6amSeP7c41N/d4a6d17ddc2a7cde024100a308449520/1.png)

_TopN section of Turnstile Analytics_

#### Challenge outcomes

When a visitor encounters Turnstile, it issues a challenge to assess whether the visitor is a human or a bot, based on various signals. The Challenge outcomes section helps you evaluate what portion of your traffic is likely human or likely bots.

The ability to easily monitor the effectiveness of Turnstile by looking at trends of Likely Human and Likely Bot metrics is important for peace of mind, knowing that the bots are being blocked and Turnstile is protecting your sites. But it’s also important to track changes in bot activity over time by monitoring challenge success and failure trends and across different attributes. You can detect anomalies in your traffic pattern and solve rates. For example, a sudden drop in solve rate overlaid with a surge in challenge attempts may indicate an attack. It is crucial to monitor bot behaviors and attacks that may be specific to your industry or to your business through Turnstile Analytics and correlate them with your internal security logs to keep your security rules up to date, to easily investigate any attacks, and to find areas of vulnerability. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6vAzZrKNrLNzU6jTFoXoDU/43ab17dcd11fe8e972caa838bfd83de0/2.png)

_Challenge outcomes section of Turnstile Analytics_

#### Solve rates

When the visitor successfully solves the challenge, the Solve rates section shows how the visitors have solved the challenge. Solve rates can be broken down into interactive solves, non-interactive solves, and pre-clearance solves. If you are using the managed mode, for example, you can see how many of your visitors required interaction with the widget and were prompted to check the box for Turnstile to verify that they are human. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1495UrrH51QNwWf0kpwO34/842d72c1f1f39789d5e0e0395f677e9a/3.png)

_Solve rates section of Turnstile Analytics_

#### Token validations

After a visitor successfully completes a Turnstile challenge, a token is generated that must be validated via the Siteverify API. The API response provides the ultimate outcome of our bot determination. Only rendering the widget on the client side without calling the Siteverify API for token validation is an incomplete implementation of Turnstile, and your site will not be protected. The Turnstile token that is returned from the challenge stage must be validated via the Siteverify API as we check if the token is valid, whether it has been redeemed already (a single token can only be redeemed once), and whether it has expired. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1GTzkvLjawlIGuwJo5G8UY/b79a50382764dee65861923a705e34d5/4.png)

_Token validation section of Turnstile Analytics_

### Let’s walk through a real world example

Common use cases of Turnstile include protecting login and sign up pages from credential stuffing, account takeover, and fraudulent account creation attacks. Let’s walk through how you can best set up Turnstile on your login pages and interpret your traffic with the new Turnstile analytics. 

You can set up two separate widgets for your login and sign up page, or you can set up one widget and use the 'action' field to distinguish traffic between these pages. The ‘cData’ field can be used to pass along custom data to keep track of each individual attempt. This field is useful to track any pertinent information from your business logic such as account ID, session ID, etc. In this case, let’s assume we are passing along a session ID along with the login attempt. This is helpful if you are trying to protect and monitor against account takeover attacks or credential stuffing attacks. cData is a custom data field that is not stored in Cloudflare systems at any time. 

#### Rendering the Turnstile widget

To place the Turnstile widget on your login page: 

```
<script src="https://challenges.cloudflare.com/turnstile/v0/api.js" async defer></script>
<form action="/login" method="POST">
  <div class="cf-turnstile" data-sitekey="your-site-key" data-action="login" data-cdata=”session123”></div>
  <input type="submit" value="Log in">
</form>
```

To place the Turnstile widget on your signup page: 

```
<form action="/signup" method="POST">
  <div class="cf-turnstile" data-sitekey="your-site-key" data-action="signup"></div>
  <input type="submit" value="Sign up">
</form>
```

#### Validating the Turnstile token with the Siteverify API 

At this point, you have placed the Turnstile widget in your login page. When a visitor visits this page, a Turnstile challenge will be issued and when the visitor completes the challenge, you will receive a Turnstile token that contains the outcome of the challenge. This must be validated via the Siteverify API like below: 

```
// This is the demo secret key. 
// In production, we recommend you store your secret key(s) safely.
const SECRET_KEY = "1x0000000000000000000000000000000AA";

async function handlePost(request) {
  const body = await request.formData();
  // Turnstile injects a token in "cf-turnstile-response".
  const token = body.get("cf-turnstile-response");
  const ip = request.headers.get("CF-Connecting-IP");

  // Validate the token by calling the
  // "/Siteverify" API endpoint.
  let formData = new FormData();
  formData.append("secret", SECRET_KEY);
  formData.append("response", token);
  formData.append("remoteip", ip);

  const url = "https://challenges.cloudflare.com/turnstile/v0/siteverify";
  const result = await fetch(url, {
    body: formData,
    method: "POST",
  });

  const outcome = await result.json();
  if (outcome.success) {
    // happy path: let the visitor continue with login/signup
  } else {
    // option 1: custom error page directing the visitor to reach out to support
    // option 2: same as happy path but flag as potential bot
  }
}
```

As you can see in the code example above, you can control the visitor experience based on the Siteverify outcome. In the case where Siteverify API said the token is valid, it’s straightforward — let the visitor continue to log in and sign up. This can be monitored by the **Valid tokens** metric in the Token validation section in the new Turnstile Analytics. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3WN26OmbvqbbvBwXTk3Nxw/76cdf8f9d9376932733ea4c4fb6841b8/5.png)

Example Invalid Token Siteverify Outcome: 

```
{
  "success": false,
  "challenge_ts": "2025-02-28T15:14:30.096Z",
  "hostname": "mybusiness.com",
  "error-codes": [],
  "action": "login",
  "cdata": "account123",
  "metadata":{
    "ephemeral_id": "x:9f78e0ed210960d7693b167e"
  }
}
```

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3hUZNIISTbFGqT0NYKMEhb/08bcd2f33de6a404faa71ca0c809a47e/6.png)

If Siteverify returns `"success": false`, this means that the token was invalid and Turnstile determined the visitor to be a bot. In this case, you have control over what you want the experience to be, such as redirecting the user to a custom error page where they can reach out to support.  

You can also flag that session (in this case, “session123”) as suspicious and require the account owner to take action. You can implement the UI so that it seems like the bot was successful in logging in to an account, but block any important actions, such as account changes or purchases. Likewise, you can alert the account owner that there has been a suspicious login attempt. 

Turnstile is a building block to help you build out your security defenses, and you can design your logic to fit your priorities across UI, UX, and security. 

#### Interpreting login page analytics

The very first thing to monitor is the Top Statistics section to look out for any anomalous traffic characteristics in the “countries”, “source ASN”, and “source user agents” metrics. By seeing the traffic distribution, you can have a better understanding of your visitors and potentially spot any anomalies. At this point, you can also take a look at “Source browsers”, “Source OS”, and “Countries” to see if that aligns with your visitor demographics. If you have a list of suspicious IP addresses that you maintain, you can cross-reference them to see their success and failure rates. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4bUtPmM4FbqN9Azh6n43fW/a9eff878603fa095a378697962cec919/7.png)

_Example TopN Section_ 

Let’s say you suspect there has been a credential stuffing attack where bots were brute forcing their way into accounts. Below is mock data of what your analytics may look like where the time window is zoomed into the time of the attack. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3IMvwPeDOiXocgc8TMmgxk/c60394dc83f456a5f00b60e40c2dd196/8.png)

_Example Challenge outcomes section_ 

You can see that time period where the number of challenges unsolved started spiking and the “likely bot” metric shot up. This shows an increase in bot traffic, indicating an attack. However, you can also see that Turnstile was able to catch these bots as they were unable to solve or even complete the challenge. 

Let’s look at another example. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5yiAMOgmFxLFSYV65oAxQO/d33d37af63e6871f98015e7650780799/9.png)

_Example Token validation section_ 

In this case, of the 11.13M tokens issued in the timeframe, 0.01% of them were invalid. This means that 0.01% of the traffic is considered to be non-legitimate visitors, despite the fact that they received the Turnstile tokens.  This is why it is crucial to always validate your tokens through the Siteverify API. What becomes more interesting is if the login credentials these suspicious visitors provided were correct credentials, which could indicate that this is a potential account takeover attack or the accounts in question have been compromised. If the login credentials were incorrect, but the attempts were in a burst, that could indicate credential stuffing attack. By correlating Turnstile analytics with your internal application data such as whether the login attempt had a correct or incorrect password, you can further identify the nature and behavior of the attacker and build out the defenses or mitigate accordingly. 

This was an example showing how Turnstile can protect and provide insights on just your login page. Imagine how this could be expanded to other use cases such as your sign-up pages, submit form pages, contact pages, checkout pages, and more. 

### Looking ahead

We are not planning on stopping here with Turnstile Analytics. Next on our roadmap is to expand Turnstile Analytics to give you more insights around client side and server side errors, so that you can further break down the traffic beyond just the challenge outcomes. We will also be incorporating Ephemeral IDs into the analytics, so that you can filter by Ephemeral ID, see top Ephemeral IDs, and the frequency of their solve attempts. 

We have many more exciting things in store for Turnstile for 2025! There is no prerequisite with Turnstile, and our free tier is unlimited in volume, so there is no barrier to get started today. Let's help make the Internet a more secure, better place, together!

Go to Source
