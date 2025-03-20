---
title: "IND-CPA$^{text{C}}$: A New Security Notion for Conditional Decryption in Fully Homomorphic Encryption"
date: 2025-01-15
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **IND-CPA$^{text{C}}$: A New Security Notion for Conditional Decryption in Fully Homomorphic Encryption**  
_Bhuvnesh Chaturvedi, Anirban Chakraborty, Nimish Mishra, Ayantika Chatterjee, Debdeep Mukhopadhyay_

Fully Homomorphic Encryption (FHE) allows a server to perform computations directly over the encrypted data. In general FHE protocols, the client is tasked with decrypting the computation result using its secret key. However, certain FHE applications benefit from the server knowing this result, especially without the aid of the client. Providing the server with the secret key allows it to decrypt all the data, including the client's private input. Protocols such as Goldwasser et. al. (STOC'13) have shown that it is possible to provide the server with the capability of conditional decryption that allows it to decrypt the result of some pre-defined computation and nothing else. While beneficial to an honest-but-curious server to aid in providing better services, a malicious server may utilize this added advantage to perform active attacks on the overall FHE application to leak secret information. Existing security notions fail to capture this scenario since they assume that only the client has the ability to decrypt FHE ciphertexts. Therefore, in this paper, we propose a new security notion named IND-CPA$^{text{C}}$, that provides the adversary with access to a conditional decryption oracle. We then show that none of the practical exact FHE schemes are secure under this notion by demonstrating a generic attack that utilizes this restricted decryption capability to recover the underlying errors in the FHE ciphertexts. Given the security guarantee of the underlying (R)LWE hardness problem collapses with the leakage of these error values, we show a full key recovery attack. Finally, we propose a technique to convert any IND-CPA secure FHE scheme into an IND-CPA$^text{C}$ secure FHE scheme. Our technique utilizes Succinct Non-Interactive Argument of Knowledge, where the server is forced to generate a proof of an honest computation of the requested function along with the computation result. During decryption, the proof is verified, and the decryption proceeds only if the verification holds. Both the verification and decryption steps run inside a Garbled Circuit and thus are not observable or controllable by the server.

Go to Source
