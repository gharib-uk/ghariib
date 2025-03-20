---
title: "How to prevent Path Traversal in NET"
date: 2018-10-23T02:18:00.000-07:00
draft: false
type: posts
categories: 
- absolute path check,asp.net,canonicalization,OWASP Top Ten,path traversal,sanitization,unzip directory traversal,validation
---
# How to prevent Path Traversal in NET

<br/>

<br/>
### Introduction

A well-known, never out of fashion and highly impact vulnerability is the Path Traversal. This technique is also known as _dot-dot-slash attack_ (../) or as a _directory traversal_, and it consists in exploiting an insufficient security validation/sanitization of user input, which is used by the application to build pathnames to retrieve files or directories from the file system, by manipulating the values through special characters that allow access to parent files.  
In Open Web Application Security Project (OWASP) terms, a path traversal attack falls under the category [A5 of the top 10 (2017)](https://www.owasp.org/index.php/Top_10-2017_A5-Broken_Access_Control): Broken Access Control, so as one of top 10 issues of 2017 we should give it a special attention.  
  

### Theoretical Concept

Most basic Path Traversal attacks can be made through the use of "../" characters sequence to alter the resource location requested from a URL. Although many web servers protect applications against escaping from the web root, different encodings of "../" sequence can be successfully used to bypass these security filters and to exploit through flawed canonicalization operations and normalization process.

  

**URL Encode :**

-   %2e%2e%2f which translates to ../
-   %2e%2e/ which translates to ../
-   ..%2f which translates to ../
-   %2e%2e%5c which translates to ..\\
-   ..%255c which translates to ..\\
-   ..%u2216 which translates to ..\\

**Valid Unicode / UTF-8 Encodings :**

-   %cc%b7 translates to   ?  (NON-SPACING SHORT SLASH OVERLAY )
-   %cc%b8 translates to    ?  (NON-SPACING LONG SLASH OVERLAY )
-   %e2%81%84 translates to   ?   (FRACTION SLASH)
-   %e2%88%95 translates to   ?   (DIVISION SLASH)
-   %ef%bc%8f translates to ?  (FULLWIDTH SLASH)

**Invalid Unicode / UTF-8 Encodings :**

-   %c1%1c translates to /
-   %c0%af translates to \\

### Practical Attack

Shall we see two attacks example, the first one exploits through an incorrect validation and sanitization of input data which are modified to access not expected resources; the second one exploits through a well-known vulnerability of some unzip libraries which doesn't use secure by default logic, allowing (via symlink) to unzip files in parent directory.

  

**Path Traversal**  
As we saw in a previous post [From Path Traversal to Source Code in Asp.NET MVC Applications](https://blog.mindedsecurity.com/2018/10/from-path-traversal-to-source-code-in.html), a Path Traversal can lead to catastrophic consequences and that is why we consider this vulnerability as a Medium/High impact.  
A request like this:  

  

Request:

GET /download\_page?id=**content.dat** HTTP/1.1  
Host: example-mvc-application.minded  
\[...\]

  

Can be tampered and exploited using ../ path sequence, and get access to configuration file.  
  

Request:

GET /download\_page?id=**..%2f..%2fweb.config** HTTP/1.1  
Host: example-mvc-application.minded  
\[...\]  
  

Response:

HTTP/1.1 200 OK  
\[...\]

<?xml version="1.0" encoding="utf-8"?>

<configuration>  
  <configSections>    <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral" requirePermission="false" />

\[...\]  
  

**Traversal in Unzip Function**

Another exploit through URI normalization abuse is the unzip directory traversal, which can be exploited using a symlink to extract file to parent directories. There are several tools to create malicious zip files, for example [Evilarc](https://github.com/ptoomey3/evilarc).  
An example of usage can be seen below:

$ python evilarc.py minded.aspx --path inetpub/wwwroot/ --os unix --depth 9 --output-file minded.zip
Creating minded.zip containing ../../../../../../../../../inetpub/wwwroot/minded.aspx

And here is the structure of the resulting zip file:  

$ unzip -l minded.zip 
Archive:  minded.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
     1254  2018-10-15 15:31   ../../../../../../../../../inetpub/wwwroot/minded.aspx
---------                     -------
     1254                     1 file 

Many common zip programs (Winzip, etc) will prevent extraction of zip files whose embedded files contain paths with directory traversal characters. However, many software development libraries do not include same protection mechanisms. This year a good list of impacted libraries has been made with [Zip Split Disclosuring Project](https://github.com/snyk/zip-slip-vulnerability), which collects all projects has been involved in this security leakage.

  

### Vulnerable code

Here we will see some vulnerable code example, which use different approach in order to attempt to fix path traversal, but without succeeding.

#### Incorrect Path Validation

When we talk about validation we refer to the verification of data being submitted, to be sure that conforms to a rule or set of rules. These could be a simple not-empty check, a complex regular expression, even a whitelist or blacklist checks.

Talking about paths, whitelist and blacklist checks aren't always possible because sometimes the expected items aren't decided before runtime execution, so it may be a good idea using a regular expression, but this must be done carefully, because defining suitable regular expressions may be practically difficult, so this may bring to security leakage.

  
See the **vulnerable** example below:  

   **Regex regex = new Regex(@"(\[a-zA-Z0-9\\s\_\\\\.\\-:\])+(.dat)");**   Match match = regex.Match(location);
   if (match.Success){}

If we try to access another file which do not have _.dat_ extension, application will prevent malicious access to the resource, like this:

  
User input          :   /../web.config  
Server validation   :   ../web.config  ->  Fail match regexp!  
Built Path          :   \\Content\\defaultContent.dat  
  
  

But since the regular expression does not verify if extension is at the latest position of matching string, this check can be exploited by providing fake path which will be ignored during resource retriving, so server does URI normalization that can be abused, like this:

  
User input          :   index.aspx?Page=fake.dat/../../web.config  
Server validation   :   fake.dat/../../web.config  ->  Success match regexp!  
Built Path          :   \\Content\\fake.dat/../../web.config  
When server will access the resource, the path will be :\\web.config.  

#### Incorrect Path Sanitization

When we talk about sanitization we refer to the manipulation of user input before it begins used in application business logic, so removing, escaping, replacing, parts of  user input in order to avoid a wrong application behavior. Talking about path, a good example of weak sanitization can be the removing of "../" characters sequence.

See the **vulnerable** example below:  
  

   location = location.Replace(@"..\\", ""); //win
   if(File.Exists(location))

This is going to remove the occurrence of "..\\" in user input, so when a path traversal is provided, it is transformed and sanitized like this:  
  

User input          :   index.aspx?Page=..\\web.config  
Server Sanitization :   ..\\web.config  ->  web.config  
Built Path          :   \\Content\\web.config   
  
  

But if we just change the back-slash ( \\ ) with slash( / ), it can be exploited again, because servers usually do URL normalization:  
  

User input          :   index.aspx?Page=../web.config  
Server Sanitization :   ../web.config  ->  ../web.config  
Built Path          :   \\Content\\../web.config   
  

One might be tempted to remove them both, but this isn't a solution because we can again exploit it throgh a double nested dot-dot-slash payload, like this :

  
User input          :   index.aspx?Page=...\\.\\web.config  
Server Sanitization :   ...\\.\\web.config  ->  ..\\web.config  
Built Path          :   \\Content\\..\\web.config   
  

While first nested "..\\" is begin removed, the second one it's ignored and bring to Path Traversal. When the server accesses the resource, the normalized path will be \\web.config.

#### Vulnerable Unzip library

When using a unzip library, you need to be careful because there may be security lackage caused by a vulnerable code, this can be a known or unknown problem in the library.  
For example if we have a look to the source of SharpZipLib library [on version which was vulnerable to traversal unzip](https://github.com/piksel/SharpZipLib/blob/0.86.0.518/src/Zip/WindowsNameTransform.cs#L130) we can see where the problem was:  

public string TransformFile(string name)
{
 if (name != null) {
  name = MakeValidName(name, \_replacementChar);
  if (\_trimIncomingPaths) {
   name = Path.GetFileName(name);
  }
  // This may exceed windows length restrictions.
  // Combine will throw a PathTooLongException in that case.
  if (\_baseDirectory != null) {
   **name = Path.Combine(\_baseDirectory, name);**
  }
 } else {
  name = string.Empty;
 }
 return name;
}

As can be seen, the basepath is simply concatenated with name of file from compressed archive, the ability to use upper-directory charaters sequence  in name of file compressed is available from zip specific, but since not all developers knows, this usually lead to path traversal issues, thats why security by default should be used in library methods, disallowing traversal path unzipping by default.  
  

### How to fix

Obviously the most effective approach is to map resource location using indirect object reference, so avoiding that source (user input) and sink (reading/writing/deleting files or directories) meet allowing exploits. However this is not always a suitable solutions , it could cost development resources or it couldn't be supported within application architecture, or just not be necessary, so in other case we can combine **path validation,** **path sanitization** and **absolute path check**;

  

The **absolute path check** means that we are going to verify from the root, if the file we are about to access is what we were expecting. In other words we segregate resources through path canonicalization, so making it absolute before using it in the application business logic. The canonicalization is a process of lossless reduction of user input to its equivalent simplest known form. In C# there is a method called "System.IO.Path.GetFullPath" which gives the canonicalized path, and we just check if starts with an authorized location.

protected string readFile(string location){
   **Regex regex = new Regex(@"(\[a-zA-Z0-9\\s\_\\\\.\\-:\])+(.dat)$");**   Match match = regex.Match(location);
   if (match.Success){
      if(File.Exists(location) **&& Path.GetFullPath(location).StartsWith(@"C:\\Applications\\Documents",StringComparison.OrdinalIgnoreCase)**)
      {
          using (StreamReader reader = new StreamReader(location))
          {
              return reader.ReadToEnd();
          }
      }
      else
      {
          return "File not found";
      }
   }
   else
   {
       return "File name not valid";
   }
}

  

#### Traversal Uzip

Before use an unzip library must be sure if has been found vulnerable to unzip directory traversal, for example checking on [Zip Split Disclosuring Project](https://github.com/snyk/zip-slip-vulnerability), on [CVE database](https://www.cvedetails.com/), or testing it as we have shown.

  
  
  
Conclusion  
  
Shall we try to do summary between approaches.  
  

Functionality

Risks

Validation  
  

Reject input which do not respect decided rules  
  

May lead to other security issue, XSS, SQL Injection even log injection  
  

Sanitization  
  

Remove unwanted characters before it begin used from application

If not in whitelist may leave some more unexpected characters

Absolute Path Check  
  

Using canonicalization verify the correct file segregation

If not validated and sanitizated the user input may lead to other security issue

  

So since Security is not a static situation, nor a destination to be reached, but rather a continuous process approaching the fix to a path traversal only with a single method can be simplistic and often not resolutive. So absolutely the best way is to use a security-oriented mentality that involves different layers of the development process (you can check out how much this orientation is in your company with the new [Minded Security Software Security 5D framework](https://www.mindedsecurity.com/index.php/services/consulting/swsecurity-5d-framework-survey)), but speaking from a technical point of view, validation, sanitization and canonicalization are 3 methods that should be complementarily used to minimize security risks.

  
  
References  

-   https://www.owasp.org/index.php/Path\_Traversal
-   http://cwe.mitre.org/data/definitions/22.html
-   https://www.owasp.org/index.php/File\_System#Path\_traversal
-   https://unicode-search.net/unicode-namesearch.pl?term=SLASH

  
  
**Author: Enrico Aleandri**

#### [Source](https://blog.mindedsecurity.com/feeds/7285552734562813771/comments/default)

<br/>
---
