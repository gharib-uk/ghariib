---
title: "ZODA: Zero-Overhead Data Availability"
date: 2025-01-10
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **ZODA: Zero-Overhead Data Availability**  
_Alex Evans, Nicolas Mohnblatt, Guillermo Angeris_

We introduce ZODA, short for 'zero-overhead data availability,' which is a protocol for proving that symbols received from an encoding (for tensor codes) were correctly constructed. ZODA has optimal overhead for both the encoder and the samplers. Concretely, the ZODA scheme incurs essentially no incremental costs (in either computation or size) beyond those of the tensor encoding itself. In particular, we show that a slight modification to the encoding scheme for tensor codes allows sampled rows and columns of this modified encoding to become proofs of their own correctness. When used as part of a data availability sampling protocol, both encoders (who encode some data using a tensor code with some slight modifications) and samplers (who sample symbols from the purported encoding and verify that these samples are correctly encoded) incur no incremental communication costs and only small additional computational costs over having done the original tensor encoding. ZODA additionally requires no trusted setup and is plausibly post-quantum secure.

Go to Source
