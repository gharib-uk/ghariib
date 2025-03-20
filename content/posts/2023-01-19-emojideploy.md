---
title: "EmojiDeploy"
date: Thu, 19 Jan 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# EmojiDeploy

<br/>

<br/>
Multiple Azure Web services use a source control management (SCM) panel powered by Kudu and enabled by default. These services were all susceptible to a CSRF vulnerability due to an overly-permissive regular expression (regex) in a filter for malformed origins. This allowed origin bypass when using a domain name structured as 'victim.scm.azurewebsites.net.\_.attacker.com' (note the use of '.\_.', which looks like an emoji). Thus, if a target Azure user were tricked into visiting a specially crafted webpage served by a domain with the above name format, an attacker could exploit this CSRF vulnerability to deploy a zip file containing a malicious payload (such as a webshell) into a target web application (via the /api/zipdeploy endpoint). This could have allowed the attacker to gain remote code execution (RCE) as the 'www' user on the target app, and potentially also lateral movement to other Azure services used by the target organization, depending on what privileges were granted to the app's managed identity.

#### [Source](https://www.cloudvulndb.org/emojideploy)

<br/>
---
