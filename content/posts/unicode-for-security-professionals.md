---
title: "Unicode for Security Professionals"
date: 2025-01-22
categories: 
  - "csharp"
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "forensics"
  - "java"
  - "ruby"
  - "security"
  - "security-awareness"
  - "security-awareness-3-0"
  - "unicode"
tags: 
  - "python"
---

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjA-bXWtq_FvVNTnTeUdUg2AcROc4d1ehWeU4upby9butCxWYPxyt4r-WNq0jeNuqnmnHWgLGbLeJJB2sTM_IrAaoEKXMyrJ45D-dEZ2uE6zlBPF8M9zQrufmcMgunUtGSpAkeq4Ivz84FI/s320/unicode_thumbnail-300x157-1%255B1%255D.png)

Unicode is the de-facto standard for multilingual character encoding. UTF-8 is the most popular encoding used that supports its hundreds of thousands of characters. Aside from the encoding (byte representation of characters), Unicode defines multiple transformations that can be applied to characters. For instance, it describes the behavior of transformations such as Uppercase.

  

The character known as Long S “ſ” (U+017F) will become a regular uppercase S “S” (U+0053). Unexpected behavior for developers can often lead to security issues. Today, we will dive into the case mapping and normalization transformations. You will see how they can contribute to logic flaws in code.

Along with this article, we are sharing a list of API to look for in source code audit. We are also publishing an interactive cheat sheet for character testing.

  

## Unicode Transformations

### Case Mapping

Unexpected behavior in transformations can sometimes lead to bugs, some of them affecting software security. While the strings “gou017FFecure” and “gosecure” are not equal, a code that applies the uppercase transformation to both strings could mistakenly interpret both strings as being equal.

```javascript
> "gou017Fecure".toUpperCase().equals("gosecure".toUpperCase())
> true
```

### Normalization

Aside from the uppercasing and lowercasing, there is normalization which is also specified by Unicode. The purpose of normalization is to simplify expressions to allow matching equal or equivalent “meaning”. The comparison could be useful when implementing a search or a sort feature for example.

For simplification, we will group the forms NFC and NFD (Normalization Form Canonical Composition/Decomposition) together and do the same with NFKC and NFKD (Normalization Form Compatibility Composition/Decomposition). While their behaviors are not the same, it is identical for the characters that interest us.

### NFC/NFD

NFC/NFD is the most common form of normalization. You are likely to see it used internally in some of the functions in your favorite language. It is much stricter compared to NFKC/NFKD. There are only three characters that will normalize to ASCII characters.

|   **Original Character** |   **Normalized Character** |
| --- | --- |
|   ; (U+037E) |   ; (U+003B) |
|   ` (U+1FEF) |   \` (U+0060) |
|   K (U+212A) |   K (U+004B) |

### NFKC/NFKD

On the other hand, NFKC is a looser method of representing the equivalence of characters. It will decompose a symbol that contains multiples letters. It will also simplify exponents and stylized characters. With this normalization form, 610 characters will produce ASCII characters. Here are a few examples.

|   **Original Character** |   **Transform Character** |
| --- | --- |
|   ª (U+00AA) |   a (U+0061) |
|   ℋ (U+210B) |   H (U+0048) |
|   Ⓐ (U+24B6) |   A (U+0041) |
|   ㍲ (U+3372) |   da (U+0064 U+0061) |
|   ﹤ (U+FE64) |   < (U+003C) |
|   … and more |   … |

How does vulnerable code look like? Security bugs revolve around logic flaws. Conditions that cannot be crossed become so, thanks to permissive Unicode comparisons. An example of a logic flaw is the one that affected Django and Github: password reset based on email submission.

Here is an example of a faulty logic. In this password reset form from Django, an email value is received, the user information is fetched and an email is sent to each user that were matched. The issue is that the search is case insensitive. A special Unicode character can be used to trigger a collision. Special Unicode characters have the chance to be converted to punycode when sent to the SMTP service.

```python
class PasswordResetForm(forms.Form):
 
    def save(self, [...]):
 
        //Email from user input
        email = self.cleaned_data["email"]
        
        // [...]
 
        //Search for an email - Case _insensitive_
        for user in self.get_users(email): 
            [...]
 
            //Send an email with the original email
            self.send_mail(
                [...],
                email, 
                [...],
            )
