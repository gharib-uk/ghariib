---
title: "McAFuse - open source McAfee FDE decryption"
date: 2025-01-15
categories: 
  - "general"
tags: 
  - "technology"
---

This post is a _guest_ post, where **Andrea Canepa** (_recently_ graduated at University of Genoa, Computer Science) will explain his **Master Thesis**. The topic is _how to handle McAfee FDE acquisitions_ when doing a DFIR investigations.

Suppose that you're asked to perform a forensic investigation on a laptop where McAfee FDE is used: the company will provide you the xml file (McAfee ePO) with the decrypting key. How would you proceed?

Our usual approach has always been to perform a full disk acquisition of the encrypted disk, as is: this is most likely the _best freeze_ of the current data you could get. Then, how would you decrypt the forensic image?

McAfee provides the **DETech** tool and a proper manual: point is, you'd have to use it by creating a boot-able WinPE device (CD, USB) to run it and decrypt the original disk, sector by sector.  

What I've done in the past is a Powershell script which will take the _DETech.zip_ file and it will "install" it inside a Windows system: to say the truth, inside a Windows VM. By using that VM together with the DETech app with the proper decryption key, the mcafee _filter_ driver will decrypt the volume on the fly when you access it: a second acquisition can then be performed to get the _volume_ decrypted. I'm not going to release that script but, if you want to adopt a similar approach, be careful to use a VM: mcafee filter driver will allow _one and only one_ decrypting key and if your host is using McAfee FDE, you're going to fast blue screen the system. Moreover a _forced_ new filter driver will most likely kill your current FDE or trigger any TPM. Go virtual.

I've never liked the previous solution, so I swept Internet to spot a research about this full disk encryption. No luck, only few commercial tools able to handle it, without details. This is the reason I proposed a Master Thesis at University of Genoa, to see if some skilled student was interested in it. In the end, Andrea was, and that's how the _**McAFuse**_ (which is not _fuse a Mac_, regardless the opportunity to do that) project started.

Before leaving the _blog post_ to Andrea, let me say thanks to **Prof. Giovanni Lagorio** and, obviously to Andrea, who manage quite a complexity in a professional way.  Disclaimer: we're all part of the **ZenHack team** (https://zenhack.it/).

Spoiler: the McAFuse project is a _blue_ project. But the SafeDisk _object_ - what is called _Drive Encryption Pre-Boot File System_ (PBFS) by McAfee - is a gold mine for configurations, cracking and, maybe, xploits: decrypted offline, encrypted when online, fun trick.  

Go on **_Andrea_**, and thank you!

### DETech and McAfee internals

Let's start our journey by taking a quick look at one of the windows presented by DETech.exe after having it installed on our VM. In particular, in the picture below we can see the _Workspace_ environment, where we can upload up to 40960 sectors from one of the mounted disks on our system.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgpl7ShA731JHYQbzZ_MBNMeL7VEfSfGxICeZoUu9RzHhFj0QAO-AbCYoJ-BMKE9nptT0uao8HuPkXPFWcocCCfdPI1b3stfC8Dk2wEhKRqtBBnqhuhuEphTlpzXhfonyWi-bIyyoCI0Rg/w400-h301/workspace.png)

Here we can play a little by encrypting and decrypting the data previously loaded. This was the starting point for the investigation! We knew in advance that the algorithm used by this technology was _**AES-256**_ in _**CBC**_ mode. How do we know it? Elementary, Watson! There is another very informative frame that DETech displays to the user, like in the picture below (n.d.r. in the following some of the data showed in the pictures are redacted to preserve the privacy of those sensitive personal information):

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiqqrfj_g2wXSMW79LK6BzFdSURmM_fXyMgY4Cxjji0JJiwDp7D4V95GL_OafSbWXVGsPTe61FSgUtyu8YnHKLvQCtS2n5EzzbHgFOEx21CXJ4zY-GqHLoDxfBTh6vx63hCO6sqNcIgsN4/w400-h269/diskinfo.png)

At this point a question is legit: how are we able to see all those information if the disk is _entirely_ encrypted? There must be some areas in memory which contain clear data, but the "only" difficulties are find and interpret them. Of course, we cannot analyze every single byte on the hard disk, so we need to develop a smart strategy that helps us finding the important stuff. So let's think simple: if the computer is able to boot, then there should be, at least, the bootstrapping code saved somewhere in clear. Usually those instruction are saved in the first(s) sector of the disk in a data structure called _Master Boot Record_ (MBR). Indeed, that was the right direction that led us to identify 3 main _internal_ components of the whole encryption system:

