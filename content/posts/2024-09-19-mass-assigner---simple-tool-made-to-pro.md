---
title: "Mass-Assigner - Simple Tool Made To Probe For Mass Assignment Vulnerability Through JSON Field Modification In HTTP Requests"
date: Thu, 19 Sep 2024 08:30:00 -0300
draft: false
type: posts
categories: 
- Mass-Assigner,Parameter,Python3,vulnerabilities
---
# Mass-Assigner - Simple Tool Made To Probe For Mass Assignment Vulnerability Through JSON Field Modification In HTTP Requests

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgmu6MGN7TQgtjTST9SXFSiRSY2sBg9g4_lWq8sl5kgH62FounVdCRJCChBFQ4_qlkXE8xcZmVlzlhuYaW9ILNC691d9Ncdgnndl8hGNaSB1X6__DxqwV1HNN6T6Y9XPtyIjrfJkwxgVCSV0RPB-GDEcXwgqnJNySk1bfC86YvfYRxu3COgign1dep2fdrX/w640-h364/Screenshot%20from%202024-09-16%2000-17-00.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgmu6MGN7TQgtjTST9SXFSiRSY2sBg9g4_lWq8sl5kgH62FounVdCRJCChBFQ4_qlkXE8xcZmVlzlhuYaW9ILNC691d9Ncdgnndl8hGNaSB1X6__DxqwV1HNN6T6Y9XPtyIjrfJkwxgVCSV0RPB-GDEcXwgqnJNySk1bfC86YvfYRxu3COgign1dep2fdrX/s1446/Screenshot%20from%202024-09-16%2000-17-00.png)

  

Mass Assigner is a powerful tool designed to identify and exploit mass assignment [vulnerabilities](https://www.kitploit.com/search/label/vulnerabilities "vulnerabilities") in web applications. It achieves this by first retrieving data from a specified request, such as fetching user profile data. Then, it systematically attempts to apply each [parameter](https://www.kitploit.com/search/label/Parameter "parameter") extracted from the response to a second request provided, one [parameter](https://www.kitploit.com/search/label/Parameter "parameter") at a time. This approach allows for the [automated](https://www.kitploit.com/search/label/Automated "automated") testing and exploitation of potential mass assignment [vulnerabilities](https://www.kitploit.com/search/label/vulnerabilities "vulnerabilities").

  

Disclaimer
----------

**This tool actively modifies server-side data. Please ensure you have proper [authorization](https://www.kitploit.com/search/label/Authorization "authorization") before use. Any unauthorized or illegal activity using this tool is entirely at your own risk.**

Features
--------

-   Enables the addition of custom headers within requests
-   Offers customization of various HTTP methods for both origin and target requests
-   Supports rate-limiting to manage request thresholds effectively
-   Provides the option to specify "ignored parameters" which the tool will ignore during execution
-   Improved the support in nested arrays/objects inside JSON data in responses

### What's Next

-   Support additional content types, such as "application/x-www-form-urlencoded"

Installation & Usage
--------------------

Install requirements

```
pip3 install -r requirements.txt
```

Run the script

```
python3 mass_assigner.py --fetch-from "http://example.com/path-to-fetch-data" --target-req "http://example.com/path-to-probe-the-data"
```

Arguments
---------

[Forbidden](https://www.kitploit.com/search/label/Forbidden "Forbidden") Buster accepts the following arguments:

```
  -h, --help            show this help message and exit  --fetch-from FETCH_FROM                        URL to fetch data from  --target-req TARGET_REQ                        URL to send modified data to  -H HEADER, --header HEADER                        Add a custom header. Format: 'Key: Value'  -p PROXY, --proxy PROXY                        Use Proxy, Usage i.e: http://127.0.0.1:8080.  -d DATA, --data DATA  Add data to the request body. JSON is supported with escaping.  --rate-limit RATE_LIMIT                        Number of requests per second  --source-method SOURCE_METHOD                        HTTP method for the initial request. Default is GET.  --target-method TARGET_METHOD                        HTTP method for the modified request. Default is PUT.  --ignore-params IGNORE_PARAMS                        Parameters to ignore during modification, separated by comma.
```

Example Usage:

```
python3 mass_assigner.py --fetch-from "http://example.com/api/v1/me" --target-req "http://example.com/api/v1/me" --header "Authorization: Bearer XXX" --proxy "http://proxy.example.com" --data '{\"param1\": \"test\", \"param2\":true}'
```

  
  

**[Download Mass-Assigner](https://github.com/Sn1r/Mass-Assigner "Download Mass-Assigner")**

[Source](http://www.kitploit.com/2024/09/mass-assigner-simple-tool-made-to-probe.html)
<br/>
---
