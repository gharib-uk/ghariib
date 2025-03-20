---
title: "Semgrep Rules for Android Application Security"
date: 2023-10-23T00:32:00.001-07:00
draft: false
type: posts
categories: 
- Android Security,MAPT,MSTG,sast,semgrep
---
# Semgrep Rules for Android Application Security

<br/>

<br/>
Introduction
------------

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgmqKNYmEPnpcNBEXzKE7L9L8Z3vMbGd70W0XIXLj48ru63EH1VB_Jlv5DF2vctrykGOBv9H9Ob_08OxOzPDNFJx6IBD1mTpbNOA5Pza7mJEv1D4GciPW8dLxEVMMNWAjOQ70RlNYmwJMhpP-Z08X-4yXe61CtZW6x9NXbqPqNDCQgiOJcIzZmGWLTarQU/w308-h308/android_image.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgmqKNYmEPnpcNBEXzKE7L9L8Z3vMbGd70W0XIXLj48ru63EH1VB_Jlv5DF2vctrykGOBv9H9Ob_08OxOzPDNFJx6IBD1mTpbNOA5Pza7mJEv1D4GciPW8dLxEVMMNWAjOQ70RlNYmwJMhpP-Z08X-4yXe61CtZW6x9NXbqPqNDCQgiOJcIzZmGWLTarQU/s1024/android_image.png)

The number of Android applications has been growing rapidly in recent years. In 2022, there were over **3.55 million** **Android apps** available in the Google Play Store, and this number is expected to continue to grow in the years to come. The expansion of the Android app market is being driven by a number of factors, including the increasing popularity of smartphones, the growing demand for mobile apps, and the ease of developing and publishing Android apps. At the same time, the number of Android app  
downloads is also growing rapidly. In 2022, there were over **255 billion** Android app **downloads** worldwide.

For this reason, introducing automatic security controls during Mobile Application Penetration Testing (MAPT) activity and the CI phase is necessary to ensure the security of Android apps by scanning for vulnerabilities before merging into the main repository.

  

---

Decompiling Android Packages
----------------------------

The compilation of Android applications is a multi-step process that involves different bytecodes, compilers, and execution engines. Generally speaking, a common compilation flow is divided into three phases:

1.  **Precompilation**: The Java source code(".java") is converted into Java bytecode(".class").
2.  **Postcompilation**: The Java bytecode is converted into Dalvik bytecode(".dex").
3.  **Release**: The ".dex" and resource files are packed, signed and compressed into the Android App Package (**APK**)

Finally, the Dalvik bytecode is executed by the Android runtime (ART) environment.

