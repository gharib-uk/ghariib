---
title: "Mobile Screenshot Prevention Cheatsheet - Testing and Fixing"
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

# Mobile Screenshot Prevention Cheat Sheet - Testing and Fixing

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhhKkaHBQr1Q_NduSGmMgU2g3AfrKQIy-Sa1vWprMpoDXzTXxq3DiOt5U2jcJ7vRe3CagBzuGOO0hjh9gBXJ905gdCwCJqeoQ0EPqHtKuAkRo1dOzpyk9zOZanlVUcaKxifgyTDR6x8Vjod/)

  

  
The following article will explain how to test mobile applications against any implemented screenshot prevention mechanism and then it will try to propose mitigations to such problem according to the context.

  

The following article is the second part of **Mobile Screenshot Prevention Cheat Sheet - Risks and Scenarios** published on IMQ Minded Security blog.

  

**TLDR**; None of the proposed solutions will provide a full protection against screenshotting. Therefore all of them shall be considered as mitigations.

  

## Auditing Screenshot Prevention

In this section we will focus on testing and preventing mobile screenshot via static (e.g. perform a secure code review or a mobile application reverse engineering task) and dynamic (e.g. test a mobile application in its execution environment) contexts.

  

First things first: anyone approaching mobile application security should carefully read the OWASP Mobile Security Testing Guide (MSTG)\[1\]\[2\].

  

#### Static Analysis

Assuming we have the source code of a mobile application and we have to check if the app implements any mitigation against screenshot attacks, either user or system generated. 

What should we look for?

  

### Android

Usually the common remediation is to set FLAG\_SECURE to LayoutParam, hence the first step is to search for it in the codebase. 

But, if we have access to the source code of an application somehow decompiled from a packaged application, it might be possible that the FLAG\_SECURE keyword could have been _obfuscated_ or _replaced_ with its integer value.

  

Therefore, the values to search for, are:

- FLAG\_SECURE (high level symbol)
- 8192 (numeric value)
- 0x00002000 (hexadecimal numeric value)

If any of those values are found, it must be checked whether it is used as parameter set to the WindowManager LayoutParams. 

  

It should be noted that this flag must be set in all the Android _Activities_ involved in the application and it would be effective for both user and system generated screenshots.

  

### iOS

Since in _iOS_ it is _not possible to prevent user generated screenshots_, we have to search for any code related to the notification system which can be used to provide awareness about the taken screenshot.

It is then possible to search for the following keywords:

- userDidTakeScreenshotNotification (Swift)
- UIApplicationUserDidTakeScreenshotNotification (Objective-C)

If any of the following keywords is found, it would be possible to then observe a function callback being executed after a user generated screenshot has been created.

  

On the other hand, for system generated screenshots, it would be necessary to analyze pieces of code which are responsible to handle the transitions between foreground and background application statuses.

Since the system generated screenshots are generated just before the application has been put in the background status, in these portions of code, if any remediation has been implemented, we are expecting to find some mechanism which would hide or obfuscate sensitive parts of the UI just before the screenshot has been generated.

It would then be possible to search for the possible values:

- applicationWillResignActive (here we should find pieces of code hiding parts of the UI, e.g. by setting the hidden property on a view)
- applicationDidBecomeActive (here we should find pieces of code showing parts of the UI, e.g. by unsetting the hidden property on a view)

  

#### Dynamic Analysis

TLDR; Let's try to generate a screenshot of the application and let's check if the application UI is shown in the device task manager.

  

### User Generated Screenshots

First of all, let's focus on how we can generate a screenshot of our running application. Basically it will depend on which OS and device type we are testing.

  

On a variety of Android devices, it is possible to generate a screenshot by using device specific combo keys. The most common combinations are:

- VOLUME DOWN + POWER
- VULUME UP + POWER

It would be also possible to generate a screenshot by using the function tile in the system tray, but it strongly depends on the OS customizations.

  

If we are using the Android emulator, we can use the specific button on the emulator toolbar:

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhnGhYA-5UuE1z3H8-p3YRGVQdTjPRY-COdHDABS2e617Hp4aA5OLt1oUr4kEBjPeJGYZ13AEjFtkEbPxH0bKYzGbRnAu_5D_UX_LErS7mgzomrz45fBE7g60AxWIjhKSJFUDObFlNDMO4/s0/android_emu1.png)

  

