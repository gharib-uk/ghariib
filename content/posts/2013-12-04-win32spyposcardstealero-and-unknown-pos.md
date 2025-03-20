---
title: "Win32SpyPOSCardStealerO and unknown POS Sniffer"
date: Wed, 04 Dec 2013 15:50:00 +0000
draft: false
type: posts
categories: 
- [object Object],[object Object],[object Object],[object Object],[object Object],[object Object]
---
# Win32SpyPOSCardStealerO and unknown POS Sniffer

<br/>

<br/>
Finally some new stuff (hmm, no)  
Let's talk about Win32/Spy.POSCardStealer.O identified by ESET.  
It's pretty lame but let's see it anyway.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjYNjhfFIJdWO1hRdU-MzDG2dNWjrlWgWgB_yM4hcyiN-6NxCvNMr9JZMhP3vhSZf1ZEmo3riXF3wViy5FSyOAFEzv8Ap6_SPMceTxixHD-90VFDQbMtTUgUsX14EBUGFpa2POXCjIOM5Y/s400/12-08-2013+21-53-55.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjYNjhfFIJdWO1hRdU-MzDG2dNWjrlWgWgB_yM4hcyiN-6NxCvNMr9JZMhP3vhSZf1ZEmo3riXF3wViy5FSyOAFEzv8Ap6_SPMceTxixHD-90VFDQbMtTUgUsX14EBUGFpa2POXCjIOM5Y/s1600/12-08-2013+21-53-55.png)

  
On the first procedure the malware will register a reg key in HKLM with 'HDebugger'  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhomG0cyBtm68IE1dk3i2uNJYeYA8JBhFJY7xV2cU6EfzFLYrkD9rspDza_eYrANcTOmtKIaZgdFZmyIqINlhlKsPZbrEqbGicDpLgTdfu3I4qXa7L-sLdK4bVTibIQ4OR8gBi0Nhke68U/s400/12-08-2013+21-56-52.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhomG0cyBtm68IE1dk3i2uNJYeYA8JBhFJY7xV2cU6EfzFLYrkD9rspDza_eYrANcTOmtKIaZgdFZmyIqINlhlKsPZbrEqbGicDpLgTdfu3I4qXa7L-sLdK4bVTibIQ4OR8gBi0Nhke68U/s1600/12-08-2013+21-56-52.png)

  
And start to search for track2:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjh2qfsWQ4-V37s9yMhuv2Lj2B-fDzpA9ZThc_Wa3J_5TdGdmqPjWGo0qn5bzxJ98813rDsbkoKw__2XzzQ11BUGv3nM-0bRRyHMpzb6aq4A6KUVIVafhoGNZYHWCAF1hgN8w-O9MHtHIU/s400/12-08-2013+22-18-31.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjh2qfsWQ4-V37s9yMhuv2Lj2B-fDzpA9ZThc_Wa3J_5TdGdmqPjWGo0qn5bzxJ98813rDsbkoKw__2XzzQ11BUGv3nM-0bRRyHMpzb6aq4A6KUVIVafhoGNZYHWCAF1hgN8w-O9MHtHIU/s1600/12-08-2013+22-18-31.png)

  
Then he call the C&C (hoqou.su/forum.php):  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhQS2AoeLKg9fPkX8hlfYMc_ZB7Yy7qy2nKFAzZNvnYI2eXgAsyh3MOPp1-CvvAzEiuciOmbwH-MaFd_9Y_k7Lee6uMy_gzsSn431LuP_3C-0eGMGVEZYg1mcbZc-EAnl4jqV4X2nutUNE/s400/12-08-2013+22-38-35.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhQS2AoeLKg9fPkX8hlfYMc_ZB7Yy7qy2nKFAzZNvnYI2eXgAsyh3MOPp1-CvvAzEiuciOmbwH-MaFd_9Y_k7Lee6uMy_gzsSn431LuP_3C-0eGMGVEZYg1mcbZc-EAnl4jqV4X2nutUNE/s1600/12-08-2013+22-38-35.png)

• dns: 1 ›› ip: 62.173.149.140 - adresse: HOQOU.SU  
  
