---
title: "Simple PHP webshell with php filter chains"
date: Sun, 16 Apr 2023 04:00:00 -0500
draft: false
type: posts
categories: 
- 
---
# Simple PHP webshell with php filter chains

<br/>

<br/>
Recently found an LFI in a PHP application and one of the cool things I learned about recently was PHP filter chains. More info here: https://www.synacktiv.com/en/publications/php-filters-chain-what-is-it-and-how-to-use-it.html However, if you are using this in a URL, it’s pretty hard to do anything too complicated since it expands the text to the point where web servers won’t accept the URL anymore (8190 characters is default max in Apache). So I used this:

#### [Source](https://malicious.link/posts/2023/simple-php-webshell-with-php-filter-chains/)

<br/>
---
