---
title: "Solving the 2015 FLARE On RE Contest - Challenge 1"
date: 2015-10-30T19:37:00.002-04:00
draft: false
type: posts
categories: 
- assembly,ctf,cybersecurity,ida,information securty,reverse engineering,Security,windows,x86
---
# Solving the 2015 FLARE On RE Contest - Challenge 1

<br/>

<br/>
After my last [post](http://www.redblue.team/2015/10/disassembling-loops-and-control.html) on disassembling control flow structures, a colleague of mine mentioned that I should participate in the 2016 _FLARE On_ challenge next year. Due to work and life obligations I have not had the opportunity to partake in the previous 2 contests but I figured now would be a good time to get some practice in and prepare for 2016. I am by no means an expert and this is very much a learning exercise for me, one that I intend to document and share. I'll be doing a series of posts (when I find time) over the next few months documenting my progress.  
  
The FLARE On contest is an annual capture-the-flag style reverse engineering competition from FireEye. If you'd like to follow along you can download the materials [here](http://flare-on.com/). Each challenge involves assessing a sample with the sole intent of finding a key (an e-mail address).  
  
**Challenge #1**  
The first file is a self-extracting zip and will dump an executable into a specified directory with the name "i\_am\_happy\_you\_are\_to\_playing\_the\_flareon\_challenge.exe". It's a long and oddly named file. I'll assume the intentional grammatical deficiencies are for comedic purposes and don't have any greater significance, for now :)  
  
Before executing this file lets check out the import table to get an idea of what it does. It's only 2KB in size so it is clearly a small application with very limited functionality.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiKiHKUf6MJ57nci-uV7OwCOzjbeq-TMxwzwwwUaokj0fIASLwiVl_Fi86OD00so3IkjMZyWTrSu-U07l26Oy_KlVlv-3Tjo42mwC7mSwpyncjpVDKfp1LLBwdPhk5Z3WNlm95FbLsLGdA/s320/imports.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiKiHKUf6MJ57nci-uV7OwCOzjbeq-TMxwzwwwUaokj0fIASLwiVl_Fi86OD00so3IkjMZyWTrSu-U07l26Oy_KlVlv-3Tjo42mwC7mSwpyncjpVDKfp1LLBwdPhk5Z3WNlm95FbLsLGdA/s1600/imports.png)

Pretty standard stuff here, no notable cryptography or networking related libraries. This binary appears setup to take some input and that's about all.  
  
When we run the file this hypothesis is confirmed. The application prints two strings, prompts the user for input, checks if it is the correct value, and then prints a string depending on whether the user input the secret key. I tried entering a variety of answers that included special characters and lengthy input but could not generate an alternate error message.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj6_Bu4JIKKasMZJJ_FjlqTnh4phLQaCQOcct01O8jwLfslcbJeEhXxLbxwmM5NOtKhsPQZkhShM7w40utwy-kd4YpT6YU1tsdSBs9h5K7XsfV1a6vjEmUi7iV810vF8T2-M8kV3NwJy2o/s320/program-execution.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj6_Bu4JIKKasMZJJ_FjlqTnh4phLQaCQOcct01O8jwLfslcbJeEhXxLbxwmM5NOtKhsPQZkhShM7w40utwy-kd4YpT6YU1tsdSBs9h5K7XsfV1a6vjEmUi7iV810vF8T2-M8kV3NwJy2o/s1600/program-execution.PNG)

At this point I elected to fire up IDA and take a peek at the disassembled routines. Before analyzing the main function though, lets review the raw hex and look for some of the strings that we observed during interactive program execution.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEip1utDlGKoNzIL3_4c6QNgHzfEQNnsYeuajvc3uCjPrZQ9EqhVelJoMZK0FnampF1o5nB0QwUnpWk73i67nDX1wq32DoTQYSUjHMI4pn-SRi9RkK9JRGYZDMMPnItbE8sO-_qcL3ZDMdM/s320/hexview-24char-string.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEip1utDlGKoNzIL3_4c6QNgHzfEQNnsYeuajvc3uCjPrZQ9EqhVelJoMZK0FnampF1o5nB0QwUnpWk73i67nDX1wq32DoTQYSUjHMI4pn-SRi9RkK9JRGYZDMMPnItbE8sO-_qcL3ZDMdM/s1600/hexview-24char-string.png)

My first observation here is that after the 'You are failure' string we see  24 characters that appear mutated and are followed by some null values. Let's keep this in mind and take a peak at the application entry point.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj5MXD0yuLJfvwvsn_N0Iwmytn1dI-QOVcy9NkPvM5XU59DdeLYoA2eF98iWjskZgi-OKxyDJqSShZy7cODFKeldWK-oVChSs6JIpq8OqG6PO6dwP8R1p1AqcDN5v8GdVAsx6GYPamq86Y/s320/main-function.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj5MXD0yuLJfvwvsn_N0Iwmytn1dI-QOVcy9NkPvM5XU59DdeLYoA2eF98iWjskZgi-OKxyDJqSShZy7cODFKeldWK-oVChSs6JIpq8OqG6PO6dwP8R1p1AqcDN5v8GdVAsx6GYPamq86Y/s1600/main-function.png)

