---
title: "Qodana 2024.3 Is Here Along With a Special Offer for New Users!"
date: 2025-01-04
categories: 
  - "general"
  - "linux"
tags: 
  - "dev"
  - "developers"
  - "development"
  - "jetbrains"
  - "software"
---

Happy New Year! In case you missed it, the Qodana team ended 2024 with a bang and released Qodana 2024.3. What’s in the latest release? New security functionality, a fresh set of Android linter rules, and more to help your team prioritize code excellence. Let’s take a look at what’s available in more detail.  To \[…\]

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/qd-featured_blog_1280x720_en-2.png)

Happy New Year! In case you missed it, the Qodana team ended 2024 with a bang and released Qodana 2024.3. What’s in the latest release? New security functionality, a fresh set of Android linter rules, and more to help your team prioritize code excellence. Let’s take a look at what’s available in more detail. 

1. Security analysis for Java and Kotlin

4. DFA for Go

7. New Android lint checks

10. 25% off for new users switching to Qodana

To The Documentation

## Robust security analysis for Java and Kotlin

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/image.gif)

Our latest release introduces a security analysis inspection for Java and Kotlin, enabled by default in the Qodana for JVM linter with the ultimate subscription plan. 

This means advanced interprocedural data flow analysis capabilities, providing deeper analysis for Java and Kotlin. 

As a result, you can find and fix security vulnerabilities, ensuring your code is both more efficient and more secure. 

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd1mdTm7BZIRtFaGpERbQzabmv4BSVAAZP0a2QahsTdzGsVAvOV44LehQ_WVbmqtkcroHRTLTOZm_zkiPvK_nflMnjebzjt662MMcTz-RgY48OMfzzvh2r8BOJKq8y49BVvsn8?key=1kf-F1bRMkTZjqmfG4HYkQjD)

Example of SQL injection vulnerability:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdfggh4qM1yB4FRNlNw6Sok6dmjaONBqDWMjhU-kXIMRxonoJjJ00MQ2JHwK2tdCFixGYwZMN-hyv3gje-laQUFJRzy-L79pDBC6FA-10ewW6zJ2-q8YqqonAi15W2F4vRTqJk?key=1kf-F1bRMkTZjqmfG4HYkQjD)

User data from the HTTP request is propagated to the statement.execute method without any sanitization. This allows manipulation of the SQL query being executed, which could be exploited to compromise security.

The inspection helps to detect and prevent the following types of vulnerabilities:

- Cross-Site Scripting (XSS)

- Command Injection

- SQL Injection

- Path Traversal  
    

This functionality is also available in IntelliJ IDEA Ultimate starting from v2024.3.1 with the Security Analysis by Qodana plugin installed.

## New DFA checks for qodana-go

Data Flow Analysis (DFA) is a form of static code analysis that checks how data flows through software programs. This kind of analysis can help you track errors occurring during value propagation. The Qodana 2024.3 release introduces three new dataflow based inspections enabled by default in the Go linter to help you improve the quality of your code.

These inspections, which were present in prior releases of GoLand and focused on local data flow within the function scope, have now appeared in the linter in this iteration after the RML engine was integrated as a platform module.

1. Constant Condition

This inspection identifies conditions that always evaluate to either true or false. It helps uncover redundant code branches and logical errors.

Here’s an example:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcwn58RqsLRtU2QXR4yX4qv9Xi0iEWDEfG2cyP7Xb9143iVBz_UeYXGHw0NgUln4HjvlQQIgU3rspbpZsX6xhOWFjrxoDZboAhBF9GS-Yy5nwq61YVWurA0nXsMnxqyuXXa8bs?key=1kf-F1bRMkTZjqmfG4HYkQjD)

2\. Nil dereference

This inspection highlights calls where the receiver might be nil. This inspection helps prevent runtime crashes and ensures safer code execution.

Let’s take a look at this below:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc8TSk9iNbsP0BTOAEy8OM6psOVnoNfllkBITLMoai8R3lN6vXLbMkYu8PTb-KAqX0PJg2du2p7jZX06b-5XTbPIfu9xZh9auerwNsBr9t8zhZl-j3G5pwuFhx2c0z7AiI_p0pI?key=1kf-F1bRMkTZjqmfG4HYkQjD)