```

Source: \[django/contrib/auth/forms.py\]

This code was fixed by using the email already stored in the database. This way even the reset form is only sent to the original email entered by the user.

## Differences in programming languages

Transformations are expected to be standardized \[1\] \[2\]. We compared the implementation in various languages. The normalizations transformation NFC, NFD, NFKC and NFKD were identical, however, the case mapping did have some small differences.

### Unicode characters producing ASCII character with lowercase transformation

|   **Character** |   **Result** |   **Python** |   **Ruby** |   **Java** |   **C#** |   **Go** |   **PHP** |   **PHP (mb\_\*)** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   İ (U+0130) |   i̇ (U+0069 U+0307) |   x |   x |   x |   x |    |    |   x |
|   K (U+212A) |   k (U+006B) |   x |   x |   x |   x |    |    |   x |

### Unicode characters producing ASCII character with uppercase transformation

|   **Character** |   **Result** |   **Python** |   **Ruby** |   **Java** |   **C#** |   **Go** |   **PHP** |   **PHP (mb\_\*)** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   ß (U+00DF) |   SS     (U+0053     U+0053) |   x |   x |   x |   \*\* |    |    |   x |
|   ı (U+0131) |   I     (U+0049) |   x |   x |   x |   x |   x |    |   x |
|   ŉ (U+0149) |   ʼN     (U+02BC     U+004E) |   x |   x |   x |    |    |    |   x |
|   ſ (U+017F) |   S     (U+0053) |   x |   x |   x |   x |   x |    |   x |
|   ǰ (U+01F0) |   J̌     (U+004A     U+030C) |   x |   x |   x |    |    |    |   x |
|   ẖ (U+1E96) |   H̱     (U+0048     U+0331) |   x |   x |   x |    |    |    |   x |
|   ẗ (U+1E97) |   T̈     (U+0054     U+0308) |   x |   x |   x |    |    |    |   x |
|   ẘ (U+1E98) |   W̊     (U+0057     U+030A) |   x |   x |   x |    |    |    |   x |
|   ẙ (U+1E99) |   Y̊     (U+0059     U+030A) |   x |   x |   x |    |    |    |   x |
|   ẚ (U+1E9A) |   Aʾ     (U+0041     U+02BE) |   x |   x |   x |    |    |    |   x |
|   ﬀ (U+FB00) |   FF     (U+0046     U+0046) |   x |   x |   x |    |    |    |   x |
|   ﬁ (U+FB01) |   FI     (U+0046     U+0049) |   x |   x |   x |    |    |    |   x |
|   ﬂ (U+FB02) |   FL     (U+0046     U+004C) |   x |   x |   x |    |    |    |   x |
|   ﬃ (U+FB03) |   FFI     (U+0046     U+0046     U+0049) |   x |   x |   x |    |    |    |   x |
|   ﬄ (U+FB04) |   FFL     (U+0046     U+0046     U+004C) |   x |   x |   x |    |    |    |   x |
|   ﬅ (U+FB05) |   ST     (U+0053     U+0054) |   x |   x |   x |    |    |    |   x |
|   ﬆ (U+FB06) |   ST     (U+0053     U+0054) |   x |   x |   x |    |    |    |   x |

There are four main observations to extract from the previous grid.

The first is that PHP does not have potential side effects when applying strtolower() or strtoupper(), it will only transform ASCII characters. When a PHP developer is introducing Unicode lowercasing, it is done willingly using the multibyte function mb\_strtolower. PHP used to provide aliasing (overloading) for the multibyte version of those functions. This is likely to introduce unexpected behaviors because the original code might not have been built with Unicode support in mind.

The second highlight is that both C# and Go only support two characters for uppercasing. In terms of internationalization, it represents an incomplete case mapping implementation. But in terms of security, this further reduces the size of the already limited character set available to perform these attacks.

\*\* The third observation is that German B in C# may not be transformed with the `toUpper()` function. It is, however, supported in APIs such as `string.Equals(input1,input2, StringComparison.CurrentCultureIgnoreCase)`.

Finally, some Unicode character will result into multiple characters where only part of those are ASCII. This has limited risk, but it is possible that output value gets transformed later to only keep the ASCII ones. For example, this python code snippet illustrates a second operation where the non-ascii characters are truncated during string decoding.

```python
>>>#First, the string is transform using uppercasing
>>> input = "u0149orthsec".upper()              # ʼNORTHSEC
>>>#Later, the string is reimported...
 >>> encoded_bytes = bytes(input,"utf-8")         # xcaxbcNORTHSEC
 >>> encoded_bytes.decode("ascii",errors='ignore')# NORTHSEC
 'NORTHSEC'
