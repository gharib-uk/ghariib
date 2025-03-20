---
title: "Cool kids pop a programmers calc in their demos"
date: 2011-07-20T09:27:00.000-04:00
draft: false
type: posts
summary: "Over time, I've heard several well-respected security professionals mention that you're not really cool unless you can pop a scientific/programmer's calculator when demoing your exploits. I mean, what right does a standard, run of the mill calculator have showing its face to a crowd of enthralled conference attendees watching the"
categories: 
- shellcode
---
# Cool kids pop a programmers calc in their demos


<br/>
Over time, I've heard several well-respected security professionals mention that you're not really cool unless you can pop a scientific/programmer's calculator when demoing your exploits. I mean, what right does a standard, run of the mill calculator have showing its face to a crowd of enthralled conference attendees watching the latest version of Windows 7 get totally pwned? And how many self-respecting security folks use a standard calc more than the programmer's calc? So rather than waste the time of the people who thought this would be cool, I thought it would be better to waste my own time to complete this silly challenge. So without further ado, prepare to do some hexadecimal math. I'd like to mention that Jacob Appelbaum (@ioerror) and Aaron Portnoy (@aaronportnoy) inspired me to write this. ;D

  

To start, I used procmon to observe how calc was interacting with the OS when switching from standard to programmer's mode.

  

