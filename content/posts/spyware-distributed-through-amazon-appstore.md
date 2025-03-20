---
title: "Spyware distributed through Amazon Appstore"
date: 2025-01-03
categories: 
  - "security"
---

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/09/300x200_Blog_042723.png)

_Authored by Wenfeng Yu and ZePeng Chen_

As smartphones have become an integral part of our daily lives, malicious apps have grown increasingly deceptive and sophisticated. Recently, we uncovered a seemingly harmless app called “BMI CalculationVsn” on the Amazon App Store, which is secretly stealing the package name of installed apps and incoming SMS messages under the guise of a simple health tool. McAfee reported the discovered app to Amazon, which took prompt action, and the app is no longer available on Amazon Appstore.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure1.jpg)

Figure 1. Application published on Amazon Appstore

### **Superficial Functionality: Simple BMI Calculation**

On the surface, this app appears to be a basic tool, providing a single page where users can input their weight and height to calculate their BMI. Its interface looks entirely consistent with a standard health application. However, behind this innocent appearance lies a range of malicious activities.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Picture2.png)

Figure 2. Application MainActivity

### **Malicious Activities: Stealing Private Data**

Upon further investigation, we discovered that this app engages in the following harmful behaviors:

1. **Screen Recording:** The app starts a background service to record the screen and when the user clicks the “Calculate” button, the Android system will pop up request screen recording permission message and start screen recording. This functionality is likely to capture gesture passwords or sensitive data from other apps. In the analysis of the latest existing samples, it was found that the developer was not ready for this function. The code did not upload the recorded mp4 file to the C2 server, and at the beginning of the startRecording() method, the developer added a code that directly returns and does not execute follow code.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Picture3.png)

Figure 3. Screen Recorder Service Code

When the recording starts, the permission request dialog will be displayed.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Picture4.png)

Figure 4. Start Recording Request.

2. **Installed App Information:** The app scans the device to retrieve a list of all installed applications. This data could be used to identify target users or plan more advanced attacks.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure5-1.png)

Figure 5. Upload User Data

3. **SMS Messages:** It intercepts and collects all SMS messages received on the device, potentially to capture one-time password (OTP), verification codes and sensitive information. The intercepted text messages will be added to Firebase (storage bucket: testmlwr-d4dd7.appspot.com).

### **Malware under development:**

According to our analysis of historical samples, this malicious app is still under development and testing stage and has not reached a completed state. By searching for related samples on VirusTotal based on the malware’s package name (com.zeeee.recordingappz) revealed its development history. We can see that this malware was first developed in October 2024 and originally developed as a screen recording app, but midway through the app’s icon was changed to the BMI calculator, and the payload to steal SMS messages was added in the latest version.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure6-1.png)

Figure 6. The Timeline of Application Development

The address of the Firebase Installation API used by this app uses the character “testmlwr” which indicates that this app is still in the testing phase.

### **App Developer Information:**

According to the detailed information about this app product on the Amazon page, the developer’s name is: “PT. Visionet Data Internasional”. The malware author tricked users by abusing the names of an enterprise IT management service provider in Indonesia to distribute this malware on Amazon Appstore. This fact suggests that the malware author may be someone with knowledge of Indonesia.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure7.jpg)

Figure 7. Developer Information

### **How to Protect Yourself**

To avoid falling victim to such malicious apps, we recommend the following precautions:

1. Install Trusted Antivirus Apps: Use reliable antivirus software to detect and prevent malicious apps before they can cause harm.
2. Review Permission Requests: When installing an app, carefully examine the permissions it requests. Deny any permissions that seem unrelated to its advertised functionality. For instance, a BMI calculator has no legitimate reason to request access to SMS or screen recording.
3. Stay Alert: Watch for unusual app behavior, such as reduced device performance, rapid battery drain, or a spike in data usage, which could indicate malicious activity running in the background.

### **Conclusion**

As cybercrime continues to evolve, it is crucial to remain vigilant in protecting our digital lives. Apps like “BMI CalculationVsn” serve as a stark reminder that even the simplest tools can harbor hidden threats. By staying alert and adopting robust security measures, we can safeguard our privacy and data.

### **IoC**

Distribution website:

- hxxps://www.amazon.com/PT-Visionet-Data-Internasional-CalculationVsn/dp/B0DK1B7ZM5/

C2 servers/Storage buckets:

- hxxps://firebaseinstallations.googleapis.com/v1/projects/testmlwr-d4dd7
- hxxps://6708c6e38e86a8d9e42ffe93.mockapi.io/
- testmlwr-d4dd7.appspot.com

Sample Hash:

- 8477891c4631358c9f3ab57b0e795e1dcf468d94a9c6b6621f8e94a5f91a3b6a

The post Spyware distributed through Amazon Appstore appeared first on McAfee Blog.
