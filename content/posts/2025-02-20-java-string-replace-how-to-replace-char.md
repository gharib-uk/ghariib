---
title: "Java String Replace How to Replace Characters and Substrings"
date: 2025-02-20T01:02:50.940Z
draft: false
type: posts
categories: 
- 
---
# Java String Replace How to Replace Characters and Substrings

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

In this article, you’ll learn a few different ways to remove a character from a `String` object in Java. Although the `String` class doesn’t have a `remove()` method, you can use variations of the `replace()` method and the `substring()` method to remove characters from strings.

**Note:** `String` objects are immutable, which means that they can’t be changed after they’re created. All of the `String` class methods described in this article return a new `String` object and do not change the original object. The type of string you use depends on the requirements of your program. Learn more about [other types of string classes](/community/tutorials/string-vs-stringbuffer-vs-stringbuilder) and [why strings are immutable in Java](/community/tutorials/string-immutable-final-java).

The `String` class has the following methods that you can use to replace or remove characters:

-   `replace(char oldChar, char newChar)`: Returns a new `String` object that replaces all of the occurrences of `oldChar` in the given string with `newChar`. You can also use the `replace()` method, in the format `replace(CharSequence target, CharSequence replacement)`, to return a new `String` object that replaces a substring in the given string.
-   `replaceFirst(String regex, String replacement)`: Returns a new `String` object that replaces the first substring that matches the regular expression in the given string with the replacement.
-   `replaceAll(String regex, String replacement)`: Returns a new `String` object that replaces each substring that matches the regular expression in the given string with the replacement.
-   `substring(int start, int end)`: Returns a new `String` object that contains a subsequence of characters currently contained in this sequence. The substring begins at the specified start and extends to the character at index end minus 1.

Notice that the first argument for the `replaceAll()` and `replaceFirst()` methods is a [regular expression](/community/tutorials/regular-expression-in-java-regex-example). You can use a regular expression to remove a pattern from a string.

**Note:** You need to use double quotes to indicate literal string values when you use the `replace()` methods. If you use single quotes, then the JRE assumes you’re indicating a character constant and you’ll get an error when you compile the program.