[![](http://4.bp.blogspot.com/-dZSZlnIUmLs/TibLw_-Sc9I/AAAAAAAAABg/BX8s1IXGnXg/s400/procmon_calc.jpg)](http://4.bp.blogspot.com/-dZSZlnIUmLs/TibLw_-Sc9I/AAAAAAAAABg/BX8s1IXGnXg/s1600/procmon_calc.jpg)

  

It turns out that it modifies the following registry key: HKCU\\Software\\Microsoft\\Calc\\layout and changes the REG\_DWORD value to 0x00000002 when switching to a programmer's calc.

  

One of the nice features of procmon is that it lets you view the stack trace of each entry. Here's the stack trace for the call to RegSetValue:

  

[![](http://4.bp.blogspot.com/-vsSs5jtMqUY/TibMFJCPjoI/AAAAAAAAABk/u-CbTNoI9EQ/s400/procmon_stack_trace.jpg)](http://4.bp.blogspot.com/-vsSs5jtMqUY/TibMFJCPjoI/AAAAAAAAABk/u-CbTNoI9EQ/s1600/procmon_stack_trace.jpg)

  

So, to get my shellcode to work, I'll likely need to understand how calc.exe modifies the registry. After loading calc.exe into IDA, I quickly determined that it calls the folowing functions, which procmon graciously gave me the offsets to:

  

-   RegCreateKeyExW
-   RegSetValueExW
-   RegCloseKey

  

IDA in conjunction with my debugger conveniently gave me all the parameters I needed to pass to these functions. For more information on the function parameters, refer to MSDN documentation.

  

The only problem is that the shellcode needs to first determine the addresses to these functions, which turns out to be the bulk of my shellcode. I should be careful when saying 'my' shellcode considering I modified SkyLined's awesome w32-exec-calc-shellcode\[[1](#4_1)\] which I believe was loosely based upon Matt Miller's paper - "Understanding Windows Shellcode\[[2](#4_2)\]."

  

It's worth noting that if your shellcode is running in the stack, careful considerations must be made when writing your assembly. Specifically, the shellcode instructions and function parameters share the same space. Because you are executing code from the stack, you have to be careful not to clobber your instructions. So what I did was move esp (which would point to the first instruction) into ebp \[MOV EBP, ESP\] and essentially create my own stack frame above the code.

  

Here is the shellcode with relevant comments for your convenience. Note that this will only work on 32-bit Windows and it has only been tested on Windows 7 SP1 :

  
w32-programmer-calc hex  

<br />8BEC33F633C06A016A016A016A0168D43710F568C01584E86A016A01648B76308B760C8B761CFF76088F45E48B368B5DE483C33C8B1B83C378035DE48B1B035DE48B4B1867E3DF8B7B20037DE48B7C8FFC037DE433D2321766D1CA5033C0AE5875F4663B5445E8E0DE75BB8B53240355E40FB7144A8B7B1C037DE48B34970375E4897485F04083F80475B633DB6A636861006C00685C00430068660074006873006F006872006F006869006300685C004D006872006500687700610068660074006853006F00538D45E0505368060002005353538D44241C506801000080FF55F06A0068750074006879006F00686C0061006A026A048D442404506A046A008D44241450FF75E0FF55F4FF75E0FF55F86A00682E6578656863616C635487042450FF55FCCC

w32-programmer-calc assembly  

<br />8BEC           MOV EBP,ESP ; Save EBP-based frame pointer<br />33F6           XOR ESI,ESI<br />33C0           XOR EAX,EAX<br />6A 01          PUSH 1      ; Placeholder for WinExec address<br />6A 01          PUSH 1      ; Placeholder for RegCloseKey<br />6A 01          PUSH 1      ; Placeholder for RegSetValueExW<br />6A 01          PUSH 1      ; Placeholder for RegCreateKeyExW<br />68 D43710F5    PUSH F51037D4 ; Hash values for RegCloseKey and WinExec<br />68 C01584E8    PUSH E88415C0 ; Hash values for RegCreateKeyExW and RegSetValueExW<br />6A 01          PUSH 1 ; Placeholder for module base address (kernel32.dll eventually)<br />6A 01          PUSH 1 ; Placeholder for open registry key handle<br />64:8B76 30     MOV ESI,DWORD PTR FS:\[ESI+30\] ; fs:\[0x30\] - Pointer to the PEB<br />8B76 0C        MOV ESI,DWORD PTR DS:\[ESI+C\]  ; PEB pointer to PEB\_LDR\_DATA struct<br />8B76 1C        MOV ESI,DWORD PTR DS:\[ESI+1C\] ; Extract entries in the initialization order module list<br />locate\_kernel32:<br />FF76 08        PUSH DWORD PTR DS:\[ESI+8\]     ; Save module base address to stack<br />8F45 E4        POP DWORD PTR SS:\[EBP-1C\]     ; Save module base address to stack<br />8B36           MOV ESI,DWORD PTR DS:\[ESI\]    ; Pointer to the next loaded module<br />8B5D E4        MOV EBX,DWORD PTR SS:\[EBP-1C\] ; Offset to dll's PE signature<br />83C3 3C        ADD EBX,3C                    ; Skip over MSDOS header<br />8B1B           MOV EBX,DWORD PTR DS:\[EBX\]    ; Offset to dll's PE signature<br />83C3 78        ADD EBX,78                    ; Offset to module's Export Address Table (EAT)<br />035D E4        ADD EBX,DWORD PTR SS:\[EBP-1C\] ; EAT offset<br />8B1B           MOV EBX,DWORD PTR DS:\[EBX\]    ; Save offset<br />035D E4        ADD EBX,DWORD PTR SS:\[EBP-1C\] ; Add to module basee address<br />find\_next\_function:<br />8B4B 18        MOV ECX,DWORD PTR DS:\[EBX+18\] ; Extract # of exported functions<br />67:E3 DF       JCXZ SHORT locate\_kernel32    ; Go to next loaded module if current module doesn't export any functions<br />extract\_function\_name:<br />8B7B 20        MOV EDI,DWORD PTR DS:\[EBX+20\] ; Extract the function name table array offset<br />037D E4        ADD EDI,DWORD PTR SS:\[EBP-1C\] ; Address of function name table array<br />8B7C8F FC      MOV EDI,DWORD PTR DS:\[EDI+ECX\*4-4\] ; Indexed function name<br />037D E4        ADD EDI,DWORD PTR SS:\[EBP-1C\] ; Current function name in ASCII<br />33D2           XOR EDX,EDX<br />function\_name\_hash:<br />3217           XOR DL,BYTE PTR DS:\[EDI\]      ; XOR current character with DL<br />66:D1CA        ROR DX,1    ; Rotate DX by one<br />50             PUSH EAX    ; Save counter<br />33C0           XOR EAX,EAX<br />AE             SCAS BYTE PTR ES:\[EDI\]        ; Is current character == NULL?<br />58             POP EAX     ; Restore counter<br />75 F4          JNZ SHORT function\_name\_hash ; Calc hash until null terminator<br />66:3B5445 E8   CMP DX,WORD PTR SS:\[EBP+EAX\*2-18\] ; Does current function match indexed hash?<br />E0 DE          LOOPDNE SHORT extract\_function\_name ; No? Get next functions name<br />75 BB          JNZ SHORT locate\_kernel32         ; Hash found? Extract function address<br />8B53 24        MOV EDX,DWORD PTR DS:\[EBX+24\]     ; Extract ordinals table relative offset<br />0355 E4        ADD EDX,DWORD PTR SS:\[EBP-1C\]     ; kernel32 ordinals table<br />0FB7144A       MOVZX EDX,WORD PTR DS:\[EDX+ECX\*2\] ; Extract current function's ordinal number<br />8B7B 1C        MOV EDI,DWORD PTR DS:\[EBX+1C\]     ; Extract the address table relative offset<br />037D E4        ADD EDI,DWORD PTR SS:\[EBP-1C\]     ; kernel32.dll address table<br />8B3497         MOV ESI,DWORD PTR DS:\[EDI+EDX\*4\]  ; Offset to function address<br />0375 E4        ADD ESI,DWORD PTR SS:\[EBP-1C\]     ; kernel32 function address<br />897485 F0      MOV DWORD PTR SS:\[EBP+EAX\*4-10\],ESI ; Save function address relative to EBP<br />40             INC EAX     ; Increase counter<br />83F8 04        CMP EAX,4   ; Found all four functions?<br />75 B6          JNZ SHORT find\_next\_function ; Yes? Start pushing params and call functions<br />33DB           XOR EBX,EBX<br />6A 63          PUSH 63     ; Push key: Unicode "Software\\Microsoft\\Calc"<br />68 61006C00    PUSH 6C0061<br />68 5C004300    PUSH 43005C<br />68 66007400    PUSH 740066<br />68 73006F00    PUSH 6F0073<br />68 72006F00    PUSH 6F0072<br />68 69006300    PUSH 630069<br />68 5C004D00    PUSH 4D005C<br />68 72006500    PUSH 650072<br />68 77006100    PUSH 610077<br />68 66007400    PUSH 740066<br />68 53006F00    PUSH 6F0053<br />53             PUSH EBX    ; lpdwDisposition<br />8D45 E0        LEA EAX,DWORD PTR SS:\[EBP-20\]; Pointer to registry key handle<br />50             PUSH EAX    ; Pointer to registry key handle<br />53             PUSH EBX    ; lpSecurityAttributes<br />68 06000200    PUSH 20006  ; samDesired (STANDARD\_RIGHTS\_WRITE, KEY\_SET\_VALUE, and KEY\_CREATE\_SUB\_KEY)<br />53             PUSH EBX    ; dwOptions<br />53             PUSH EBX    ; lpClass<br />53             PUSH EBX    ; reserved<br />8D4424 1C      LEA EAX,DWORD PTR SS:\[ESP+1C\] ; lpSubKey<br />50             PUSH EAX    ; lpSubKey - Pointer to "Software\\Microsoft\\Calc"<br />68 01000080    PUSH 80000001                 ; hKey (HKCU)<br />FF55 F0        CALL DWORD PTR SS:\[EBP-10\]    ; Call kernel32!RegCreateKeyExW<br />6A 00          PUSH 0      ; Push key value name: Unicode "layout"<br />68 75007400    PUSH 740075<br />68 79006F00    PUSH 6F0079<br />68 6C006100    PUSH 61006C<br />6A 02          PUSH 2      ; Setting for programmer's calc :D<br />6A 04          PUSH 4      ; cbData - Size of data<br />8D4424 04      LEA EAX,DWORD PTR SS:\[ESP+4\]  ; lpData - Pointer to programmer's calc value<br />50             PUSH EAX    ; lpData - Pointer to programmer's calc value<br />6A 04          PUSH 4      ; dwType - REG\_DWORD<br />6A 00          PUSH 0      ; reserved<br />8D4424 14      LEA EAX,DWORD PTR SS:\[ESP+14\] ; lpValueName - pointer to 'layout' Unicode string<br />50             PUSH EAX ; lpValueName - pointer to 'layout' Unicode string<br />FF75 E0        PUSH DWORD PTR SS:\[EBP-20\]    ; hKey - Open registry key handle<br />FF55 F4        CALL DWORD PTR SS:\[EBP-C\]     ; Call kernel32!RegSetValueExW<br />FF75 E0        PUSH DWORD PTR SS:\[EBP-20\]    ; hKey - Open registry key handle<br />FF55 F8        CALL DWORD PTR SS:\[EBP-8\]     ; Call kernel32!RegCloseKey<br />6A 00          PUSH 0<br />68 2E657865    PUSH 6578652E                 ; "calc.exe"<br />68 63616C63    PUSH 636C6163<br />54             PUSH ESP    ; pointer to address of ascii string: "calc.exe"<br />870424         XCHG DWORD PTR SS:\[ESP\],EAX   ; uCmdShow - Bring calculator to the front of the desktop<br />50             PUSH EAX   ; pointer to address of ascii string: "calc.exe"<br />FF55 FC        CALL DWORD PTR SS:\[EBP-4\]     ; Call kernel32!WinExec<br />CC             INT3       ; Debugger trap

  
Areas of improvement:  

-   Remove all nulls. This would be relatively easy but I am lazy. Perhaps in a future version.
-   Only one byte of this needs to be modified to pop a scientific calc on Vista. This can be left as an exercise for the readers.
-   This could scan for the OS version and determine dynamically whether to pop a scientific (pre-Windows 7) vs programmer's calc. Do you really want to be popping calcs on anything besides Windows 7 though? ;D
-   My assembly is likely total crap. But it works. I'm sure someone out there could trim a couple dozen bytes off it but I'll leave that as a challenge for the assembly ninjas out there.

  

Please comment, try it out on your 32-bit Windows 7 machine for yourself, and let me know if it doesn't work and I might make an effort to fix it. Enjoy!

  

References:  
  
1\. Berend-Jan "SkyLined" Wever, w32-exec-calc-shellcode, [http://code.google.com/p/w32-exec-calc-shellcode/](http://code.google.com/p/w32-exec-calc-shellcode/)  
  
2\. Matt Miller, "Understanding Windows Shellcode", December 6, 2003, [http://www.hick.org/code/skape/papers/win32-shellcode.pdf](http://www.hick.org/code/skape/papers/win32-shellcode.pdf)

![](https://blogger.googleusercontent.com/tracker/6052198192158185644-282741896559422533?l=exploit-monday.com)

<br/>
---