On iOS it depends on which physical keys are available on the target device. The two possible combo keys are:

- POWER + HOME
- VOLUME UP + POWER

Finally, if we are using the iOS simulator, we can use the combo key CMD + S or we can use the drop down menu entry under File -> New Screen Shot.

  

In all the above cases, the positive probe is the successful generation of a screenshot containing the same views actually on the device's screen, without any kind of modification and without any kind of notification generated on the device.

  

If the running application is properly secured, we will see the following notification being generated on Android:

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi6p8-NUrd_Jb45zVMg1-brf4LRYvqkKYLO8N1UWwBiSGxVvt0KmBstFaDHyDrORHRBGB9kzglqH6BjTdb1Hf9wkhxgmcYBnuhj7Obqm_b1v5kwstZ1hdcgQoV1WVPasOyuFeg_LNNrYgE/s320/android_notification1.png)

  

On the other hand, since in iOS it is not possible to prevent a user to generate a screenshot of the application, if the running app is implementing a mitigation against screenshot generation it is not actually possible to define which behavior a pentester would see. 

Some possible mitigations are:

- The screenshot is partially obfuscated
- A notification warns the user about the just generated screenshot
- The application shows a message which informs the user about the just generated screenshot

Beware that also a transparent and invisible mechanism could be implemented on iOS in order to warn the user about the screenshot generation, so dynamic analysis could not be sufficient in order to fully understand if an app is implementing some mitigation mechanism.

  

### System Generated Screenshots (AKA Task Manager Screenshots)

On both the operating systems, the test is the same and is pretty straightforward:

- Open the target application
- Send the application in background by pressing the HOME button
- Open the system task manager (e.g. by tapping the dedicated button on Android or by double tapping the home button on iOS, etc)

At this point, if an application screenshot is shown identical as it was possible to observe when the app was in foreground, it means that no prevention mechanism was implemented.

On the other hand, if a blank screen is shown or if the image is partially obfuscated, it does mean that the app is actually implementing some sort of full or partial prevention mechanism against system generated screenshots.

  

## Fixing Screenshot Issues

  

Which APIs are provided to prevent or mitigate the issue?

####   

### Android

One flag to rule them all.

Basically Android offers the LayoutParam FLAG\_SECURE \[3\] which can be used to prevent both user and system generated screenshots.

  

Simple and straightforward.  

  

### iOS

On iOS the developer is not provided the ability to prevent user generated screenshots. This is due to the strict UI/UX guidelines provided by Apple which are preventing developers from interfering with the standard OS behavior.

Two kinds of APIs are interesting from the security point of view, when talking about screenshot prevention mechanisms.

  

For **user generated** screenshots it is possible to use the userDidTakeScreenshotNotification \[4\] system wide notification. In this way, it would be possible to run code just after a screenshot was generated. 

The most common approach is to generate a notification or an alert which warns the user about the generated screenshot. This would inform the user and will raise a red flag in case of the screenshot was not explicitly and voluntarily generated.

  

On the other hand, for **system generated** screenshots, it is possible to refer to the applicationWillResignActive \[5\] application's lifecycle callback. This callback is invoked just before any application is moved from the foreground to the background and enables developers to execute code just before the system generated screenshots are created.

These operations can include:

- Putting an overlay over the whole application window
- Hiding/masking sensitive parts of the UI
- Navigating to other application parts which are not showing sensitive information

  

## Stop talking! Show me the code!

This section is intentionally left without any comment, since explanations have been provided in the above sections.

Just the source code that can be used in various scenarios and situations.

Pro tip: If some data is VERY sensitive, such as the combination of a credit card PAN, its PIN code and the CVV2 code, don't put it in your views. If some data is not displayed on the screen, it can't be screenshotted!

  

But since in the real world such approach is very often not applicable, let's dig into some mitigation implementations.

  

#### Android

Protects from:

- User generated screenshots: ✔️
- System Generated screenshots: ✔️

  

### **Kotlin**