```

Transformations are not limited to the uppercase and lowercase as you will see in the next section.

## Auditing Source Code

If you are reviewing code from applications, you may be wondering which API (functions) should be reviewed. We have compiled a list below of Unicode case mapping and normalization functions. We have also included less obvious functions that under the hood applied these transformations. Standard libraries include many of those hidden transformations. We have included only those that are likely to be used when implementing security controls.

### C# (.NET)

|   **Case mapping** |   **Normalization** |   **Other** |
| --- | --- | --- |
| strings.ToLower(input)   strings.ToUpper(input)   **Regex.IsMatch(input, regex,RegexOptions.IgnoreCase);   string.Equals(input1, input2,StringComparison.CurrentCultureIgnoreCase);   new   CultureInfo(..).CompareInfo.Compare(input1,input2,   CompareOptions.IgnoreNonSpace)** | input.Normalize(NormalizationForm.FormC)   input.Normalize(NormalizationForm.FormKC) | IdnMapping().GetAscii(input)   **Uri(input).Host /   Uri(input).IdnHost /   Uri(input).SafeDnsHost** |

Aside from the obvious functions, regex evaluation with the IgnoreCase option supports Unicode transformations. A developer should not assume that ASCII characters are the only ones to match. values extracted from regex group will not be converted implicitly.

```csharp
Regex.IsMatch("hacu212A", "HACK", RegexOptions.IgnoreCase) == true
```

The Uri class implicitly lowercases the hostname when the class is initialized. This would only be a security issue if the hostname is validated before with a different parser.

```csharp
new Uri("http://faceboou212A.com").Host == "facebook.com"
```

### Go

|   **Case mapping** |   **Normalization** |   **Other** |
| --- | --- | --- |
| strings.ToLower(input)   strings.ToUpper(input)   **bytes.ToLower(input)   bytes.ToUpper(input)** | norm.NFC.String(input)   norm.NFKC.String(input) |  |

Go has a small number of functions to look for. One interesting aspect is that byte arrays can be lowercased and uppercased. The language assumes that byte arrays are UTF-8 encoded.

### Java

|   **Case mapping** |   **Normalization** |   **Other** |
| --- | --- | --- |
| input.toLowerCase()   input.toUpperCase() | Normalizer.normalize(url,   Normalizer.Form.NFC);      Normalizer.normalize(url,   Normalizer.Form.NFKC); | IDN.toASCII(input)   **new URI(input).toASCIIString()   SAXParser().parse(input\_path)** |

URIs are not transformed by default in Java. Calling toASCIIString() will normalize characters in the canonical form (NFC). StreamSource and SAXParser classes, used for parsing of large XML files, normalize URIs and file names received. Normalization is likely to open small opportunity for path traversal in the context of blacklisted routes. “/BACKUP/” could possibly be reachable with “BAC**u212A**UP”. The behavior will occur when using a File as argument. The string and InputSource arguments do not have this side effect.

### Python

|   **Case mapping** |   **Normalization** |   **Other** |
| --- | --- | --- |
| input.lower()   input.upper()  re.compile(regex, re.IGNORECASE).match(input)   input.casefold()   | unicodedata.normalize(‘NFC’, input)   unicodedata.normalize(‘NFKC’, input) | urllib.parse.urlparse(input).hostname |

Just like in C#, parsing a URL can lead to an implicit lowercasing of the hostname.

```python
>>> import urllib.parse
>>> urllib.parse.urlparse("http://iu006bea.com").hostname ==  
urllib.parse.urlparse("http://ikea.com").hostname
```

### Ruby

|   **Case mapping** |   **Normalization** |   **Other** |
| --- | --- | --- |
| input.upcase   input.downcase  input.match(/REGEX/im)   | input.unicode\_normalize(:nfc)   input.unicode\_normalize(:nfkc) |  |

We did not find any special APIs. The URI class is lowercasing the hostname internally. However, there is an exception that is raised if any Unicode characters other than ASCII are specified. The URI class has therefore no chance of causing harm.

## Cataloging the Characters

Characters that can be transformed to ASCII characters are the most interesting for security professionals. They are likely to be confused for another intended character. The previous code snippets showed some examples that could be used in faulty logic.

To help both developers and pentesters, we have built an interactive cheat sheet. This cheat sheet can be used by developers to build regression test cases to make sure no characters are being misinterpreted. For pentesters, the list can be used to help build payloads in the context of black-box testing. For each interesting Unicode characters, we have listed the most common encodings to facilitate the integration in JSON, HTTP GET parameters, and XML payloads.

  

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgolzApu0Z7VCZrZGlARPYkiIVs7Z5r7J11jLVMbkqGClatMqaEyEfrNHEP3vqm0JhsnSwIIlQRClZhW1FC9MUbwfT3s__E5uY9lsylHRV1MnL3rOJLyOuXNO9HkFsJ31RbQQSYWexT13lG/w640-h450/unicode-image-1-1024x719-1%255B1%255D.png) |
| --- |
|   <table align="center" cellpadding="0" cellspacing="0" class="tr-caption-container" style="margin-left: auto; margin-right: auto;"><tbody><tr><td class="tr-caption" style="text-align: center;"><span style="text-align: left;">Interactive Cheat Sheet</span></td></tr></tbody></table>   |

  

## Conclusion

Although the security problems emerging from these transformations are rare, we hope that this article can help when performing code review of security-critical component that use these. By providing an exhaustive list of characters, we hope that developers will easily have good coverage of potential glitches when doing manual dynamic tests or automate regression tests.

This article does not cover any specific vulnerabilities and is meant as a reference. However, we are going to release a follow-up blog post with specific vulnerabilities in popular software products such as the Oracle JDK and Apache’s HTTPClient library. Do not miss this!

Go to Source
