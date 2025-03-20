---
title: "Egghunter I - Vulnserver GMON"
date: 2025-01-02
categories: 
  - "security"
---

Introduzione Una delle sfide durante la scrittura di un exploit è la gestione dello spazio che serve per inserire lo shellcode malevolo. Certe volte si trovano Stack Buffer Overflow, ma ci sono talmente pochi byte per l’inserimento dello shellcode che è necessario trovare un metodo alternativo. L’egghunter (letteralmente “cacciatore di uova”) è una piccola porzione di codice che, una volta inserito, andrà alla ricerca di una particolare stringa (l’uovo appunto) in tutta la memoria del programma.

Go to Source