Do a sleep of 120000 ms (2 minutes):  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiGU4ski1jTqWdze5u-lVozxzMcBaAJK9tnSKCikyrbGdM55FwauI5oDPjhRl56gKXlrvdQZovQdSMi58YWexSvv0AVYeG84bNNzqhfGLTlcRJzuGNgMx00OWWvrBKZO6f8o2KsKlgFHzE/s400/12-08-2013+22-44-18.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiGU4ski1jTqWdze5u-lVozxzMcBaAJK9tnSKCikyrbGdM55FwauI5oDPjhRl56gKXlrvdQZovQdSMi58YWexSvv0AVYeG84bNNzqhfGLTlcRJzuGNgMx00OWWvrBKZO6f8o2KsKlgFHzE/s1600/12-08-2013+22-44-18.png)

  
And redo into the track2 research procedure.  
When finaly something is found the malware took the PID of the program, the process name and the mem adress:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhs94RCypx9kcQdfNYArvsfwTXOyHcyi_qhgISMWufkovdLnxSHY0EUdsX6t1NNkwJfyKIGjRw_fwQ4NbcLNPATbQRGt8t3Ga1qJQNAOwzxYdzLMGx0E328hZTWTPgjq6cU53m_KXMfjt0/s400/12-08-2013+23-00-47.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhs94RCypx9kcQdfNYArvsfwTXOyHcyi_qhgISMWufkovdLnxSHY0EUdsX6t1NNkwJfyKIGjRw_fwQ4NbcLNPATbQRGt8t3Ga1qJQNAOwzxYdzLMGx0E328hZTWTPgjq6cU53m_KXMfjt0/s1600/12-08-2013+23-00-47.png)

Then he send it to the C&C...  
  
POST req example:  
%5BPID%201224%20%28MSR.exe%29%5D%0D%0A%20ADDR%20000B2F90%3A%20%224111111111111111%3D13071010000000000666%22%0D%0A%5BEOF%5D%0D%0A  
  
This malware can't receive orders, and don't have a special mechanism.  
On another sample, i've found another domain: rolex216.8s.nl/go/go.php  
• dns: 1 ›› ip: 41.223.53.155 - adresse: ROLEX216.8S.NL  
  
This malware was downloaded from a downloader who now download another malware who brute force wordpress sites (maybe i will talk about this one soon).  
  
