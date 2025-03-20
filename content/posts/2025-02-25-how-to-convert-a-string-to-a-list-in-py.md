---
title: "How to Convert a String to a List in Python Step-by-Step Guide"
date: 2025-02-25T00:00:00.000Z
draft: false
type: posts
categories: 
- python
---
# How to Convert a String to a List in Python Step-by-Step Guide

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

In Python, strings and lists are two fundamental data structures often used together in various applications. Converting a Python string to a list is a common operation that can be useful in many scenarios, such as data preprocessing, text analysis, and more.

This tutorial aims to provide a comprehensive guide on how to convert a [string](/community/tutorials/python-string-functions) to a [list in Python](/community/tutorials/understanding-lists-in-python-3), covering multiple methods, their use cases, and performance considerations. By the end of this tutorial, you will have a solid understanding of how to effectively convert strings to lists in Python, enabling you to tackle a variety of tasks with confidence.

[Python Convert String to List](#python-convert-string-to-list)[](#python-convert-string-to-list)
-------------------------------------------------------------------------------------------------

Let’s look at a simple example where we want to convert a string to list of words i.e. split it with the separator as white spaces.

```
s = 'Welcome To JournalDev'
print(f'List of Words ={s.split()}')
```

Output: `List of Words =['Welcome', 'To', 'JournalDev']`

If you are not familiar with f-prefixed string formatting, please read [f-strings in Python](/community/tutorials/python-f-strings-literal-string-interpolation)

If we want to split a string to list based on whitespaces, then we don’t need to provide any separator to the split() function. Also, any leading and trailing whitespaces are trimmed before the string is split into a list of words. So the output will remain same for string `s = ' Welcome To JournalDev '` too. Let’s look at another example where we have CSV data into a string and we will convert it to the list of items.

```
s = 'Apple,Mango,Banana'
print(f'List of Items in CSV ={s.split(",")}')
```

Output: `List of Items in CSV =['Apple', 'Mango', 'Banana']`

[Python String to List of Characters](#python-string-to-list-of-characters)[](#python-string-to-list-of-characters)
-------------------------------------------------------------------------------------------------------------------

Python String is a sequence of characters. We can convert it to the list of characters using [list()](/community/tutorials/python-list) built-in function. When converting a string to list of characters, whitespaces are also treated as characters. Also, if there are leading and trailing whitespaces, they are part of the list elements too.

```
s = 'abc$ # 321 '

print(f'List of Characters ={list(s)}')
```

Output: `List of Characters =['a', 'b', 'c', '

#### [Source](https://www.digitalocean.com/community/tutorials/python-convert-string-to-list)

<br/>
---
, ' ', '#', ' ', '3', '2', '1', ' ']` If you don’t want the leading and trailing whitespaces to be part of the list, you can use [strip() function](/community/tutorials/python-trim-string-rstrip-lstrip-strip) before converting to the list.

```
s = ' abc '

print(f'List of Characters ={list(s.strip())}')
```

Output: `List of Characters =['a', 'b', 'c']` That’s all for converting a string to list in Python programming.

You can checkout complete python script and more Python examples from our [GitHub Repository](https://github.com/journaldev/journaldev/tree/master/Python-3/basic_examples/).

[Different Methods for Converting a String to a List](#different-methods-for-converting-a-string-to-a-list)[](#different-methods-for-converting-a-string-to-a-list)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

### [1\. Using `split()`](#1-using-split)[](#1-using-split)

The `split()` method is the most common way to convert a string into a list by breaking it at a specified delimiter.

```
string = "apple,banana,cherry"
list_of_fruits = string.split(",")
print(list_of_fruits)  # Output: ['apple', 'banana', 'cherry']
```

### [2\. Using List Comprehension](#2-using-list-comprehension)[](#2-using-list-comprehension)

If you need more control over how elements are added to the list, list comprehension is a powerful option.

```
string = "hello"
list_of_chars = [char for char in string]
print(list_of_chars)  # Output: ['h', 'e', 'l', 'l', 'o']
```

### [3\. Using `json.loads()` for Structured Data](#3-using-json-loads-for-structured-data)[](#3-using-json-loads-for-structured-data)

For parsing JSON-encoded strings, `json.loads()` is the preferred method.

```
import json
string = '["apple", "banana", "cherry"]'
list_of_fruits = json.loads(string)
print(list_of_fruits)  # Output: ['apple', 'banana', 'cherry']
```

[Comparison of Methods](#comparison-of-methods)[](#comparison-of-methods)
-------------------------------------------------------------------------

Method

Use Case

Performance

`split()`

Simple delimited strings

Fast

List Comprehension

Character-by-character conversion

Moderate

`json.loads()`

Parsing structured data

Depends on size

[Handling Inconsistent Delimiters](#handling-inconsistent-delimiters)[](#handling-inconsistent-delimiters)
----------------------------------------------------------------------------------------------------------

If delimiters vary, use regular expressions with `re.split()`.

```
import re
string = "apple,banana;cherry"
list_of_fruits = re.split(r'[;,]', string)
print(list_of_fruits)  # Output: ['apple', 'banana', 'cherry']
```

[Converting Nested Data Structures](#converting-nested-data-structures)[](#converting-nested-data-structures)
-------------------------------------------------------------------------------------------------------------

Convert JSON-like strings to nested lists.

```
import json
string = '{"apple": ["red", "green"], "banana": ["yellow", "green"]}'
nested_list = json.loads(string)
print(nested_list)  # Output: {'apple': ['red', 'green'], 'banana': ['yellow', 'green']}
```

[Performance Benchmarks](#performance-benchmarks)[](#performance-benchmarks)
----------------------------------------------------------------------------

Measure the efficiency of different methods with large datasets.

```
import time

# Method 1: split()
start_time = time.time()
for _ in range(1000000):
    string = "apple,banana,cherry"
    list_of_fruits = string.split(",")
end_time = time.time()
print(f"Time taken for split(): {end_time - start_time} seconds")

# Method 2: List Comprehension
start_time = time.time()
for _ in range(1000000):
    string = "hello"
    list_of_chars = [char for char in string]
end_time = time.time()
print(f"Time taken for List Comprehension: {end_time - start_time} seconds")

# Method 3: json.loads()
start_time = time.time()
for _ in range(1000000):
    import json
    string = '["apple", "banana", "cherry"]'
    list_of_fruits = json.loads(string)
end_time = time.time()
print(f"Time taken for json.loads(): {end_time - start_time} seconds")
```

[FAQs](#faqs)[](#faqs)
----------------------

### [1\. Can we convert a string to a list in Python?](#1-can-we-convert-a-string-to-a-list-in-python)[](#1-can-we-convert-a-string-to-a-list-in-python)

Yes, you can convert a string to a list using methods like `split()`, list comprehension, or `json.loads()`. For example, using `split()`:

```
string = "apple,banana,cherry"
list_of_fruits = string.split(",")
print(list_of_fruits)  # Output: ['apple', 'banana', 'cherry']
```

### [2\. How to convert a string back to a list?](#2-how-to-convert-a-string-back-to-a-list)[](#2-how-to-convert-a-string-back-to-a-list)

You can use `.split()` for delimited strings or `json.loads()` for structured data. For example, using `json.loads()`:

```
import json
string = '["apple", "banana", "cherry"]'
list_of_fruits = json.loads(string)
print(list_of_fruits)  # Output: ['apple', 'banana', 'cherry']
```

### [3\. What is `list()` in Python?](#3-what-is-list-in-python)[](#3-what-is-list-in-python)

The `list()` function is a built-in Python function that converts an iterable (like a string, tuple, or set) into a list. This is particularly useful when you need to manipulate individual elements of an iterable or when you want to convert a string into a list of characters. For example, `list("hello")` would return `['h', 'e', 'l', 'l', 'o']`.

### [4\. How do you convert a string into a list in Python without `split()`?](#4-how-do-you-convert-a-string-into-a-list-in-python-without-split)[](#4-how-do-you-convert-a-string-into-a-list-in-python-without-split)

You can use list comprehension or `list(string)` for character-by-character conversion. For example, using list comprehension:

```
string = "hello"
list_of_chars = [char for char in string]
print(list_of_chars)  # Output: ['h', 'e', 'l', 'l', 'o']
```

### [5\. What is the difference between `split()` and `list()` in Python?](#5-what-is-the-difference-between-split-and-list-in-python)[](#5-what-is-the-difference-between-split-and-list-in-python)

`split()` divides a string by a delimiter, while `list()` converts each string character into a separate list element. For example:

```
# Using split()
string = "apple,banana,cherry"
list_of_fruits = string.split(",")
print(list_of_fruits)  # Output: ['apple', 'banana', 'cherry']

# Using list()
string = "hello"
list_of_chars = list(string)
print(list_of_chars)  # Output: ['h', 'e', 'l', 'l', 'o']
```

### [6\. How do you handle nested data in a string while converting to a list?](#6-how-do-you-handle-nested-data-in-a-string-while-converting-to-a-list)[](#6-how-do-you-handle-nested-data-in-a-string-while-converting-to-a-list)

Use `json.loads()` to parse structured JSON data into nested lists. Here’s an example:

```
import json
string = '{"apple": ["red", "green"], "banana": ["yellow", "green"]}'
nested_list = json.loads(string)
print(nested_list)  # Output: {'apple': ['red', 'green'], 'banana': ['yellow', 'green']}
```

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

In conclusion, converting a string to a list in Python is a crucial skill for any developer working with text data. This tutorial has provided a detailed exploration of the various methods available for this conversion, including the use of `split()`, `join()`, and list comprehension. Additionally, we have discussed the importance of considering performance and use cases when selecting a method.

By mastering these techniques, you will be well-equipped to handle a wide range of tasks involving string-to-list conversions, from simple text processing to complex data analysis. Whether you are working on data preprocessing, text analysis, or other applications, the ability to effectively convert strings to lists will be an invaluable asset in your Python programming toolkit.

1.  For more information on string manipulation in Python, check out [Python String Functions](/community/tutorials/python-string-functions).
    
2.  If you need to check if a string contains another string, refer to [Python Check If String Contains Another String](/community/tutorials/python-check-if-string-contains-another-string).
    
3.  Additionally, to find the length of a list in Python, see [Find the Length of a List in Python](/community/tutorials/find-the-length-of-a-list-in-python).
    
4.  To learn how to add elements to a list in Python, visit [Python Add to List](/community/tutorials/python-add-to-list).
    
5.  If you want to learn how to join a list of strings into a single string, check out [Python Join List](/community/tutorials/python-join-list).
    

These tutorials will further enhance your understanding of Python string and list manipulation.

#### [Source](https://www.digitalocean.com/community/tutorials/python-convert-string-to-list)

<br/>
---
