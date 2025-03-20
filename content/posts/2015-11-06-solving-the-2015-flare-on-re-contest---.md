---
title: "Solving the 2015 FLARE On RE Contest - Challenge 2"
date: 2015-11-06T15:34:00.000-05:00
draft: false
type: posts
categories: 
- assembly,ctf,cybersecurity,forensics,ia32,ida,reverse engineering,x86
---
# Solving the 2015 FLARE On RE Contest - Challenge 2

<br/>

<br/>
You can find the solution to part 1 [here](http://www.redblue.team/2015/10/solving-2015-flare-on-re-contest.html).  
  
Challenge 1 was fairly straight forward. The executable in it's entirety was rather small and wholly contained within the main application entry point. The 'key' was a hard-coded value in the executable that stood out visually (when analyzed in a hex-viewer) and the decryption routine was a simple XOR bitwise operation.  As we will see, challenge 2 introduces additional layers of complexity in the form of a slightly larger application with numerous function calls, and a more convoluted decryption routine.  
  
So let's dive in!  
  
**Challenge #2**  
It seems the trend of poor grammar continues! The file name for the second problem is 'very\_success' and is missing a file extension. I made a copy of the file and added '.exe' to the file name. This worked just fine as I was able to run the file. Similar to the first challenge, we are presented with a simple command prompt upon execution that requests user input and prints a string based on our input.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh4K5XlUOEPPFf4Bgh4q6_ySewjU8XppB9q_FWw0vlzW1oFLhX8B-WlwO-Ao8taPk9u-El2o5edr6-g0oFxWYMIRUvsxagpf5SAtiFCrjwLINeQfMJC-LNkWpcwcxNpYMbBEOeJ3vZYfgE/s320/image1.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh4K5XlUOEPPFf4Bgh4q6_ySewjU8XppB9q_FWw0vlzW1oFLhX8B-WlwO-Ao8taPk9u-El2o5edr6-g0oFxWYMIRUvsxagpf5SAtiFCrjwLINeQfMJC-LNkWpcwcxNpYMbBEOeJ3vZYfgE/s1600/image1.PNG)

The import table for the file is largely the same as the first problem. Let's disassemble the application and see what we can infer from IDA.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj-8Rg3v85g70HY9A6C7wcmS6YWM77fZou-VdV3bS1AKKNK-NAVGhlURHkWlrN-w1-5u-R6GeTpOVYd3g5g-DQJ38nizsWM3F6xxoGXZ58OwRwjxyElz92H3ROaGjDnLo-j89D5xHMzFac/s1600/image2-functions.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj-8Rg3v85g70HY9A6C7wcmS6YWM77fZou-VdV3bS1AKKNK-NAVGhlURHkWlrN-w1-5u-R6GeTpOVYd3g5g-DQJ38nizsWM3F6xxoGXZ58OwRwjxyElz92H3ROaGjDnLo-j89D5xHMzFac/s1600/image2-functions.PNG)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhuMQfzciVNOPzGYWXPdfDf1JHFLjSIU9e6l6NukiFMX0_XqwVVobXxaFyKkl_PZnXyXc7n_7EATwRvs7-_c6gogVYJ0iWY46TLHn-8poj2ntHFiP4e7J03jTU1X4FHUaeoO35-m3eIzuM/s1600/image3-start.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhuMQfzciVNOPzGYWXPdfDf1JHFLjSIU9e6l6NukiFMX0_XqwVVobXxaFyKkl_PZnXyXc7n_7EATwRvs7-_c6gogVYJ0iWY46TLHn-8poj2ntHFiP4e7J03jTU1X4FHUaeoO35-m3eIzuM/s1600/image3-start.PNG)

