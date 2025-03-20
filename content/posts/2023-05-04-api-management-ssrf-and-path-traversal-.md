---
title: "API Management SSRF and path traversal vulnerabilities"
date: Thu, 04 May 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# API Management SSRF and path traversal vulnerabilities

<br/>

<br/>
Azure API Management is an API gateway service meant to help organizations to create, manage, secure, and monitor APIs across all of their environments. Researchers found three high severity vulnerabilities in the service, two of which are SSRF (Server Side Request Forgery) vulnerabilities, and the third is a path traversal bug. The SSRF issues affected the Azure API Management CORS proxy (which handles schema retrieval) and hosting proxy (which routes API requests to the correct server). An attacker successful in exploiting each of these SSRF vulnerabilities could fake requests from these legitimate servers and thereby gain access to internal Azure services. However, the researchers did not determine the effective impact of this access level, and it's therefore possible that Azure had security measures in place which would have blocked further lateral movement. The path-traversal vulnerability allowed for an unrestricted file upload to the Azure developer portal server. The portal's authenticated mode allows users to upload static files and images to be displayed within the portal website, but this vulnerability could have allowed an attacker to upload code instead, and then potentially execute it on the server itself.

#### [Source](https://www.cloudvulndb.org/api-mgmt-ssrf-path-traversal)

<br/>
---
