---
title: "Further Improvements in AES Execution over TFHE: Towards Breaking the 1 sec Barrier"
date: 2025-01-19
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Further Improvements in AES Execution over TFHE: Towards Breaking the 1 sec Barrier**  
_Sonia Belaïd, Nicolas Bon, Aymen Boudguiga, Renaud Sirdey, Daphné Trama, Nicolas Ye_

Making the most of TFHE advanced capabilities such as programmable or circuit bootstrapping and their generalizations for manipulating data larger than the native plaintext domain of the scheme is a very active line of research. In this context, AES is a particularly interesting benchmark, as an example of a nontrivial algorithm which has eluded \`\`practical'' FHE execution performances for years, as well as the fact that it will most likely be selected by NIST as a flagship reference in its upcoming call on threshold (homomorphic) cryptography. Since 2023, the algorithm has thus been the subject of a renewed attention from the FHE community and has served as a playground to test advanced operators following the LUT-based, $p$-encodings or several variants of circuit bootstrapping, each time leading to further timing improvements. Still, AES is also interesting as a benchmark because of the tension between boolean- and byte-oriented operations within the algorithm. In this paper, we resolve this tension by proposing a new approach, coined \`\`hippo'', which consistently combines the (byte-oriented) LUT-based approach with a generalization of the (boolean-oriented) $p$-encodings one to get the best of both worlds. In doing so, we obtain the best timings so far, getting a single-core execution of the algorithm over TFHE from $46$ down to $32$ seconds and approaching the $1$ second barrier with only a mild amount of parallelism. We should also stress that all the timings reported in this paper are consistently obtained on the same machine which is often not the case in previous studies. Lastly, we emphasize that the techniques we develop are applicable beyond just AES since the boolean-byte tension is a recurrent issue when running algorithms over TFHE.

Go to Source