[Remove a Character from a String in Java](#remove-a-character-from-a-string-in-java)[](#remove-a-character-from-a-string-in-java)
----------------------------------------------------------------------------------------------------------------------------------

You can remove all instances of a character from a string in Java by using the `replace()` method to replace the character with an empty string. The following example code removes all of the occurrences of lowercase “`a`” from the given string:

```
String str = "abc ABC 123 abc";
String strNew = str.replace("a", "");
```

```
Outputbc ABC 123 bc
```

[Remove Spaces from a String in Java](#remove-spaces-from-a-string-in-java)[](#remove-spaces-from-a-string-in-java)
-------------------------------------------------------------------------------------------------------------------

You can remove spaces from a string in Java by using the `replace()` method to replace the spaces with an empty string. The following example code removes all of the spaces from the given string:

```
String str = "abc ABC 123 abc";
String strNew = str.replace(" ", "");
```

```
OutputabcABC123abc
```

[Remove a Substring from a String in Java](#remove-a-substring-from-a-string-in-java)[](#remove-a-substring-from-a-string-in-java)
----------------------------------------------------------------------------------------------------------------------------------

You can remove only the first occurrence of a character or substring from a string in Java by using the `replaceFirst()` method to replace the character or substring with an empty string. The following example code removes the first occurrence of “`ab`” from the given string:

```
String str = "abc ABC 123 abc";
String strNew = str.replaceFirst("ab", "");
```

```
Outputc ABC 123 abc
```

[Remove all the Lowercase Letters from a String in Java](#remove-all-the-lowercase-letters-from-a-string-in-java)[](#remove-all-the-lowercase-letters-from-a-string-in-java)
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

You can use a regular expression to remove characters that match a given pattern from a string in Java by using the `replace.All()` method to replace the characters with an empty string. The following example code removes all of the lowercase letters from the given string:

```
String str = "abc ABC 123 abc";
String strNew = str.replaceAll("([a-z])", "");
```

```
OutputABC 123
```

[Remove the Last Character from a String in Java](#remove-the-last-character-from-a-string-in-java)[](#remove-the-last-character-from-a-string-in-java)
-------------------------------------------------------------------------------------------------------------------------------------------------------

There is no specific method to replace or remove the last character from a string, but you can use the [String substring()](/community/tutorials/java-string-substring) method to truncate the string. The following example code removes the last character from the given string:

```
String str = "abc ABC 123 abc";
String strNew = str.substring(0, str.length()-1);
```

```
Outputabc ABC 123 ab
```

[Try it out](#try-it-out)[](#try-it-out)
----------------------------------------

The following example file defines a class that includes all of the method examples provided in this article, and prints out the results after invoking each method on the given string. You can use this example code to try it out yourself on different strings using different matching patterns and replacement values.

If you have Java installed, you can create a new file called `JavaStringRemove.java` and add the following code to the file:

JavaStringRemove.java

```

public class JavaStringRemove {

 public static void main(String[] args) {
     String str = "abc ABC 123 abc";

  // Remove a character from a string in Java
  System.out.println("String after removing all the 'a's = "+str.replace("a", ""));

  // Remove spaces from a string in Java
  System.out.println("String after removing all the spaces = "+str.replace(" ", ""));

  // Remove a substring from a string in Java 
  System.out.println("String after removing the first 'ab' substring = "+str.replaceFirst("ab", ""));

  // Remove all the lowercase letters from a string in Java
  System.out.println("String after removing all the lowercase letters = "+str.replaceAll("([a-z])", ""));

  // Remove the last character from a string in Java
  System.out.println("String after removing the last character = "+str.substring(0, str.length()-1));
 }

}
```

Compile and run the program:

```
javac JavaStringRemove.java
java JavaStringRemove
```

You get the following output:

```
OutputString after removing all the 'a's = bc ABC 123 bc
String after removing all the spaces = abcABC123abc
String after removing the first 'ab' substring = c ABC 123 abc
String after removing all the lowercase letters =  ABC 123
String after removing the last character = abc ABC 123 ab
```

Each method in the `JavaStringRemove` example class operates on the given string. The output shows that the characters specified in each method have been removed from the string.

[Regex in String Replacement](#regex-in-string-replacement)[](#regex-in-string-replacement)
-------------------------------------------------------------------------------------------

In Java, `replaceAll()` enables regex pattern replacements. This method uses regular expressions to match patterns in the string and replace them with a specified replacement. Here’s an example of how to use `replaceAll()` to replace all digits in a string with asterisks:

```
String text = "123abc456";
String replaced = text.replaceAll("\\d", "*");
System.out.println(replaced); // "***abc***"
```

In this example, the regular expression `\\d` matches any digit. The `replaceAll()` method then replaces each matched digit with an asterisk (`*`).

You can also read more about regular expressions in this tutorial on [Regular Expression in Java - Java Regex Example](/community/tutorials/regular-expression-in-java-regex-example).

[Immutable Strings in Java](#immutable-strings-in-java)[](#immutable-strings-in-java)
-------------------------------------------------------------------------------------

In Java, strings are immutable, which means that once a string is created, its contents cannot be modified. This immutability is a fundamental property of the `String` class in Java. When you attempt to modify a string, a new string object is created, and the original string remains unchanged. This behavior is in contrast to mutable objects, which can be modified after creation.

The immutability of strings in Java has several implications. For instance, when you use methods like `replace()`, `replaceAll()`, or `substring()`, a new string object is created, and the original string is not modified. This can be beneficial for thread safety and caching, as strings can be safely shared without worrying about their contents being modified.

However, the immutability of strings can also lead to performance issues if not handled properly. For example, if you need to perform multiple modifications on a string, creating a new string object for each modification can be inefficient. To address this, Java provides the `StringBuilder` class, which is a mutable sequence of characters. Using `StringBuilder` can improve efficiency when performing multiple string modifications, as it allows you to modify the string without creating a new object for each change.

```
// This code block demonstrates the use of StringBuilder to modify a string in Java.
// It creates a StringBuilder object with the initial string "hello world".
StringBuilder sb = new StringBuilder("hello world");
// The replace method is then used to modify the string by replacing the characters
// starting from index 6 (inclusive) to 11 (exclusive) with the string "Java".
// This effectively changes the original string "hello world" to "hello Java".
sb.replace(6, 11, "Java");
// Finally, the modified string is printed to the console using the toString method.
System.out.println(sb.toString()); // "hello Java"
```

You can refer to this tutorial on [Java strings](/community/tutorials/java-string) for more details.

[FAQs](#faqs)[](#faqs)
----------------------

### [1\. How do you replace a character in a Java string?](#1-how-do-you-replace-a-character-in-a-java-string)[](#1-how-do-you-replace-a-character-in-a-java-string)

Use the `replace()` method:

```
String str = "hello";
String newStr = str.replace('l', 'p');
System.out.println(newStr); // "heppo"
```

### [2\. What is the difference between `replace()` and `replaceAll()`?](#2-what-is-the-difference-between-replace-and-replaceall)[](#2-what-is-the-difference-between-replace-and-replaceall)

Feature

`replace()`

`replaceAll()`

Usage

Works with plain text (characters or substrings).

Uses regex for replacements.

Example

`String str = "hello"; str.replace("l", "p");`

`String str = "123abc456"; str.replaceAll("\\d", "*");`

### [3\. Can I replace a string using regex in Java?](#3-can-i-replace-a-string-using-regex-in-java)[](#3-can-i-replace-a-string-using-regex-in-java)

Yes, use `replaceAll()`:

```
String text = "123abc456";
String replaced = text.replaceAll("\\d", "*");
System.out.println(replaced); // "***abc***"
```

### [4 Why is my Java string not changing after using `replace()`?](#4-why-is-my-java-string-not-changing-after-using-replace)[](#4-why-is-my-java-string-not-changing-after-using-replace)

Java strings are immutable, meaning they cannot be changed once created. This is important to keep in mind when using methods like `replace()`, as they do not modify the original string but instead return a new string with the modifications. To see the effects of these methods, you must store the result in a variable or use it directly. Here’s an example:

```
String str = "hello";
// The replace method returns a new string with the modifications, it does not change the original string.
// To see the modified string, we need to store the result in a variable or use it directly.
str = str.replace("hello", "hi");
System.out.println(str); // "hi"
```

### [5\. How can I replace multiple different substrings in a Java string?](#5-how-can-i-replace-multiple-different-substrings-in-a-java-string)[](#5-how-can-i-replace-multiple-different-substrings-in-a-java-string)

Replacing multiple substrings in a Java string can be achieved using two approaches: chaining multiple `replace()` calls or utilizing regular expressions with `replaceAll()`. Let’s explore both methods with examples.

**Method 1: Chaining `replace()` calls**

This approach involves calling the `replace()` method multiple times on the original string, each time replacing a different substring. Here’s an example:

```
String text = "I love Java and coding.";
String replaced = text.replace("Java", "Python").replace("coding", "development");
System.out.println(replaced); // "I love Python and development."
```

In this example, we first replace “Java” with “Python”, and then replace “coding” with “development”. The order of replacements does not matter in this case, as each replacement is independent of the others.

**Method 2: Using regular expressions with `replaceAll()`**

Alternatively, you can use regular expressions to replace multiple substrings in a single call to `replaceAll()`. Here’s an example:

```
String replaced = text.replaceAll("Java|coding", "Python");
```

In this example, the regular expression “Java|coding” matches either “Java” or “coding”. The `replaceAll()` method then replaces each occurrence of these substrings with “Python”. This approach is more concise and efficient when dealing with multiple replacements.

Both methods are effective for replacing multiple substrings in a Java string. The choice between them depends on the complexity of the replacements and personal preference.

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

In this article, you learned various ways to remove characters from strings in Java using methods from the `String` class, including `replace(),` `replaceAll(),` `replaceFirst(),` and `substring().` Continue your learning with more [Java tutorials](/community/tags/java).

Please refer to these tutorials to learn more about String operations in Java:

1.  [Java String Replace: How to Replace Characters and Substrings](/community/tutorials/java-string)
2.  [Regular Expression in Java: Regex Example](/community/tutorials/regular-expression-in-java-regex-example)
3.  [How to Use Operators in Java](/community/tutorials/how-to-use-operators-in-java)

#### [Source](https://www.digitalocean.com/community/tutorials/java-remove-character-string)

<br/>
---
