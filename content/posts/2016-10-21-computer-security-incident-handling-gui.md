---
title: "Computer Security Incident Handling Guide - A presentation based off of the NIST paper"
date: 2016-10-20T20:59:00.002-04:00
draft: false
type: posts
---
# Computer Security Incident Handling Guide - A presentation based off of the NIST paper

<br/>

<br/>
A few years ago during an interview at Mandiant I was asked to create a presentation based on the NIST [Computer SecurityIncident Handling Guide](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf), a good primer on incident handling that I would recommend every NetSec  professional to read.  
  
Although the presentation is light in description, the basic outline remains. If the content interests you I would highly recommend reading the NIST report.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi8on4ClAkFjq-axLX3rtRgYOQICrDWPf4UFQUMhtYvMVpqCPN1gYAhZKvlKShES-HuOlxPMOHt6Bx-F64PNHjY2_5oh2lCME55P4WerfCgNXLxg1grHHMHPGueHVXEtRQY1SvmOhpwXzMi/s640/Slide1.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi8on4ClAkFjq-axLX3rtRgYOQICrDWPf4UFQUMhtYvMVpqCPN1gYAhZKvlKShES-HuOlxPMOHt6Bx-F64PNHjY2_5oh2lCME55P4WerfCgNXLxg1grHHMHPGueHVXEtRQY1SvmOhpwXzMi/s1600/Slide1.PNG)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiqaPH4a_kYZMnLBcU5Bnwheq4tFEBi-suVJ0OTlCN77PXZ2Rr6kF8gqHmSV_nrOnbQfXp8SRwTQOVMdUzojJ352NmR32Q-3eFBBXOh5atPehhcGxFi5Itjwu4pNy5jNDBo9NoAhS8I0qOo/s640/Slide2.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiqaPH4a_kYZMnLBcU5Bnwheq4tFEBi-suVJ0OTlCN77PXZ2Rr6kF8gqHmSV_nrOnbQfXp8SRwTQOVMdUzojJ352NmR32Q-3eFBBXOh5atPehhcGxFi5Itjwu4pNy5jNDBo9NoAhS8I0qOo/s1600/Slide2.PNG)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjhzdyEeICdpV0TYwTjAqVOWOrz5vp_dkyNz15qGadsD2RDsQLK005CGyT8i-WPqVNfG4QHh3BciIssgu0T3FhH4gIbgFA3_Pd3x4BIqWfS3d8RRTbUmgF2wGagbKM3lNinBBOlit0UYiYf/s640/Slide3.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjhzdyEeICdpV0TYwTjAqVOWOrz5vp_dkyNz15qGadsD2RDsQLK005CGyT8i-WPqVNfG4QHh3BciIssgu0T3FhH4gIbgFA3_Pd3x4BIqWfS3d8RRTbUmgF2wGagbKM3lNinBBOlit0UYiYf/s1600/Slide3.PNG)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjvtgWhd-B69hqyZK8F-gP3z74OflumOVoGEDEL9yJ1kWhPLy_yfF5QA-bg0TU5xkpUEA1VD0HUNa35fsJgPuybEJJC6z-nIDFexBgwCi28bFYv1rXo1zQDOcBniAcSYyxfR51WV9j0JaaC/s640/Slide4.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjvtgWhd-B69hqyZK8F-gP3z74OflumOVoGEDEL9yJ1kWhPLy_yfF5QA-bg0TU5xkpUEA1VD0HUNa35fsJgPuybEJJC6z-nIDFexBgwCi28bFYv1rXo1zQDOcBniAcSYyxfR51WV9j0JaaC/s1600/Slide4.PNG)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgV7LXHrseYWPsZdGewcMsliHi7pUTQiRPTfKNekIx_OLi6faoiFW-qY1wKvLGSAR3wvuZld-RC-XIsZkpW00fJe1dkLPPx1YNOaksswBpf1cCds3bSdXytQR6tpREv20AMUDlbDsd2JRlJ/s640/Slide5.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgV7LXHrseYWPsZdGewcMsliHi7pUTQiRPTfKNekIx_OLi6faoiFW-qY1wKvLGSAR3wvuZld-RC-XIsZkpW00fJe1dkLPPx1YNOaksswBpf1cCds3bSdXytQR6tpREv20AMUDlbDsd2JRlJ/s1600/Slide5.PNG)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiAzO-kM7-QXp0qls8s_tZ2PG3QpL-e2SFig_AgPfcI1pBGgSVlqTLSA5ZYIisRX8uICidUaHvW4YNJdCAQDknSB_3v0dZisdvx22-hly8zK9SlQhk2oaNm66CP8Gykxb7m7CmOXO_rOq8v/s640/Slide6.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiAzO-kM7-QXp0qls8s_tZ2PG3QpL-e2SFig_AgPfcI1pBGgSVlqTLSA5ZYIisRX8uICidUaHvW4YNJdCAQDknSB_3v0dZisdvx22-hly8zK9SlQhk2oaNm66CP8Gykxb7m7CmOXO_rOq8v/s1600/Slide6.PNG)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgop0yMtjIX4HVFyNi2EUHW9jtmtf6AisgXhTtbPwO6XUWZ8sIR7mfBFkSGrcGsGg0u1EB3V39MM63V9DDBZ8Ka1_Vt43REDNLeJ5U8Vmq0PESIztfUTzVdbl7wRN10pYSL1yH4FI69Gf4P/s640/Slide7.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgop0yMtjIX4HVFyNi2EUHW9jtmtf6AisgXhTtbPwO6XUWZ8sIR7mfBFkSGrcGsGg0u1EB3V39MM63V9DDBZ8Ka1_Vt43REDNLeJ5U8Vmq0PESIztfUTzVdbl7wRN10pYSL1yH4FI69Gf4P/s1600/Slide7.PNG)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEipznYCiZHAc4UvS7AJlg8Gi-LO2CcU6haoekhDghC4XLiBhWfH7cQazkCX0M_c3tmY4QpuahroFz0tR1WMdhotcUCP51C23u_BcKd4lzXIHh8EZ_P1kYr_tuGMN8SkF7qFA_bZfnRqsYJA/s640/Slide8.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEipznYCiZHAc4UvS7AJlg8Gi-LO2CcU6haoekhDghC4XLiBhWfH7cQazkCX0M_c3tmY4QpuahroFz0tR1WMdhotcUCP51C23u_BcKd4lzXIHh8EZ_P1kYr_tuGMN8SkF7qFA_bZfnRqsYJA/s1600/Slide8.PNG)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgniP3dURjQ5WZmfKRgCctVynrJ2_seTN1gmKTiI_dUTsRaV6fn9RK_YKSjH86JNN2cl7affNADZWJXdKJXtkL3bVMsmEW5h_wDROhAooOzLuwQC2TnyyJb79HXOd-1-biqCKnw0TaEGDR6/s640/Slide9.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgniP3dURjQ5WZmfKRgCctVynrJ2_seTN1gmKTiI_dUTsRaV6fn9RK_YKSjH86JNN2cl7affNADZWJXdKJXtkL3bVMsmEW5h_wDROhAooOzLuwQC2TnyyJb79HXOd-1-biqCKnw0TaEGDR6/s1600/Slide9.PNG)

#### [Source](https://www.redblue.team/feeds/8648419756274894326/comments/default)

<br/>
---
