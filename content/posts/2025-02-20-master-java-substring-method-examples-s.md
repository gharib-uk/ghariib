---
title: "Master Java Substring Method Examples Syntax and Use Cases"
date: 2025-02-20T00:00:00.000Z
draft: false
type: posts
categories: 
- java
---
# Master Java Substring Method Examples Syntax and Use Cases

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

The substring() method in Java is a powerful tool for extracting parts of a string. This method always returns a new string, and the original string remains unchanged because [String is immutable in Java](/community/tutorials/string-immutable-final-java).

In this tutorial, we’ll cover its syntax, use cases, and potential pitfalls while providing practical examples and solutions to common errors.

[Syntax of substring() Method](#syntax-of-substring-method)[](#syntax-of-substring-method)
------------------------------------------------------------------------------------------

```
public String substring(int beginIndex)
public String substring(int beginIndex, int endIndex)
```

**Parameters:**

-   `beginIndex` - the starting index (inclusive).
    
-   `endIndex` (optional) - the ending index (exclusive).
    

[Java String substring() Methods](#java-string-substring-methods)[](#java-string-substring-methods)
---------------------------------------------------------------------------------------------------

[![java string substring, substring in java](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2012/11/java-string-substring.png)](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2012/11/java-string-substring.png) Java String substring method is overloaded and has two variants.

1.  `substring(int beginIndex)`: This method returns a new string that is a substring of this string. The substring begins with the character at the specified index and extends to the end of this string.
2.  `substring(int beginIndex, int endIndex)`: The substring begins at the specified beginIndex and extends to the character at index endIndex - 1. Thus the length of the substring is (endIndex - beginIndex).

[String substring() Method Important Points](#string-substring-method-important-points)[](#string-substring-method-important-points)
------------------------------------------------------------------------------------------------------------------------------------

1.  Both the string substring methods can throw `IndexOutOfBoundsException` if any of the below conditions met.
    -   if the beginIndex is negative
    -   endIndex is larger than the length of this String object
    -   beginIndex is larger than endIndex
2.  beginIndex is inclusive and endIndex is exclusive in both substring methods.

[Java String `substring()` Example](#java-string-substring-example)[](#java-string-substring-example)
-----------------------------------------------------------------------------------------------------

Here is a simple program for the substring in java.

```
package com.journaldev.util;

public class StringSubstringExample {

 public static void main(String[] args) {
  String str = "www.journaldev.com";
  System.out.println("Last 4 char String: " + str.substring(str.length() - 4));
  System.out.println("First 4 char String: " + str.substring(0, 4));
  System.out.println("website name: " + str.substring(4, 14));
 }
}
```

Output of the above substring example program is:

```
Last 4 char String: .com
First 4 char String: www.
website name: journaldev
```

### [Example 1: Extracting a Part of a String](#example-1-extracting-a-part-of-a-string)[](#example-1-extracting-a-part-of-a-string)

```
// Define a string
String str = "Hello, DigitalOcean!";
// Use the substring method to extract a part of the string
// The substring begins at the specified beginIndex (inclusive) and extends to the character at index endIndex - 1 (exclusive)
String subStr = str.substring(7, 18);
// Print the extracted substring
System.out.println(subStr); // Output: DigitalOcean
```

### [Example 2: Extracting from a List of Strings](#example-2-extracting-from-a-list-of-strings)[](#example-2-extracting-from-a-list-of-strings)

```
// This code block demonstrates how to extract the first three characters from each string in a list of words and store them in a new list.

// First, a list of words is defined using Arrays.asList.
List<String> words = Arrays.asList("apple", "banana", "cherry");

// The stream() method is used to create a stream from the list of words.
// The map() method is then used to transform each word in the stream into a new string that contains only the first three characters of the original word.
// This is achieved by calling the substring(0, 3) method on each word, which returns a new string starting from the first character (index 0) up to but not including the fourth character (index 3).
// Finally, the collect() method is used to collect the results of the mapping operation into a new list of strings.
List<String> substrings = words.stream()
    .map(word -> word.substring(0, 3))
    .collect(Collectors.toList());

// The resulting list of substrings is then printed to the console.
System.out.println(substrings); // Output: [app, ban, che]
// The output shows that the first three characters of each word have been successfully extracted and stored in a new list.
```

### [Checking Palindrome using substring() Method](#checking-palindrome-using-substring-method)[](#checking-palindrome-using-substring-method)

We can use the `substring()` method to check if a String is a palindrome or not.

```
package com.journaldev.util;

public class StringPalindromeTest {
 public static void main(String[] args) {
  System.out.println(checkPalindrome("abcba"));
  System.out.println(checkPalindrome("XYyx"));
  System.out.println(checkPalindrome("871232178"));
  System.out.println(checkPalindrome("CCCCC"));
 }

 private static boolean checkPalindrome(String str) {
  if (str == null)
   return false;
  if (str.length() <= 1) {
   return true;
  }
  String first = str.substring(0, 1);
  String last = str.substring(str.length() - 1);
  if (!first.equals(last))
   return false;
  else
   return checkPalindrome(str.substring(1, str.length() - 1));
 }
}
```

Here we are checking if the first letter and the last letter is the same or not. If they are not the same, return false. Otherwise, call the method again recursively passing the substring with the first and last letter removed.

You can checkout more string examples from our [GitHub Repository](https://github.com/journaldev/journaldev/tree/master/CoreJavaProjects/CoreJavaExamples/src/com/journaldev/string).

Here is a table comparing various use cases of the substring() method.

Use Case

Description

Example

Extracting a part of a string

Extracts a part of a string based on start and end indices.

`String part = "substringExample".substring(0, 10);`

Validating a string prefix

Checks if a string starts with a specific prefix.

`if ("substringExample".substring(0, 3).equals("sub")) { ... }`

Validating a string suffix

Checks if a string ends with a specific suffix.

`if ("substringExample".substring("substringExample".length() - 3).equals("ple")) { ... }`

Checking for a palindrome

Checks if a string is the same forwards and backwards.

`boolean isPalindrome = "madam".substring(0, 1).equals("madam".substring("madam".length() - 1));`

[Common Errors and Debugging](#common-errors-and-debugging)[](#common-errors-and-debugging)
-------------------------------------------------------------------------------------------

### [Handling `StringIndexOutOfBoundsException` Errors](#handling-stringindexoutofboundsexception-errors)[](#handling-stringindexoutofboundsexception-errors)

The `substring()` method throws a `StringIndexOutOfBoundsException` if:

1.  The `beginIndex` is negative.
    
2.  The `endIndex` is greater than the string length.
    
3.  If the `beginIndex` > `endIndex`.
    

#### Example of Error and Solution

```
// This will throw StringIndexOutOfBoundsException because:
// String length=4, requested end index=6
String text = "Java";
try {
    String sub = text.substring(2, 6);
} catch (StringIndexOutOfBoundsException e) {
    System.out.println("Invalid substring range: " + e.getMessage());
}
```

**Solution:** Always validate indices before calling `substring()`.

```
if (beginIndex >= 0 && endIndex <= text.length() && beginIndex < endIndex) {
    String sub = text.substring(beginIndex, endIndex);
}
```

### [Misuse of `substring()` with Null or Empty Strings](#misuse-of-substring-with-null-or-empty-strings)[](#misuse-of-substring-with-null-or-empty-strings)

#### Handling Null Strings

```
String str = null;
try {
    String result = str.substring(0, 2);
} catch (NullPointerException e) {
    System.out.println("Cannot extract substring from null string.");
}
```

**Solution:** Check for `null` before calling `substring()`.

```
if (str != null) {
    String result = str.substring(0, 2);
} else {
    // Handle null case appropriately
    System.out.println("Cannot extract substring from null string.");
    result = ""; // or set default value
}
```

So, here is the final code block:

```
String str = null;
String result = "";

if (str != null && str.length() >= 2) {
    result = str.substring(0, 2);
} else {
    System.out.println("Cannot extract substring - null or too short");
}

System.out.println("Result: " + result);
```

Error Type

Cause

Solution

`StringIndexOutOfBoundsException`

Using an index out of range

Validate index before calling `substring()`

`NullPointerException`

Calling `substring()` on null

Check for null before calling `substring()`

Performance Issues

Using `substring()` frequently in large strings

Consider `StringBuilder` or `StringBuffer`

[Some practical Use Cases of `substring()`](#some-practical-use-cases-of-substring)[](#some-practical-use-cases-of-substring)
-----------------------------------------------------------------------------------------------------------------------------

### [Extracting File Extensions](#extracting-file-extensions)[](#extracting-file-extensions)

```
String filename = "document.pdf";
String extension = filename.substring(filename.lastIndexOf(".") + 1);
System.out.println(extension); // Output: pdf
```

### [Parsing URLs](#parsing-urls)[](#parsing-urls)

```
String url = "https://www.digitalocean.com/community/tutorials/java-string-substring";
String domain = url.substring(8, url.indexOf("/", 8));
System.out.println(domain); // Output: www.digitalocean.com
```

[Comparing `substring()` with `split()`](#comparing-substring-with-split)[](#comparing-substring-with-split)
------------------------------------------------------------------------------------------------------------

Feature

substring()

split()

Use Case

Extracts a part of a string

Splits a string into multiple parts

Performance

Faster for simple extractions

Slower due to regex processing

Returns

Substring

Array of substrings

[Frequently Asked Questions (FAQs)](#frequently-asked-questions-faqs)[](#frequently-asked-questions-faqs)
---------------------------------------------------------------------------------------------------------

### [1\. How do you find the substring method in Java?](#1-how-do-you-find-the-substring-method-in-java)[](#1-how-do-you-find-the-substring-method-in-java)

The `substring()` method is a built-in function in Java’s String class. You can use `string.substring(startIndex, endIndex)` to extract a portion of a string. Here’s an example code block to illustrate its usage:

```
String originalString = "Hello, World!";
String extractedSubstring = originalString.substring(7, 13); // Extracts "World!"
System.out.println("Extracted Substring: " + extractedSubstring);
// Output: Extracted Substring: World!
```

### [2\. How to get a substring from a list in Java?](#2-how-to-get-a-substring-from-a-list-in-java)[](#2-how-to-get-a-substring-from-a-list-in-java)

You can use Java Streams to extract substrings from a list:

```
List<String> words = Arrays.asList("apple", "banana", "cherry");
List<String> substrings = words.stream()
    .map(word -> word.substring(0, 3))
    .collect(Collectors.toList());
```

### [3\. What is `charAt()` in Java?](#3-what-is-charat-in-java)[](#3-what-is-charat-in-java)

The `charAt(int index)` method in Java returns the character at a specified index in a string.

```
String word = "Java";
char letter = word.charAt(1);
System.out.println(letter); // Output: a
```

### [4\. How do I find the substring of a string?](#4-how-do-i-find-the-substring-of-a-string)[](#4-how-do-i-find-the-substring-of-a-string)

Use `substring(beginIndex, endIndex)`:

```
String str = "Hello, World!";
String sub = str.substring(0, 5);
System.out.println(sub); // Output: Hello
```

### [5\. How do you extract part of a string in Java?](#5-how-do-you-extract-part-of-a-string-in-java)[](#5-how-do-you-extract-part-of-a-string-in-java)

The `substring()` method is the best way to extract parts of a string. Here’s an example of how to use it:

```
String originalString = "Hello, World!";
String extractedSubstring = originalString.substring(7, 13); // Extracts "World!"
System.out.println("Extracted Substring: " + extractedSubstring);
// Output: Extracted Substring: World!
```

Another approach is using `split()` if you need multiple parts. Here’s an example of how to use it:

```
String originalString = "Hello, World!";
String[] splitStrings = originalString.split(", "); // Splits into "Hello" and "World!"
System.out.println("Split String 1: " + splitStrings[0]);
System.out.println("Split String 2: " + splitStrings[1]);
// Output:
// Split String 1: Hello
// Split String 2: World!
```

### [6\. What is the difference between `substring()` and `split()` in Java?](#6-what-is-the-difference-between-substring-and-split-in-java)[](#6-what-is-the-difference-between-substring-and-split-in-java)

The `substring()` method extracts a single portion of a string, while `split()` divides a string into an array based on a delimiter.

### [7\. Can the substring method throw an exception?](#7-can-the-substring-method-throw-an-exception)[](#7-can-the-substring-method-throw-an-exception)

Yes, `substring()` can throw a `StringIndexOutOfBoundsException` if the indices are invalid. Always validate your indices before calling `substring()`. Here’s an example of how to validate indices and avoid this exception:

```
String str = "Hello, World!";
int startIndex = 0;
int endIndex = 5; // Ensure endIndex is within the string length

// Validate indices before calling substring()
if (startIndex >= 0 && endIndex <= str.length()) {
    String sub = str.substring(startIndex, endIndex);
    System.out.println(sub); // Output: Hello
} else {
    System.out.println("Invalid indices for substring operation.");
}
```

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

In conclusion, extracting parts of a string in Java is a crucial skill for any developer. The `substring()` method is a powerful tool for extracting a single portion of a string, while `split()` is useful for dividing a string into multiple parts based on a delimiter. It’s essential to understand the differences between these methods and how to use them effectively in your code.

Additionally, being aware of the potential for `StringIndexOutOfBoundsException` when using `substring()` and taking steps to validate indices can help prevent errors in your code. By mastering these techniques, you’ll be able to manipulate strings with ease and write more efficient, effective code.

For further reading, check out these related Java tutorials:

1.  [Java String Tutorial](/community/tutorials/java-string).
2.  [Java Remove Character from String](/community/tutorials/java-remove-character-string).
3.  [String Programs in Java](/community/tutorials/string-programs-in-java).

#### [Source](https://www.digitalocean.com/community/tutorials/java-string-substring)

<br/>
---
