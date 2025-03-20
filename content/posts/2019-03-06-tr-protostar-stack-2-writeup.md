---
title: "TR Protostar Stack 2 Writeup"
date: Wed, 06 Mar 2019 12:12:38 +0000
draft: false
type: posts
categories: 
- Walkthrough,Protostar,Stack 2,WriteUp
---
# TR Protostar Stack 2 Writeup

<br/>

<br/>
[CanYouPwnMe! - For Cyber Security Researchers](https://canyoupwn.me) [CanYouPwnMe! - For Cyber Security Researchers - cypm!](https://canyoupwn.me)

Stack2.c Amaç: “you have correctly got the variable to the right value” satırını yazdırmak. #include <stdlib.h> #include <unistd.h> #include <stdio.h> #include <string.h> int main(int argc, char \*\*argv) { volatile int modified; char buffer\[64\]; char \*variable; variable = getenv("GREENIE"); if(variable == NULL) { errx(1, "please set the GREENIE environment variable\\n"); } modified = 0; strcpy(buffer, variable); if(modified \[…\]

[TR | Protostar: Stack 2 Writeup](https://canyoupwn.me/tr-protostar-stack-2-writeup/) [Emir Kurt](https://canyoupwn.me/author/0xf61/)

#### [Source](https://canyoupwn.me/tr-protostar-stack-2-writeup/)

<br/>
---
