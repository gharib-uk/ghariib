---
title: "From Pwn2Own Automotive: More Autel Maxicharger Vulnerabilities"
date: 2025-01-04
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "cybersecurity"
  - "security"
  - "zero_day"
  - "zeroday"
---

This blog post highlights two additional vulnerabilities in the Autel Maxicharger that were exploited at Pwn2Own Automotive 2024. Details of the patches are also included.

Autel has been informed and has deployed a firmware update (v1.35) to address both of these issues. If you want to read about other Autel bugs reported at Pwn2Own, you check out our earlier blog here.

**The First Vulnerability: CVE-2024-23967 (ZDI-CAN-23230)**

Researchers from Computest Sector 7 identified and exploited a stack-based buffer overflow in v1.32 of the Autel firmware. This vulnerability stems from the decoding of base64 encoded data that can be controlled by an attacker. The encoded data is decoded into a fixed-size stack buffer with no bounds checks, leading to a stack buffer overflow and remote code execution.

The vulnerable function is responsible for handling ACMP messages received from a websockets server (that can be attacker-controlled). The exact meaning of “ACMP” is unknown but the researchers’ best guess is that it stands for something along the lines of “Autel Cloud Management Protocol” and does not appear to be related to Allen-Cahn Message Passing.

Once the device receives an ACMP message, it is decoded as JSON and parsed with the help of the popular `cJSON` library. The JSON is expected to contain a nested key (named `data`) that contains a base64 encoded string. This encoded string is extracted from the JSON and passed to a custom base64 decoding function that decodes straight into a 1024-byte stack buffer (named `b64_decoded_buf`). As there are no length checks, the base64 decoding function can be exploited to trigger a stack buffer overflow.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/89c3e938-1a6a-4461-9dcc-6b1b8e676c44/1+-+p2o_2024_autel_b64_overflow.png?format=1000w)

As the screenshot shows, `base64_decode()` doesn't take length into account.

**The Patch**

To validate the patch in the updated firmware, the Ghidra Version Tracking tool was used. Comparing v1.32 against the latest v1.35 highlights a simple change (the right side shows the patch):

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/4b75abc1-a724-4685-afcb-40dfc25a1462/2+-+p2o_2024_autel_b64_overflow_patch.png?format=1000w)

An if statement has been added to check that the length of the attacker-controlled base64 encoded string is less than `0x556` (1366) bytes. If that check passes, the string is decoded into the 1024-byte stack buffer. This is a valid patch as the largest possible encoded string would be 1365 bytes in length, which, when base64 decoded, will output 1023 bytes, so that it fits into the stack buffer.

**Observations**

Even though the patch fixes the issue, it isn't an ideal solution. The if statement protects against a buffer overflow only in this specific function. Other code that may call `base64_decode()` could easily introduce another buffer overflow. In v1.35 there are no other references to `base64_decode()`, but that doesn't mean there won't be in the future. In its current state, the `base64_decode()` function takes the encoded input string and a pointer to an output buffer to hold the decoded string. A better implementation would take a third parameter that holds the length of the output buffer. This way, `base64_decode()` could internally check to ensure the number of bytes placed into the output buffer doesn't exceed the output buffer’s length. If it does, it could return an error. This would eliminate the need to place a length check before every invocation of `base64_decode()`.

Interestingly, the `base64_encode()` function does account for the output buffer’s length, but for some reason, the corresponding decode function doesn't.

Another solution would be to use the Mbed TLS base64 functions as both versions of the firmware already include the Mbed TLS library. These functions take length parameters to prevent buffer overflows.

**The Second Vulnerability: CVE-2024-23957 (ZDI-CAN-23241)**

The PHP Hooligans / Midnight Blue researchers also identified and exploited a stack buffer overflow in v1.32 of the Autel firmware. The overflow can be triggered when an attacker sends a large hex string, which is then decoded into a fixed-size stack buffer with no bounds checking.

The vulnerable function is related to the Dynamic Load Balancing (DLB) protocol that distributes a power load between a network of EV chargers. As part of the initial inter-charger communications, each charger receives a shared AES-128 key inside a JSON RPC payload. The key is encoded as a hex string and passed as an argument to the called RPC function.