> > import android.os.Bundle
> > 
> > import android.support.v7.app.AppCompatActivity
> > 
> > import android.view.WindowManager
> > 
> >   
> > 
> > class MainActivity : AppCompatActivity() {
> > 
> >   override fun onCreate(savedInstanceState: Bundle?) {
> > 
> >     super.onCreate(savedInstanceState)
> > 
> >     setContentView(R.layout.activity\_main)
> > 
> >     window.setFlags(
> > 
> >       WindowManager.LayoutParams.FLAG\_SECURE,
> > 
> >       WindowManager.LayoutParams.FLAG\_SECURE
> > 
> >     )
> > 
> >   }
> > 
> > }

  

  

### **Java**

  

> import android.os.Bundle;
> 
> import android.view.WindowManager;
> 
>   
> 
> public class MainActivity extends Activity {
> 
>   
> 
>     @Override
> 
>     protected void onCreate(Bundle savedInstanceState) {
> 
>         super.onCreate(savedInstanceState);
> 
>   
> 
>         getWindow().setFlags(
> 
>             WindowManager.LayoutParams.FLAG\_SECURE,
> 
>             WindowManager.LayoutParams.FLAG\_SECURE
> 
>         );
> 
>   }
> 
> }

  

  

References: \[6\]

  

#### iOS

Protects from:

- User generated screenshots: 🤷 (partial)
- System Generated screenshots: ❌

  

### **Swift**

> NotificationCenter.default.addObserver(
> 
>     forName: UIApplication.userDidTakeScreenshotNotification,
> 
>     object: nil,
> 
>     queue: .main) { notification in
> 
>         //Notify the user, log the action, etc...
> 
> }

  

### **Objective-C**

> NSOperationQueue \*mainQueue = \[NSOperationQueue mainQueue\];
> 
> \[\[NSNotificationCenter defaultCenter\] addObserverForName:UIApplicationUserDidTakeScreenshotNotification
> 
>                                                   object:nil
> 
>                                                    queue:mainQueue
> 
>                                               usingBlock:^(NSNotification \*note) {
> 
>                                                  //Notify the user, log the action, etc...
> 
>                                               }\];

  

References: \[6\] \[7\]

  

  

Protects from:

- User generated screenshots: ❌
- System Generated screenshots: ✔️

  

> \- (void)applicationWillResignActive:(UIApplication \*)application {
> 
>     // hide main window
> 
>     self.window.hidden = YES;
> 
> }
> 
>   
> 
> \- (void)applicationDidBecomeActive:(UIApplication \*)application {
> 
>     // show window back
> 
>     self.window.hidden = NO;
> 
> }

  

References: \[8\]

  

#### React Native

Protects from:

- Android User generated screenshots: ✔️
- Android System Generated screenshots: ✔️
- iOS User generated screenshots: 🤷 (partial)
- iOS System Generated screenshots: ❌

  

### **Android**

> > import android.os.Bundle;
> > 
> > import com.facebook.react.ReactActivity;
> > 
> > import android.view.WindowManager;
> > 
> >   
> > 
> > public class MainActivity extends ReactActivity {
> > 
> >   
> > 
> >     @Override
> > 
> >     protected void onCreate(Bundle savedInstanceState) {
> > 
> >         super.onCreate(savedInstanceState);
> > 
> >   
> > 
> >         getWindow().setFlags(
> > 
> >             WindowManager.LayoutParams.FLAG\_SECURE,
> > 
> >             WindowManager.LayoutParams.FLAG\_SECURE
> > 
> >         );
> > 
> >   }
> > 
> > }

  

  

### **iOS**

