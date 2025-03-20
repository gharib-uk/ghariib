---
title: "A Primer on Disassembling Function Calls and Understanding Stack Frames in x86"
date: 2015-09-14T20:57:00.000-04:00
draft: false
type: posts
---
# A Primer on Disassembling Function Calls and Understanding Stack Frames in x86

<br/>

<br/>
I've spent the better part of the last three months reviewing a plethora of exploit development tutorials and training material. Exploit development is a relatively specialized skill-set that serves a fairly niche market. The majority of offensive security professionals that I work with or encounter on a day-to-day basis perform Web Application assessments, Vulnerability Scanning, network Penetration Tests, Red-Teaming, Social Engineering and advise on security architecture design and philosophy. Individually, each of these categories require significant technical aptitude and in aggregate they represent a vast body of knowledge that necessitates both a formal education (typically) and several years of experience or self-study to achieve any sort of notable competence or proficiency.

  

It seems that the generalist will likely go the way of the dinosaur as more specializations  and greater levels of complexity emerge (consequently requiring more ingenuity by a practitioner in any given area). This trend is observable in security firms that now segregate employees in increasing numbers of disparate groups with narrowing focus. Thomas Dixon began writing about this concept back in 1992 and I feel Andrew McAfee and Erik Brynjolfsson have helped to rejuvenate the discussion in recent years. 

  

So why invest several months studying material in this area? I don't hunt bugs or sell exploit code for a living. Surely, this is an inefficient use of time.

  

There are a few reasons. Although I do not hunt bugs or write my own exploit code on a daily basis, I am highly dependent on those who do. The Vulnerability Scan that I run uses signatures identified by researchers that relate to specific software flaws. The Exploitation Framework that I employ requires exploit and shell code. Dependency can often feel like subordination. Your success, failure and innovation may be stifled or limited by that which you are dependent on. This is true in many aspects of life, but demonstrably more-so in engineering-centric careers. To achieve true independence, and thus freedom, we have to attack knowledge dependencies. 

  

The material I am presenting here is nothing revelatory and is likely explained better and more thoroughly somewhere else. Nonetheless, I am documenting it as part of my ongoing learning efforts and hope that it may help clarify the subject for others. This is not a comprehensive introduction to assembly language and assumes some basic knowledge.

  

Deciphering assembly is tedious at the best of times. The expressions feel archaic and are not designed with legibility in mind. Someone who is new to exploit development and reverse engineering will often feel lost while stepping through a sequence of instructions. Demystifying assembly is largely a matter of interpreting instruction set definitions and recognizing patterns that you can translate to familiar high level language concepts. There are a number of patterns or routines that become apparent the more time you spend in a debugger. Perhaps the most frequent occurrence is the function call. 

  

A function call is executed when a program wishes to pass execution from the named application entry point (commonly known as main) or another function to a specific sub-routine.  A function may contain parameters and local variables. A number of things must occur for an application to successfully execute a function call and pass execution to the set of instructions contained therein. This set of tasks is referred to as the function prologue. 

  

The prologue stores values on and prepares the stack so that the function can execute it's instructions. It will also save reference points that give the application the ability to return from the function to the previous location in memory that executed the function call. 

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgeEbgC3T-mYFXT6icgSl1ku-R03AqCC48RdGhdCJJ4VtRjrT_syT8mS8e8dpsLrX1cgg0suPwUsn1wioFJKbJwjpcACWVDXpP17tQSEDhreJQCUi2hQ12S3I602NpSkskX2JcN1uACStI/s320/functions1.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgeEbgC3T-mYFXT6icgSl1ku-R03AqCC48RdGhdCJJ4VtRjrT_syT8mS8e8dpsLrX1cgg0suPwUsn1wioFJKbJwjpcACWVDXpP17tQSEDhreJQCUi2hQ12S3I602NpSkskX2JcN1uACStI/s1600/functions1.PNG)

  

