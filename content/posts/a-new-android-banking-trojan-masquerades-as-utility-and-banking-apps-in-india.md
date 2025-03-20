---
title: "A New Android Banking Trojan Masquerades as Utility and Banking Apps in India"
date: 2025-01-03
categories: 
  - "security"
---

![](https://www.mcafee.com/blogs/wp-content/uploads/2023/06/300x200_Blog_030624.png)

_Authored by Dexter Shin_

Over the years, cyber threats targeting Android devices have become more sophisticated and persistent. Recently, McAfee Mobile Research Team discovered a new Android banking trojan targeting Indian users. This malware disguises itself as essential services, such as utility (e.g., gas or electricity) or banking apps, to get sensitive information from users. These types of services are vital for daily life, making it easier to lure users. We have previously observed malware that masquerades as utility services in Japan. As seen in such cases, utility-related messages, such as warnings that gas service will disconnect soon unless the bill is checked, can cause significant alarm and prompt immediate action from the users.

We have identified that this malware has infected 419 devices, intercepted 4,918 SMS messages, and stolen 623 entries of card or bank-related personal information. Given the active malware campaigns, these numbers are expected to rise. McAfee Mobile Security already detects this threat as Android/Banker. For more information, visit McAfee Mobile Security

### **Phishing through messaging platforms like WhatsApp**

As of 2024, India is the country with the highest number of monthly active WhatsApp users. This makes it a prime target for phishing attacks. We’ve previously introduced another Banker distributed via WhatsApp. Similarly, we suspect that the sample we recently found also uses messaging platforms to reach individual users and trick them into installing a malicious APK. If a user installs this APK, it will allow attackers to steal the victim’s financial data, thereby accomplishing their malicious goal.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/figure-1.jpg)

Figure 1. Scammer messages reaching users via Whatsapp (source: reddit)

### **Inside the malware**

The malware we first identified was pretending to be an app that allowed users to pay their gas bills. It used the logo of PayRup, a digital payment platform for public service fees in India, to make it look more trustworthy to users.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/figure-2.png)

Figure 2. Malware disguised as gas bills digital payment app

Once the app is launched and the permissions, which are designed to steal personal data such as SMS messages, are granted, it asks the user for financial information, such as card details or bank account information. Since this malware pretends to be an app for paying bills, users are likely to input this information to complete their payments. On the bank page, you can see major Indian banks like SBI and Axis Bank listed as options.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/figure-3.png)

Figure 3. Malware that requires financial data

If the user inputs their financial information and tries to make a payment, the data is sent to the command and control (C2) server. Meanwhile, the app displays a payment failure message to the user.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/figure-4.png)

Figure 4. Payment failure message displayed but data sent to C2 server

One thing to note about this app is that it can’t be launched directly by the user through the launcher. For an Android app to appear in the launcher, it needs to have “android.intent.category.LAUNCHER” defined within an <intent-filter> in the AndroidManifest.xml. However, since this app doesn’t have that attribute, its icon doesn’t appear. Consequently, after being installed and launched from a phishing message, users may not immediately realize the app is still installed on their device, even if they close it after seeing messages like “Bank Server is Down”, effectively keeping it hidden.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/figure-5.png)

Figure 5. AndroidManifest.xml for the sample

### **Exploiting Supabase for data exfiltration**

In previous reports, we’ve introduced various C2 servers used by malware. However, this malware stands out due to its unique use of Supabase, an open-source database service. Supabase is an open-source backend-as-a-service, similar to Firebase, that provides PostgreSQL-based database, authentication, real-time features, and storage. It helps developers quickly build applications without managing backend infrastructure. Also, it supports RESTful APIs to manage their database. This malware exploits these APIs to store stolen data.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/figure-6.png)

Figure 6. App code using Supabase

A JWT (JSON Web Token) is required to utilize Supabase through its RESTful APIs. Interestingly, the JWT token is exposed in plain text within the malware’s code. This provided us with a unique opportunity to further investigate the extent of the data breach. By leveraging this token, we were able to access the Supabase instance used by the malware and gain valuable insights into the scale and nature of the data exfiltration.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/figure-7.png)

Figure 7. JWT token exposed in plaintext

During our investigation, we discovered a total of 5,558 records stored in the database. The first of these records was dated October 9, 2024. As previously mentioned, these records include 4,918 SMS messages and 623 entries of card information (number, expiration date, CVV) and bank information (account numbers, login credentials like ID and password).

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/figure-8.png)

Figure 8. Examples of stolen data

### **Uncovering variants by package prefix**

The initial sample we found had the package name “gs\_5.customer”. Through investigation of their database, we identified 8 unique package prefixes. These prefixes provide critical clues about the potential scam themes associated with each package. By examining the package names, we can infer specific characteristics and likely focus areas of the various scam operations.

| Package Name | Scam Thema |
| --- | --- |
| ax\_17.customer | Axis Bank |
| gs\_5.customer | Gas Bills |
| elect\_5.customer | Electrical Bills |
| icici\_47.customer | ICICI Bank |
| jk\_2.customer | J&K Bank |
| kt\_3.customer | Karnataka Bank |
| pnb\_5.customer | Punjab National Bank |
| ur\_18.customer | Uttar Pradesh Co-Operative Bank |

Based on the package names, it seems that once a scam theme is selected, at least 2 different variants are developed within that theme. This variability not only complicates detection efforts but also increases the potential reach and impact of their scam campaigns.

### **Mobile app management of C2**

Based on the information uncovered so far, we found that the malware actor has developed and is actively using an app to manage the C2 infrastructure directly from a device. This app can send commands to forward SMS messages from the victim’s active phones to specified numbers. This capability differentiates it from previous malware, which typically manages C2 servers via web interfaces. The app stores various configuration settings through Firebase. Notably, it utilizes Firebase “Realtime Database” rather than Firestore, likely due to its simplicity for basic data retrieval and storage.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/figure-9.png)

Figure 9. C2 management mobile application

### **Conclusion**

Based on our research, we have confirmed that 419 unique devices have already been infected. However, considering the continual development and distribution of new variants, we anticipate that this number will steadily increase. This trend underscores the persistent and evolving nature of this threat, emphasizing the need for careful observation and flexible security strategies.

As mentioned at the beginning of the report, many scams originate from messaging platforms like WhatsApp. Therefore, it’s crucial to remain cautious when receiving messages from unknown or uncertain sources. Additionally, given the clear emergence of various variants, we recommend using security software that can quickly respond to new threats. Furthermore, by employing McAfee Mobile Security, you can bolster your defense against such sophisticated threats.

### **Indicators of Compromise (IOCs)**

APKs:

| SHA256 | Package Name | App Name |
| --- | --- | --- |
| b7209653e226c798ca29343912cf21f22b7deea4876a8cadb88803541988e941 | gs\_5.customer | Gas Bill Update |
| 7cf38f25c22d08b863e97fd1126b7af1ef0fcc4ca5f46c2384610267c5e61e99 | ax\_17.customer | Client Application |
| 745f32ef020ab34fdab70dfb27d8a975b03e030f951a9f57690200ce134922b8 | ax\_17.number | Controller Application |

Domains:

- https\[://\]luyagyrvyytczgjxwhuv.supabase.co

Firebase:

- https\[://\]call-forwarder-1-default-rtdb.firebaseio.com

The post A New Android Banking Trojan Masquerades as Utility and Banking Apps in India appeared first on McAfee Blog.