The called function proceeds to extract the hex-encoded key from the JSON RPC parameters and then decodes it into a fixed-size stack buffer. The stack buffer is 16 bytes but takes up 20 bytes on the stack due to alignment. This is the correct size for an AES-128 key, however, since the attacker can supply a much longer string, a buffer overflow can occur.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/79971164-b691-4d88-8d30-445038928a01/3+-+p2o_2024_autel_hex_decoding.png?format=1000w)

Unlike the previous `base64_decode()` issue, the `hex_decode()` function does take a length (the encoded key length), but this doesn't prevent the overflow as the length is never checked against the size of the buffer.

**The Patch**

Again, the Version Tracking tool was used to compare the vulnerable function to the patched function. The highlighted sections on the right side show the patch:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/253a5bd0-9cda-4cff-86f8-ed49466ead60/4+-+p2o_2024_autel_hex_decoding_patch.png?format=1000w)

The screenshot shows an if statement has been added to ensure that `hex_encoded_key_len` is less than a certain value (`0x21`, or decimal 33) and is not equal to 0. The stack buffer is declared as 16 bytes, so this patch is valid, since a hex-encoded string of 32 bytes or less will successfully decode into a 16-byte buffer.

**Observations**

In a similar fashion to the first patch, the fix could be more effective. For the same reasons previously described, another buffer overflow vulnerability could be introduced if the `hex_decode()` function is used by another piece of code that doesn't check lengths with an explicit if statement. Currently, `hex_decode()` is only called by the patched function, but this may change with future releases.

Ideally, the length of the output buffer should be passed to `hex_decode()` so it can internally ensure a buffer overflow cannot happen.

**Conclusion**

It is a positive sign that Autel has responded to the vulnerabilities and has released a patch to help protect its customers, even if the patches are not ideal.

EV chargers are being deployed rapidly in homes around the world and represent safety risks when not secured. We are looking forward to Automotive Pwn2Own again in January 2025, and we will see if EV charger vendors have improved their product security. We hope to see you there.

You can find me on Twitter @ByteInsight, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

This blog post highlights two additional vulnerabilities in the Autel Maxicharger that were exploited at Pwn2Own Automotive 2024. Details of the patches are also included.

Autel has been informed and has deployed a firmware update (v1.35) to address both of these issues. If you want to read about other Autel bugs reported at Pwn2Own, you check out our earlier blog here.

**The First Vulnerability: CVE-2024-23967 (ZDI-CAN-23230)**

Researchers from Computest Sector 7 identified and exploited a stack-based buffer overflow in v1.32 of the Autel firmware. This vulnerability stems from the decoding of base64 encoded data that can be controlled by an attacker. The encoded data is decoded into a fixed-size stack buffer with no bounds checks, leading to a stack buffer overflow and remote code execution.

The vulnerable function is responsible for handling ACMP messages received from a websockets server (that can be attacker-controlled). The exact meaning of “ACMP” is unknown but the researchers’ best guess is that it stands for something along the lines of “Autel Cloud Management Protocol” and does not appear to be related to Allen-Cahn Message Passing.

Once the device receives an ACMP message, it is decoded as JSON and parsed with the help of the popular `cJSON` library. The JSON is expected to contain a nested key (named `data`) that contains a base64 encoded string. This encoded string is extracted from the JSON and passed to a custom base64 decoding function that decodes straight into a 1024-byte stack buffer (named `b64_decoded_buf`). As there are no length checks, the base64 decoding function can be exploited to trigger a stack buffer overflow.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/89c3e938-1a6a-4461-9dcc-6b1b8e676c44/1+-+p2o_2024_autel_b64_overflow.png?format=1000w)

As the screenshot shows, `base64_decode()` doesn't take length into account.

**The Patch**

To validate the patch in the updated firmware, the Ghidra Version Tracking tool was used. Comparing v1.32 against the latest v1.35 highlights a simple change (the right side shows the patch):

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/4b75abc1-a724-4685-afcb-40dfc25a1462/2+-+p2o_2024_autel_b64_overflow_patch.png?format=1000w)

