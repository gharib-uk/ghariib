---
title: "Ransomware IR with PowerForensics and the USN Journal"
date: 2016-11-21T17:44:00.003-05:00
draft: false
type: posts
categories: 
- cybersecurity,forensics,incident,ransomware,USN journal
---
# Ransomware IR with PowerForensics and the USN Journal

<br/>

<br/>
Well it's certainly been a while since I made a post!  
  
I last blogged in February about Malware analysis and you can find that post here. [http://www.redblue.team/2016/02/a-soft-introduction-to-malware-analysis.html](http://www.redblue.team/2016/02/a-soft-introduction-to-malware-analysis.html). My thanks go to Dave and Abdul for keeping content coming while I was slacking. It's hard to believe the year is almost over. It was a pretty busy one for me, I changed roles, changed companies, and moved across the country. I'll use that as my excuse for being too lazy to write anything :)  
  
I also wanted to say thanks to everyone who has viewed this blog. In the last year or so, we've managed to accumulate over 100,000 views! Honestly, the sole intent of starting this site was to document our own study efforts but we've had good feedback and hope to share content more consistently in the new year. This field is changing rapidly, and the scope of knowledge is constantly expanding. Hopefully we can help you feel a little less overwhelmed!  
  
For this post, I wanted to document an experience I had on a recent investigation and share some thoughts about how I tackled this challenge, to hopefully benefit your investigations. Specifically, we'll be looking at; Ransomware, live Incident Response using PowerForensics, and the USN Journal.  
  
So let's dig in.  
  

#### Our Scenario

Customer ABC requested assistance investigating a suspected case of Ransomware. An end user reported that file extensions had changed and they could no longer open documents. They also received a note demanding payment. The organization is curious to identify the source of the infection.

  

However there are two primary complications (both of which may be readily encountered in the real world). 

1.  The users browser history is gone (either wiped by malware or by the user themselves - they may have been browsing inappropriate content and are concerned about the IT staff identifying this).We aren't concerned about recovering this data, we want to look for an alternate method to investigate the infection.
2.  The company does not retain centralized logs, nor do they have an adequate local firewall log retention policy. There are no web browsing/firewall logs to obtain. This is of course, a finding in and of itself, but again, our focus here is working around these limitations

  

In a typical situation, carving browser history, and retrieving proxy/firewall logs would likely be sufficient in identifying malicious traffic but for this post we are assuming it is not available. It's important to have a variety of options when performing digital forensics and incident response. Conditions will often be less than ideal to it's important to find creative alternatives.

  

For this scenario we will primarily use a single tool to perform live Forensics on the compromised system, [PowerForensics](https://github.com/Invoke-IR/PowerForensics) (from DFIR guru [Jared Atkinson](https://twitter.com/jaredcatkinson)). This is an extremely powerful forensics framework composed of PowerShell (version 2 and higher) scripts that we can use to accomplish common DFIR tasks. Jared has an installation walk through but I found some of the subtle nuances of it weren't addressed in detail so I'd like to cover that here.

  

**Note**: Forensic best practices would advise you to take a complete, forensically sound, write-blocked image of the system, if possible, prior to executing any actions. You should always analyze an image and avoid working on a live system if it is confirmed as compromised. In this post we are performing live triage as it is being done in a 'lab' environment and for convenience sake. Depending on the situation, analyzing live systems may be entirely acceptable and preferred but this is completely circumstantial and is based on the complexity and potential legal outcomes of your investigation (and authorization of your superiors/client).

  

#### Tool Setup

On the infected machine:

1.  Download PowerForensics: [https://github.com/Invoke-IR/PowerForensics](https://github.com/Invoke-IR/PowerForensics).
2.  Right-Click the ZIP file, check unblock, click apply. This is required to properly install this module.
3.  Open a PowerShell command prompt as administrator.
4.  Run the command 'Set-ExecutionPolicy Unrestricted'

1.  If you have issues here consult this blog post: [https://blogs.msdn.microsoft.com/pasen/2011/12/07/set-executionpolicy-windows-powershell-updated-your-execution-policy-successfully-but-the-setting-is-overridden-by-a-policy-defined-at-a-more-specific-scope/](https://blogs.msdn.microsoft.com/pasen/2011/12/07/set-executionpolicy-windows-powershell-updated-your-execution-policy-successfully-but-the-setting-is-overridden-by-a-policy-defined-at-a-more-specific-scope/)

6.  Close and re-open the PowerShell prompt as administrator
7.  Make note of your PowerShell version using the following command:  
    $PSVersionTable.PSVersion  
    This is important as you will need to use PowerForensicsv2 depending on the installed version of PowerShell. For example, Windows Server 2008 has version 2 of PowerShell installed by default.
8.  Now you need to identify your User Module Path. Use the following command:  
    $Env:PSModulePath  
    \[Environment\]::GetEnvironmentVariable("PSModulePath")  
    This should print out a path to a series of PowerShell modules folder. There may be a series of paths but you can use one similar to: C:\\Users\\_<username>_\\Documents\\WindowsPowerShell\\Modules
9.  The path identified above is where you need to unzip PowerForensics too. Remember, if your system is running PowerShell version2, unzip PowerForensicsV2 to that location.  
    **Note**: It looks like the latest version bundles in v2 so you should be able to handle different versions in-line. You may not need to worry about unzipping the correct version.
10.  Now you can use the following commands to import PowerForensics and view the available functions it comes packaged with:  
    
    Get-Module \-ListAvailable \-Name PowerForensics
    Import-Module PowerForensics
    Get-Command \-Module PowerForensics
    

#### Background

There are lots of fantastic modules that Jared has built, but in particular we'll be using the "Get-ForensicUsnJrnl" command. You can pass it a path using the VolumeName switch to retrieve USN artifacts for a specific volume. Use the Get-Help module to obtain usage syntax for each cmdlet (Get-Help Get-ForensicUsnJrnl).

  

Why are we concerned with this specific cmdlet? To answer that we need a bit of background.

  

**NTFS** is a Microsoft proprietary file system, that is to say it is responsible for keeping track of and managing files on disk. As computing requirements have grown more complex, file system standards have evolved to meet those needs and have expanded to include a variety of different features such as hard links, alternate data streams, and so on. 

  

The **USN Journal** is a feature of NTFS which maintains a record of changes made to a specified volume. Each volume maintains a USN Journal log. The USN Journal is a system metafile, and can not be viewed in a regular directory listing. On most systems it is enabled by default as Windows uses it for a number of core system operations (like file replication services). When NTFS objects are added, changed, or deleted, an entry is recorded in the USN Journal for a respective volume. This log does not contain enough information to 'reverse' a change, it is simply a record of activity that includes the type of change, the file object impacted.

  

Each USN entry is a data structure of the following format (**Note**: there are 3 different USN structure formats depending on the version of Windows you are running, the one described below is the oldest format, however they are all pretty similar):

  

typedef struct {
  DWORD         RecordLength;
  WORD          MajorVersion;
  WORD          MinorVersion;
  DWORDLONG     FileReferenceNumber;
  DWORDLONG     ParentFileReferenceNumber;
  USN           Usn;
  LARGE\_INTEGER TimeStamp;
  DWORD         Reason;
  DWORD         SourceInfo;
  DWORD         SecurityId;
  DWORD         FileAttributes;
  WORD          FileNameLength;
  WORD          FileNameOffset;
  WCHAR         FileName\[1\];
} USN\_RECORD\_V2, \*PUSN\_RECORD\_V2, USN\_RECORD, \*PUSN\_RECORD;

  

There are several important fields here, namely; FileName, Reason, Usn, TimeStamp, FileReferenceNumber.

  

The reason field in particular records a flag that corresponds with 20+ different possible NTFS operations such as:

-   **USN\_REASON\_BASIC\_INFO\_CHANGE**
-   ****USN\_REASON\_CLOSE****
-   ******USN\_REASON\_DATA\_OVERWRITE******
-   ********USN\_REASON\_FILE\_CREATE********
-   **********USN\_REASON\_FILE\_DELETE**********

These operations are fairly self-explanatory. 

  

The USN Journal is of value to our analysis because browser operations create temporary files when accessing pages. These temporary files record Journal entries. This means that even if we lose our firewall/proxy logs and browser history, there may still be a way to identify what actions occurred at the time frame in question.

  

**Note**: It is possible to, as with anything, tamper with forensic evidence and as such an attacker could theoretically purge the USN Journal using the following command: fsutil usn deletejournal /d c:

Similarly, CCleaner/PrivaZer/etc may also clear MFT/USN Journal records. Detecting the presence of those tools may be possible through alternative artificats such as AMCache or Prefetch files.

  

#### Analysis

Keep one thing in mind - when you are dealing with the USN Journal you will be dealing with hundreds of thousands, if not millions of records. Determining what happened can be a time consuming effort so it is extremely important to narrow your focus down to a very small time frame (possibly an hour or two).

  

In our scenario, we are dealing with a a Ransomware infection, which typically leaves a ransom note on the desktop. We can perhaps use this to narrow our time frame.

  

Using PowerForensics we can pull out the MFT record for this file:

> File Name: !MADEUPFILENAME.html  
> Master File Table Record Index: 24151  
> MFT Entry for Index 24151:  
> FullName             : C:\\Users\\greg\\Desktop\\!MADEUPFILENAME.html  
> Name                 : !MADEUPFILENAME.html  
> SequenceNumber       : 32  
> RecordNumber         : 24151  
> ParentSequenceNumber : 2  
> ParentRecordNumber   : 22060  
> Directory            : False  
> Deleted              : False  
> ModifiedTime         : 3/12/1601 1:17:27 PM  
> AccessedTime         : 3/12/1601 1:17:27 PM  
> ChangedTime          : 6/8/2016 1:27:40 AM  
> BornTime             : 3/12/1601 1:17:27 PM  
> FNModifiedTime       : 6/8/2016 1:27:40 AM  
> FNAccessedTime       : 6/8/2016 1:27:40 AM  
> FNChangedTime        : 6/8/2016 1:27:40 AM  
> FNBornTime           : 6/8/2016 1:27:40 AM

  

There are some discrepancies here between the FN and SI time stamps which may be due to stomping. However the $FN attributes accurately reflect an infection time of **6/8/2016 1:27:40 AM UTC**. This is converted to **6/7/2016 6:27:40 PM** **EST** which is roughly when the user asserts they noticed an issue.  
  
But this might not be the first instance in which malware started executing, as the ransom note is usually dropped after file system encryption routine has been completed. We know that after each file is encrypted, the extension is changed to .crypz in our scenario. Consequently, we also can assert that the time of compromise would have to have happened just prior to the first file extension being renamed. We are essentially working backwards through the attack lifecycle.  
  
  

1.  Ransom Note HTML file dropped
2.  Files encrypted, extension changed
3.  Malicious payload executed
4.  Malicious payload dropped
5.  Exploit kit successful exploitation
6.  Browser instantiates Flash/JS to execute vulnerability
7.  Web page redirect to malicious landing page

  
So, we can constrain our review of the USN Journal to the first instance of .crypz minus 30 minutes to be safe. It is also useful to grep on specific extensions such as .js, .zip, .exe, .dll, .swf, .htm, and others that may be involved in the attack lifecycle to further constrain your data set.  
  
Using this technique I was able to identify the following sequence of events:  

VolumePath               : \\\\.\\C:  
Version                  : 2.0  
RecordNumber             : 33080  
FileSequenceNumber       : 28  
ParentFileRecordNumber   : 206296  
ParentFileSequenceNumber : 97  
Usn                      : 4743722112  
TimeStamp                : 6/8/2016 1:22:51 AM  
Reason                   : DATA\_EXTEND, FILE\_CREATE, CLOSE  
SourceInfo               : 0  
SecurityId               : 0  
FileAttributes           : ARCHIVE, NCI  
FileName                 : **(REDACTEDWEBPAGE)**.jpg  
  
VolumePath               : \\\\.\\C:  
Version                  : 2.0  
RecordNumber             : 33299  
FileSequenceNumber       : 209  
ParentFileRecordNumber   : 206165  
ParentFileSequenceNumber : 763  
Usn                      : 4743722424  
TimeStamp                : 6/8/2016 1:22:51 AM  
Reason                   : FILE\_CREATE  
SourceInfo               : 0  
SecurityId               : 0  
FileAttributes           : ARCHIVE, NCI

FileName                 : dsF5h3S\[1\].htm

  

VolumePath               : \\\\.\\C:  
Version                  : 2.0  
RecordNumber             : 33707  
FileSequenceNumber       : 106  
ParentFileRecordNumber   : 899  
ParentFileSequenceNumber : 21  
Usn                      : 4743725296  
TimeStamp                : 6/8/2016 1:22:51 AM  
Reason                   : FILE\_CREATE  
SourceInfo               : 0  
SecurityId               : 0

FileAttributes           : 8208

FileName                 : aojwoejfi.obtainwhite.top

  

VolumePath               : \\\\.\\C:  
Version                  : 2.0  
RecordNumber             : 33303  
FileSequenceNumber       : 620  
ParentFileRecordNumber   : 244618  
ParentFileSequenceNumber : 3  
Usn                      : 4743722896  
TimeStamp                : 6/8/2016 1:22:51 AM  
Reason                   : FILE\_CREATE  
SourceInfo               : 0  
SecurityId               : 0  
FileAttributes           : ARCHIVE, NCI  
FileName                 : truck-yield-damage-objection-journey-punish-dizzy-sl

  

                           ide-argument\[1\].swf

  
The HTM file was recovered from the file system, essentially when loaded it redirects to the obtainwhite.top domain which instantiates the SWF flash object. The randomized sub-domain (as well as odd TLD '.top), and randomized SWF file name are both highly suspect.  
  
Not surprisingly the SWF flash object was ZLIB compressed. After unpacking, it was obviously an Exploit Kit landing page used to exploit some older (2014) browser vulnerabilities. The ransomware variant was a much newer iteration at the time.  
  
Virustotal results (almost 6 months later) are somewhat discouraging for this domain:  
https://www.virustotal.com/en/url/fce8be210eecf3088d1436a627fd29893872e1e8ae04f2385dcdc92b66cede02/analysis/  
  
Registrant information for this domain:  
Updated Date: 2016-06-08T00:20:29Z Creation Date: 2016-06-08T00:09:59Z Registry Expiry Date: 2017-06-08T00:09:59Z Sponsoring Registrar: Alpnames Limited Sponsoring Registrar IANA ID: 1857 Domain Status: clientTransferProhibited https://www.icann.org/epp#clientTransferProhibited Registrant ID: alp\_54912665 Registrant Name: Clive Hoetnez Registrant Organization: N/A Registrant Street: N/A Registrant City: Smallvile Registrant State/Province: Arkansas Registrant Postal Code: 43547 Registrant Country: US Registrant Phone: +1.447898 Registrant Phone Ext: Registrant Fax: Registrant Fax Ext: Registrant Email: zizsdcqe@6paq.com  
  
6paq is a random temporary email provider.  
  

#### Conclusion

I didn't really want to cover much else specifically in this post. This is really only one component of an investigation of this type. There are a number of other artifacts and avenues you would want to go down to draw broader conclusions about your case. Mostly, my intent was to demonstrate some of the thought process in tackling a fairly common problem with a somewhat creative solution and demonstrate the importance of this quality in DFIR work. Hopefully you learned something from this post, please feel free to leave comments as usual.

#### [Source](https://www.redblue.team/feeds/1616673388888673627/comments/default)

<br/>
---
