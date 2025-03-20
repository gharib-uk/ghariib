---
title: "Mobile Screenshot prevention Cheat Sheet - Risks and Scenarios"
date: 2025-01-22
categories: 
  - "android-security"
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "ios-security"
  - "mobile-security"
  - "screenshot-security"
  - "security"
  - "security-awareness"
---

# Mobile Screenshot Prevention Cheat Sheet - Risks and Scenarios

The following article will try to analyze and explain risks and attack scenarios affecting mobile applications without any implemented prevention mechanism against screenshotting.

  

## Briefly, what is the problem?

Extremely summarizing, mobile applications need to implement screenshot prevention mechanisms in order to avoid an attacker to steal sensitive data, such as credentials or private information, that are shown on the screen during the application execution.

  

The attacker could act by directly accessing the victim’s device, generating a screenshot using the device specific combo keys and then he would be able to send the generated screenshot to himself over the network. This kind of attack does not need any specific privilege or compromised device capability in order to get exploited. Just physical access to the device.

  

On the other hand, the attacker could act by using a malware delivered on the victim’s device in some successful way, such as an untrusted application that may be downloaded from an unofficial store or that can be sent directly to the victim. As soon as the victim’s device gets infected, the attacker is now able to access private data and, based on the privileges acquirable on the infected device, he would be able to generate screenshots and/or access system generated screenshots which are stored on the device and may contain useful information to carry on further attacks.

  

## Attack Scenarios

Basically the following three attack scenarios can be used by an attacker in order to successfully exploit the vulnerability, based on which access level is achievable on the targeted device:

  

> **\- The attacker has physical access to the targeted device**: let’s assume the victim left his device unlocked at his desktop with a running application which is showing sensitive information. In this scenario it would be possible for an attacker to generate a screenshot using the device specific combo keys sending it to its own device through the network.  

> **\- The attacker has planted a background running malware on the targeted device**: let’s assume that the attacker has delivered a malicious application on the victim’s device by uploading a compromised application on the store or by getting the victim to download an application from an untrusted source and the target device has a compromised status that can allow the malware to have privileged access to system calls. In this scenario the attacker would be able to silently generate device screenshots that can be then sent to the attacker himself through the network, exfiltrating sensitive data. 

> **\- The attacker has planted a foreground running malware on the targeted device**: let’s assume that the attacker has delivered a malicious application on the victim’s device by uploading a compromised application on the store or by getting the victim to download an application from an untrusted source and the target device has modified status that can allow the attacker to read files outside of the application sandbox. In this scenario the attacker would be able to access system generated screenshots that may contain sensitive information and can then exfiltrate these files through the network.

## Risks

The main risk related to this kind of vulnerability is sensitive information stealing.

Any information shown on the device display can be stolen if not explicitly protected.

  

So if an application is displaying private information in any of its views, it must be protected.

  

An attacker can use automated OCR (Optical Character Recognition) analyzers in order to automatically process the gathered screenshots and extract any information contained therein.

  

Any extracted information can then be then exfiltrated to an attacker controlled target in several possible ways, which would not be covered by this article.

  

  

The following list is just an example of which kind of data can be stolen:

  

> - \- **Passwords**
>     
> - \- **PII** (Personal Identifiable Information)
>     
> - \- **PHI** (Personal Healthcare Information)
>     
> - \- **Private data** such as appointments, notes, etc.
>     
> - \- **Contacts**
>     
> - \- **Email and message contents**
>     
> - \- \[...\]  
>     

  

## Real world: a concrete attack

As already stated, there are two kinds of approaches an attacker can use, based on how his device is  accessed. Both of them will be explained in the following paragraphs.

  

In both scenarios, we will assume that the Bill, the victim, is using the _SuperSecureMail_ application. Bill resets his password, and the _SuperSecureMail_ application generates a set of temporary credentials, showing them on the screen so that Bill will be able to copy them and access back to his mail account.

  

#### Scenario 1: The attacker has physical access to victim’s device

Bill is in his office, and he has just generated temporary credentials in his _SuperSecureMail_ application. Now, since the application has generated a complex and strong password, Bill wants to write them on a piece of paper in order to later perform a login to his mailbox, but unfortunately he ran out of paper, so he decides to go to the office next door in order to borrow some. Doing this, Bill leaves his mobile device unlocked on his desk.

  