So right away we see that there are three subroutines. This application is a little bit larger. You can check out my article [here](http://www.redblue.team/2015/09/a-primer-on-disassembling-function.html) on disassembling and identifying function calls in assembly if you'd like a better understanding. These calls will introduce function prologues, epilogues, compiler specific conventions, local variable declaration, space allocation for passed arguments, and use of the ESP and EBP registers for computation on the stack. So although a function call is trivial from a programming perspective, it certainly adds some complexity at the assembly layer if you are unfamiliar with these conventions.  
  
In the first function we see a call to sub\_401000. This is followed by scasd, stosb, lodsb and a jmp to loc\_401096+1. These commands are a little less common and the jmp to loc\_401096+1 is somewhat odd but I'm going to ignore this for now and follow the programs intended execution flow. Let's drill into sub\_401000.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhCyE_lKp2AIZYifHjbkgVQpcr6Uvj_G89BbPpRdp-6f1bsuEupzMyjC6mJKmfNL62AwxR97DkVixPyYuYwJegK1Sv8MthsChbl2xXPCka_Kecm1kizUe66xNYc-dVscHGZFgUOQvxcCoE/s320/image4-sub401000.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhCyE_lKp2AIZYifHjbkgVQpcr6Uvj_G89BbPpRdp-6f1bsuEupzMyjC6mJKmfNL62AwxR97DkVixPyYuYwJegK1Sv8MthsChbl2xXPCka_Kecm1kizUe66xNYc-dVscHGZFgUOQvxcCoE/s1600/image4-sub401000.png)

Well this feels very familiar! We call GetStdHandle and then print a string ("You crushed that last one...") to STDOUT with WriteFile. We then read user input from STDIN using ReadFile. This value is stored in variable unk\_402159 (with a memory offset of the same value). Several values, including our user input variable unk\_402159, are pushed to the stack and then a call to sub\_401084 is executed.  
  
This sub-routine is where things start to get more interesting. There are two functions at work here.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiVwqs9mOQnA-k9Df4khrmjVOQlG8hdURrflGmuGkVop9t55uc7vd5y9tgw4pvFTkCXPOGQSPtFbH9qj-7UkD7g2XuEEW6xmmceksGNshTI3zyapmENjbGGYC9ASh4Cr4EQWnQerTTYNzU/s320/image5-boundscheck.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiVwqs9mOQnA-k9Df4khrmjVOQlG8hdURrflGmuGkVop9t55uc7vd5y9tgw4pvFTkCXPOGQSPtFbH9qj-7UkD7g2XuEEW6xmmceksGNshTI3zyapmENjbGGYC9ASh4Cr4EQWnQerTTYNzU/s1600/image5-boundscheck.PNG)

In the first we see a local variable declared, and space for the three arguments is allocated. The function prologue begins and a few empty registers are pushed to the stack. EBX is XOR'd by itself (resulting in a value of 0 in that register as well). Then 25 (hexadecimal) is loaded into ECX and a comparison to the memory at offset EBP+arg\_8 is made. This serves effectively as a length check! If we enter a value with a length of at least 25h than this comparison passes. Otherwise loc\_401096 is executed and a short jump to loc\_4010D7 occurs (which effectively quits the application).  
  
If the length check passes then we proceed to the second part of our function.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiwTxnELflfo4eUxQopcobzz0IQyBcZledYuw-ClX17rgTvy2e5ysmRfGu6sjHYeKQtOpQ-j4x_KpO3LHz7bV0vJw9kc6sRo2Ugg26J_mtIjFHk9KLqO9wp4dnW85PU60C4fyJcr6v3fnE/s320/image6-decryhption.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiwTxnELflfo4eUxQopcobzz0IQyBcZledYuw-ClX17rgTvy2e5ysmRfGu6sjHYeKQtOpQ-j4x_KpO3LHz7bV0vJw9kc6sRo2Ugg26J_mtIjFHk9KLqO9wp4dnW85PU60C4fyJcr6v3fnE/s1600/image6-decryhption.PNG)

A-hah! Here is the decryption routine. The dereferenced value at EBP+ARG\_4 (0018FF64 which has stack contents of 00402159 which is the address to the starting value in our user input variable) is loaded into ESI. A return address is then loaded into EDI. Oddly, EDI+ECX-1 is then loaded into EDI with the LEA command. ECX is 25, subtract 1 = 24. EDI contains 004010E4 + 24 = 00401108. This is an odd address. Let's take a quick look in a hex-viewer to see what resides at this location.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgxRUYrMEqUt3GCVXQBi-UOooXzGWfdXLVS9cyzAZR84XeAIyn1slcveoQ7V-GhjNXgsOma_2vVkYdQYFbQL7S3HNuM5XYXGS8oFnkIoB-L1uN-4az3zd6RhJMecM-_gKqnV0Y-N__vW9s/s320/image7-key.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgxRUYrMEqUt3GCVXQBi-UOooXzGWfdXLVS9cyzAZR84XeAIyn1slcveoQ7V-GhjNXgsOma_2vVkYdQYFbQL7S3HNuM5XYXGS8oFnkIoB-L1uN-4az3zd6RhJMecM-_gKqnV0Y-N__vW9s/s1600/image7-key.PNG)