In this image we can see that our application has reached an instruction that will transfer control to some function. This is evident by the existence of the CALL instruction. The CALL itself does two things:

1.  Push the contents of EIP + the byte length of the current command. This is done to preserve the contents of the return address location on the stack.
2.  Execute a JMP to the location of the function we are calling so as to transfer execution flow. 

In the main thread we can see that the CALL instruction is located at memory offset 00401143 and is 5 bytes in length. The value of EIP  + 5 is pushed onto the stack as this is the location of the next instruction in main. EIP is then loaded with the address of our function and a JMP is executed and we step into this function.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjBVpY4nFFcvGQaJMgtPs6-sH18jK0MSeMiIi-i2tQWrK7B6PcY0LGhV2C-G0qDtd41UoSas32vsB0SsmrcEi-7UMsaCKIvsJ_kI-zc7CNwue-1Qsgyy09sWSG6pNcNpnCdYlSV2razjOI/s320/functions2.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjBVpY4nFFcvGQaJMgtPs6-sH18jK0MSeMiIi-i2tQWrK7B6PcY0LGhV2C-G0qDtd41UoSas32vsB0SsmrcEi-7UMsaCKIvsJ_kI-zc7CNwue-1Qsgyy09sWSG6pNcNpnCdYlSV2razjOI/s1600/functions2.PNG)

  

Execution flow is transferred to memory offset 00401020 (the prior address that was loaded into EIP which contains the start of the functions prologue). The prologue will almost always execute the following two instructions:

1.  PUSH EBP
2.  MOV EBP, ESP

EBP is the base pointer for the stack. It contains the static relative base location on the stack for our current frame and can be used to reference parameters that were pushed onto the stack or local variables contained within the current frame. ESP is the stack pointer and points to the top of the stack. It allows us to push and pop data off of the stack. EBP is pushed onto the stack to ensure that we have a base value that can be referenced when manipulating variables and values for the instructions contained within our function. The value of ESP is then moved into EBP to ensure that EBP points to the top of the stack.  

  

A PUSH command can be thought of as two consecutive events:

1.  SUB ESP, 4 - ESP is decremented by 4 bytes (as stack addressing grows downwards) to ensure that it points to a new location at the 'top' of the stack.
2.  MOV \[ESP\], X - our value is moved into this new location.

A POP instruction will decrement ESP by 4 bytes.

  

It is also common to encounter a SUB ESP, X instruction in the function prologue. 

  

Consider the following function call:

> _void ExampleFunction()  
> {  
>      int a, b;  
> }_

In this case our function has two local variables. To create space on the stack we would subtract 8 from ESP to grow our address space downward 8 bytes (enough space to store the memory offset for our two local variables). 

  

If parameters are passed to the function then they are pushed onto the stack in reverse order prior to the function call itself being executed. This infers that EBP + some offset can be used to reference function parameters and EBP - some offset can reference local values (alternatively one may simply utilize ESP).

  

The function epilogue is simply the inverse of the prologue:

1.  MOV ESP, EBP - Revert ESP to its prior value to free space on the stack.
2.  POP EBP - Restore EBP to its prior value.
3.  RET X - Execute a RET command to return to the prior calling function or main thread

RET can simply be thought of as a JMP to the value of EIP + calling function byte length that was pushed onto the stack prior to jumping to our function. After our function prologue is unwound we refer to this value to hop back to the next instruction address from our calling thread.  

  

Although there are compiler specific calling conventions that may introduce additional instructions, this summary should provide you with the information to recognize and interpret function calls in assembly. 

  

I may do a series that focuses on identifying and interpreting common high-level language expressions in assembly as I continue to dive into this material. Let me know if you have comments or feedback.

  

Related Links:

http://www.intel.com/content/dam/www/public/us/en/documents/manuals/64-ia-32-architectures-software-developer-manual-325462.pdf

#### [Source](https://www.redblue.team/feeds/6689113948803995866/comments/default)

<br/>
---