At this point the attacker can pick Bill’s device, and he can see the credentials on the screen. He has a little time before Bill’s return, so he decides to pick a screenshot of what is on the screen using the key combination that is specific to that device and then he sends himself the generated screenshot over a Bluetooth connection.

  

Since the application was **not implementing any screenshotting countermeasure**, the attacker now has a screenshot containing Bill’s credentials on its device and he can use the obtained information to access Bill’s mailbox and/or perform further attacks.

  

#### Scenario 2: The attacker has access to victim’s device through a malware

This scenario is a bit more complex than the previous one since the attacker needs to plant a malware to Bill’s device before starting the attack, moreover Bill has a device which is running a mobile operating system which implements a permissive access control policy and allows processes to perform privileged operations (e.g. the device is rooted or jailbroken).

  

There are many ways an attacker could use in order to trick Bill to install an attacker controlled malware. In this case the attacker knows that Bill loves to play with mobile games which are full of cute kittens.

Using this information, the attacker creates a real mobile game with the desired look but he also inserts malicious pieces of code into the application. These pieces of code will run in a specific mode in order to retrieve system generated screenshots of any mobile application installed on Bill’s device. 

  

After generating this mobile game, the attacker sends an email to Bill saying “_Hey Bill, attached there is a mobile game that we think you would love! It is a preview and we care about your opinion, so it is free at this time but it is only for you_!”.

  

Bill loves cute kittens. 

  

Bill installs the game and gets infected unknowingly by the attacker’s malware.

  

At this point, as in the previous scenario Bill has just generated temporary credentials in his _SuperSecureMail_ application. Now, since the application has generated a complex and strong password Bill copies this password on the Notes application of his device.

  

Now, when Bill switches between the _SuperSecureMail_ application and the Notes application, while bringing the _SuperSecureMail_ application to the background the mobile operating system generates a screenshot of what the application is displaying in that moment. 

The screenshot is a standard image that will be shown in the device’s task manager and that will be stored in a specific path on the device’s file system that varies based on the operating system version.

  

Finally, the malware can scan the file system searching for operating system generated screenshots. When it finds them, it would be possible to exfiltrate these images to an attacker controlled domain where the data can be processed and the contained sensitive information can be extracted.

  

It must be noted that the aforementioned scenarios are _only two_ of many kind of attacks that may take an attacker to exploit this kind of vulnerability. However they are two really common scenarios that can happen in the real world.

  

## How to do this? Let's see the code!

The following sample of code\[1\] is showing how is it possible to generate a screenshot from an **Android** background service, using superuser’s privileges:

  

> Process sh = Runtime.getRuntime().exec("su", null,null); 
> 
> OutputStream os = sh.getOutputStream(); 
> 
> os.write(("/system/bin/screencap -p " + "/sdcard/img.png").getBytes("ASCII")); 
> 
> os.flush(); 
> 
> os.close(); 
> 
> sh.waitFor();
> 
>   
> 
> Bitmap screen = BitmapFactory.decodeFile(Environment.getExternalStorageDirectory()+ File.separator +"img.png");
> 
> ByteArrayOutputStream bytes = new ByteArrayOutputStream(); screen.compress(Bitmap.CompressFormat.JPEG, 15, bytes); 
> 
> File f = new File(Environment.getExternalStorageDirectory()+ File.separator + "test.jpg"); 
> 
> f.createNewFile(); 
> 
> FileOutputStream fo = new FileOutputStream(f); fo.write(bytes.toByteArray());
> 
> FileOutput fo.close();

  

In a similar way the following example of **iOS** code \[2\] can be used in a Mobile Substrate module, running in a jailbroken device, to generate a screenshot of any application:

  

> UIKIT\_EXTERN CGImageRef UIGetScreenImage();
> 
> CGImageRef ref = UIGetScreenImage(); 
> 
> UIImage\* img = \[UIImage imageWithCGImage:ref\];
> 
> CGImageRelease(ref);

  

  

The following library developed by Google can be used to generate screenshots from an **Android background service** without having access to superuser’s privileges:

- https://code.google.com/archive/p/android-screenshot-library/
    

In a similar way, if the attacker is targeting **Android versions 21+,** the following native framework can be used in order to achieve the same result, getting a screenshot generated on a standard device:

- https://developer.android.com/reference/android/media/projection/MediaProjection
    

