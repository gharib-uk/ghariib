---
title: "TR Protostar Stack 1 Writeup"
date: Wed, 09 Jan 2019 13:00:08 +0000
draft: false
type: posts
categories: 
- Walkthrough,Protostar,Stack 1,WriteUp
---
# TR Protostar Stack 1 Writeup

<br/>

<br/>
[CanYouPwnMe! - For Cyber Security Researchers](https://canyoupwn.me) [CanYouPwnMe! - For Cyber Security Researchers - cypm!](https://canyoupwn.me)

Stack1.c Amaç: “you have correctly got the variable to the right value” satırını yazdırmak. #include <stdlib.h> #include <unistd.h> #include <stdio.h> #include <string.h> int main(int argc, char \*\*argv) { volatile int modified; char buffer\[64\]; if(argc == 1) { errx(1, "please specify an argument\\n"); } modified = 0; strcpy(buffer, argv\[1\]); if(modified == 0x61626364) { printf("you have correctly got \[…\]

[TR | Protostar: Stack 1 Writeup](https://canyoupwn.me/tr-protostar-stack-1-writeup/) [Emir Kurt](https://canyoupwn.me/author/0xf61/)

#### [Source](https://canyoupwn.me/tr-protostar-stack-1-writeup/)

<br/>
---
