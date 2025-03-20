---
title: "Password Breaking A to Z"
date: 2025-01-15
categories: 
  - "general"
  - "gpu"
  - "nvidia"
  - "password-recovery"
tags: 
  - "comprehensive"
  - "edpr"
  - "passwords"
---

![](https://blog.elcomsoft.com/wp-content/uploads/2024/07/passwords-az.jpg)

Our blog features numerous articles on breaking passwords and accessing encrypted data, ranging from simple “how-to” guides to comprehensive manuals. However, many of the questions we are frequently asked are not about the technical stuff but rather the very basics of password recovery. Can you break that password? Is it legal? How much time do you think it will take to break this one? We do have the answers, but they require digging through the extensive content of our blog. To address this, we’ve created a comprehensive A to Z article that not only answers many common questions but also links to our previous posts.

## Why do we attack passwords (and not encryption)?

We attack passwords instead of encryption because modern encryption methods are extremely secure and practically unbreakable due to their complexity and long key lengths. The industry-standard AES-256 algorithm used in practically all encrypted formats employs 256-bit keys, creating an astronomical number of possible combinations that would take an infeasible amount of time to crack through brute force. Passwords, on the other hand, are the entry point to this encryption and are often much weaker due to human limitations in remembering complex strings. Therefore, it’s more practical and efficient to target the password itself rather than attempting to break the encryption algorithm.

## What is “strong encryption”?

Strong encryption is a type of encryption that can’t be cracked in a reasonable amount of time by directly attacking the encryption key. A strong encryption method should have no vulnerabilities that would allow an attacker to significantly reduce the time needed to break it. To decrypt data protected by strong encryption, one must know the password or possess the original encryption key. The AES encryption algorithm with 256-bit keys is considered secure as no vulnerabilities have been discovered during the many years of its use. Therefore, if strong encryption is used, attacking the password is the only viable way to access the encrypted data.

## Will quantum computing change that?

Quantum computing does have the potential to change the landscape of encryption cracking. While classic encryption methods are incredibly secure against attacks with traditional computing methods, quantum computers can process information in fundamentally different ways. Grover’s algorithm, which quantum computers could use, would reduce the effective key length of AES-256 to 128 bits, immensely reducing the required time to brute-force the encryption key, yet even 128-bit keys are practically unbreakable. However, AES-256 is still considered secure in the face of quantum attacks because it would require a quantum computer with capabilities far beyond what is currently feasible. Therefore, while quantum computing poses future challenges (or opportunities, depending on which side you are), it does not yet fundamentally change the approach of attacking passwords as the entry point to encryption.

## Is breaking passwords legal?

The short answer is “it depends”. Regulations jurisdictions have different rules; in some countries suspects must reveal their passwords when interrogated by authorities (welcome to France, the country of Liberté). Obviously, no one can stop you from breaking your own lost or forgotten password; however, if this password protects access to your data stored in some online service, it does not actually matter that the account is yours as you cannot legally attack it. In other words, breaking passwords is perfectly legal if you work with local data and the data is yours, or if you have the permission from the legal owner, or if you work for legal authorities and follow the local regulations. Cracking someone else’s data may be a criminal offence, but there is a huge gray area.

> Everything You Wanted to Ask About Cracking Passwords

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“Everything You Wanted to Ask About Cracking Passwords” — ElcomSoft blog" src="https://blog.elcomsoft.com/2020/10/everything-you-wanted-to-ask-about-cracking-passwords/embed/#?secret=iBTFFneFjI#?secret=dsNzI4MVCN" data-secret="dsNzI4MVCN" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

## Is a million passwords a second a lot?

We have a tool that leverages the computational power of modern GPUs alongside with modern multi-core processors to maximize the speed of password attacks. Benchmarks demonstrate the recovery speed on various hardware configurations for different encryption formats. These speeds vary widely; for some formats, even the best hardware can only test a few passwords per second, while for others, speeds can reach millions passwords per second.

So, is a million passwords per second a lot or a little? The thing is, this is not the right question. The correct question to ask would be either “What kind of passwords can be broken withing a certain timeframe at a rate of a million passwords per second?” or “How long will it take to break a certain password at a rate of a million passwords per second?” To answer these, we published some formulas that will allow calculating the answer.

> The ABC’s of Password Cracking: The True Meaning of Speed

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“The ABC’s of Password Cracking: The True Meaning of Speed” — ElcomSoft blog" src="https://blog.elcomsoft.com/2020/11/the-abcs-of-password-cracking-the-true-meaning-of-speed/embed/#?secret=yH4XmslKTu#?secret=hNCvcjBRRH" data-secret="hNCvcjBRRH" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

In the first scenario, there is a typical situation where neither the length nor complexity of the password are known in advance, but there is a certain time limit on how long we can spend on the attack. In the second scenario, the limit is on the maximum password length and complexity (for example, we only try passwords containing digits and Latin letters in both cases plus a small set of special characters), while we calculate the time required to try all possible combinations.

For example, if a certain password can be attacked at the speed of 10 million passwords per second, it would take no more than five minutes to recover a password consisting of only 5 Latin letters in both cases. If the speed is 100 passwords per second and the password is at least 7 characters long and has symbols from the extended character range, the maximum attack time increases to around 700 billion seconds, or approximately 22,000 years. You can find formulas for calculating attack times and much more useful information in our guide.

> May the \[Brute\] Force Be with You!

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“May the [Brute] Force Be with You!” — ElcomSoft blog" src="https://blog.elcomsoft.com/2020/10/may-the-brute-force-be-with-you/embed/#?secret=f1g3gxg6An#?secret=jOxYqLUbZA" data-secret="jOxYqLUbZA" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

## How to benchmark password recovery speeds?

Benchmarking password recovery is a bit more complex than the nice graphs may suggest. We always test using a full brute-force attack method with a fixed password length and a specific character set limitation. In addition, the testing is always performed by running a brute-force attack. The brute-force attack allows us to measure the pure attack speed on a particular  GPU or CPU model. Other attack methods, such as mask attacks, dictionary attacks, or more complex hybrid attacks, require additional computations that may limit the utilization of the graphics card. Finally, we don’t start measurements immediately. Instead, we wait for several minutes for the attack to ‘settle’, giving the tool some time to load and compile the required code into the GPU.

> Elcomsoft Lab: Benchmarking Password Recovery Speeds

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“Elcomsoft Lab: Benchmarking Password Recovery Speeds” — ElcomSoft blog" src="https://blog.elcomsoft.com/2023/06/elcomsoft-lab-benchmarking-password-recovery-speeds/embed/#?secret=MCzBLgAGAG#?secret=dbY3wdGoTq" data-secret="dbY3wdGoTq" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

## Why do we need video cards? What about distributed attacks?

Most password protection methods rely on multiple rounds of hash iterations to slow down brute-force attacks. Even the fastest processors choke when trying to break a reasonably strong password. Video cards can be used to speed up the recovery with GPU acceleration, yet the GPU market is currently overheated, and most high-end video cards are severely overpriced. Today, we’ll test a bunch of low-end video cards and compare their price/performance ratio.

Making use of GPU cores instead of the CPU helps break passwords faster. Even the slowest built-in GPU with a TDP of several watts may demonstrate performance comparable to a 190W CPU without a sweat. High-end GPUs such as the NVIDIA RTX 4080 can break passwords up to 500 times faster compared to a common Intel Core i7 CPU, while mid-range video cards delivering up to a 250x boost.

GPU acceleration offloads computational-intensive calculations from the computer’s CPU onto the video card’s Compute Units (CUs). A dedicated video card can deliver the speed far exceeding the metrics of a high-end CPU. Even a modest integrated GPU (the one built into a CPU) may be able to match or exceed the performance of the central processor while consuming significantly less electricity and dissipating a fraction of the heat produced by the CPU with similar load.

> GPU Acceleration On The Cheap: Using Affordable Video Cards to Break Passwords Faster

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“GPU Acceleration On The Cheap: Using Affordable Video Cards to Break Passwords Faster” — ElcomSoft blog" src="https://blog.elcomsoft.com/2022/02/gpu-acceleration-on-the-cheap-using-affordable-video-cards-to-break-passwords-faster/embed/#?secret=OBAxxjjgTG#?secret=wLFXoB4TID" data-secret="wLFXoB4TID" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

Often enough, even multiple high-performance graphics cards might not be enough to successfully recover a password in reasonable timeframe. In such cases, distributed computing (see, Elcomsoft Distributed Password Recovery) comes to the rescue. The effectiveness of distributed computing versus using GPUs is highly dependent on on the data format and the hashing algorithm. If the data can be accelerated on a GPU, even basic graphics cards can outperform a large network of non-accelerated computers. However, several computers, each equipped with several of powerful GPUs, can provide a significant advantage over a single computer. Notably, some algorithms cannot be accelerated on GPUs at all, making distributed computing the only option for speeding up the attack.

Therefore, while a distributed network is generally better, each computer in the network should be equipped with powerful GPUs for optimal performance.

## Will video cards eventually replace CPUs?

This is highly unlikely. GPUs will not replace CPUs because of their fundamental architectural differences. GPUs excel at parallelizing a single operation across thousands of threads, making them highly efficient for tasks like password cracking. However, for everyday tasks, CPUs are more suitable because their cores can operate fully independently. This independence allows CPUs to handle a variety of tasks simultaneously, whereas each GPU core is slower than an individual CPU core and can only perform the same operation concurrently. Therefore, while GPUs are faster for specific parallelizable tasks, CPUs are necessary for the diverse and independent tasks encountered in daily computing.

## Which video card is best for breaking passwords?

If you are shopping for a new system, buy the most powerful NVIDIA board of the current generation that fits your budget. If you already have previous-generation GPUs, you can continue using those if they are powerful enough; if not, see above. Please note that previous-generation GPUs are generally not worth it if you are buying new, even if the price seems attractive. More on the subject:

> NVIDIA RTX 40 Series Graphics Cards: The Faster and More Efficient Password Recovery Accelerators

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“NVIDIA RTX 40 Series Graphics Cards: The Faster and More Efficient Password Recovery Accelerators” — ElcomSoft blog" src="https://blog.elcomsoft.com/2023/05/nvidia-rtx-40-series-graphics-cards-the-faster-and-more-efficient-password-recovery-accelerators/embed/#?secret=AqX5kMdemD#?secret=p5eLe11AYv" data-secret="p5eLe11AYv" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

The current NVIDIA range is complicated. There are multiple models of 4060 alone, as well as multiple variations of 4070 and 4080 to choose from. The following article will help you havigate NVIDIA’s product range:

> Navigating NVIDIA’s Super 40-Series GPU Update: A Guide for IT Professionals

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“Navigating NVIDIA’s Super 40-Series GPU Update: A Guide for IT Professionals” — ElcomSoft blog" src="https://blog.elcomsoft.com/2024/02/navigating-nvidias-super-40-series-gpu-update-a-guide-for-it-professionals/embed/#?secret=OUmSLFK52i#?secret=Xx8og7IGfj" data-secret="Xx8og7IGfj" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

## What about FPGA/ASIC?

They are great in theory, less so in real life due to their high cost and limited software support. Granted, one could build a cost-effective ASIC, but their price only goes down enough with economy of the scale. There is simply not enough demand from the password breaking perspective to build a cost-effective ASIC, and let us not forget about the still limited software support. If you are looking for efficient acceleration hardware, look for energy-efficient GPUs instead.

## How to build an efficient computer for breaking passwords

Power consumption and power efficiency are two crucial parameters that are often overlooked in favor of sheer speed. When building a workstation with 24×7 workload, absolute performance numbers become arguably less important compared to performance per watt.

> Building an Efficient Password Recovery Workstation: Power Savings and Waste Heat Management

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“Building an Efficient Password Recovery Workstation: Power Savings and Waste Heat Management” — ElcomSoft blog" src="https://blog.elcomsoft.com/2022/07/building-an-efficient-password-recovery-workstation-power-savings-and-waste-heat-management/embed/#?secret=zQIrIyDkT3#?secret=kjInnJNtW2" data-secret="kjInnJNtW2" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

We compared power efficiency and cooling solutions of various video cards:

> Building an Efficient Password Recovery Workstation: NVIDIA RTX Passwords-per-Watt Benchmarks

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“Building an Efficient Password Recovery Workstation: NVIDIA RTX Passwords-per-Watt Benchmarks” — ElcomSoft blog" src="https://blog.elcomsoft.com/2022/07/building-an-efficient-password-recovery-workstation-nvidia-rtx-passwords-per-watt-benchmarks/embed/#?secret=fYv9SnL0eT#?secret=lxzWnsnto3" data-secret="lxzWnsnto3" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

## Building a data center: calculating energy consumption

When building a data center with multiple computers equipped with power-hungry GPUs, it is crucial to calculate the total energy consumption of the system as a whole to ensure accurate planning for power delivery and heat dissipation.

For instance, consider a setup with one hundred GPUs, each with a maximum consumption of 300W, installed in a number of workstations. Alongside these, there would be the required number of UPS. Together, this setup would require at least 45KW of power. Not only will you need adequate power delivery, but also sufficient air conditioning to cool the room.

## How encryption works? What is the difference between passwords and encryption keys?

Passwords are used to protect access to documents, databases, compressed archives, encrypted disks, and many other things one can think of. When it comes to encryption, passwords are almost never stored, encrypted or not. Instead, passwords are “hashed”, or transformed with a one-way function. The result of the transformation, if one is performed correctly, cannot be reversed, and the original password cannot be “decrypted” from the result of a hash function.

Raw password hashes are rarely used directly as encryption keys. For example, many disk encryption tools can use passwords to encrypt a so-called “protector”, which in turn is used to protect the “key encryption key”, which in turn is used to protect the “media encryption key”, which in turn is finally used to encrypt and decrypt data. Notably, there are kinds of protectors that do not use passwords at all (instead, they can employ TPM, recovery keys, or USB flash drives for unlocking the disk). Password is the only thing that can be broken by trying multiple different combinations. More on passwords, hashing, and encryption:

> It’s Hashed, Not Encrypted

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“It’s Hashed, Not Encrypted” — ElcomSoft blog" src="https://blog.elcomsoft.com/2020/09/its-hashed-not-encrypted/embed/#?secret=C5NvqRPfiW#?secret=l6gfWmtABX" data-secret="l6gfWmtABX" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

Typically, a password is hashed using multiple rounds of a one-way transformation function, then stored in the file’s header, which allows verifying the password without actually decrypting the encrypted content. The encryption key itself is different from the saved hash, but password attacks are performed against this hash. If the correct password is found, the encryption key is calculated separately. Sometimes, the password hash is not stored in the file’s header, requiring part or all of the data to be decrypted to verify the password, which slows down the attack. The speed of such an attack depends on the size of data needing decryption. One common format using this approach is RAR4 archives. The later version, RAR5, is no longer using this approach.

> Breaking RAR5 and 7Zip Passwords

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“Breaking RAR5 and 7Zip Passwords” — ElcomSoft blog" src="https://blog.elcomsoft.com/2021/04/breaking-rar5-and-7zip-passwords/embed/#?secret=ThzqJfRFJ0#?secret=lPhSO4KOQC" data-secret="lPhSO4KOQC" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

## What kinds of attacks are available?

There are several methods for recovering the original password ranging from brute force to very complex rule-based attacks.

Theoretically, during a brute force attack one would try _all_ possible password combinations up to a certain length, but in practice, this is often limited to a subset of characters (like uppercase and lowercase Latin letters, digits, and a few special characters). Since the full Unicode set has 149,186 characters, running a brute-force attack on the entire character set is simply infeasible. Even a three-character password composed of the full Unicode set would take an impractically long time to crack. In reality, passwords rarely include symbols from such diverse and extended character sets, so brute force attacks are usually limited to certain alphabets.

> Use The Brute Force, Luke

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“Use The Brute Force, Luke” — ElcomSoft blog" src="https://blog.elcomsoft.com/2023/01/use-the-brute-force-luke/embed/#?secret=Ymk3YHWWoj#?secret=VEMDRoBhiD" data-secret="VEMDRoBhiD" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

Brute-force attacks are the fastest, but due to the largest number of passwords one needs to try during these attacks, brute-force is the last resort when all other options are exhausted. Since brute force is extremely inefficient for longer passwords, other types of attacks were invented to reduce the number of passwords to try. Dictionary attacks use words from the dictionary of English language (and/or the user’s native language) as possible passwords. Various other attacks using masks, mutations, and custom rules are also available.

> A Word About Dictionaries

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“A Word About Dictionaries” — ElcomSoft blog" src="https://blog.elcomsoft.com/2023/03/a-word-about-dictionaries/embed/#?secret=vRV8t0nMOA#?secret=U3ujqmqoh1" data-secret="U3ujqmqoh1" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

## Order matters

With the many types of password recovery attacks, when would you use each one of them, and in what order? The following article explains how to order password recovery jobs based on the types of data and information available about the owner.

> Building a Password Recovery Queue

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“Building a Password Recovery Queue” — ElcomSoft blog" src="https://blog.elcomsoft.com/2023/03/building-a-password-recovery-queue/embed/#?secret=zYHMogAgvV#?secret=SPAUYSJFY6" data-secret="SPAUYSJFY6" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

## GPU and CPU utilization

When it comes to encryption, data formats differ in various ways. One of the major differences is the type of hardware that can be used for running and accelerating password recovery attacks. There are three main scenarios:

1. Brute force supports GPU acceleration, with minimal CPU usage.
2. Brute force only works on CPUs, making GPUs irrelevant. GPU acceleration not supported.
3. Both GPUs and CPUs are utilized, with GPUs doing the bulk of the work and CPUs providing additional support (sometimes using multiple cores).

#### GPU-accelerated attacks

GPUs excel at performing multiple simple calculations in parallel across many cores. This makes them ideal for brute force attacks on formats that can be split into a large number of simple tasks. When a job can be split to run across thousands of GPU cores, the few cores of the computer’s CPU become insignificant, as they add minimal performance gains while increasing overhead.

Most current data formats are optimized for GPUs, but we observe a slow shift toward algorithms designed to be GPU-resistant.

#### CPU-only attacks (GPU-resistant algorithms)

Some algorithms are designed to resist GPU-accelerated attacks. This can be achieved by various means. For one, GPU cores handle simple tasks well but falter with more complex computations required by some password-to-binary key transformations. Such algorithms must run on CPUs, significantly slowing down the attack speed.

#### GPU-assisted attacks with high CPU utilization

Some algorithms utilize both GPU and CPU cores. Typically, the GPU handles most of the workload, while the CPU performs essential supporting tasks. For instance, a powerful 16-core CPU can assist the GPU, but it won’t match the brute force speed achievable by the GPU alone.

Non-brute force attacks, which are often more efficient, still heavily load the CPU. These “smart” attacks are slower than brute force because they rely on batches of passwords, which must be generated. On certain formats, the CPU may struggle to generate password batches quickly enough to fully load the GPU, thus requiring the generation to be run on the GPU itself, which further strains the GPU and slows the attack.

> Breaking Passwords on Alder Lake CPUs

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“Breaking Passwords on Alder Lake CPUs” — ElcomSoft blog" src="https://blog.elcomsoft.com/2022/05/breaking-passwords-on-alder-lake-cpus/embed/#?secret=sMzc4GlpBy#?secret=RljhI7S6mc" data-secret="RljhI7S6mc" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

#### Memory-intensive algorithms

Some hashing algorithms are designed to thwart GPU attacks by requiring significant memory usage. For example, the Scrypt algorithm, used in BestCrypt, ensures that even the weakest computers can check a single password without issues, but attempting to run many checks in parallel on a GPU would quickly exhaust its memory. This deliberate design choice makes such algorithms GPU-resistant.

> Breaking BestCrypt Volume Encryption 5

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“Breaking BestCrypt Volume Encryption 5” — ElcomSoft blog" src="https://blog.elcomsoft.com/2021/12/breaking-bestcrypt-volume-encryption-5/embed/#?secret=X0X3PjapaH#?secret=eKLiR1Go0E" data-secret="eKLiR1Go0E" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

## More information

We will keep posting about password recovery, yet we have many existing articles on the subject. Below are just a few of them.

> Decrypting Password-Protected DOC and XLS Files in Minutes

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“Decrypting Password-Protected DOC and XLS Files in Minutes” — ElcomSoft blog" src="https://blog.elcomsoft.com/2022/04/decrypting-password-protected-doc-and-xls-files-in-minutes/embed/#?secret=GFoFwrWSDn#?secret=m7EGWZRxio" data-secret="m7EGWZRxio" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

> Microsoft Office encryption evolution: from Office 97 to Office 2019

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“Microsoft Office encryption evolution: from Office 97 to Office 2019” — ElcomSoft blog" src="https://blog.elcomsoft.com/2019/10/microsoft-office-encryption-evolution-from-office-97-to-office-2019/embed/#?secret=4Tyeefbnzu#?secret=tlRcNU6QCV" data-secret="tlRcNU6QCV" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

Password leaks can be used to accelerate cold attacks:

> How to Break 30 Per Cent of Passwords in Seconds

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“How to Break 30 Per Cent of Passwords in Seconds” — ElcomSoft blog" src="https://blog.elcomsoft.com/2017/02/how-to-break-30-per-cent-of-passwords-in-seconds/embed/#?secret=uO2CsDyWRn#?secret=RLIAyhLVq0" data-secret="RLIAyhLVq0" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

> How to Break 70% of Passwords in Minutes

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“How to Break 70% of Passwords in Minutes” — ElcomSoft blog" src="https://blog.elcomsoft.com/2017/02/how-to-break-70-of-passwords-in-minutes/embed/#?secret=mfc8NnEs9L#?secret=AzMYmhjxvy" data-secret="AzMYmhjxvy" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

> Password Crackers’ Gold Mine: Browser Passwords

<iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" title="“Password Crackers’ Gold Mine: Browser Passwords” — ElcomSoft blog" src="https://blog.elcomsoft.com/2021/06/password-crackers-gold-mine-browser-passwords/embed/#?secret=RDTt1kJryQ#?secret=5em2lqfZrR" data-secret="5em2lqfZrR" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

Go to Source