#### Example - 1: accessing system generated screenshots

In this paragraph is shown how an attacker can access system generated screenshots for the Safari application.

For this example, the Safari application has been opened and then it has been put on the background. This action resulted in the following image being shown in the _iOS_ task manager:

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhUKHbNLNJTtDHpqZyNn0NmGWhTIJX6JCxe6Zvz0eo5d7Y2uDRl1GWd_WpHJk0cAb2N8ITrNJ8vuv28FOxo0Ms7bTVSsYOO8wmW4yQ-IFCSk8PXsAOq-KrS-s4W-EoNpi1u4NzuBG9m72k/s320/IMG_0036.PNG)

  

  

System generated screenshots are located at the following location on **iOS 12:**

> > /var/mobile/Containers/Data/Application/{APPLICATION\_BUNDLE\_ID}/Library/Caches/Snapshots/{APPLICATION\_PACKAGE\_NAME}

  

For example, in this case, Safari system generated screenshots are located at the following location:

> > /var/mobile/Containers/Data/Application/B9008870-3F72-4B0E-ADEC-D867FBA060A2/Library/Caches/Snapshots/com.apple.mobilesafari

  

The following screenshot is showing the content of the above folder:

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiXDe6yMNOmk_zwwn-kx87xhvPfKDeB8jEPf-0_C1Pp5kpToEOrwvzNqVFpe6w2iugDpfBHMrtelW6DwUKORdu80UUj3NQc9uQ-uHQM9k3NkRj6CmYDB0fAWaTqcNTy2LLFaC2qMQhEBl8/s320/IMG_0037.PNG)

  

  

Finally, opening the file named _E341AE9B-B12A-4E8B-AB08-1DA156CDF9E2@2x.ktx_ we can see that the content is the screenshot that was shown in the **iOS** task manager shown before:

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiAr1t5GEnSqyA6QewOFMJLTX2kz6TZtb1WGkV0JfOoFeWu25N_FcD6lQT5tT8GHUdPHRH9kS66OerQudgerB8RKK2lXbFvgOSgz3MXdHc5f-A9qAhR07Zo3c_7IP59Gxax7KjLcgmzCkI/s320/IMG_0038.PNG)

  

#### Example - 2: accessing user generated screenshots

In a similar way, user generated screenshots can be accessed from any application which the permission to access the device photo library was granted.

In this example, we are explicitly generating a screenshot of the Safari application.

  

User generated screenshots can be found at the following location in iOS12:

> > /var/mobile/Media/DCIM/100APPLE/

  

Accessing this location, and opening the file named IMG\_0039.PNG we can finally access the previously generated screenshot:

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgVH6mDlDqyoGDNBlmsuLzchyphenhyphen1Rgo_G2LNZxwUjWlYihMPC0Qaq8g9P11hV3-yIh1XD1hZEJhcWYS5iDxlG7Ku8GMebQHA6jnh9C6bm6looT4r7WC19_SVF0dbZtZl2iCuxwZ0p6G0Qa-8/s320/IMG_0039.PNG)

  

  

## Conclusions and next steps

This article analyzed which risks and scenarios are afflicting mobile applications which are not explicitly implementing mobile screenshotting prevention, trying to clearly explain what an attacker can do in order to gather personal data and sensitive information.

  

What can a user do in order to prevent sensitive data from being stolen leveraging this kind of attack? 

  

It must be noted that the user by itself can not totally prevent these types of attacks, but he can mitigate them by using the following best practices:

- Do not let people to have unauthorized physical access to the device
    
- Do not install any application coming from a untrusted sources such as email attachments, unofficial application stores or any other source available on the web.
    
- Do not modify the device and operating system statuses by using procedures such as rooting, jailbreaking or installing modified versions of the mobile operating system.
    

  

The next question would be: how can a mobile application be checked if it implements Mobile Screenshot countermeasures? How can this issue be fixed?

  

These topics will be covered in one of the next blog posts with the title: “**Mobile Screenshot Prevention Cheat Sheet - Testing and Fixing**”.

  

  

References: 

\[1\] https://stackoverflow.com/questions/8779700/screenshot-from-background-service-of-another-application-programmatically

\[2\] https://stackoverflow.com/questions/9746711/programmatically-take-screenshots-on-ios-from-anywhere

  

Go to Source
