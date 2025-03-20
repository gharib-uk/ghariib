---
title: "AWS AppFlow WooCommerce SSRF"
date: Mon, 06 Nov 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS AppFlow WooCommerce SSRF

<br/>

<br/>
The AppFlow WooCommerce connector allowed specification of a full URL. The connector included details of response content when the URL offered an unexpected response. This means you could make arbitrary GET requests to any URL from the WooCommerce connector, and view the response content. The response in the error was truncated to 500 characters.

#### [Source](https://www.cloudvulndb.org/aws-appflow-woocommerce-ssrf)

<br/>
---
