---
title: "TR Protostar Stack 0 Writeup"
date: Sun, 06 Jan 2019 12:12:28 +0000
draft: false
type: posts
categories: 
- Walkthrough,Protostar,Stack 0,WriteUp
---
# TR Protostar Stack 0 Writeup

<br/>

<br/>
[CanYouPwnMe! - For Cyber Security Researchers](https://canyoupwn.me) [CanYouPwnMe! - For Cyber Security Researchers - cypm!](https://canyoupwn.me)

Stack0.c Amaç: “you have changed the ‘modified’ variable” satırını yazdırmak. #include <stdlib.h> #include <unistd.h> #include <stdio.h> int main(int argc, char \*\*argv) { volatile int modified; char buffer\[64\]; modified = 0; gets(buffer); if(modified != 0) { printf("you have changed the 'modified' variable\\n"); } else { printf("Try again?\\n"); } }   İstediğimiz cümleyi yazdırabilmemiz için programın akışını değiştirilebilmemiz \[…\]

[TR | Protostar: Stack 0 Writeup](https://canyoupwn.me/tr-protostar-stack-0-writeup/) [Emir Kurt](https://canyoupwn.me/author/0xf61/)

#### [Source](https://canyoupwn.me/tr-protostar-stack-0-writeup/)

<br/>
---
