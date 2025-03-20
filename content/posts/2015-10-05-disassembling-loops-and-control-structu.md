---
title: "Disassembling Loops and Control Structures in x86"
date: 2015-10-05T00:49:00.002-04:00
draft: false
type: posts
categories: 
- assembly,c++,ia32,machine language,reverse engineering,Security,x86
---
# Disassembling Loops and Control Structures in x86

<br/>

<br/>
One of the [first topics](http://www.redblue.team/2015/09/a-primer-on-disassembling-function.html) we discussed focused on understanding how function calls and stack frames are represented in assembly languages. I wanted to follow up on that as we haven't posted on this subject matter since then. Both this post and my prior one are geared towards those with an interest in learning more about assembly and have only introductory level knowledge.  
  
Although most of the [worlds developers](http://www.tiobe.com/index.php/content/paperinfo/tpci/index.html) have long since ascended or are in the process of moving to _high-level_ and _very-high-level_ languages, a subset of industries and disciplines still necessitate a heavy focus on _low-level_ languages. The cyber-security industry is a prime example in that both defenders and attackers, to some degree, work intimately with dis-assemblers and debuggers.  
  
On the offensive side of the equation developers focus on writing memory efficient _shellcode_ in assembly to interact with operating system application programming interfaces in order to exploit a system. Exploit developers may also dis-assemble and debug an application in assembly to gain a better understanding of it's internal structure so as to identify weaknesses. Dis-assembly and debugging is core to the 'hacker' ethos in that it is the mechanism by which one seeks to explore and understand the inner workings of an application or object.  
  
But this process cuts both ways. Defenders spend a great deal of time dis-assembling and debugging malware to understand how it works and what impact it may or will have on a system. Moreover, fuzzing, dis-assembly, and debugging of legitimate applications is often done with altruistic intention and leads to more secure software.  
  
For many tier-1 and tier-2 security analysts, crossing the bridge to a research position is difficult in that it requires significant familiarity with assembly language concepts. I don't often meet a lot of computer science graduates who are fluent in IA32. Most entrants to the security field find themselves working in various security applications for a couple of years unless they score an entry level threat research or junior A/V analyst position. These are more niche areas so the opportunities are not as common.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgRe_9IcbpT5uMjLVkEvN9x2UvsVd-0H1GPm4rtgFuivhZ6BEhxCfbjZg0RwWQXWV6akCqcaw5nOP96sAnAJBpAgSwdvdzuOi3A1tocCzBI1a26UisGdeahbXnUEl3zMvGG8FWl1KqdaFk/s320/roflbot.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgRe_9IcbpT5uMjLVkEvN9x2UvsVd-0H1GPm4rtgFuivhZ6BEhxCfbjZg0RwWQXWV6akCqcaw5nOP96sAnAJBpAgSwdvdzuOi3A1tocCzBI1a26UisGdeahbXnUEl3zMvGG8FWl1KqdaFk/s1600/roflbot.jpg)

I mentioned in my previous post that I think one of the best ways to understand assembly is by training the eye to recognize assembly routines that translate to common high-level language functionality. The [Intel 64 Architecture Manual](http://www.intel.com/content/dam/www/public/us/en/documents/manuals/64-ia-32-architectures-software-developer-vol-1-manual.pdf) isn't a particularly thrilling read and I don't spend all day writing programs in NASM, so for me, coding in C++ and then disassembling an executable was a quick way to build _visual fluency._ It is much easier if you can visually recognize a particular pattern and immediately know what it is doing instead of picking your way through every operation when viewing an application in IDA or Ollydbg. Legibility is obviously much more natural in high-level languages and so this takes some practice. I wanted to go through a few examples in this post.  
  
**Loops and Conditional Statements**  
Loops and conditional statements are one of the first concepts a programmer will learn and are found in all programs of moderate complexity. A loop is an iterative or repetitive task designed to carry out some action until a specific condition is met. Loops can be expressed in a number of different ways depending on the logical scenario. It is normal to encounter:  

-   If-Then Statements
-   For Loops
-   While Loops
-   Do-While Loops
-   Infinite Loops

It's important to understand that all of these loop functions rely on a similar underlying conditional decision making process. A For-Loop contains an initialized variable, a comparative statement, an iterative counter, and an action that is performed. These shared characteristics become more apparent when such a process is dis-assembled. 

  

**Control Flow**

We utilize control flow statements to modify the orderly execution of an application. Control flow commands allow us to move non-sequentially around an application, branching out and executing different routines based on required conditions. High-level languages address a lot of the minutiae relating to the management of this process. When we dis-assemble loops and conditional statements we see that variables, comparators and control flow commands are at the core of these components.  
  
Consider the following example:  

> for (int i=0; i < 10; i++){  
>      cout << "Hello";  
> }

Can be expressed as:  

> int i = 0;  
> do{  
> cout << "Hello";  
> i++;}while(i<10);

**Assembly**  
To implement this functionality in IA32 we utilize the compare (CMP) and (JMP) statements, along with registers, and stored values on the stack.  
  
Let's walk through the prior example. At function loc\_415F00 we see:  

> mov \[ebp + i\] , 0  
> jmp short loc\_415F0F

First we move the value of 0 into the value 'i' which is referenced as an offset from the EBP register. Think of EBP as the base value from which all variables can be accessed inside functions or the application entry point. This is essentially our variable initialization (declaration would occur in the .BSS or .DATA segments of the PE file).  
  
Now at function loc\_415F0F we find:  

> cmp \[ebp + i\] , 0Ah  
> jge short loc\_415F30

We see a comparison command that compares the current value of 'i' (which was just set to 0) to 0Ah. The 'h' indicates this value is expressed in hexadecimal and when translated to decimal we know this is the number 10.  
  
The second command is an example of a conditional jump. The JMP command is an unconditional jump in that there is no requirement for it execute a jump. If EIP points at this command it will execute and redirect control flow. A conditional jump requires that a certain criteria is met before it redirects EIP. In our example the command can be interpreted as saying point EIP to the address of loc\_415F30 if the second operand (0Ah) is greater than or equal to (JGE) the first operand (our variable 'i'). These commands represent our comparative statement.  
  
We can then proceed to function loc\_415F30:  

> mov eax, \[ebp + i\]  
> add eax, 1  
> mov \[ebp + i\], eax  
> jmp loc\_415F0F

This set of instructions comprise the iteration required by a For-Loop. The value of 'i' (0) is moved into the register EAX which is then incremented by 1 and then pushed back into the memory offset of our variable 'i'. We then jump back to loc\_415F0F so that our comparison and iteration occurs until the CMP statement sees that \[ebp + i\] is greater than or equal to 0Ah. When this condition is satisfied the program will skip the conditional branch and EIP will move to the instruction following the jump statement.  
  
The popular dis-assembler IDA does a great job of visually representing this process as seen below:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjVThQifzj2TaXkufsZIodb5FDQOOiUWzEBhg4qbPF4_3y9a5KuvI_JNMfDIADWCi4nacw9t2Fl8WwBIS7J-oesvPjgwSQ7HCkjkGCQWd4XEBFBUBd_X5ZTXXVHhfp9QWX995-PInIGO8w/s320/ida.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjVThQifzj2TaXkufsZIodb5FDQOOiUWzEBhg4qbPF4_3y9a5KuvI_JNMfDIADWCi4nacw9t2Fl8WwBIS7J-oesvPjgwSQ7HCkjkGCQWd4XEBFBUBd_X5ZTXXVHhfp9QWX995-PInIGO8w/s1600/ida.png)

The thick blue line represents the For-Loop. The red line indicates the that the required condition was not met while oppositely the green line implies that it was satisfied. Similar sequences may not be as legible in a debugger.  
  
While this is not the most advanced material I hope that it serves to clarify the subject and help you in your journey!

#### [Source](https://www.redblue.team/feeds/3133351207874349304/comments/default)

<br/>
---