- **SafeBoot MBR**: the first sector of the evidence we were working on, it contains the _signature_ (in red) and a pointer to the _SafeBootDiskInf__Sector_  
    
    ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhfMJsJQNlEFJIiZmnM5kb9qB_TIV7L2ZFEKKbXfEqD2STpUWBFZ7pdkiI02E2Xe-v1aOcLSPE-h1c7wbg9ZJNlEHR6_fT12htv0cgdiQEwOFAsB1yUC12puymQQirBUYN6g1G24pkH0ng/w400-h36/sbfsmbr.PNG)
    
      
    
-  **SafeBootDiskInf Sector**: here we can find in plaintext the same information showed in the Disk Information window. This and the following sectors in the same memory region are linked together with a double-linked list, whose pointers are tracked at the end of the sector itself, along the special signature _SBfs_ (n.d.r. SafeBoot File System, or at least what we inferred). In this area we can look at:

- **SafebootDiskInf** signature
- **Disk ID**, an integer auto-incremental identifier of the disk
- **Disk Crypt List**, sector pointer, where we where we can find the number of defined encrypted areas of this logical disk, the physical first and last sector of the region, and the number of sectors in the region
- **Disk GUID**, a unique little-endian 16-bytes value which has to be read in the following format: XXXX-XX-XX-X-X-X-X-X-X-X-X
- **Algorithm ID**, an identifier for the cryptographic primitive adopted, in this case 18 (0x12 in hexadecimal) means AES-256-CBC
- **Sector Map**, an address, express in sectors, of the Sector Map area, which is the subject of the next section
- **Sector count**, the number of sector in the region
- **Key check**, a useful value for recovery operations in case the XML file with the key went missing  
      
    
    ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjzT65-6qm-RACwXxxvoLVe9k5ysy95tkTH1yB80zFPz8OUbrCbX076N-sWLF7dSoB66wLM1Ty7eolwz1oZHL1_jbsxEjfuJPPLyICLqvCVsEFUVt5wKPrLIfj_NLp_TXW8x4yorNBDQRI/w400-h100/SafeBootDiskInf.PNG)
    

- **SectorMap Sector**: here we can see what resemble an Hash-Map data structure: the "keys" are sector numbers, while the "values" are lengths. If we read n sectors starting from each of those pointers top-bottom, and we concatenate the buffers we obtain an entire FAT partition that was hidden and fragmented before.  
      
    
    ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgmY0scr4ksAcSvmywbHyrhHbDlqB4tHSFC8WK8GgPsURGRBGpDp6lsKO8D7vBcZglJi6wtP_1FxWrMz_i5_58nsJq6qWaes9RIdtWBzhudS7qdXWWoKP6ePmt8peunseWAw9UPRDeOHTw/w400-h115/sectormap.PNG)  
    

The latter partition, as _@dfirfpi_ already mention before, is a real goldmine for DFIR examiners and can be used for both _blue team_ operations to retrieve useful information about the suspect, but also for the _red team_ ones, since it contains a lot of configurations that can be tweaked to ease an entire class of exploits. For example, it contains the maximum number of attempts allowed to insert the master password that could be increased to perform a brute-force attack to recover the secret of the user.  

### Reverse Engineering experience

To build an application that can serve the decrypted disk, we must be able to understand how McAfee product implements its cryptography under the hood. Given that it is using the **AES-256** algorithm in **CBC** **mode** (note that this is the default configuration, but there are more available), of which we own the secret key, we are still missing an important piece of information to be able to read in the clear the data: the _initialization vector_.

To describe better our experience, we divide this section in 3 parts:  

- **DETech application**
- **MfeEpePc driver**
- **MfeCCDE driver**

First of all we begun by reversing some of the features presented by the DETech applicative, starting with the decryption of the disk. After having mounted the encrypted device with the help of _Arsenal Image Mounter_, and ingested the program with the XML file containing the key, we investigate the flow of data after having clicked on the "_Decrypt Workspace_" button in the related window. And here our first discovery: all the cryptographic operations are performed at _**kernel level**._ The passage between these two "worlds" is clearly visible in the picture below.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiEcQGe0jXWqssLK7vtEoaZ-Pi5jlOdsPy3toHTMAhzyTvlPk4rBuyph3WH0ElfADbWummV7HCFjVG9lk0nJQrw1OB70yYY5JvjQzwXUjsRcYKj_VCp98p-io3X9j5tWzkkEFfPNHA4rBw/w400-h26/ghidradevioctl.PNG)

  

After invoking the **_DeviceIoControl_** function from the Windows API core library, the control flow goes to the first of the two driver involved in this process, namely **MfeEpePc**. The latter is a filter driver that, given the buffer received from the user-space application, invoke different callbacks. The most important one, at some point in the middle of the instruction flow, jump (literally, with an unconditional _jmp_ statement) to a function exposed at a very specific address in memory by the other driver, **MfeCCDE**, as we can see in the next picture.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgsEEMt6pc2c1bTIWtkrWJYwIookpqdKL-90u4xGdSSYck1XCKlPal12hrTtjtFRg9Gl7u4WbXX_8js0ZklBQrdpv4KYX6wZ4Pn0wATnInedhPc0tiYFiT86WvmT7IX7Rl7-IaSDT6mUQs/w400-h70/ghidracallr11.PNG)

  

