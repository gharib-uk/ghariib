---
title: "SEH Overflow I - Vulnserver GMON"
date: 2025-01-02
categories: 
  - "security"
---

Introduzione Con questo articolo continuiamo la serie Stack Overflow passando ad un nuovo argomento, ossia il SEH Structured exception handling. Un gestore di eccezioni è un costrutto di programmazione utilizzato per fornire un modo strutturato per gestire le condizioni di errore a livello di sistema e di applicazione. A livello di codice solitamente si trova sotto forma di un costrutto di questo tipo try { //fai qualcosa } catch { //se va in errore fai altro } Ogni volta che viene incontrato un try block, un puntatore al corrispondente exception handler è salvato sullo stack nella struttura \_EXCEPTION\_REGISTRATION\_RECORD.

Go to Source