Well this is interesting. Our initial length check, and the ECX-1 calculation are the same. This is a pretty strong indication that the characters from 004010E4 to 00401108 are the 'key'.  
  
At this point we've probably gathered as much useful information that can be derived from reviewing the disassembled instructions. There are a lot of register comparisons that occur in the suspected decryption function so it becomes easier to analyze what transpires if we know what the registers actually contain at run-time. We need to use a debugger to gain this view into the applications execution flow. I prefer Immunity but Olly or any other debugger will suffice.  
  
Load your Debugger and Open 'Very Success.exe'. Right click in the disassembled instruction window and select View -> Module 'very\_suc'. Select the line directly above the call to ReadFile and press F2 to set a breakpoint. This will halt application execution when it reaches this instruction so that we can manually step into and over subsequent instructions and see each change as it effects the registers and stack.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEguB-CuDIcdbLFcYT48bFfFtaNnNjnk-UX9UShqJ_K96w3pZ12fnJAb37HP6Zsw2mkW79xbeiR4C-aFC3isiVBSaHgMNfcxVcKQOPADaIxjHg7ZsSKm12PkmmSTv5uSekeDlG_c4bJQtJ8/s320/image8-breakpointset.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEguB-CuDIcdbLFcYT48bFfFtaNnNjnk-UX9UShqJ_K96w3pZ12fnJAb37HP6Zsw2mkW79xbeiR4C-aFC3isiVBSaHgMNfcxVcKQOPADaIxjHg7ZsSKm12PkmmSTv5uSekeDlG_c4bJQtJ8/s1600/image8-breakpointset.PNG)

Press F9 to run the program, F7 to step into the first routine, and F9 again to hit our breakpoint. Press F8 twice and then enter a value in the application command prompt window that is greater than 25h in length. Lets continue to press F8 to step over each instruction until we reach CALL very\_sec.00401084. Press F7 to step into this function. This is our bounds check routine. Continue to press F8 until you reach LEA EDI, DWORD PTR DS:\[EDI+ECX-1\] (which you should recall from the prior IDA analysis of this function). Application execution is now sitting in our decryption routine.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhTw0Tnaid_GgFp9sTylUI5bodrxyXy9LErJCJ5X6nF0hZMkig1UDzNiCkPTE5RxT_ssxGGz-lZWNkLmoO6kJdxqoAf-Ow7m0aUN3kKOh8wNCkOWd0yLk9SwjHhLWyjy-jd3zmpzF4FzY0/s320/image9.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhTw0Tnaid_GgFp9sTylUI5bodrxyXy9LErJCJ5X6nF0hZMkig1UDzNiCkPTE5RxT_ssxGGz-lZWNkLmoO6kJdxqoAf-Ow7m0aUN3kKOh8wNCkOWd0yLk9SwjHhLWyjy-jd3zmpzF4FzY0/s1600/image9.PNG)

Let's examine the stack and register contents to better understand our current landscape. EAX contains 0018FF84 which is a stack offset. ECX holds 25 (the length check). EDI points to the suspect string. ESI points to the user entered string.  
  
We also see some 16 bit and 8 bit register references. If you are unfamiliar with this please see the following diagram:  
  
                   EAX  
|<-------------- 32 bits ----------->|  
+---+---+---+---+---+---+---+---+  
| 0  | 0  | 1 | 8 |  0 | 1 |  C |  7 |  
+---+---+---+---+---+---+---+---+  
                              AX  
                     |<-- 16 bits -->|  
                     +---+---+---+---+  
                      | 0 | 1 | C |  7 |  
                     +---+---+---+---+  
                      | 8bits | 8bits |  
                         AH      AL  
  
I'm going to walk through each instruction in the decryption routine to help explain things to those unfamiliar with some of the operations that take place.  
  
MOV DX, BX  
This instruction moves the low 16 bits from BX into DX effectively zeroing out those bits.  
  
AND DX, 3 performs a bitwise AND operation which effectively does nothing at this point in time.  
  