Here we can find an implementation of the AES primitive along its mode (CBC in our case). At this point the whole reversing process got a lot easier! We just put a breakpoint in the _WinDBG Preview_ debugger at that address to see the real value of the initialization vector that we were seeking from the beginning. The last two images of this sections show respectively a fragment of the implementation of the CBC mode and an AES round.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhIYRpzW47ghNY-yh5gk26BBdDhcFFbD7Vj9PSa1_izuLhRZEtMvR1hhStF8pV6IatIGp1TqQVhMvSIcaTFIE2iiNDc7F8HSh9Y0sIiP5tB3LjFXwwBT8HP8xHaggBfLLLg7c9Wz7DuzEU/w400-h94/ghidraprepdec.PNG)

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi1UmvshxabpssI2Y6jUab2ZiBuTpsT9XqSDRYKW02M4krSx62wV85UFLMa5-44DSdRjUP1N0NZPRBNrMNUgQGMiIjlFfJ-pPvJ_PL29WcITdWsnCimY6leq7rrdLMW2Y0xcN4LDaNjmsw/w400-h269/ghidrageniv.PNG)

  

For the sake of completeness, I describe the shape of the initialization vector that we have found, since it played a very important role in our solution. It is a 16 bytes value, divided in four 32-bits integers, each of them representing the _number of the sector_ we are trying to encrypt/decrypt. For example, if we had worked on the 1000-th sector, which is 0x3e8 in hexadecimal, its corresponding IV would have been: 00 00 03 e8 00 00 03 e8 00 00 03 e8 00 00 03 e8.

  

For the most curious among you, this blogpost was a big inspiration, and an influential source of knowledge. It helped a lot in understanding the most intricate parts of the reversing process, a highly suggested reading!

  

### McAFuse

After all the research work, it was time to do some coding! We decided to implement an open-source tool that is shaped as a CLI util to help forensics examiners in the analysis of devices encrypted with this _McAfee_ product. From a technical point of view, it was implemented in **Python3** language, with the help of _FUSE_ framework. FUSE stands for _File system in USEr space_. Basically it allows the programmers to write file systems, that usually are very complex piece of code, with ease by only overriding the behavior of the most common and basics operations, like _write()_, or _read()_, for example. To present the user an interesting interface we decided to implement a _static, read-only_ file system with only two files:

- **SafeBoot.disk**: the FAT partition that McAfee FDE solution install on the disk that encrypts, presented above
- **encdisk.img**: the partition or the disk encrypted, depending on the parameter passed to the program

If the XML file containing the key is provided, then the user can see both files. In case the latter is missing, they can see only the first of the two files.  
  
To emphasize the "_**read-onlyness**_" property of the exposed file system it is sufficient to say that we implemented this property at two different levels:  

- the _write()_ operation was not implemented, instead for each request of this type our code throws an exception
- the _permissions_ on the files are 0444, that means _read-only_ for _owner, group_ and _others_  
    

At the moment **McAFuse** is available only on GNU/Linux. For those who are interested in all the CLI parameters accepted by our program, please have a look at the _README_ file present in the root directory of our project on GitHub (link below).

  
  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEihufRg6At4HEtvGLAucDt3LFWQhti8s_sTaT8vKUHIH7Jheqo4VFXYeLUl3nqfEJ45crn05Ir0ZXhXEHXN-0lD5b0Lkv-nC8noGVuVSZ7xDrOu_xFDT5nqdAy1vb0WPiG6FkdWuUaz8rI/w400-h133/mcafuse1.png)

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhR9vNMC0amzI32p_EqXWFGJTwdyUSlXZCXYxZN4lp0jxyLwxB0DcN5v6JjyEMd-mv9lIMsoP8-4rRhrGPJvDWgwip0olpWebSK2tDG9vn4FS7xjFkKZLpCj8qt-leFO4IjotsVOsoY7mg/w400-h234/mcafuse2.png)

### Conclusions

After 5 months of hard work this project came to a very successful end. We were able to implement a CLI tool to simplify the life of the DFIR examiners, letting them handle evidence with an easier and safer procedure than before. I could not have been able to terminate the work without the support I received from both my supervisors, _@dfirfpi_ and _Prof. Giovanni Lagorio_, for this reason I am very grateful to them. 

  

For those who are interested in the tool, you can find it here. Feel free to play with the code, and eventually provide a PR to improve it, I will be available to further discuss modifications, and improvements. Thank you all for the attention, and happy investigations!

**_@dfirfpi_** _here_ - Thank you Andrea for your great work! For those who want to keep in touch with Andrea, here are his _links_: linkedin, github and email.  

Go to Source