An if statement has been added to check that the length of the attacker-controlled base64 encoded string is less than `0x556` (1366) bytes. If that check passes, the string is decoded into the 1024-byte stack buffer. This is a valid patch as the largest possible encoded string would be 1365 bytes in length, which, when base64 decoded, will output 1023 bytes, so that it fits into the stack buffer.

**Observations**

Even though the patch fixes the issue, it isn't an ideal solution. The if statement protects against a buffer overflow only in this specific function. Other code that may call `base64_decode()` could easily introduce another buffer overflow. In v1.35 there are no other references to `base64_decode()`, but that doesn't mean there won't be in the future. In its current state, the `base64_decode()` function takes the encoded input string and a pointer to an output buffer to hold the decoded string. A better implementation would take a third parameter that holds the length of the output buffer. This way, `base64_decode()` could internally check to ensure the number of bytes placed into the output buffer doesn't exceed the output buffer’s length. If it does, it could return an error. This would eliminate the need to place a length check before every invocation of `base64_decode()`.

Interestingly, the `base64_encode()` function does account for the output buffer’s length, but for some reason, the corresponding decode function doesn't.

Another solution would be to use the Mbed TLS base64 functions as both versions of the firmware already include the Mbed TLS library. These functions take length parameters to prevent buffer overflows.

**The Second Vulnerability: CVE-2024-23957 (ZDI-CAN-23241)**

The PHP Hooligans / Midnight Blue researchers also identified and exploited a stack buffer overflow in v1.32 of the Autel firmware. The overflow can be triggered when an attacker sends a large hex string, which is then decoded into a fixed-size stack buffer with no bounds checking.

The vulnerable function is related to the Dynamic Load Balancing (DLB) protocol that distributes a power load between a network of EV chargers. As part of the initial inter-charger communications, each charger receives a shared AES-128 key inside a JSON RPC payload. The key is encoded as a hex string and passed as an argument to the called RPC function.

The called function proceeds to extract the hex-encoded key from the JSON RPC parameters and then decodes it into a fixed-size stack buffer. The stack buffer is 16 bytes but takes up 20 bytes on the stack due to alignment. This is the correct size for an AES-128 key, however, since the attacker can supply a much longer string, a buffer overflow can occur.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/79971164-b691-4d88-8d30-445038928a01/3+-+p2o_2024_autel_hex_decoding.png?format=1000w)

Unlike the previous `base64_decode()` issue, the `hex_decode()` function does take a length (the encoded key length), but this doesn't prevent the overflow as the length is never checked against the size of the buffer.

**The Patch**

Again, the Version Tracking tool was used to compare the vulnerable function to the patched function. The highlighted sections on the right side show the patch:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/253a5bd0-9cda-4cff-86f8-ed49466ead60/4+-+p2o_2024_autel_hex_decoding_patch.png?format=1000w)

The screenshot shows an if statement has been added to ensure that `hex_encoded_key_len` is less than a certain value (`0x21`, or decimal 33) and is not equal to 0. The stack buffer is declared as 16 bytes, so this patch is valid, since a hex-encoded string of 32 bytes or less will successfully decode into a 16-byte buffer.

**Observations**

In a similar fashion to the first patch, the fix could be more effective. For the same reasons previously described, another buffer overflow vulnerability could be introduced if the `hex_decode()` function is used by another piece of code that doesn't check lengths with an explicit if statement. Currently, `hex_decode()` is only called by the patched function, but this may change with future releases.

Ideally, the length of the output buffer should be passed to `hex_decode()` so it can internally ensure a buffer overflow cannot happen.

**Conclusion**

It is a positive sign that Autel has responded to the vulnerabilities and has released a patch to help protect its customers, even if the patches are not ideal.

EV chargers are being deployed rapidly in homes around the world and represent safety risks when not secured. We are looking forward to Automotive Pwn2Own again in January 2025, and we will see if EV charger vendors have improved their product security. We hope to see you there.

You can find me on Twitter @ByteInsight, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

Go to Source
