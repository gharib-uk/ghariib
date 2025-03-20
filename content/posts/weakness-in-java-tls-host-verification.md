---
title: "Weakness in Java TLS Host Verification"
date: 2025-01-22
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "forensics"
  - "java"
  - "security"
  - "security-awareness"
  - "security-awareness-3-0"
  - "unicode"
  - "vulnerability"
tags: 
  - "tls"
---

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjNfphy82Pua1WHsihHar7aITooIPp_m5k0rm3TQcJ1yvrxbIFz2F-FicOyTfMSRck8-fLd17HcSGV-UQz3b5U0YmvTtU10hMRI1uQXi_hsPJkK3lsSIxgP1c3H3uxuKEC-53WIBmsUHdTz/w400-h169/oracle_thumbnail_feature%255B1%255D.png)

Unicode-related vulnerabilities have seen an increase in momentum in the past year. Last year, a Black Hat presentation by Jonathan Birch detailed how character normalization NFC/NFKC can lead to glitches in URL and host manipulation. Recently, two vulnerabilities were found in password reset functionality. The two affected applications were Django and Github. In the previous blog post, we have presented API transforming code points with potential side effects. In this post, we present one of our findings: a vulnerability affecting Oracle JDK and Open JDK host verification in the TLS communication. We are also including details from a similar weakness in Apache HTTP client.

  

## Root Cause

Character transformations have been implemented in many cases with the intent to display a string to the user. In most languages, the lowercase and uppercase operation will convert specific characters according to the Unicode standard. For a character in the ASCII range, the behavior is predictable. However, some non-ASCII characters could be transformed to a character in the ASCII range in specific circumstances. We consider character collisions when two distinct Unicode characters sequences are converted to the same sequence. These conversions can interfere in the context of security verification.

Here are few examples where a single Unicode character will be converted to one or multiple characters in the ASCII range.

### Python

```python
"u00DF".upper() => "SS"
```

### Java

```java
IDN.toASCII("u2116dejs.org") => "nodejs.org"
```

### .NET

```aspnet
new URI("https://faceboou212A.com").Host => 
"https://facebook.com"
```

These conversions can have security impacts if they happen during or after a security verification. It could cause different input strings to be considered equal after conversion.

## Case Study 1: Oracle HostnameChecker (CVE-2020-14577)

The following code is taken from the class HostnameChecker. It compares the host that it is connecting to (name) and the host that is extracted from the SSL/TLS certificate (template).

```java
private boolean isMatched(String name, String template, boolean chainsToPublicCA) {
       // Normalize to Unicode, because PSL is in Unicode.        
       name = IDN.toUnicode(IDN.toASCII(name));        
       template = IDN.toUnicode(IDN.toASCII(template));
       [....]        
       return matchAllWildcards(name, template);
```

The transformation **IDN.toUnicode() when combined with IDN.toASCII()** can lead to an unexpected code point. Some extended Unicode characters get translated to the ASCII range causing collisions with other characters.

The final hostname stored in either name or template will not properly describe the remote host.

| **Input** | **IDN.toUnicode(IDN.toASCII(Input))** |
| --- | --- |
| U+FF41 ａ | a |
| U+FF45 ｅ | e |
| \[…\] |   |
| U+212A K | k |
| U+2116 ℕ | no |
| U+2121 ℡ | tel |

_Approximately 321 Unicode code points are problematic and can cause this vulnerability._

### Risk

An attacker could potentially create a malicious certificate or a certificate signing request (CSR) with the Unicode character U+FF41 to create a collision with a domain name that contains an “a”. This is possible because the common name is part of the subject field in X.509 certificate, which supports UTF-8.

In practice, host checking is only one part of the verification, the second being the chain of trust control. It is very difficult to get an Internet Root authority to sign a CSR with a special common name. Unless there is a weak security control on the authorities’ side, it should not be possible.

Two very narrow scenarios are possible to get the required chain of trust:

- The targeted client is trusting an internal certificate authority (ie: Windows Server Certificate Authority allows CSR with UTF-8 extended character).
- The attacker has compromised a certificate authority and he is hiding the malicious certificate with Unicode collision., which is very unlikely.

As you can see, this weakness – although hazardous – will not easily allow an attacker to intercept traffic from all Java applications.