> > import React from 'react'
> > 
> > import { AppState, Platform, View } from 'react-native'
> > 
> >   
> > 
> > const SecurityScreen = () => <View />
> > 
> >   
> > 
> > const showSecurityScreenFromAppState = appState =>
> > 
> >   \['background', 'inactive'\].includes(appState)
> > 
> >   
> > 
> > const withSecurityScreenIOS = Wrapped => {
> > 
> >   return class WithSecurityScreen extends React.Component {
> > 
> >     state = {
> > 
> >       showSecurityScreen: showSecurityScreenFromAppState(AppState.currentState)
> > 
> >     }
> > 
> >   
> > 
> >     componentDidMount () {
> > 
> >       AppState.addEventListener('change', this.onChangeAppState)
> > 
> >     }
> > 
> >     componentWillUnmount () {
> > 
> >       AppState.removeEventListener('change', this.onChangeAppState)
> > 
> >     }
> > 
> >     onChangeAppState = nextAppState => {
> > 
> >       const showSecurityScreen = showSecurityScreenFromAppState(nextAppState)
> > 
> >   
> > 
> >       this.setState({ showSecurityScreen })
> > 
> >     }  
> > 
> >   
> > 
> >     render() {
> > 
> >       return this.state.showSecurityScreen
> > 
> >         ? <SecurityScreen />
> > 
> >         : <Wrapped {...this.props} />
> > 
> >     }
> > 
> >   }
> > 
> > }
> > 
> >   
> > 
> > const withSecurityScreenAndroid = Wrapped => Wrapped
> > 
> >   
> > 
> > export const withSecurityScreen = Platform.OS === 'ios'
> > 
> >   ? withSecurityScreenIOS
> > 
> >   : withSecurityScreenAndroid

  

  

Then, in your App component:

  

> import { withSecurityScreen } from './withSecurityScreen'
> 
> ...
> 
> export default withSecurityScreen(App);

  

  

Pre bundled libraries are also available on NPM \[10\].

  

References: \[9\] \[10\]

  

#### Cordova / Ionic

There are many Cordova plugins which can help on achieving the goal. 

A good place to start is the **cordova-plugin-prevent-screenshot-coffice** open source repository.

  

Protects from:

- Android User generated screenshots: ✔️
- Android System Generated screenshots: ✔️
- iOS User generated screenshots: 🤷 (partial)
- iOS System Generated screenshots: ❌

References: \[11\]

  

#### Commercial Solutions

Several commercial solutions are available on the market, however since there was no easy way to test any of them, this blog post will not promote or endorse any commercial tool.

  

## Conclusions - Wrapping it up

TLDR; None of the proposed solutions will provide a full protection against screenshotting, therefore all of them should be considered as mitigations.

  

Since all the OSs handle the application status transitions in a different way, and different versions of  OSs behave in a different way, it will always be possible to generate some sort of bypass of any implemented mitigation mechanism.

Moreover, let's say that if an application is running on an untrusted - jailbroken or rooted - device  environment it is easy for an attacker to hook and bypass any kind of protection with some effort.

  

The chosen mitigation should then be biased on which data the application is showing on the screen and how much sensitive is this data.

Examples of such mitigation bypass are available in literature, such as \[12\].

  

## References

\[1\] - https://mobile-security.gitbook.io/mobile-security-testing-guide/ios-testing-guide/0x06d-testing-data-storage#testing-auto-generated-screenshots-for-sensitive-information-mstg-storage-9 

\[2\] - https://mobile-security.gitbook.io/mobile-security-testing-guide/android-testing-guide/0x05d-testing-data-storage#finding-sensitive-information-in-auto-generated-screenshots-mstg-storage-9

\[3\] - https://developer.android.com/reference/android/view/WindowManager.LayoutParams#FLAG\_SECURE

\[4\] - https://developer.apple.com/documentation/uikit/uiapplication/1622966-userdidtakescreenshotnotificatio

\[5\] - https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622950-applicationwillresignactive

\[6\] - https://medium.com/nomtek/screenshot-preventing-on-mobile-apps-9e62f51643e9

\[7\] - https://stackoverflow.com/questions/13484516/ios-detection-of-screenshot

\[8\] - http://pinkstone.co.uk/how-to-control-the-preview-screenshot-in-the-ios-multitasking-switcher/

\[9\] - https://medium.com/@jonaskuiler/creating-a-security-screen-on-ios-and-android-in-react-native-97703092e2de

\[10\] - https://www.npmjs.com/package/react-native-obscure

\[11\] - https://github.com/flotrugliocoffice/cordova-plugin-prevent-screenshot-coffice

\[12\] - https://medium.com/@techhelpkb/bypass-an-android-apps-screenshot-restriction-34ee4b79b284

  

  

  

  

Go to Source