Still with POS Malware a 'new' threat (Detected only with generic signatures) appeared.  
[https://www.virustotal.com/fr/file/746cb8cf77b0b00f14c424731948d8fc13378978d193d75f944b12c25e98e0e2/analysis/1376958328/](https://www.virustotal.com/fr/file/746cb8cf77b0b00f14c424731948d8fc13378978d193d75f944b12c25e98e0e2/analysis/1376958328/)  
I got this sample since august from a guys who found this on his POS systems.  
In 3 months there is still no one who have do an accurate signature.  
  
At first he will create two directories 'System\\Hidden' inside %APPDATA%\\Microsoft\\Windows  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgNOL_49r4bNm7PwWFN_h-uXbjW6ZE1u-5az53J5QRDVpAwJjBRT0x1of9A8y-oHc98mKIb6PcT2g6owLGtjUUJ3clWbQpm1AnZAR5Xm6EdOK4fk3Rq4DsZenuF42zm4ng2W7famATN3QA/s400/20-08-2013+00-37-35.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgNOL_49r4bNm7PwWFN_h-uXbjW6ZE1u-5az53J5QRDVpAwJjBRT0x1of9A8y-oHc98mKIb6PcT2g6owLGtjUUJ3clWbQpm1AnZAR5Xm6EdOK4fk3Rq4DsZenuF42zm4ng2W7famATN3QA/s1600/20-08-2013+00-37-35.png)

  
Do a directory test to know from where the executable is launched:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh8So20MwT1g2W6lp-B4_YJF0lzLv1PhsN0EFFEWueUo3Xqp1y6YOQHSB5UyR1GWyl_hbiGX_SNeg2tGADul9ZwNSGM6FvMk17WcM-Qk8WXVHT4iDw3BN-uaGJoQtphgp64NhKnAxQG2Kk/s400/20-08-2013+01-36-35.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh8So20MwT1g2W6lp-B4_YJF0lzLv1PhsN0EFFEWueUo3Xqp1y6YOQHSB5UyR1GWyl_hbiGX_SNeg2tGADul9ZwNSGM6FvMk17WcM-Qk8WXVHT4iDw3BN-uaGJoQtphgp64NhKnAxQG2Kk/s1600/20-08-2013+01-36-35.png)

  
Copy the EXE and launch the copy:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhHMWveezx07LJ8WM4fMD-f8ftF7z_fX92hF0lSZ4J0EUyDr4m5IU-m5L_igPji7Yp8wmdf_QbqLJdr-6eD9WL12Z9B_l7AzUYtUnj2Kxo5h90wIlUoqRRXLebC6KX8KnWnnVrGx1hnrY4/s400/20-08-2013+01-21-12.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhHMWveezx07LJ8WM4fMD-f8ftF7z_fX92hF0lSZ4J0EUyDr4m5IU-m5L_igPji7Yp8wmdf_QbqLJdr-6eD9WL12Z9B_l7AzUYtUnj2Kxo5h90wIlUoqRRXLebC6KX8KnWnnVrGx1hnrY4/s1600/20-08-2013+01-21-12.png)

  
A registry key "Svchost-Windows-Redquired" is created for persistence  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhfc_dAI7dDs2k5xN5-C6VJFHHRTp6Pw5KpIUmJzahtJ0LBn2z7QQXWJUIur0jpzZnsAxJk6Emp8-EWbSuMJIdbWZebaDl_hpuKY03JgjTeJE9GQlk_sP1KDBi0o5sK8Lsgel8WSqJI-YI/s400/20-08-2013+01-24-40.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhfc_dAI7dDs2k5xN5-C6VJFHHRTp6Pw5KpIUmJzahtJ0LBn2z7QQXWJUIur0jpzZnsAxJk6Emp8-EWbSuMJIdbWZebaDl_hpuKY03JgjTeJE9GQlk_sP1KDBi0o5sK8Lsgel8WSqJI-YI/s1600/20-08-2013+01-24-40.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiwzypdlKqUIMfhxzuflgNHqpORgxZGXAjcmG9WCkXJ2I4vBWKmj8tMK1J78MMHXNIIIjVdogobvjxxDqbcwrqVc0P-iHS1tP9y0fcZhR67LCiRGUa6-tY-LotM5Qq5G9RxPNI6SU6h01Y/s400/20-08-2013+01-26-46.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiwzypdlKqUIMfhxzuflgNHqpORgxZGXAjcmG9WCkXJ2I4vBWKmj8tMK1J78MMHXNIIIjVdogobvjxxDqbcwrqVc0P-iHS1tP9y0fcZhR67LCiRGUa6-tY-LotM5Qq5G9RxPNI6SU6h01Y/s1600/20-08-2013+01-26-46.png)

  
Enter in a procedure to remove the original file:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjxqH32kFyjDPfF30Xh7kWm253jM5fydaXQldR-3vaDdMsRvJFo7OweiD8m0NVGauC9OPH-v4_i2d1shmkiDqsC2DM8yuYiMBkVqowN3LBgsWeSjVLJW3EhIQsU1CGE0fF_qG3tuHd7TIk/s400/20-08-2013+01-28-11.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjxqH32kFyjDPfF30Xh7kWm253jM5fydaXQldR-3vaDdMsRvJFo7OweiD8m0NVGauC9OPH-v4_i2d1shmkiDqsC2DM8yuYiMBkVqowN3LBgsWeSjVLJW3EhIQsU1CGE0fF_qG3tuHd7TIk/s1600/20-08-2013+01-28-11.png)

/c del C:\\DOCUME~1\\ADMINI~1\\Bureau\\svchost.exe >> NUL  
And as excepted send a exit code just after...  
  