### Proof-of-Concept

If you want to see the weakness by yourself, we have published the proof-of-concept submitted to Oracle and OpenJDK. It includes the python script to generate malicious certificates.

Here is a view of the malicious certificate:

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEilVHy59Wor4XhNfdUJaQaTt8XCiVd50F09VHO-_ylOqHXruo4stEA2NrVHBEiYrImH6wrWXhauUVeWbac0pWnVBpySHRXra25UU77pxWLsGoEv4JHi0I4aCtgsmcDA6MTUWdEsIfM0xZjS/w400-h254/oracle_image_1%255B1%255D.png)

  

  

Microsoft Active Directory Certificate Services is one of the systems that would allow hostname with extend Unicode characters.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjnJ-vymwgduWVM5b-r48_kuojsyqvPl9yW8xvHazaXsrSzaWtOybRFlfsVvFM7eef0WiqZSpCM_4j6YVLCfKxzPvqpIBbzfe6JsB5Fg-37ubnXTicRv47MQacx446PB1uZcktwTDcw9a_v/w640-h536/oracle_image_2%255B1%255D.png)

  

  

### Proposed Solution

Avoiding **IDN.toASCII()** would solve the problem, as it avoids potential collisions with domains using special Unicode characters.

### Timeline

- Vulnerability reported: January 14, 2020
- Acknowledgment that an investigation has started: January 14, 2020
- Fix was released: July 2020

## Case Study 2: HTTPClient 4.5.10

The Apache library HTTPClient had a similar weakness.

In HTTPClient 4.4 and higher, the class PublicSuffixMatcherLoader was vulnerable to improper handling of Unicode encoding (CWE-176). This class is used by the DefaultHostnameVerifier class, which is in charge of verifying the hostname during the TLS handshake.

### Deep Dive Into the Code

Looking at the matches() method in the PublicSuffixMatcherLoader class, we can see that it relies on the getDomainRoot() method.

```java
public boolean matches(final String domain, final DomainType expectedType) {
        if (domain == null) {
                return false;
        }
       final String domainRoot = getDomainRoot(
       domain.startsWith(".") ? domain.substring(1) : domain, expectedType);
       return domainRoot == null;
}
```

The key implementation details of the matching are inside the getDomainRoot() method. Before the domain name in the certificate is compared against the requested one, a lowercase transformation is applied:

```java
public String getDomainRoot(final String domain, final DomainType expectedType) {
        [...]
        final String normalized = domain.toLowerCase(Locale.ROOT);
        String segment = normalized;
        String result = null;
```

The transformation String.toLowerCase() can lead to an unexpected code point. The risk of lowercasing the domain is much lower compared to the IDN.toASCII() that was used in the JDK implementation. There are only two characters that could cause issues: The Kelvin degree symbol and the Capital I with a dot above.

| **Input** | **Input.toLowerCase()** |
| --- | --- |
| K (U+212A) | k (U+006b) |
| İ (U+0130) | i̇ (U+0069 U+0307) |

Here is a small proof-of-concept that demonstrates a malicious certificate with a Kelvin degree symbol. If the hostname verification failed, it would normally throw an exception.

```java
val verifier = DefaultHostnameVerifier();

val cert = mock(X509Certificate::class.java);
`when`(cert.getSubjectX500Principal()).thenReturn(X500Principal("CN=montrehacu212A.ca, 
OU=Fake, O=Fake, C=CA"))

verifier.verify("montrehack.ca", cert);
```

### Risk

This issue found in HTTPClient has similar risks to the previously presented JDK issue. It is however limited to domain names with the letter k.

### Fix

You can view the commit made by the HttpClient team for more detail on the fix.

### Timeline

- Vulnerability reported: January 9, 2020
- Vulnerability fixed: January 14, 2020
- Release: January 23, 2020

## Conclusion

This article summarizes our findings in the Java Runtime Environment and the library HTTPClient. As explained previously, the weakness affecting the HostnameChecker is not allowing TLS interception completely. An attacker would still need to get a certificate signed by a trusted authority.

## References

Hacking GitHub with Unicode’s dotless ‘i’  
The Fall Of Mighty Django, Exploiting Unicode Case Transformations

Go to Source
