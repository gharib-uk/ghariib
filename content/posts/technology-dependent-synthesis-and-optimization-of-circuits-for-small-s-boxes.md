---
title: "Technology-Dependent Synthesis and Optimization of Circuits for Small S-boxes"
date: 2025-01-25
categories: 
  - "cryptography"
  - "security"
tags: 
  - "cryptography"
  - "security"
---

ePrint Report: **Technology-Dependent Synthesis and Optimization of Circuits for Small S-boxes**  
_Zihao Wei, Siwei Sun, Fengmei Liu, Lei Hu, Zhiyu Zhang_

Boolean formula minimization is a notoriously hard problem that is known to be $varSigma\_2^P$-complete. Circuit minimization, typically studied in the context of a much broader subject known as synthesis and optimization of circuits, introduces another layer of complexity since ultimately those technology-independent epresentations (e.g., Boolean formulas and truth tables) has to be transformed into a netlist of cells of the target technology library. To manage those complexities, the industrial community typically separates the synthesis process into two steps: technology-independent optimization and technology mapping. In each step, this approach only tries to find the local optimal solution and relies heavily on heuristics rather than a systematic search. However, for small S-boxes, a more systematic exploration of the design space is possible. Aiming at the global optimum, we propose a method which can synthesize a truth table for a small S-box directly into a netlist of the cells of a given technology library. Compared with existing technology-dependent synthesis tools like LIGHTER and PEIGEN, our method produces improved results for many S-boxes with respect to circuit area. In particular, by applying our method to the $mathbb{F}\_{2^4}$-inverter involved in the tower field implementation of the AES S-box, we obtain the currently known lightest implementation of the AES S-box. The search framework can be tweaked to take circuit delay into account. As a result, we find implementations for certain S-boxes with both latency and area improved.

Go to Source