Generally, the target of a Mobile Application Penetration Testing (MAPT) activity is in the form of an APK file. The decompilation of the both aforementioned bytecodes is possible and can be performed through the use of tools such as [Jadx](https://github.com/skylot/jadx).

```
jadx -d ./out-dir target.apk
```

  

---

OWASP MAS
---------

The [OWASP MAS](https://mas.owasp.org/) project is a valuable resource for mobile security professionals, providing a comprehensive set of resources to enhance the security of mobile apps. The project includes several key components:

-   **OWASP MASVS**: This resource outlines requirements for software architects and developers who aim to create secure mobile applications. It establishes an **industry standard** that can be used as a benchmark in mobile app security assessments. Additionally, it clarifies the role of software protection mechanisms in mobile security and offers requirements to verify their effectiveness.

-   **OWASP MASTG**: This comprehensive manual covers the processes, techniques, and tools used during mobile application security analysis. It also includes an exhaustive set of **test cases** for verifying the requirements outlined in the OWASP Mobile Application Security Verification Standard (MASVS). This serves as a foundational basis for conducting thorough and consistent security tests.

-   **OWASP MAS Checklist**: This checklist aligns with the tests described in the MASTG and provides an output template for mobile security testing.

[![OWASP MAS](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhhdKu4Zh5wr5AX_VDzDIKJ4OxF1_nfAY0N6KYHhjv5tHJzptABxQ_5QcPgBu4VbmBWRO8NAy6PrDN_z8kwF_WTkCbabyQF1Kuvm8Vb9f9ywXGm9xfyyFACFGlWFTRRU1eNn9PQ8sV1IdAffzkQmGVc0cfPxyYyqsXOWD5qnCi4cjtn4WyKCW6xXcxOnuA/s16000/owasp_mas.png)](goog_142454313)

[https://mas.owasp.org/](https://mas.owasp.org/)

  

---

Semgrep
-------

[Semgrep](https://semgrep.dev/) is a Static Application Security Testing (SAST) tool that performs **intra-file analysis**, allowing you to define code patterns for detecting misconfigurations or security issues by analyzing one file at a time in isolation. Some advantages of using Semgrep include:

-   It does not require that the source code is uploaded to an external cloud.
-   It does not require that the target source code is buildable and have all dependencies. It can work only with a single source file.
-   It is exceptionally **fast**.
-   It allows you to write your **custom patterns** very easily.

Once Semgrep is integrated into your **CI pipeline**, it automatically scans your code for potential vulnerabilities every time you commit changes. This helps identify and address vulnerabilities early in the development process, improving your software's security.

  

#### Key Insights on Semgrep

First of all, install Semgrep with the following command:

```
python3 -m pip install semgrep
```

Semgrep accepts two fundamental input:

-   **Rules collection**: A collection is composed by ".yaml" files, alternatively referred to as "rules". A rule includes a series of patterns designed to identify or exclude specific elements within the target source code.
-   **Target source code**: This denotes the source code subject to analysis. It may also encompass partial code or code with certain dependencies omitted.

The four main elements you can find inside a [Semgrep rule](https://semgrep.dev/docs/writing-rules/pattern-syntax/) yaml file are:

...

Match a sequence of zero or more items such as arguments, statements, parameters, fields, characters.

"..."

Match any single hardcoded string.

$A

Match variables, functions, arguments, classes, object methods, imports, exceptions, and more.

<... e ...>

Match an expression ("e") that could be deeply nested within another expression.

  

Moreover, Semgrep provides several **experimental modes** that could be really useful in more difficult situations:

-   **[taint](https://semgrep.dev/docs/writing-rules/data-flow/taint-mode/)**: It enables the data-flow analysis feature allowing to specify **sources** and **sinks**.
-   **[join](https://semgrep.dev/docs/writing-rules/experiments/join-mode/overview/)**: It allows to use **multiple rules** on more than one file and to join the results.
-   **[extract](https://semgrep.dev/docs/writing-rules/experiments/extract-mode/#introduction)**: It allows work with source file that contains **different programming languages**.

Suppose to have a rules collection in the directory "myrules/" and a target source code "mytarget/". To launch a Semgrep scan is very simple:

```
semgrep -c ./myrules ./mytarget
```

  

The Project:
------------

Semgrep Rules for Android Application Security
----------------------------------------------

#### The proposal

In March 2023, the **IMQ Minded Security** team, with the purpose of contributing to the ethical hacking and mobile development communities, began the "Semgrep Rules for Android Application Security" project. The primary objective of this project is to provide a collection of Semgrep rules that cover the static tests described in the OWASP Mobile Application Security Testing Guide (MASTG) for Android applications. The project has been publicly released on the IMQ Minded Security official GitHub page:

-   [https://github.com/mindedsecurity/semgrep-rules-android-security](https://github.com/mindedsecurity/semgrep-rules-android-security)

  

Currently, the project boasts more than 10 internal and external contributors with different degrees of seniority.

**Supervisor**:

-   Stefano Di Paola (Twitter: [@WisecWisec](https://twitter.com/WisecWisec))

**Project leader**:

-   Riccardo Cardelli (Github: [@gand3lf](https://github.com/gand3lf))

**Contributors (In alphabetical order)**: 

-   Andrea Agnello (Github: [@](https://github.com/andrenoli)[AndreNoli](https://github.com/andrenoli))
-   Christian Cotignola (Twitter: [@b4dsheep](https://twitter.com/b4dsheep))
-   Federico Dotta (Twitter: [@apps3c](https://twitter.com/apps3c))
-   Giacomo Zorzin (Mastodon: [@gellge](https://infosec.exchange/@gellge))
-   Giovanni Fazi (Github: [@giovifazi](https://github.com/giovifazi))
-   Martino Lessio (Twitter: [@Martinolessio](https://twitter.com/martinolessio))
-   Maurizio Siddu (Github: [@akabe1](https://github.com/akabe1))
-   Michele Di Bonaventura (Twitter: [@cyberaz0r](https://twitter.com/cyberaz0r))
-   Michele Tumolo (Twitter: [@0s0urce](https://twitter.com/0s0urce))
-   Riccardo Granata (Github [@riccardogranata](https://github.com/riccardogranata))

The "_[Semgrep Rules for Android Application Security](https://github.com/mindedsecurity/semgrep-rules-android-security)_" project does not cover the entire OWASP MASTG tests due to the intrinsic constraints of a mobile application SAST activity:

-   The back-end source code is out of scope.
-   The dynamic tests are out of scope.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhri04T_tuzbsGPMMCkTEq9hgn7d4LvuGSVq8-gdJYivcKf5gY6gdQxnew1bF01N1os4i_pfMjiAg9NbQkhNDksijUCNQAM3yfESgnq-IlBrF0yKZDrV0JwleHiNwG6x16Dcg6JnqUFuP2ryZJR7Sifs5pPpBLGPqOioHgWs8yq5bHQKxd8C37KSM5wYuM/s16000/mastg_1_5.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhri04T_tuzbsGPMMCkTEq9hgn7d4LvuGSVq8-gdJYivcKf5gY6gdQxnew1bF01N1os4i_pfMjiAg9NbQkhNDksijUCNQAM3yfESgnq-IlBrF0yKZDrV0JwleHiNwG6x16Dcg6JnqUFuP2ryZJR7Sifs5pPpBLGPqOioHgWs8yq5bHQKxd8C37KSM5wYuM/s1353/mastg_1_5.png)

  

To use the project during an MAPT activity is very simple:

  

```
# Download the target APK and the rules of the current project 
$ git clone https://github.com/mindedsecurity/semgrep-rules-android-security
# Extract and decompile the source code from the target APK file
$ jadx -d target_src target.apk
# To use the .semgrepignore file launch the scan from the project folder
$ cd semgrep-rules-android-security/# Run Semgrep with the new security rules
$ semgrep -c ./rules/ ../target_src/
```

  

---

Some Implemented Rules
----------------------

The rules implemented are **more than 40** and this section contains a detailed description of four of the Semgrep rules included in the _"Semgrep Rules for Android Application Security"_ project.

  

#### MSTG-STORAGE-3

The detailed description of the current test can be visited at the following MASTG 1.5.0 reference:

-   [https://github.com/OWASP/owasp-mastg/blob/v1.5.0/Document/0x05d-Testing-Data-Storage.md#testing-logs-for-sensitive-data-mstg-storage-3](https://github.com/OWASP/owasp-mastg/blob/v1.5.0/Document/0x05d-Testing-Data-Storage.md#testing-logs-for-sensitive-data-mstg-storage-3)

As stated in the MSTG-STORAGE-3 test, the purpose involves identifying any **sensitive data** within both system and **application logs**.

For doing this, it is important to retrieve two information:

1.  The methods and classes that are delegated to perform logging operations.
2.  A regex that can be used to identify potentially sensitive data and attributes names.

The first information can be partially retrieved in the _MSTG-STORAGE-3_ description. The identified methods are the following:

-   _Log.v | Log.i | Log.w | Log.e | Log.wtf_
-   _System.out.print | System.err.print | System.out.println | System.err.println_
-   _Logger.log | Logger.info | Logger.logp | Logger.logrb | Logger.severe | Logger.warning_

Please note that the "_Log.d_" method is not included in the list because it prints log data only if the Android manifest contains the flag "_android:debuggable_" set to true. This requirement is verified by another implemented rule.

About the second information, the regex used to identify secrets inside the source code is the following:

_.\*(?i)(key|secret|password|pwd|passwd|token|salt|seed|salt|bearer|otp|crypt|auth(?-i)|IV).\*_

  

The following snippet shows the result translated in Semgrep rule pattern language:

```
    message: The application writes sensitive data in application logs.
    patterns:
      - pattern-either:
          - pattern: Log.v(...);
          - pattern: Log.i(...);
          - pattern: Log.w(...);
          - pattern: Log.e(...);
          - pattern: Log.wtf(...);
          - pattern: System.$X.print(...);
          - pattern: System.$X.println(...);
          - pattern: (BufferedWriter $X).write(...);
          - pattern: (Logger $X).log(...);
          - pattern: (Logger $X).info(...);
          - pattern: (Logger $X).logp(...);
          - pattern: (Logger $X).logrb(...);
          - pattern: (Logger $X).severe(...);
          - pattern: (Logger $X).warning(...);
      - pattern-regex: .*(?i)(key|secret|password|pwd|...|bearer|otp|crypt|auth(?-i)|IV).*
```

Patterns nested under a "patterns" node operate with a **logical AND** condition, whereas the "pattern-either" is used to represent a **logical OR** condition. The following pattern is equivalent to the logical condition _(A OR B) AND C_.

```
patterns:
    - pattern-either:
        - pattern: A
        - pattern: B
    - pattern: C
```

The final version of the rule can be consulted here:

-   [https://github.com/mindedsecurity/semgrep-rules-android-security/blob/main/rules/storage/mstg-storage-3.yaml](https://github.com/mindedsecurity/semgrep-rules-android-security/blob/main/rules/storage/mstg-storage-3.yaml)

  

#### MSTG-ARCH-9

The purpose of this test is to verify that the application **enforces updates**. This requirement regards applications with **L2** as **verification level**, such as banking applications, healthcare portals, government application, e-commerce platforms, and more. To ensure that these targets have the latest security patches installed, they should be required to use the most recent version of the application.

The details of the test can be consulted at the following link:

-   [https://github.com/OWASP/owasp-mastg/blob/v1.5.0/Document/0x05h-Testing-Platform-Interaction.md#testing-enforced-updating-mstg-arch-9](https://github.com/OWASP/owasp-mastg/blob/v1.5.0/Document/0x05h-Testing-Platform-Interaction.md#testing-enforced-updating-mstg-arch-9)

To update an Android application programmatically, at least three steps are required:

1.  Check for updates ("_getAppUpdateInfo(...)_").
2.  Request the update ("_startUpdateFlowForResult(...)_").
3.  Check if the update is completed successfully.

The fundamental step that should be detected by the Semgrep rule is the second one. Unfortunately, it is necessary to face with a Semgrep **limitation**:

_At the current version, Semgrep does not provide a mechanism to detect the absence of a pattern_.

(Visit the link for further information: [https://github.com/returntocorp/semgrep/issues/7363](https://github.com/returntocorp/semgrep/issues/7363))

  

In other words, when dealing with a source code composed of N files, it is not possible to detect the absence of a pattern among all these files using Semgrep. However, it is possible to detect the absence of a pattern within a **single** specific file.

  

Thus, the adopted strategy is the following:

1\. The main activity is the file that is most likely to enforce the update. This is necessary to confine the search to one single file. To identify the main activity class, it is sufficient to look for the activity with the action "_android.intent.action.MAIN_" and the category "_android.intent.category.LAUNCHER_" inside the "intent-activity" element.

  

```
<!-- AndroidManifest.xml -->
<activity android:name="com.myexample.test.SplashScreen">
  <intent-filter>
    <action android:name="android.intent.action.MAIN"/>
    <category android:name="android.intent.category.LAUNCHER"/>
  </intent-filter>
</activity>
```

  

2\. The application must call the method "startUpdateFlowForResult" to enforce the application update.

  

Clearly, this rule requires dealing with files in different format: XML and Java.

In this situation, it is possible to use the Semgrep join mode, that allows to combine results from different rules:

-   [https://semgrep.dev/docs/writing-rules/experiments/join-mode/overview/](https://semgrep.dev/docs/writing-rules/experiments/join-mode/overview/)

The "on" directive allows to specify constraints that must match between the result of the rules. Consider the following example of join mode:

```
rules:
  -id: join-rule-id
  mode: join
  join:
    rules:
      - id: rule1
        languages: # . . . 
        pattern: # . . .
      - id: rule2
        languages: # . . .
        pattern: # . . .
    on:
      - 'rule1.$A == rule2.$B' # 1
      - 'rule1.$X > rule2.$Y' # 2
```

  

Consider the "on" conditions in AND logic:

-   "_\# 1_": the metavariable _$A_ is equal to the metavariable _$B_
-   "_\# 2_": the metavariable _$X_ contains the metavariable _$Y_

The following snippet reports the implemented rule to cover the MSTG-ARCH-9 test.

```
    mode: join
    join:
        rules:
          - id: activity-without-update
            languages:
                - java
            patterns: # 1
                - pattern: |
                    public class $CLASSNAME extends $ACTIVITY{
                        public void onCreate(...){...}
                    }
                - pattern-not: |
                    public class $CLASSNAME extends $ACTIVITY{
                        $X(...){
                        ...
                        (AppUpdateManager $Y).startUpdateFlowForResult(...);
                        ...
                        }
                    }
                - focus-metavariable:
                    - $CLASSNAME
          - id: main-activity
            languages:
                - xml
            patterns: # 2
                - pattern: |
                    <activity ... android:name="$ACT" ...> ...
                    <intent-filter> ...
                    <action android:name="android.intent.action.MAIN"/> ...
                    <category android:name="android.intent.category.LAUNCHER"/> ...
                    </intent-filter> ...
                    </activity>
                - focus-metavariable:
                    - $ACT
        on:
          - 'main-activity.$ACT > activity-without-update.$CLASSNAME'
```

  

The first pattern ("_\# 1_") matches any Java class that does not use the method "startUpdateFlowForResult" focusing the class name with the "focus-metavariable" directive. The second pattern ("_\# 2_") retrieves, inside the "AndroidManifest.xml" file, the name of the main activity.

  

The condition "_main-activity.$ACT > activity-without-update.$CLASSNAME_" means that the metavariable _$ACT_ (from the "main-activity" rule) has to contain the value of the metavariable _$CLASSNAME_ (from the "activity-without-update" rule).

  

Combining what has been written, the rule presented verifies exactly that the main activity of the target application contains a call to the standard method used to update the Android applications.

The most updated version of the rule can be consulted here:

-   [https://github.com/mindedsecurity/semgrep-rules-android-security/blob/main/rules/arch/mstg-arch-9.yaml](https://github.com/mindedsecurity/semgrep-rules-android-security/blob/main/rules/arch/mstg-arch-9.yaml)

####   

#### MSTG-STORAGE-9

To prevent malicious applications from taking **screenshots** of **sensitive information**, it is a good security practice to disable the screenshot functionality when using applications that display sensitive information.

The OWASP MASTG provides details on how to correctly configure the application:

-   [https://github.com/OWASP/owasp-mastg/blob/v1.5.0/Document/0x05d-Testing-Data-Storage.md#finding-sensitive-information-in-auto-generated-screenshots-mstg-storage-9](https://github.com/OWASP/owasp-mastg/blob/v1.5.0/Document/0x05d-Testing-Data-Storage.md#finding-sensitive-information-in-auto-generated-screenshots-mstg-storage-9)

This topic has been extensively described in this blog in 2021 by the software security expert Martino Lessio:

-   [https://blog.mindedsecurity.com/2021/05/mobile-screenshot-prevention-cheatsheet.html](https://blog.mindedsecurity.com/2021/05/mobile-screenshot-prevention-cheatsheet.html)

The Android Window object can be retrieved using the "_getWindow_" method and contains all the methods to interact with the application screen view.

In particular, the "_setFlags_" and "_addFlags_" methods permit to associate some features to the window.

  

```
public void setFlags (int flags, int mask)
public void addFlags (int flags)
```

  

One of these features is "**FLAG\_SECURE**", that states:

"Treat the content of the window as secure, preventing it from appearing in screenshots or from being viewed on non-secure displays."

The constant value associated to the flag is 8192 (_0x00002000_).

  

To detect an insecure use of the "setFlags" method, the following rule can be used:

  

```
- patterns:
  - pattern-either:
    - pattern: getWindow().setFlags($P1, $P2)
    - pattern: (...).getWindow().setFlags($P1, $P2)
    - pattern: (Window $W).setFlags($P1, $P2)
  - metavariable-comparison:
      comparison: $P1 & 8192 == 0
```

  

Moreover, the default value of the window flags does not include the "_FLAG\_SECURE_" by default, so we have to detect all activities that do not include this flag explicitly:

  

```
- patterns:
  - pattern: public class $CLASS extends $ACT{ ... }
  - pattern-not: |
      public class $CLASS{...
        $M(...){...
          (...).addFlags(...);
        ...}
      ...}
  - pattern-not: |
      public class $CLASS{...
        $M(...){...
          (...).setFlags(...);
        ...}
      ...}
  - metavariable-regex:
      metavariable: $ACT
      regex: .*Activity.*
```

  

Unfortunately, it is not possible to exhaustively cover the misuse of "addFlags" due to a Semgrep limitation. In other words, it is not possible to search for a method that does not contain an "addFlags" call with a parameter that adheres to specific characteristics.

In this case, what we can do is the following attempt:

  

```
- patterns:
  - pattern-either:
    - pattern: getWindow().addFlags($P1, $P2)
    - pattern: (...).getWindow().addFlags($P1, $P2)
    - pattern: (Window $W).addFlags($P1, $P2)
  - metavariable-comparison:
      comparison: $P1 & 8192 == 0
```

  

Similarly to the previous case, this snippet detect each "_addFlags_" call that does not use the "_FLAG\_SECURE_".

This approach can introduce false positives, such as:

  

```
public class MainActivity extends AppCompatActivity {
     private void test(){
          getWindow().addFlags(222); // This triggers the rule, generating a false positive
          getWindow().addFlags(8192); // FLAG_SECURE
     }
}
```

  

The most updated version of the rule can be consulted here:

-   [https://github.com/mindedsecurity/semgrep-rules-android-security/blob/main/rules/storage/mstg-storage-9.yaml](https://github.com/mindedsecurity/semgrep-rules-android-security/blob/main/rules/storage/mstg-storage-9.yaml)

####   

#### MSTG-NETWORK-4

The last rule shown on this post is described at the following link:

-   [https://github.com/OWASP/owasp-mastg/blob/v1.5.0/Document/0x05g-Testing-Network-Communication.md#testing-custom-certificate-stores-and-certificate-pinning-mstg-network-4](https://github.com/OWASP/owasp-mastg/blob/v1.5.0/Document/0x05g-Testing-Network-Communication.md#testing-custom-certificate-stores-and-certificate-pinning-mstg-network-4)

Typically, tests regarding SSL pinning are performed dynamically using tools such as Frida, Objection and others. These tools help penetration testers to bypass the implementation of SSL pinning but does not analyze in details the presence of potential misconfigurations.

  

To validate the **SSL pinning configuration**, it is necessary to analyze both the Network Security Configuration file ("network\_security\_config.xml") and the Java source code of the application.

  

Thus, the rule has been splitted as follows:

-   **mstg-network-4.1.yaml** ⇒ Rule to analyze misconfiguration inside the "network\_security\_config.xml" file.
-   **mstg-network-4.1.xml** ⇒ Example of "network\_security\_config.xml" vulnerable file.
-   **mstg-network-4.2.yaml** ⇒ Eule to detect security issues inside the Java source code.
-   **mstg-network-4.2.java** ⇒ Example of insecure use of pinning inside the Java source code.

The rule "_mstg-network-4.1.yaml_" performs four checks:

-   It verifies the presence of the pin expiration date.
-   It verifies that the pin expiration date is not expired.
-   It verifies the presence of a backup pin.
-   It verifies that the trust-anchors does not use user-level certificates.

```
    pattern-either:
      # Pin expiration not present
      - patterns:
          - pattern: <pin-set ... />
          - pattern-not: <pin-set expiration="..." />
      # Pin expired
      - patterns:
          - pattern: <pin-set expiration="$X" />
          - metavariable-comparison:
              comparison: strptime($X) < today()
      # Backup pin not present
      - patterns:
          - pattern: <pin-set ... />
          - pattern-not: <pin-set><pin/><pin/></pin-set>
      # Trust anchors contains user certificates
      - patterns:
          - pattern: <trust-anchors>...<certificates src="user" />...</trust-anchors>
```

  

On the other side, the "_mstg-network-4.2.java_" performs four additional checks:

-   It verifies that the "_SSLContext_" is correctly initialized.
-   It verifies that the "_TrustManagerFactory_" is correctly initialized.
-   It verifies that the certificate pin does not use the SHA1 deprecated hashing function.
-   It verifies that an "_HttpsURLConnection_" object calls the "connect" method only if the "_SSLSocketFactory_" has been configured.

```
    pattern-either:
      - pattern: (SSLContext $X).init($P1, null, $P3);
      - pattern: (TrustManagerFactory $X).init(null);
      - patterns:
          - pattern: new CertificatePinner.Builder().add("$D", "$P")
          - metavariable-regex:
              metavariable: $P
              regex: .*(?i)(sha1/).*
      - patterns:
          - pattern: (HttpsURLConnection $X).connect();
          - pattern-not-inside: |
              (HttpsURLConnection $X).setSSLSocketFactory(...);
              ...
              (HttpsURLConnection $X).connect();
```

  

  

Tests and Results
-----------------

The project's goal does not consist in generating a large number of false positives that are subsequently reviewed by penetration testers. Instead, the real goal is to find a balance that **minimizes both false positives and false negatives**, thereby maximizing the reliability of each rule.

To test the implemented Semgrep rules, this specific workflow has been used:

1.  Download an Android application.
2.  Extract and decompile the code with _JADX_.
3.  Upload the source code on a cloud storage.
4.  Analyze the application source code, one application at the time.

  

During the first three phases, we have collected **280 Android applications** belonging to different topics with a very high number of total download: **more than 111 billions**!

The following graph shows the number of application per categories that have been included in the analysis.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhuqkKfeYeKeBbNsfu1Rj981rw_sBdBzgenkjVCd6LyLIN3ivf27ZirNBv6qdu-ABx6l6FWFUGNkT62PDmeNDSf5gC4bU20X-EiczNO_UHLe3rvBFSsJP3uYgltnPHlMXAiYNxp8itVv3ZSu3W9YGWkC1T-NdoVfco-yoh6beVi7zapA1jvK3cXWaMvOpc/s16000/app_categories.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhuqkKfeYeKeBbNsfu1Rj981rw_sBdBzgenkjVCd6LyLIN3ivf27ZirNBv6qdu-ABx6l6FWFUGNkT62PDmeNDSf5gC4bU20X-EiczNO_UHLe3rvBFSsJP3uYgltnPHlMXAiYNxp8itVv3ZSu3W9YGWkC1T-NdoVfco-yoh6beVi7zapA1jvK3cXWaMvOpc/s1352/app_categories.png)

  

The results are interesting under the quantitative point of view. During the stress test we aimed to verify:

-   The presence of rules associated to an high number of results, potentially false positives.
-   The average number of findings per application. This value has to be maintained in a reasonable range in order to allow a security expert to read and verify each result.
-   The speed of each rule in order to detect eventual bottleneck.

The following graph shows the results of the stress test:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjZQWE5pYNQ-qLjUHAlXc-QiYxQqu2BAf6uJFH1n6cj1timOgyXgNzl52X6_n3PZO4zTXO5mL6QhTVE5wPpu4ucQkxQZmxBagHxG1m0pcy70cP_pYADgybs4FpqLbHVv8NV3xYOqSzdiw9sYUv7HFYgsk26C7A6_6ibFT3IKo6_cpMRUekIT0NsOUo2wJ0/s16000/app_results.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjZQWE5pYNQ-qLjUHAlXc-QiYxQqu2BAf6uJFH1n6cj1timOgyXgNzl52X6_n3PZO4zTXO5mL6QhTVE5wPpu4ucQkxQZmxBagHxG1m0pcy70cP_pYADgybs4FpqLbHVv8NV3xYOqSzdiw9sYUv7HFYgsk26C7A6_6ibFT3IKo6_cpMRUekIT0NsOUo2wJ0/s1359/app_results.png)

  

Analyzing the results of the scan it has been possible to infer the following information:

-   Total number of findings: **107173**.
-   Average number of findings per application: **382**.
-   Most impacted categories: **storage**, **code**, **platform**.
-   The rule about memory leaks potentially produces an high number of findings ("MSTG-CODE-8.X").

Future
------

The general idea for the future of the project is to keep the rules updated. Specifically, the project will increasingly focus on improving the following three aspects.

  

**The new OWASP MATGS 2.0**:

Currently, the project is aligned with version 1.5 of the OWASP MASTG, but version **[2.0](https://mas.owasp.org/news/)** is on the horizon and promises to make tests more atomic and consistent. The IMQ Minded Security team is actively working to prepare for this important update by aligning the current rules with the future OWASP MASTG version.

  

**High impact rules**:

To cover an entire testing guide with a tool is an ambitious project that requires time, effort, and expertise. For this reason, we will continue to release new rules focusing on detecting the presence of high-impact vulnerabilities. Stay tuned and always update the ruleset!

  

**Generalization of rules**:

There are many different ways to implement the same code, depending on the specific programming language, the desired algorithm, and the programmer's personal preferences. For these reasons, it is difficult to implement rules that cover every specific scenario. The current version of the project focuses on the most standard cases and best practices, but some rules need to be updated to detect various forms of the same vulnerable pattern.

  

Talks & Events
--------------

-   **11 Sep 2023**: OWASP Italy Day

[![OWASP Italy Day - Slides](https://blogger.googleusercontent.com/img/a/AVvXsEgSWC3_LYw1zoIE-Vn9Old6dW7lozNDxhrOUPsYOmjXvaBjZqSJCoIH0Vg9dcb1sRLW2b3i1QsH0iT8Gll7znuSzMcGoJWIAsVW_L1u9QJnxDIB6LsCcDGYxg0lrC86PeRVVtBIU905fjn2w-jAdypzz23WvJeYLIGVd5Opl_eCfILARWYwJ03GNscrXbo=w464-h261)](https://github.com/OWASP/www-chapter-italy/blob/96201991fdfef280a67d3b41b85e7715d53115d8/assets/images/Riccardo%20Cardelli%20-%20OWASP%20Italy%20Day%202023%20-%2011th%20Sept%20PoliMi.pdf)

  

-   **03 Ago 2023**: DevSecCon - YouTube Live

    

  

hljs.highlightAll();

#### [Source](https://blog.mindedsecurity.com/feeds/3427156372692168997/comments/default)

<br/>
---
