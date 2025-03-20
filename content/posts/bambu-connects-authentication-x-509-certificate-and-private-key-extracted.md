---
title: "Bambu Connect’s Authentication X.509 Certificate and Private Key Extracted"
date: 2025-01-20
---

![](https://hackaday.com/wp-content/uploads/2025/01/bambu_connect_extracted_certificate_key_source.jpg?w=800)

Hot on the heels of Bambu Lab’s announcement that it would be locking down all network access to its X1-series 3D printers with new firmware, the X.509 certificate and private key from the Bambu Connect application have now been extracted by \[hWuxH\]. This application was intended to be the sole way for third-party software to send print jobs to Bambu Lab hardware as we previously reported.

The Bambu Connect app is a fairly low-effort Electron-based affair, with some attempt at obfuscation and encryption, but not enough to keep prying eyes out. The de-obfuscated `main.js` file can be found here (archived), with the certificate and private key clearly visible. These are used to encrypt HTTP traffic with the printer, and is the sole thing standing in the way of tools like OrcaSlicer talking with authentication-enabled Bambu Lab printers.

As for what will be the next steps by Bambu Lab, it’s now clear that security through obfuscation is not going to be very effective here. While playing whack-a-mole with (paying) users who are only interested in using their hardware in the way that they want is certainly an option, this might be a wake-up call for the company that being more forthcoming with their userbase would be in anyone’s best interest.

We await Bambu Lab’s response with bated breath.

Go to Source