So what's do the fresh copy inside the 'good' folder ?  
Firstly he take the jump due to the directory test.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjhimIB6bcZdU0f2qB7OSrkqIoamK3XI-ahmh688pG3pEI0TdT3XFBjVqmU4Hrf-jm9m3dVt27UHZo0Mfwd6gwv_3lgdUpjG0dkrddy2PyA6NmwV8BFPhhMweWe0tf8W3UikhuyG1tjba0/s400/20-08-2013+01-42-34.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjhimIB6bcZdU0f2qB7OSrkqIoamK3XI-ahmh688pG3pEI0TdT3XFBjVqmU4Hrf-jm9m3dVt27UHZo0Mfwd6gwv_3lgdUpjG0dkrddy2PyA6NmwV8BFPhhMweWe0tf8W3UikhuyG1tjba0/s1600/20-08-2013+01-42-34.png)

  
On the procedure he will compute a string based on GetSystemFileTime, then he start to enumerate process.  
He will open them one by one, read the memory and look for track 2 in a subroutine.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEioyj6c_yLKGbeCM6BTRzy4qr8SjcVi7w6lKXCt_o7_48zt-c_j9NiFbRMtMPmez-8tDLeYsqP1pH-Gzd3IoOz5xo9ytkDwB_ZuA3sQecLrATJQzNBMNsvQVpswd2KJi54INKShqAByYhs/s400/20-08-2013+02-05-04.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEioyj6c_yLKGbeCM6BTRzy4qr8SjcVi7w6lKXCt_o7_48zt-c_j9NiFbRMtMPmez-8tDLeYsqP1pH-Gzd3IoOz5xo9ytkDwB_ZuA3sQecLrATJQzNBMNsvQVpswd2KJi54INKShqAByYhs/s1600/20-08-2013+02-05-04.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEguhyqmXfSLusQS8CRiqfmvCU-f40SbXTvB8u9MnkfeqRcjHM9Uvbcv-WqUDYY9pLACAqcEcJofGabz-njT9dTAfZ2sbC0IOFy4xnRALqMBasYKnG2aPz0zCinUxbg4_e2mMs3o6ElW3yQ/s400/20-08-2013+01-54-04.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEguhyqmXfSLusQS8CRiqfmvCU-f40SbXTvB8u9MnkfeqRcjHM9Uvbcv-WqUDYY9pLACAqcEcJofGabz-njT9dTAfZ2sbC0IOFy4xnRALqMBasYKnG2aPz0zCinUxbg4_e2mMs3o6ElW3yQ/s1600/20-08-2013+01-54-04.png)

Usual stuff.  
They search by partern from the second part of tracks 2 '=13' '=14' '=15' etc..  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgnpVZFmbXL_Mdve65WTNnHqPvkFN_feLysXFvZ_tMcL5uKABmjn7GYbLW1fDZktfiM90kU7BQOi5vivUFkXhiFBaZWKF5XEQBfTs3h5BgSD05jsYoNPJ0kJKRn2nNO8y6ziLq_VJBUxbk/s400/25-08-2013+18-31-13.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgnpVZFmbXL_Mdve65WTNnHqPvkFN_feLysXFvZ_tMcL5uKABmjn7GYbLW1fDZktfiM90kU7BQOi5vivUFkXhiFBaZWKF5XEQBfTs3h5BgSD05jsYoNPJ0kJKRn2nNO8y6ziLq_VJBUxbk/s1600/25-08-2013+18-31-13.png)

  
A file 'Sys.dll' is created:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhTRHsmW9FK7NSg5_tepgc6UJP9uxe380405jF3iyNm7nH0Q_gfGMb17Lh0Onticl3zvuhJ8m7SFO93vkoacNE2Aw1sguhuZmHZmqW0j-eczLC1Uxug4Xe6T1VtTVmcX8WPjOKlPamHnSg/s400/22-08-2013+15-21-44.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhTRHsmW9FK7NSg5_tepgc6UJP9uxe380405jF3iyNm7nH0Q_gfGMb17Lh0Onticl3zvuhJ8m7SFO93vkoacNE2Aw1sguhuZmHZmqW0j-eczLC1Uxug4Xe6T1VtTVmcX8WPjOKlPamHnSg/s1600/22-08-2013+15-21-44.png)

timestamped with  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgKV3ToVuOItbevbjcT2rc2mtj82d1t75sp0aPRaT0SOQZqqzcZPepNVKS-SiV6d8lhqnIiv4MlLmk-dVxJhnVlWoSp6IqpLtMIzchD8wc1mZc-RBVu6-XwKVR8gESlPN5niYcAmU8Lkxs/s1600/25-08-2013+18-21-10.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgKV3ToVuOItbevbjcT2rc2mtj82d1t75sp0aPRaT0SOQZqqzcZPepNVKS-SiV6d8lhqnIiv4MlLmk-dVxJhnVlWoSp6IqpLtMIzchD8wc1mZc-RBVu6-XwKVR8gESlPN5niYcAmU8Lkxs/s1600/25-08-2013+18-21-10.png)

