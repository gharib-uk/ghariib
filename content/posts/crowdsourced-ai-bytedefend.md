---
title: "Crowdsourced AI += ByteDefend"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "virus"
---

We are pleased to announce the integration of a new solution into our Crowdsourced AI initiative. This model, developed by Dr. Ran Dubin from the Department of Computer Science at Ariel University and head of ByteDefend Cyber Lab at the Ariel Cyber Innovation Center, is designed to analyze suspicious macros in Microsoft Office files, including Word, Excel, and PowerPoint.

VirusTotal's Crowdsourced AI initiative leverages various AI models and community contributions to strengthen cyber defense strategies. Like any other security solution, AI-based models are not infallible, but they offer invaluable contributions by complementing other technologies in analyzing and detecting new threats. The integration of ByteDefend enhances VirusTotal's Code Insight capabilities, currently with up to three independent AI engines for Microsoft Office documents.

Here is the most recent example at the time of writing: all three models agree that the analyzed XLS file is malicious, each providing different levels of detail.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgydwdCxcKwERL4wgEnAViayMSu98fdvfELnu9ED2wj4T7z-0pTjUEgzN2fLIfcJYwH-hLAAL2V-KQHbKFB4dMpZOYNze7iSaK2CAMxtGuGPaWDpqe15fMcpuaCnmLkB3GMMx42AAxbR_JUmkMavbCTn1WV4PxjE6Wn8jCp0uPAm7vUaMxmh9oSr4-sKs8/s1600/Screenshot%202024-05-15%2010.20.29.png)

  

Here's another example where the models don't agree. ByteDefend flags a DOC file as malicious, while Hispasec's engine says it's benign. These disagreements are interesting because even though the final verdict can be subjective depending on the context (what's risky in one situation might not be in another), the models clearly explain how the macros work. This gives the human analyst all the information they need to make the final call..

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjKtNO6bhTR3Ro7YA_Zf6ufkn10ibB9yDGbhhFIMeUOKikk3Bnsjai1JfefD4Z5keS2WLzVkbkjOK4vZYgrCs0Is3OO0Yn7ei7JOA-ptl0VDayM-atIg6MYvxPmrgtpyzYmYL1qICkHPwvDfn8X71yRmw7_gfxT9nzHdOUi5SW2b-AT5LCHRLkTrC60rVY/s1600/Screenshot%202024-05-15%2010.36.22.png)

  

AI reports’ results are available via VT Intelligence, allowing the use of the "bytedefend\_ai\_analysis:" modifier to search into the resulting AI’s output, and "bytedefend\_ai\_verdict:" to search by verdict - malicious or benign. As an example, below we show the results of searching for ByteDefend reports where "telegram" is mentioned and the verdict is "malicious". This search is performed using the following query: _bytedefend\_ai\_analysis:telegram and bytedefend\_ai\_verdict:malicious_

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhemgfeZMXh6RHwgAcKrKNDDYlz5oJ7IyLdbyNHtw4lptAkBeTlLkSdlX2fZ6Eoft5P018uPBBiG1Jj_-bXD2rXAsO8osbU4_7cwPwn6vvjXhuywUvRvf-Q5mEJuaEdLmcQhdGgoBp6Os3hpaOrPhRzBIfq3YbnCVyhbsIKwibIdDGaH1dKUmIyVVh479U/s1600/Screenshot%202024-05-15%2011.14.15.png)

  

We extend our thanks to Dr. Ran Dubin and the ByteDefend Cyber Lab for their valuable contribution to VirusTotal's Crowdsourced AI initiative. We are continuously working to expand this effort by welcoming more contributors with diverse skills and expertise. Our goal is to build a collaborative and powerful defense strategy to tackle the constantly evolving landscape of cyber threats. We encourage others in the security community to join us in this effort.

We are pleased to announce the integration of a new solution into our Crowdsourced AI initiative. This model, developed by Dr. Ran Dubin from the Department of Computer Science at Ariel University and head of ByteDefend Cyber Lab at the Ariel Cyber Innovation Center, is designed to analyze suspicious macros in Microsoft Office files, including Word, Excel, and PowerPoint.

VirusTotal's Crowdsourced AI initiative leverages various AI models and community contributions to strengthen cyber defense strategies. Like any other security solution, AI-based models are not infallible, but they offer invaluable contributions by complementing other technologies in analyzing and detecting new threats. The integration of ByteDefend enhances VirusTotal's Code Insight capabilities, currently with up to three independent AI engines for Microsoft Office documents.

Here is the most recent example at the time of writing: all three models agree that the analyzed XLS file is malicious, each providing different levels of detail.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgydwdCxcKwERL4wgEnAViayMSu98fdvfELnu9ED2wj4T7z-0pTjUEgzN2fLIfcJYwH-hLAAL2V-KQHbKFB4dMpZOYNze7iSaK2CAMxtGuGPaWDpqe15fMcpuaCnmLkB3GMMx42AAxbR_JUmkMavbCTn1WV4PxjE6Wn8jCp0uPAm7vUaMxmh9oSr4-sKs8/s1600/Screenshot%202024-05-15%2010.20.29.png)

  

Here's another example where the models don't agree. ByteDefend flags a DOC file as malicious, while Hispasec's engine says it's benign. These disagreements are interesting because even though the final verdict can be subjective depending on the context (what's risky in one situation might not be in another), the models clearly explain how the macros work. This gives the human analyst all the information they need to make the final call..

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjKtNO6bhTR3Ro7YA_Zf6ufkn10ibB9yDGbhhFIMeUOKikk3Bnsjai1JfefD4Z5keS2WLzVkbkjOK4vZYgrCs0Is3OO0Yn7ei7JOA-ptl0VDayM-atIg6MYvxPmrgtpyzYmYL1qICkHPwvDfn8X71yRmw7_gfxT9nzHdOUi5SW2b-AT5LCHRLkTrC60rVY/s1600/Screenshot%202024-05-15%2010.36.22.png)

  

AI reports’ results are available via VT Intelligence, allowing the use of the "bytedefend\_ai\_analysis:" modifier to search into the resulting AI’s output, and "bytedefend\_ai\_verdict:" to search by verdict - malicious or benign. As an example, below we show the results of searching for ByteDefend reports where "telegram" is mentioned and the verdict is "malicious". This search is performed using the following query: _bytedefend\_ai\_analysis:telegram and bytedefend\_ai\_verdict:malicious_

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhemgfeZMXh6RHwgAcKrKNDDYlz5oJ7IyLdbyNHtw4lptAkBeTlLkSdlX2fZ6Eoft5P018uPBBiG1Jj_-bXD2rXAsO8osbU4_7cwPwn6vvjXhuywUvRvf-Q5mEJuaEdLmcQhdGgoBp6Os3hpaOrPhRzBIfq3YbnCVyhbsIKwibIdDGaH1dKUmIyVVh479U/s1600/Screenshot%202024-05-15%2011.14.15.png)

  

We extend our thanks to Dr. Ran Dubin and the ByteDefend Cyber Lab for their valuable contribution to VirusTotal's Crowdsourced AI initiative. We are continuously working to expand this effort by welcoming more contributors with diverse skills and expertise. Our goal is to build a collaborative and powerful defense strategy to tackle the constantly evolving landscape of cyber threats. We encourage others in the security community to join us in this effort.

Go to Source