Our application is initialized and GetStdHandle is called. This function retrieves a handle to the specified standard device (standard input, standard output, standard error). User input is taken and written to the memory offset for variable 'byte\_402158'. Now we enter the core logic of the application.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj-wcNx7Qpt85ddS9bRs_4EszSAxlGHFQU_oMnBoN2llsj46kiUXzp9zDir9Pza5ubKfG0_3e_rr2-PAhrTJw_W79mM6pJgieRpsLwAP9NjE6rxNPhnCQA1Xz7bdn4vOmfpQi2CuDDDd7c/s320/xor-core-logic.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj-wcNx7Qpt85ddS9bRs_4EszSAxlGHFQU_oMnBoN2llsj46kiUXzp9zDir9Pza5ubKfG0_3e_rr2-PAhrTJw_W79mM6pJgieRpsLwAP9NjE6rxNPhnCQA1Xz7bdn4vOmfpQi2CuDDDd7c/s1600/xor-core-logic.png)

The first routine takes the first low order byte (x86 is little-endian and the AL register is 8 bits) from the ECX register and moves it into AL. A [bit-wise XOR operation](https://en.wikipedia.org/wiki/Bitwise_operation) is then performed against the AL register and the hex value '7D'. The result is then compared to a value stored at the memory offset of variable 'byte\_402140'. If the values do not match then we get the error message. However, if the value does match then ECX is incremented and the XOR comparison occurs again. It iterates through this loop 0x18 times. In decimal this is 24 loops, which interestingly is the length of the mutated character we noted earlier. The memory offset referenced by variable 'byte\_402140' is the location of this suspicious string as well.  
  
At this point there is enough evidence indicating that the mutated characters are in fact the secret key. I thought about writing a program to XOR each byte at that offset by 0x7D but got lazy and used the Python interpreter.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgfbNb36mAFu9c0ksfwnCHM38B1ttMCP-Ceg06AFrTxEzPLPCDAb90gM1fcOYFaKaroSW4Eqtf4SkN_aPfT3bnaw5fGrxEq_EYQxb2q4Tx3_jWHrPd5ZqA0h5_lNQYgbF1bt6QXu0vKiC8/s320/xorvalues-python.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgfbNb36mAFu9c0ksfwnCHM38B1ttMCP-Ceg06AFrTxEzPLPCDAb90gM1fcOYFaKaroSW4Eqtf4SkN_aPfT3bnaw5fGrxEq_EYQxb2q4Tx3_jWHrPd5ZqA0h5_lNQYgbF1bt6QXu0vKiC8/s1600/xorvalues-python.PNG)

  
Once you have computed each value, assemble the string and convert the hexadecimal values to ASCII.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhZ8ZKGkUESUuHGenwUHoHQUk6XFqJLQsdfm7XqjX4yagzE4w6Eg-EpvpGOtEzINxFVp9GTDEjuvKSAk0l3tisV4Wki3XnyH2x3Ffmtmbyd78bOL_0cKgVM7dOxgVAgDeYGws4A-v3Ikt0/s320/hex-ascii-conversion.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhZ8ZKGkUESUuHGenwUHoHQUk6XFqJLQsdfm7XqjX4yagzE4w6Eg-EpvpGOtEzINxFVp9GTDEjuvKSAk0l3tisV4Wki3XnyH2x3Ffmtmbyd78bOL_0cKgVM7dOxgVAgDeYGws4A-v3Ikt0/s1600/hex-ascii-conversion.PNG)

  
Well that certainly looks like an email address. Sure enough, when we enter this as our key value it is successful.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiEZvx4Y1HBViL8oPbE7qfSvwZaOuBwpRdMvW-uG7qhnRz5MqVYEiZnfhsBob7kNpmcqpbVgvQL9DuTSDNLqTzqcpLKTaSH3vafHJRmCPRbIpvFTY-8ofxDBPeMcAB-jNQWt0JsIU6U8d4/s320/solved.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiEZvx4Y1HBViL8oPbE7qfSvwZaOuBwpRdMvW-uG7qhnRz5MqVYEiZnfhsBob7kNpmcqpbVgvQL9DuTSDNLqTzqcpLKTaSH3vafHJRmCPRbIpvFTY-8ofxDBPeMcAB-jNQWt0JsIU6U8d4/s1600/solved.PNG)

  
Ta-da! This first challenge was fairly straightforward and didn't take long at all but I assume the difficulty level will ramp up quite quickly.

#### [Source](https://www.redblue.team/feeds/1755251800487206585/comments/default)

<br/>
---