(encoded)  
And wrote:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjzZoQq73WkppAiWREiOl5hy53qtc6hOaMK6ddluwmR0yCd2TTO-c5nTEFG6uiAbm9Jid6q1PZ1wAjPk8RNegVANigdVSZEuJlwglxxKqYRYJslPpiza7KMhxcZ16jM3n1VDTI9ik2ocqE/s400/22-08-2013+15-43-37.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjzZoQq73WkppAiWREiOl5hy53qtc6hOaMK6ddluwmR0yCd2TTO-c5nTEFG6uiAbm9Jid6q1PZ1wAjPk8RNegVANigdVSZEuJlwglxxKqYRYJslPpiza7KMhxcZ16jM3n1VDTI9ik2ocqE/s1600/22-08-2013+15-43-37.png)

  
Do a sleep of 450000 ms (7 1/2 minutes)  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgzoPtWPRpk82KHlxP45HROhbBNOHw8O_qwfq2ZALgaNXsFC_3t0dc9iu8ETFNTIEbDPbAZ4uNRFsTsIcqREQrOUZFl1BkECqvL-NL8ljNjUJkmCve9lcjnmq8fNqE5-nLPQ76_gfevjhY/s400/20-08-2013+02-24-42.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgzoPtWPRpk82KHlxP45HROhbBNOHw8O_qwfq2ZALgaNXsFC_3t0dc9iu8ETFNTIEbDPbAZ4uNRFsTsIcqREQrOUZFl1BkECqvL-NL8ljNjUJkmCve9lcjnmq8fNqE5-nLPQ76_gfevjhY/s1600/20-08-2013+02-24-42.png)

  
if a dump is found the dump is encoded:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiN-tL4fLosqquCqOb1MOnGI6z6PgdRXNfLlcFzgdK9f42_rPIaHy7Sld-6wCuYRXCFn2gVr9NTMe4028RJGox-QFExV8fzoiaDbd4ez-ebxV-6Eca5f00BGbPT14eJJUu1tFIxVqK2RCM/s400/25-08-2013+17-51-28.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiN-tL4fLosqquCqOb1MOnGI6z6PgdRXNfLlcFzgdK9f42_rPIaHy7Sld-6wCuYRXCFn2gVr9NTMe4028RJGox-QFExV8fzoiaDbd4ez-ebxV-6Eca5f00BGbPT14eJJUu1tFIxVqK2RCM/s1600/25-08-2013+17-51-28.png)

And wrote in Sys.dll.  
  
Then they are sent one by one to the C&C:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiG4iQUNKPc0zOyXGBFmR0sZWjkXwnEtYJK_orCDZQbZrfTDmaBUCE2v1ErVDVK-igy3YFG5j0ji3dCjlswo8CFOhUWSwxQA6fW_LCAL6E3C_FctR8g6tikhWIcSMFbq66-e3RkeuAi5CE/s400/25-08-2013+18-06-25.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiG4iQUNKPc0zOyXGBFmR0sZWjkXwnEtYJK_orCDZQbZrfTDmaBUCE2v1ErVDVK-igy3YFG5j0ji3dCjlswo8CFOhUWSwxQA6fW_LCAL6E3C_FctR8g6tikhWIcSMFbq66-e3RkeuAi5CE/s1600/25-08-2013+18-06-25.png)

  
http://mcsup.cc/8edf4bc26f9c526ff846c9068f387dac/?update=daily&random=563245325050324532495458495358  
http://mcsup.cc/8edf4bc26f9c526ff846c9068f387dac/redirect.php  
http://mcsup.cc/8edf4bc26f9c526ff846c9068f387dac/website.php  
5.9.96.235  
The md5 hash '8edf4bc26f9c526ff846c9068f387dac' is 'zabeat'  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgbYeeuNj2cDCsB_Saqac-zeVxqZ79UAmjeRev0hnjlWVjagTTsivEH3enZn_M4ev2O6Z0aQFi9pPkQaEcmMREEEOB7rAuOQ67BshzaasknyKayaoQ-8P910kiRPxGvtup0qJVCRUcctiQ/s400/25-09-2013+09-53-55.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgbYeeuNj2cDCsB_Saqac-zeVxqZ79UAmjeRev0hnjlWVjagTTsivEH3enZn_M4ev2O6Z0aQFi9pPkQaEcmMREEEOB7rAuOQ67BshzaasknyKayaoQ-8P910kiRPxGvtup0qJVCRUcctiQ/s1600/25-09-2013+09-53-55.png)

#### [Source](https://www.xylibox.com/2013/12/win32spyposcardstealero-and-unknown-pos.html)

<br/>
---
