---
title: "Bypassing Certificate Pinning on Flutter-based Android Apps A new guide"
date: 2024-05-07T04:22:00.000-07:00
draft: false
type: posts
categories: 
- Certificate Pinning,Flutter,Mobile Security,reverse engineering
---
# Bypassing Certificate Pinning on Flutter-based Android Apps A new guide

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/a/AVvXsEiLZOgVn35LqPDpfHUk6wcqLOjrvV6JioR2CPitS6-4WhuB6jSaudX8wXAwTaHk3U7ebucLS-R5vO6GRTF2dC-cjzUcm-qv1IQLhkB-zDUYLavj06fTkgqAbwsix1lSP2klyN8rqETtdsCcQFAv_FOKE9AGSNQ0TOyi3q5sxBSkyE_lhziAZbmO1DDnroxg)](https://blogger.googleusercontent.com/img/a/AVvXsEiLZOgVn35LqPDpfHUk6wcqLOjrvV6JioR2CPitS6-4WhuB6jSaudX8wXAwTaHk3U7ebucLS-R5vO6GRTF2dC-cjzUcm-qv1IQLhkB-zDUYLavj06fTkgqAbwsix1lSP2klyN8rqETtdsCcQFAv_FOKE9AGSNQ0TOyi3q5sxBSkyE_lhziAZbmO1DDnroxg)

One of the preliminary activities when analyzing mobile application, _more usually than not_, is to be able to sniff HTTP/S traffic via **a MitM proxy**. 

This is quite straightforward in the case of naive applications, but can be quite challenging when applications use [certificate pinning](https://owasp.org/www-community/controls/Certificate_and_Public_Key_Pinning) techniques. In this post I'll try to explain the methodology I used to make this possible for a **Flutter-based** **Android** sample application in a reliable way.

  

#### **Introduction**

It was indeed the need to _bypass_ a certificate validation on a _Flutter framework_ during a mobile application penetration testing activity for a customer of ours, that led to this research. 

  

As a first approach, as usual, we tried [some](https://github.com/NVISOsecurity/disable-flutter-tls-verification/blob/main/disable-flutter-tls.js) of the [specific](https://blog.nviso.eu/2022/08/18/intercept-flutter-traffic-on-ios-and-android-http-https-dio-pinning/ https://blog.nv) [exploits](https://iso.eu/2019/08/13/intercepting-traffic-from-android-flutter-applications/)/[bypasses](https://alijujara.medium.com/proxying-traffic-on-android-flutter-applications-d3f70655df66) we found on the web. 

Alas, in this case, they failed.

  

Some of the main concepts that are going to be explained, actually, overlap in what those articles contain; what it differs is the technique used for identifying and hooking at runtime the routine used for certificate verification.

  

While minimizing effort was a key objective in bypassing certificate pinning controls, the chosen approach turned out to be overly generic.  Unlike pattern matching techniques, which can be tailored to specific scenarios, this method becomes unreliable across different platforms, architectures, and even builds.

  

While the article's findings may not address all the case studies, it proposes an effective methodology that can be readily applied, with minimal adjustments, to similar scenarios. 

  

Moreover, as a different approach, [reFlutter](https://github.com/Impact-I/reFlutter) would be a valid tool for such a similar job. Anyway, as it statically patches code to deactivate runtime checks, if the mobile application was signed and/or had file integrity checks, static approach would definitely be challenging.  

  

In the next paragraphs, we're going to cover the following topics:

-   Why Flutter is so different
-   Flutter based APK structure
-   Force traffic forwarding to my proxy
-   Diving into BoringSSL source code
-   Reverse-engineering on libflutter.so
-   Frida hooking script

Finally, in order to give some testbed, we created a basic Flutter application which fetches data from the URL [https://dummy.restapi.example.ltd/api/v1/employees](https://dummy.restapiexample.com/api/v1/employees), using code that correctly implements **[certificate pinning](https://owasp.org/www-community/controls/Certificate_and_Public_Key_Pinning)**.

The entire application code, the build and the [Frida](https://frida.re/) script are available on Github at the following link:

  

[https://github.com/mindedsecurity/flutter-cert-check-bypass/](https://github.com/mindedsecurity/flutter-cert-check-bypass/)

  

**Why Flutter is so Different**

This is, obviously, not the first time we have to deal with bypassing certificate pinning, so, why is Flutter so different from other scenarios?

-   Flutter is a Google's open source portable toolkit able to create natively compiled applications for mobile, web, and desktop from a single codebase. 
-   The final Flutter applications are written in Dart, whilst the framework engine is built mainly in C++. 
-   The compilation obviously differs on the target platform: for Android, the engine is compiled using the Android's NDK (for this blogpost we are going to focus only on the Android ARMv8 platform). 

  

In this development environment more blazoned SSL pinning bypass [methods](https://mas.owasp.org/MASTG/techniques/android/MASTG-TECH-0012/), would not work, as the real checks are implemented in "native code", thus results compiled in machine code and many details may change among different framework builds. 

  

In addition, for our activities of analysis of the HTTP request sent to remote servers we would like to forward the application's traffic throughout a forward HTTP proxy:

\- the first idea that an unaware analyst could have is to set this in the Network settings of Android system, but this would not work at all. 

  

Flutter, at low level, uses **dart:io** library for managing socket, file and other I/O support in non browser-based applications. 

As **dart:io** documentation [says](https://api.dart.dev/stable/3.2.6/dart-io/HttpClient-class.html), by default the HttpClient uses the proxy configuration available from the environment, i.e it will take into account enviroment variables **http\_proxy, https\_proxy, no\_proxy, HTTP\_PROXY, HTTPS\_PROXY, NO\_PROXY** which, on Android based OSs, are not affected by any settings change, since every application runs in a child process of Zygote [inheriting](https://developer.android.com/topic/performance/memory-overview#SharingRAM) its environment variables, that are loaded at system boot.

  

#### **Flutter Based APK Structure**

The **resource/lib** folder inside the APK contains the compiled Dart code (compiled to native binary code using the Dart VM's Ahead-of-Time compilation) for the Flutter application. 

The compiled code is stored in various folders organized according to the target platform, such as _arm64-v8a_ for 64-bit ARM, _armeabi-v7a_  for 32-bit ARM etc. 

Specifically the two main shared objects are:

-   **libapp.so**: which contains the compiled native code that corresponds to the Dart code of your Flutter app. This native code is executed when the Flutter app is launched on a device.
-   **libflutter.so**: containing the native code of the Flutter engine, which powers the Flutter framework and enables the execution of Flutter apps on Android devices. This library includes the core functionalities of the Flutter framework

  

#### **Forcing Traffic Forwarding to the Proxy**

We know that the Flutter application will not honor the proxy settings at system level, thus we need a finer technique for fundamental part.  
Once granted root privileges on our Android device (Google Pixel 4a), we have the possibility to set an _iptables_ rule for the redirection of the desired HTTP traffic table throughout the a Burp Pro proxy instance running on my laptop:

1.  connect the android device via USB and run an adb shell as root.  
    **adb shell su**
2.  setup the redirection:  
    **iptables -t nat -A OUTPUT -p tcp -d destination-host.domain --dport** **443 \\****\-j DNAT --to-destination 127.0.0.1:8080**
3.  reverse map the port 8080 of the android device over the proxy running on my laptop on port 8080:  
    **adb reverse tcp:8080 tcp:8080**

_**Note:** proxydroid could also be adopted equivalently to the iptables rule: it is just a frontend for iptables._

  

Finally, it is also important to point out that we need to set up our Burp listening server as [invisible](https://portswigger.net/burp/documentation/desktop/tools/proxy/invisible), so that it won't be necessary to explicitly set a proxy on the device, and the proxy itself will act as a perfect man-in-the-middle and directly intercept the SSL/TLS handshaking.  

  

At this point the traffic is redirected to Burp as shown in a screenshot of the test application taken just after the triggering of the sample HTTP request and after the setting up of the proxy interception:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjRti7fd5GSfOrfWL17BFZJQhS0CcG8pYU9MeHJI1u6GMTnxw67RbDLoYHM8zzcVIdswTKfBQtOgyAHe1ixqpb4sbWlbtQ27rI59fCmzMgRqvRi18r50C-gRY4F05mYddg5IQT3PojifoAQMt9TDmPh9eeDPrqCgaikimOrnGeKNgll-vk2qNkAbXPQTPY/w315-h725/error1.1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjRti7fd5GSfOrfWL17BFZJQhS0CcG8pYU9MeHJI1u6GMTnxw67RbDLoYHM8zzcVIdswTKfBQtOgyAHe1ixqpb4sbWlbtQ27rI59fCmzMgRqvRi18r50C-gRY4F05mYddg5IQT3PojifoAQMt9TDmPh9eeDPrqCgaikimOrnGeKNgll-vk2qNkAbXPQTPY/s1794/error1.1.png)

Sample application's error message after having forced traffic to Burp Pro proxy instance

  

  

The error is pretty verbose about the code which triggered the **HandshakeException**.

Someone at this point might say: _"Well, production applications usually don't show such verbose errors!"_. 

We're using our test bed, once we have the **know-how** to bypass such controls, we'll be able to replicate it to any Flutter-based application. 

#### **Diving into BoringSSL Source Code**

Flutter Engine relies on **[_boringssl_](https://github.com/google/boringssl)** library for SSL/TLS implementation, which is the Google's fork of the well known **[_openssl_](https://www.openssl.org/).** 

As the error suggested, the certificate verification which trapped was performed in [**boringssl/ssl/handshake.cc** at line 393](https://github.com/google/boringssl/blob/d69e8b46184b6fd844a4a92b4a6f4347d08ee439/ssl/handshake.cc#L392), coherently with what displayed in the previous screenshot, there is the raising of the represented error concerning the failure of the certificate verification. 

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg48P69JoMk3H_-u0De3peDNYLfqzAVQjou030hQBd1ecqPhDQ8mATJx1nFdpsCIec7yZA8OYnnYesPrR-vxVMu5rQmo4-Slus285c8KvUJTZ1KZit-7JofE9ag6ZpDNteVg4Wsguog_T6bBHVyIFd9ME9NP2EhOpsxRhUHEFbhIxKvP0WtFM_ogHadRwY/w641-h555/handshake_Errortrigger.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg48P69JoMk3H_-u0De3peDNYLfqzAVQjou030hQBd1ecqPhDQ8mATJx1nFdpsCIec7yZA8OYnnYesPrR-vxVMu5rQmo4-Slus285c8KvUJTZ1KZit-7JofE9ag6ZpDNteVg4Wsguog_T6bBHVyIFd9ME9NP2EhOpsxRhUHEFbhIxKvP0WtFM_ogHadRwY/s1588/handshake_Errortrigger.png)

Point of triggering in BoringSSL source code of the TLS certificate verification error

  

  

The interesting part of this function is the **_else_** block immediately above: assuming no **_custom\_verify\_callback_** has been set, the code is invoking _**ssl\->ctx\->x509\_method\->****session\_verify\_cert\_chain**_ and is setting **_ret_** by evaluating the boolean result of the same. 

  

That method is a function pointer referring **_ssl\_crypto\_x509\_session\_verify\_cert\_chain_**, which is defined in **_borisngssl/ssl/ssl\_x509.cc_.** To bypass the check was only needed to make this function return always **_true_**.  

#### **Reverse-Engineering on libflutter.so**

At this point, we unpack the application's apk and open in IDA the **_resources/lib/arm64-v8a/libflutter.so_** shared object; which is the Flutter core engine natively compiled for the platform **Android ARM64**. 

The ELF file was stripped and had just a single exported symbol: ****_JNI\_Onload_**** (address **_0x00382940_**). For this reason, identifying the portion of assembly which was related to **_ssl\_crypto\_x509\_session\_verify\_cert\_chain_** was not so trivial.

  

Here follows the integral source code of that function.

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjp6oM00_LOlYPbS7V9-yqPqF27VaRJpLgnXg_OGJgP65ayRH-0n0RXYel1HfCg9uRzHueaA_o4IuT3-aVrdJN3_GVf9Alhwa6Ul-Qy8yKg21asCNbbxCMHEFFPSW7z8soiCn4bpyVigv-BM74de4EJI55TsYDISy_faaPkrnGcp2NTdQyIJ5s3VZo244s/w660-h1077/function_to_hook_s.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjp6oM00_LOlYPbS7V9-yqPqF27VaRJpLgnXg_OGJgP65ayRH-0n0RXYel1HfCg9uRzHueaA_o4IuT3-aVrdJN3_GVf9Alhwa6Ul-Qy8yKg21asCNbbxCMHEFFPSW7z8soiCn4bpyVigv-BM74de4EJI55TsYDISy_faaPkrnGcp2NTdQyIJ5s3VZo244s/s1799/function_to_hook_s.png)

**_ssl\_crypto\_x509\_session\_verify\_cert\_chain_** implementation taken from BoringSSL repository

  

As it can be seen, the function at a certain point uses two string literals: "_ssl\_client_" and "_ssl\_server_". Those are both stored in the **.rodata** section of _**libflutter.so**._ So, the strategy used was to analyze the _XREFS_ (cross-references) of those strings to find a function which locally addressed both. The single procedure which used both was that at address **_0x005FDF64_**.

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhb7wQM7GZy6suq8TTqP2N1jR8ASxiQRP_3FCLbmzbll_M45ROBHlMvxdWwv_n7nDj92wA55u9mCYiJaGl05NpsbMAOq2-5v1qgDg4a3u2jAdFQqSko6gtRIio5NjppBUkfEHqbnODWcnDSLBdhCicz7ai0kVwwGrzLBOff2vbOci-CmLN2ypdBF5ROzUU/w632-h728/decompiled.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhb7wQM7GZy6suq8TTqP2N1jR8ASxiQRP_3FCLbmzbll_M45ROBHlMvxdWwv_n7nDj92wA55u9mCYiJaGl05NpsbMAOq2-5v1qgDg4a3u2jAdFQqSko6gtRIio5NjppBUkfEHqbnODWcnDSLBdhCicz7ai0kVwwGrzLBOff2vbOci-CmLN2ypdBF5ROzUU/s1723/decompiled.png)

Decompiled **ssl\_crypto\_x509\_session\_verify\_cert\_chain** by IDA Pro 7.7 

  

  

#### **Frida Hooking Script**

After identifying the native subroutine that should have been hooked, it is possible to implement a simple Frida script to intercept calls to the function  **_ssl\_crypto\_x509\_session\_verify\_cert\_chain_** and force its return value to **_true_** (i.e., 0x01).

To find the absolute address which depends on the memory mapping of **_libflutter.so_** into application proces,s the reader will have to:

-   Calculate the relative logical offset between _**ssl\_crypto\_x509\_session\_verify\_cert\_chain** **and JNI\_Onload:**_ this is done by simply subtract the JNI\_Onload logical address inside libflutter.so, which is readable into the shared object exports table, to the **ssl\_crypto\_x509\_session\_verify\_cert\_chain** logical address address, whose value was previously found with the technique shown in the previous section; in this sample analysis they are respectively **_0x005FDF64_ **and** _0x00382940_**, thus the resulting offset would be:  
     **_0x005FDF64 - 0x00382940 = 0x0027b624_**.
-   Since **JNI\_OnLoad** is an exported symbol, the dynamic linker updates the global offset table (_GOT_) upon its first invocation. This update provides the logical address of **JNI\_OnLoad** relative to the shared object's memory layout within the process.  Therefore, we can easily access it using the **enumerateExports**() method in the Frida script.
-   Add the offset obtained to the ****_JNI\_Onload_**** address for this process, to get the current ****_ssl\_crypto\_x509\_session\_verify\_cert\_chain_**** address (i.e. the address to which the function 

  

  

function hook\_ssl\_crypto\_x509\_session\_verify\_cert\_chain(address){

Interceptor.attach(address, {

onEnter: function(args) { console.log("Disabling SSL certificate validation") },

onLeave: function(retval) { console.log("Retval: " + retval); retval.replace(0x1);}

});

}

function disable\_certificate\_validation(){

var m = Process.findModuleByName("libflutter.so");

console.log("libflutter.so loaded at ", m.base);

var jni\_onload\_addr = m.enumerateExports()\[0\].address;

console.log("jni\_onload\_address: ", jni\_onload\_addr);

// Adding the offset between

// ssl\_crypto\_x509\_session\_verify\_cert\_chain and JNI\_Onload = _0x0027b624_

let addr = ptr(jni\_onload\_addr).add(_0x0027b624_);

console.log("ssl\_crypto\_x509\_session\_verify\_cert\_chain\_addr: ", addr);

let buf = Memory.readByteArray(addr, 12);

console.log(hexdump(buf, { offset: 0, length: 64, header: false, ansi: false}));

hook\_ssl\_crypto\_x509\_session\_verify\_cert\_chain(addr);

  

}

setTimeout(disable\_certificate\_validation, 1000)

  
  

  

  

#### **Conclusion**

At this point we are ready to execute Frida server via adb and run the above script with this command:

**_frida -U -f com.application -l script.js_**; 

We managed to bypass the certificate verification and intercepted HTTPS traffic:

  

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEignDxzEvhcR-mXy6_voevoTho_8IQCtXj6SYMezpjIwXT2rSCqpsVD7JszCGh5HBwqbxQ5ox9LDEIMPY0LEOI5ffyVSpgxE_-l2oyVt4t4tp8R2PjKm23htfKrXOVmhwbZdN9IiGyiXmEzTHbgo5aoCQrcDj_TBhUTdxwekuFcDGWd1QhRVtsNjMxYV84/w621-h287/successful.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEignDxzEvhcR-mXy6_voevoTho_8IQCtXj6SYMezpjIwXT2rSCqpsVD7JszCGh5HBwqbxQ5ox9LDEIMPY0LEOI5ffyVSpgxE_-l2oyVt4t4tp8R2PjKm23htfKrXOVmhwbZdN9IiGyiXmEzTHbgo5aoCQrcDj_TBhUTdxwekuFcDGWd1QhRVtsNjMxYV84/s2087/successful.png)

Frida script launching 

  

  

  

Here follows the result of the bypass, showing the Burp Pro intercepting the request and its related response.

  
   
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEinZ4PXz3Y-5E5an5e0fFvyvBOgVpG2CXZN8aleGF7xPmSIBrOjtLMhnPiBvL7tf38dEOyN_Rno3gC-L_dw2FWut4L5Ahc9fuc1pxjmCcyH8vxtxVOBihn6iTChvdIa8uqurEGJybGZpz0FqdS11NmD-p1aGO90Uf54PZf6TL9QJ13sK0284k1DEo8hUs0/w610-h387/request_successful.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEinZ4PXz3Y-5E5an5e0fFvyvBOgVpG2CXZN8aleGF7xPmSIBrOjtLMhnPiBvL7tf38dEOyN_Rno3gC-L_dw2FWut4L5Ahc9fuc1pxjmCcyH8vxtxVOBihn6iTChvdIa8uqurEGJybGZpz0FqdS11NmD-p1aGO90Uf54PZf6TL9QJ13sK0284k1DEo8hUs0/s1798/request_successful.png)

Request triggered by the Flutter App intercepted by Burp Pro

#### [Source](https://blog.mindedsecurity.com/feeds/109688310409158712/comments/default)

<br/>
---