3\. Error maybe not nil

This inspection highlights calls without proper error handling. The error variable should always be checked for nil before the value variable can be used.

Here’s an example of error handling to avoid:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe-zy0KZ4lJizG7MEKxTXcw1N9xQTx_dgVOJxaEtYPc1WXCrstQUBQOdmRyjkr2cqdXo_xA2sBtdilI5LlCJinCoD7tw-FKhNtQfp9t7e9S6DDxQ624u2Cx_okwDwzahuAFqgXw?key=1kf-F1bRMkTZjqmfG4HYkQjD)

app.Close invocation is safe from nil errors only when it’s checked against err == nil . In this example, app.Close was invoked without err variable checking.

This inspection highlights these kinds of instances.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfbbdpj3X0HEBtjDhLYoAqPV_ryQ1_PiqM4GJoxoXTxQbb4YKbRsgjgbjmfrc7ayNmYOh_HIMAEeMtiiYXpDewEhpGFKAXKag-Sgjm_vTUzXBumfy5Edd2o7ROHrCtEzJVy4b8?key=1kf-F1bRMkTZjqmfG4HYkQjD)

Simply update your Qodana linter to 2024.3 to enable this new functionality by default.

## Android Lint checks added to qodana-android checks

Good news! The new Android linter checks are enabled by default in your qodana.recommended profile in 2024.3 and can also be included in custom profiles. By updating these inspections, we aim to enhance the overall development experience, particularly for Android. 

### Learn more about our checks

The Android linter provides inspections for Android-specific code to help you improve code quality and maintainability. Let’s explore a few examples below. 

### A newer approach for displaying popups

#### Layout inflation without a parent (ID: AndroidLintInflateParams)

Using traditional XML inflation for displaying popups in Android is a popular approach, but one issue with inflating popups this way is that your popup\_layout.xml may rely on information from a parent view. As a result, you might run into challenges if the context of that view changes. 

The alternative is to try _Layout Inflation Without A Parent_. Find out more about this inspection here. 

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXduLT8hkckn5tkDUjFFYvUDxfT0YRNsbB_ldGdBTOo9pnsuZVCrjJwUtVipMYvG2hLmxcOnC20YIwfGbucKMDEQRfAHnUGCmOp5trmrnpc8XdirfPDZ50i4f9AGHT8pwtmwzARS?key=1kf-F1bRMkTZjqmfG4HYkQjD)

### Ensuring user understanding

#### Incomplete transition (ID: AndroidLintMissingTranslation)

Fully translating your app is crucial if you want to reach a diverse audience and ensure a good user experience for various demographics. It can also impact sales and decrease confusion. Make sure you don’t miss anything with this lint check.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc7tDmcj_paOn1z0wG6rzKk5AlXk7M4mgC8NzlN5iz8Qfr-PiVHzw0H21D73E8lsTNedt6DxK2QrX5VTHiHdx5iGegFV3k7OtegBndJXONyakXNZw3j-lPCRUjTTN5AX0xYep3M?key=1kf-F1bRMkTZjqmfG4HYkQjD)

### Maintaining responsiveness

Nested Layout Weights (ID: AndroidLintNestedWeights)

When a LinearLayout with non-zero weights is nested inside another LinearLayout with non-zero weights, then the number of measurements increases exponentially. In some cases, the responsiveness of the application will decrease if this pattern is repeated enough. You can maintain responsiveness by checking for the usage of weight attributes in nested LinearLayout components within an Android layout. 

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcicbcBTnLWdCib1JzbB-LXG7DyJvikCl1ZWTQlZ_SDlQXz61gWJHZuKijz3tQZ0L98Is-QRGP78ugmdT8MoPlX9qoc31PGNIj4E6VVG5nHmswP6WcgRQIecwtbk7FZBYpWu2n3?key=1kf-F1bRMkTZjqmfG4HYkQjD)

These are just some of the new checks you can use with Qodana on your Android project. Find the full list here. 

## **Switch to Qodana for code analysis and get 25% off**

Qodana gets better with every release and provides a cost-effective way for teams to build confidence in code quality. 

With this in mind, we’re offering you 25% off your first year of Qodana if you switch from a comparable commercial solution. Click on the button below to speak to our team. 

Switch To Qodana

Go to Source
