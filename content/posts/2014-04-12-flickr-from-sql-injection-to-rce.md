---
title: "Flickr from SQL Injection to RCE"
date: Sat, 12 Apr 2014 12:20:20 +0000
draft: false
type: posts
categories: 
- Uncategorized
---
# Flickr from SQL Injection to RCE

<br/>

<br/>
Today i will write about a new vulnerability i found in Flickr.com – How I got MYSQL root password of Flickr Database – RCE on Flickr server    Flickr Photo Books http://blog.flickr.net/en/2013/11/19/introducing-flickr-photo-books/ I got a three parameters vulnerable, when you create a page, then click on checkout, Catch the requests items=105946833&cacheBust=1394640636132&method=flickr.products.orders.create&csrf=1394665CSRFCODE&api\_key=608aa99d6d45b5ba6d0a9b23645d64d6&format=json&hermes=1&hermesClient=1&reqId=js4z8lz&nojsoncallback=1 $items was vulnerable -> Blind \[…\]

#### [Source](https://pwnrules.com/flickr-from-sql-injection-to-rce/)

<br/>
---