MOV AX, 1C7  
1C7 is a hard-coded value and it is loaded into the lower 16 bits for EAX (which now contains 001801C7). EAX is then pushed onto the stack.  
  
LODS BYTE PTR DS:\[ESI\] stores the byte from ESI into the AL register. Since ESI points to the user input string, this takes our first value and stores in inside the low 8 bits of EAX. This is important as it is the beginning of our string comparison test against a presumed harcoded key value. ESI increments.  
  
PUSHFD decrements our stack pointer by 4 and pushes the contents of the EFLAGS register to the stack (00000203).  
  
XOR AL, BYTE PTR SS:\[ESP+4\] is a crucial step. This instruction performs an XOR bitwise operation on the AL register (first byte of user input) against ESP+4. ESP points to the top of the stack (which currently contains the EFLAGS register contents). If we add four (moving backwards in the stack as its addressing grows downward) we reference 001801C7. Specifically the value C7. So if our user input was 'AAAAAAAAAAAAAAAAAAAAAAA' than this operation would be XOR 41, C7. The result is stored in AL.  
  
XCHG DL, CL swaps the contents of these two registers effectively placing the value 25 in DL and 00 in CL.  
  
ROL AH, CL is another important command. It will rotate CL number of bits left in AH. CL contains 00 so nothing occurs during this iteration of the loop.  
  
POPFD pops the EFLAGS register back off the stack.  
  
ADC AL, AH is a third crucial command. ADC stands for 'add with carry'. Effectively the SRC operand is added with the carry flag to the DST operand. The result is stored in DST. Continuing with our example user input from before, AL contains 86 after the XOR operation. The carry flag is set to 1 and AH contains 1. 86+1+1 = 88 placed into AH. EAX now contains 00180188.  
  
XCHG is used again to swap the contents of DL and CL. EDX is XOR'd by itself to zero the register.  
  
AND EAX, 0FF is called which effectively zeros the contents of EAX excluding AL.  
  
ADD BX, AX sets EBX to 0x00000088.  
  
And now to the most important command..  
  
SCAS BYTE PTR ES:\[EDI\] compares the byte, word, or double word specified with the memory operand with the value in the AL, AX, or EAX register, and sets flags in the EFLAGS register according to the results. This checks our XOR, ROL, and ADC computed first byte of user input against the byte at EDI (00401108 which is our suspected key if you recall).  
  
If our value is correct then EDI decrements (as it will need to check the next byte in the suspect key) and the loop iterates again. If incorrect then the application jumps to 004010D7 and quits.  
  
Lets try to summarize this activity:  
1\. 004010E4 - 00401108 contains our key  
2\. 00402159 - contains our user input  
3\. The decryption routine does an XOR against C7, a ROL, and an ADC on each byte of user input and compares it to a corresponding byte in the key.  
4\. If there is a match it continues until fully decrypted. Otherwise it quits.  
  
Since we know the operations that occur, we can perform each step in reverse order to decrypt the key.  
  
A8 9A 90 B3 B6 BC B4 AB 9D AE F9 B8 9D B8 AF BA A5 A5 BA 9A BC B0 A7 C0 8A AA AE AF BA A4 EC AA AE EB AD AA AF  
  
A8 - 2 = A6 XOR by C7 = 61 Convert hexadecimal to ASCII = lowercase 'a'  
  
You also have to factor the ROL command as ROL AH,CL on the third character (where CL = 02 and AH = 01) turns AH to 04. The ADC instruction then becomes AL + 4 + 1 and it would be a subtraction of 5 instead of 2.  
  
I did these computations by hand and stepped through everything manually in the debugger. After computing each value manually I tested the input in a debugger and observed the XOR, ROL, ADC events to ensure that everything was being processed correctly and adjusted my calculations appropriately if an incorrect value was seen.  
  
I'm sure you could build a quick python script to calculate everything with a proper ROL implementation but it didn't take that long by hand and was good practice. This was definitely more challenging but still manageable. The application and decryption routine was not complex enough to necessitate renaming variables for clarity in IDA or programming anything in python.  
  
**Result:**  
a\_Little\_b1t\_harder\_plez@flare-on.com

#### [Source](https://www.redblue.team/feeds/441447975402216836/comments/default)

<br/>
---
