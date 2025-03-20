---
title: "DEP Bypass I - Vulnserver TRUN"
date: 2025-01-02
categories: 
  - "security"
---

Introduzione La Data Exectution Prevention (DEP) è una funzione di protezione della memoria inserita da Windows XP in poi e consente al sistema di contrassegnare una o più pagine di memoria come non eseguibili. Ciò significa che il codice non può essere eseguito da quella regione di memoria, il che rende più difficile lo sfruttamento di buffer overflow. Se un’applicazione tenta di eseguire codice da una pagina di dati protetta, si verifica un’eccezione di violazione dell’accesso alla memoria e, se l’eccezione non viene gestita, il processo chiamante viene terminato.

Go to Source
