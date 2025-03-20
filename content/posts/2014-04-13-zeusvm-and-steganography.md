---
title: "ZeusVM and steganography"
date: Sun, 13 Apr 2014 19:20:00 +0000
draft: false
type: posts
categories: 
- [object Object],[object Object],[object Object],[object Object],[object Object],[object Object]
---
# ZeusVM and steganography

<br/>

<br/>
Months ago, researchers observed an evolution of ZeusVM, time to get back on this family.  
  
For informations,  
The first ZeusVM sample i've seen using steganography was the 21 November 2013.  
The IP of the C&C have Russian origin: [212.44.64.202](https://www.virustotal.com/en/ip-address/212.44.64.202/information/)  
A Sutra TDS who redirect on Nuclear Exploit pack was pushing the payload, Roman of abuse.ch blacklisted 212.44.64.202 one month later on his [Zeus tracker](https://zeustracker.abuse.ch/monitor.php?host=212.44.64.202).  
  
The first guy who publicly wrote about ZeusVM change is probably Jerome Segura of [Malwarebytes](http://blog.malwarebytes.org/security-threat/2014/02/hiding-in-plain-sight-a-story-about-a-sneaky-banking-trojan/).  
Actually the latest version i've saw in the wild is 1.0.0.5, and if you want a hash: e4c31d18b92ad6e19cb67be2e38c3bd1 (sample is fresh of today)  
  
Let's have a look on the first server that i've see now... 212.44.64.202.  
Pony, Multilocker, Mailers, Grum and an older version of ZeusVM (without steganography) was also hosted on this server but that not the topic.  
  
The filename of login scripts and ZeusVM configs were hardnamed in russian, like:  
borodinskoesrajenie.jpg (http://en.wikipedia.org/wiki/Battle\_of\_Borodino)  
vhodtolkodlyaelfov.php (only elves can enter)  
logovoelfov.php (elf's den)  
domawniypitomec.php (domestic animal)  
jivotnoe.php (animal)  
larecotkryt.php (the chest is open)  
And so on.. overall the panel design seem back to the original zeus style (not like the previous 'generation' of ZeusVM with casper)  
  
/kec/:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjskgkmq1Ur6-adBTRJZ_MtOPQ2atkCH_xXUJhI5y27lWi9I9_sNBfZ-6vZjHTaK1vuXfIh1IA-Ip-l8irPmteIEnRciBP_HlfSpKk1O8rePVx3o9ZW3lXKLKq3eBcKh9-geCsjsGlb7kA/s1600/16-02-2014+16-30-34.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjskgkmq1Ur6-adBTRJZ_MtOPQ2atkCH_xXUJhI5y27lWi9I9_sNBfZ-6vZjHTaK1vuXfIh1IA-Ip-l8irPmteIEnRciBP_HlfSpKk1O8rePVx3o9ZW3lXKLKq3eBcKh9-geCsjsGlb7kA/s1600/16-02-2014+16-30-34.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgLjLZh5NilmyCk_2aXzOMVf1yz94XDVUcshJQVyjf52qLLbxkJJhd0RhViqdTpUPsWK0sQSdFRac8RQ19CXSRFgqkA-QowLqizF4o_YEgA45OHOdoL8wp-Pz5UGsHasqm1V14E4bLUsEE/s1600/16-02-2014+16-37-13.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgLjLZh5NilmyCk_2aXzOMVf1yz94XDVUcshJQVyjf52qLLbxkJJhd0RhViqdTpUPsWK0sQSdFRac8RQ19CXSRFgqkA-QowLqizF4o_YEgA45OHOdoL8wp-Pz5UGsHasqm1V14E4bLUsEE/s1600/16-02-2014+16-37-13.png)

  
/luck/:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiJUlQ8Tw3EzKbrWwaykIp8Qxu4GCFlOherBqZ68wIteXMJAPb08hMqVI7eJrXH2kDYPpgRCsazR4PjSxJ1Q2C2C_VXxz98fVPXuKYuidisPK3T55YCYGEREOxQuvWvnn25_ixBtG7Zi3M/s1600/16-02-2014+16-39-29.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiJUlQ8Tw3EzKbrWwaykIp8Qxu4GCFlOherBqZ68wIteXMJAPb08hMqVI7eJrXH2kDYPpgRCsazR4PjSxJ1Q2C2C_VXxz98fVPXuKYuidisPK3T55YCYGEREOxQuvWvnn25_ixBtG7Zi3M/s1600/16-02-2014+16-39-29.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiFHtA_CUYTTHOCuxf5JZ7rvhcJGWP8-igdbQ90C9eoQIgFs52E55R5-OCSh_m5AhkWIC66_Plcse0NRaQigHH5XtlbpwkjIpgmz8a35vCFJQIW0VJWERffGsi21ehhQgY2XXsslmW1DeQ/s1600/16-02-2014+16-40-26.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiFHtA_CUYTTHOCuxf5JZ7rvhcJGWP8-igdbQ90C9eoQIgFs52E55R5-OCSh_m5AhkWIC66_Plcse0NRaQigHH5XtlbpwkjIpgmz8a35vCFJQIW0VJWERffGsi21ehhQgY2XXsslmW1DeQ/s1600/16-02-2014+16-40-26.png)

  
/ass/:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjT70YPY2XQff06TpuNXPOsrexXf58LWYVzlLkrZ1-TDlg2JlOi3zIIV0FhCDCwoQvqzKg7JgkJvo5W2LttlUb7zsYiGgSVmqPuAKZrYrCa5iX3K4ELsR0iSYaxCYQ-ofO6G7xy6C8VfqU/s1600/16-02-2014+16-42-43.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjT70YPY2XQff06TpuNXPOsrexXf58LWYVzlLkrZ1-TDlg2JlOi3zIIV0FhCDCwoQvqzKg7JgkJvo5W2LttlUb7zsYiGgSVmqPuAKZrYrCa5iX3K4ELsR0iSYaxCYQ-ofO6G7xy6C8VfqU/s1600/16-02-2014+16-42-43.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhjmggwqnzu7loOvquEd9yeIr_FNfxxR7Po3R4uiKhYENv7d46NDBbk1Dp6f7dVoCT1Y90XCLaN8e6NrxeRVLNvYeMdo37MP7QX8cVQ-x9zncpyFdZpxFBFEH6kbzbzSNAo0yLsj_FX1rY/s1600/16-02-2014+16-43-23.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhjmggwqnzu7loOvquEd9yeIr_FNfxxR7Po3R4uiKhYENv7d46NDBbk1Dp6f7dVoCT1Y90XCLaN8e6NrxeRVLNvYeMdo37MP7QX8cVQ-x9zncpyFdZpxFBFEH6kbzbzSNAo0yLsj_FX1rY/s1600/16-02-2014+16-43-23.png)

  
/kbot/:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEixyV5VGIy2KVLkmkVdJ0HS_akWS6HjK_xrgLWPtk_F3mBdgfpbtGZxEwKJIW9EIXcLfwllQ2gCPySoPTE5iT-lWc3BypXMXEcxusEyAiePyHxE1uuT3_Lkf0VCkBHsufLO3jD3O6Lxp5I/s1600/16-02-2014+16-46-00.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEixyV5VGIy2KVLkmkVdJ0HS_akWS6HjK_xrgLWPtk_F3mBdgfpbtGZxEwKJIW9EIXcLfwllQ2gCPySoPTE5iT-lWc3BypXMXEcxusEyAiePyHxE1uuT3_Lkf0VCkBHsufLO3jD3O6Lxp5I/s1600/16-02-2014+16-46-00.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgVF3b9m8VJ3EnERaqpkUlkawqFV1-LSqZSIkYws0rGMAyMFHPKwONDFNXjBeat-gT988SvncWGiwhbR7zGX3oN6rPq88Km9dV9GTDT75sUVnf-YZQp5X-764KVoZY0qDzFQCCa-xKJRL8/s1600/16-02-2014+16-47-31.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgVF3b9m8VJ3EnERaqpkUlkawqFV1-LSqZSIkYws0rGMAyMFHPKwONDFNXjBeat-gT988SvncWGiwhbR7zGX3oN6rPq88Km9dV9GTDT75sUVnf-YZQp5X-764KVoZY0qDzFQCCa-xKJRL8/s1600/16-02-2014+16-47-31.png)

  
/ksks/:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh_MQoFrD0jrPj8iYcL_z3X5iROL3fbRZRz3GkM9Un2xU6Eg94Br4g_IeK_rFmpxPod56FgoFjJJvelP59ZHNbHvxcJ1Ayzroec4nNl_ABY1zCIDSjlGSk_saQkmuJZfoBG8bNCaESxGSg/s1600/16-02-2014+16-49-12.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh_MQoFrD0jrPj8iYcL_z3X5iROL3fbRZRz3GkM9Un2xU6Eg94Br4g_IeK_rFmpxPod56FgoFjJJvelP59ZHNbHvxcJ1Ayzroec4nNl_ABY1zCIDSjlGSk_saQkmuJZfoBG8bNCaESxGSg/s1600/16-02-2014+16-49-12.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjzcBgrUSCYiLbKZFITWyk5yr6Fwz_5RocXbT4yEE37zGxjgWDw5iF9WwA9UJjwpo70WTcpTb7HzFYVb6GBfx8N22wTSUdxtXHZFhf3jbQZJPQkLg_KmRpyE0P90mgf_7cp0eLH9JRAt1k/s1600/16-02-2014+16-49-45.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjzcBgrUSCYiLbKZFITWyk5yr6Fwz_5RocXbT4yEE37zGxjgWDw5iF9WwA9UJjwpo70WTcpTb7HzFYVb6GBfx8N22wTSUdxtXHZFhf3jbQZJPQkLg_KmRpyE0P90mgf_7cp0eLH9JRAt1k/s1600/16-02-2014+16-49-45.png)

  
/one/:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgBdP5W4HRkoDBmm170ItxPJPoibic3JcPFl7IW6jyyDps8fcGFaaAFtXExgc4OOm4YBdh8Sy-HSwPBuFvtmDlMEJObO59kIxMalBH2A4UYWFIxk_fZcRiZE1ouUgtax7g_axc9gfsZ9qI/s1600/16-02-2014+16-52-53.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgBdP5W4HRkoDBmm170ItxPJPoibic3JcPFl7IW6jyyDps8fcGFaaAFtXExgc4OOm4YBdh8Sy-HSwPBuFvtmDlMEJObO59kIxMalBH2A4UYWFIxk_fZcRiZE1ouUgtax7g_axc9gfsZ9qI/s1600/16-02-2014+16-52-53.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhx_zem6KPOPtGeofeO5U52xR2rq-S9jXNPD4OFGHCRLzI_MBdGUElG4A4U787mh5KsxwzJc8bRsjfZkvGaEXn_iebs-OODMH30sZEIMJ47vbz5y3VnChtQNjx7NrXKIDqe_vj_1g0EBqg/s1600/16-02-2014+16-53-28.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhx_zem6KPOPtGeofeO5U52xR2rq-S9jXNPD4OFGHCRLzI_MBdGUElG4A4U787mh5KsxwzJc8bRsjfZkvGaEXn_iebs-OODMH30sZEIMJ47vbz5y3VnChtQNjx7NrXKIDqe_vj_1g0EBqg/s1600/16-02-2014+16-53-28.png)

  
/two/ (unused):  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhI1Gz-Y5znfhyZ8OpIhn5at7fAACLXcSlXWW1JV57Kc132jfhyphenhyphenAK8Z2P2CzIqjcERMR4CUBETqUFXtRfJqmhobRGXEjdH3vgKhgVWciUOZCUojPJzs5U9J70tAAzJAx0DBnrBjE90TbqQ/s1600/16-02-2014+16-56-58.png)/](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhI1Gz-Y5znfhyZ8OpIhn5at7fAACLXcSlXWW1JV57Kc132jfhyphenhyphenAK8Z2P2CzIqjcERMR4CUBETqUFXtRfJqmhobRGXEjdH3vgKhgVWciUOZCUojPJzs5U9J70tAAzJAx0DBnrBjE90TbqQ/s1600/16-02-2014+16-56-58.png)

  
/three/ (unused):  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgK1AUMxogYV5Hd2AgBQT7XqRZ423cNlAaYrDbD8D89A4o00iPVrjdVmYDSi0E8zdyLZsnpRRcuZuiceD-TEvs2mj8Hf6IBc_N0jA4KA_b-EqtB7jOgn-laSjN5JN1EcEUFel5z_H9YyMc/s1600/16-02-2014+16-59-05.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgK1AUMxogYV5Hd2AgBQT7XqRZ423cNlAaYrDbD8D89A4o00iPVrjdVmYDSi0E8zdyLZsnpRRcuZuiceD-TEvs2mj8Hf6IBc_N0jA4KA_b-EqtB7jOgn-laSjN5JN1EcEUFel5z_H9YyMc/s1600/16-02-2014+16-59-05.png)

  
/four/ (unused):  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhpDNKq2aHJidyE1XAhhFeHO0xZxxxqBTYBzYIedbFDAbHC7XM_vf43WJvtpTL5ijihtn8AI2rXVUxWSmgYcekySC84bq0wr7LND8D45CRzWLE86ka1dV6GF3KmtrcXYjEgN8OT-J_ZKQ8/s1600/16-02-2014+17-00-42.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhpDNKq2aHJidyE1XAhhFeHO0xZxxxqBTYBzYIedbFDAbHC7XM_vf43WJvtpTL5ijihtn8AI2rXVUxWSmgYcekySC84bq0wr7LND8D45CRzWLE86ka1dV6GF3KmtrcXYjEgN8OT-J_ZKQ8/s1600/16-02-2014+17-00-42.png)

  
Now, for decoding those ZeusVM images, as described by Jerome, you just need to strip the image and do the following: Base64+RC4+VisualDecrypt+UCL Decompress  
  
Here are some 'malicious' image from 212.44.64.202:  
mix.jpg:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhjjZm2m4qpJdB8GqR3_toRaQpKTLmBoHrQunuFMS8eRBF1QrPXNnbl4uS8cJCQTfPEsv7FKphqg_-mUiHIicl9IfPE-yXOuQpjp_-vVXRfTXrge8Dr-sRQvCkLchTd60EsWFo1PnivZmQ/s1600/22-02-2014+19-00-47.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhjjZm2m4qpJdB8GqR3_toRaQpKTLmBoHrQunuFMS8eRBF1QrPXNnbl4uS8cJCQTfPEsv7FKphqg_-mUiHIicl9IfPE-yXOuQpjp_-vVXRfTXrge8Dr-sRQvCkLchTd60EsWFo1PnivZmQ/s1600/22-02-2014+19-00-47.png)

mix.jpg:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgpMSLBUT-EBog7YTQt3jG7PKzOVjqOGxZjebLzbagNdXR_j7shuwiOwNUnm3LyK6niybMZQWWGvujjnOQyjPLeVvAB_Frw9XwYcMJEVsfS_zX1dBO0jNsZHk4kCAXPcvOlXNAVUHn3Ogo/s1600/22-02-2014+18-51-59.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgpMSLBUT-EBog7YTQt3jG7PKzOVjqOGxZjebLzbagNdXR_j7shuwiOwNUnm3LyK6niybMZQWWGvujjnOQyjPLeVvAB_Frw9XwYcMJEVsfS_zX1dBO0jNsZHk4kCAXPcvOlXNAVUHn3Ogo/s1600/22-02-2014+18-51-59.png)

mix.jpg:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgUHYDJrmYp259orC6XEAGb5uqeCPekqvKqSjp3wTbJ9ccLEIbuvdL8URW1-i2aRe98BbCz8kS-mlDmjNtkz8MPS-7kmosMITGY7Y4sywpMNNKIILpk4O3GsFvRVtvPTnQSEVF1K7mi10I/s1600/22-02-2014+19-02-40.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgUHYDJrmYp259orC6XEAGb5uqeCPekqvKqSjp3wTbJ9ccLEIbuvdL8URW1-i2aRe98BbCz8kS-mlDmjNtkz8MPS-7kmosMITGY7Y4sywpMNNKIILpk4O3GsFvRVtvPTnQSEVF1K7mi10I/s1600/22-02-2014+19-02-40.jpg)

mix.jpg:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEii0AVykGJlOu7k1efWs7_fDYjLUSpnPzh_ahbBvlhQezqwkI9ibq5ovqSzmOxtA3nWRaYuxXtv3eMRt7a6vFf5D1z6fKQMlkST9abEMMqvzPowLpayLA6z9YqpPFHM5mOyrvqF-E-Paz4/s1600/22-02-2014+19-03-19.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEii0AVykGJlOu7k1efWs7_fDYjLUSpnPzh_ahbBvlhQezqwkI9ibq5ovqSzmOxtA3nWRaYuxXtv3eMRt7a6vFf5D1z6fKQMlkST9abEMMqvzPowLpayLA6z9YqpPFHM5mOyrvqF-E-Paz4/s1600/22-02-2014+19-03-19.png)

config.jpg:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEivsyhW3jiGeKTBBmJKH2zvEqBMtufWpYfxUXLO7Z78A18fVwjmTilkaR2pizpasGmFkttinDss531SYcyAieFuM8YG83quXUsVRXvhIjvqKEQTYkS_7q6NCPjlOYGQRF5T1jcz1jJ-Z5g/s1600/22-02-2014+18-54-43.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEivsyhW3jiGeKTBBmJKH2zvEqBMtufWpYfxUXLO7Z78A18fVwjmTilkaR2pizpasGmFkttinDss531SYcyAieFuM8YG83quXUsVRXvhIjvqKEQTYkS_7q6NCPjlOYGQRF5T1jcz1jJ-Z5g/s1600/22-02-2014+18-54-43.png)

kartamestnosti.jpg:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjReZvM8XlkiaXeOjOSLf-ZLFufdU3Gn241piEcN0i4eVs5PS_3161nfp8GimqT-sdwtGWN-zwjJo4lCIRR1nYD9cBpqvANhkpIkBgAJ7yXa8CukBLUaGqiqrc0xHlTuSfb2AL-TsWuHEM/s1600/22-02-2014+18-56-27.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjReZvM8XlkiaXeOjOSLf-ZLFufdU3Gn241piEcN0i4eVs5PS_3161nfp8GimqT-sdwtGWN-zwjJo4lCIRR1nYD9cBpqvANhkpIkBgAJ7yXa8CukBLUaGqiqrc0xHlTuSfb2AL-TsWuHEM/s1600/22-02-2014+18-56-27.jpg)

webi\_test.jpg:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh8C5-fInWeHkDHEC0FyPYduIlBDJI_oGpAGJi_hxY8Wei7X_SOMiqQrxDYYZl7En8DVwcpb_hxhWjB8eReoBbdeEfiMDlUfN4k0TSRQk6ENzW-SAYp_NT8evgUqeFz4PlS75K6iTK9WFQ/s1600/22-02-2014+18-57-09.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh8C5-fInWeHkDHEC0FyPYduIlBDJI_oGpAGJi_hxY8Wei7X_SOMiqQrxDYYZl7En8DVwcpb_hxhWjB8eReoBbdeEfiMDlUfN4k0TSRQk6ENzW-SAYp_NT8evgUqeFz4PlS75K6iTK9WFQ/s1600/22-02-2014+18-57-09.png)

uwliottrekera.jpg:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEib2Oljv4GXu30-ZG0_4gSr0TcDKA_ZKnQqkqSiydXAD7iky1xo9kgsCgGpLB_p8XCXb4wl5rf0K5omugrMFvHr9ZPA-neOGTLVl8abxhvHFKSPuLI6dJ25vOlSsOgBQTkubgANmQF1Mrg/s1600/22-02-2014+18-49-45.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEib2Oljv4GXu30-ZG0_4gSr0TcDKA_ZKnQqkqSiydXAD7iky1xo9kgsCgGpLB_p8XCXb4wl5rf0K5omugrMFvHr9ZPA-neOGTLVl8abxhvHFKSPuLI6dJ25vOlSsOgBQTkubgANmQF1Mrg/s1600/22-02-2014+18-49-45.png)

 test\_vnc2.jpg:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgYvLScj_laQZ1iRh9KTGdHClfSztnkX2P9auVr3w61gJs42sP7MMwf_X1A1RylJ1vdTtvk6CaoYY0GkXjc1WzXNk1OCufYJ6VgUfDRD8SVqhA1oRVOL3D52DlOIEXuXlNYpnr1D5aRvs4/s1600/22-02-2014+18-50-43.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgYvLScj_laQZ1iRh9KTGdHClfSztnkX2P9auVr3w61gJs42sP7MMwf_X1A1RylJ1vdTtvk6CaoYY0GkXjc1WzXNk1OCufYJ6VgUfDRD8SVqhA1oRVOL3D52DlOIEXuXlNYpnr1D5aRvs4/s1600/22-02-2014+18-50-43.jpg)

x64hook.jpg:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEikJg9KkqfRICJVXPcPs1vnKRhMDQWNuwfx9PKpAL8_KGL1JNQAc-A0xzrDIu8DDDlXed9JmgVDyyeI1zlf2yiv6ef6VPt9lhPJGV-yVj_xpqt6juQcNCQDyZ04cqqs080rdZG9UkU_RGU/s1600/22-02-2014+18-58-21.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEikJg9KkqfRICJVXPcPs1vnKRhMDQWNuwfx9PKpAL8_KGL1JNQAc-A0xzrDIu8DDDlXed9JmgVDyyeI1zlf2yiv6ef6VPt9lhPJGV-yVj_xpqt6juQcNCQDyZ04cqqs080rdZG9UkU_RGU/s1600/22-02-2014+18-58-21.jpg)

  
Some configs was done for tests:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiZLlp11rexkVvUMBpLA-kL6XujmT87LYjEMreXb9DE80g4tmqFli1LxBl_n7ipRU5V_LYab9DC4nyqke1xLbyE4PViZA70QwH9k4iMPD-owX1CFxjZD8qWJ75GebbQS2sv5ZHtwiiu8tY/s1600/22-02-2014+19-13-30.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiZLlp11rexkVvUMBpLA-kL6XujmT87LYjEMreXb9DE80g4tmqFli1LxBl_n7ipRU5V_LYab9DC4nyqke1xLbyE4PViZA70QwH9k4iMPD-owX1CFxjZD8qWJ75GebbQS2sv5ZHtwiiu8tY/s1600/22-02-2014+19-13-30.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiVrzTcuIsFs7J8koqlh5kBtthtR-vZM_NHaYsaBb-a8hQALs6JBd1FC44Ny4k03w9R8oiwP_gRNUaLY0NDxU8mt3H5DGS1CEvEFSGI_H9VmXi-93Y3ZKZ0FHqXpX3gRz3cgsAF-z9LVPA/s1600/22-02-2014+19-14-10.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiVrzTcuIsFs7J8koqlh5kBtthtR-vZM_NHaYsaBb-a8hQALs6JBd1FC44Ny4k03w9R8oiwP_gRNUaLY0NDxU8mt3H5DGS1CEvEFSGI_H9VmXi-93Y3ZKZ0FHqXpX3gRz3cgsAF-z9LVPA/s1600/22-02-2014+19-14-10.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj1YzqX7WikwNAciSZ9LnvOfFxpQbNI36cJDZhYolAM0gZEcz5Df8C34cNAJwhkskbhuvEcoybdIM1u5K93dMCYmJrd00jLrL_NprvSKoKRckI1JoZ7gAG-ab79tm1zQN7SGzsXUMNZjhY/s1600/22-02-2014+19-14-43.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj1YzqX7WikwNAciSZ9LnvOfFxpQbNI36cJDZhYolAM0gZEcz5Df8C34cNAJwhkskbhuvEcoybdIM1u5K93dMCYmJrd00jLrL_NprvSKoKRckI1JoZ7gAG-ab79tm1zQN7SGzsXUMNZjhY/s1600/22-02-2014+19-14-43.png)

  
And some wasn't for test, targeting banks with MiTB.  
Malicious code injection, on a ZeusVM botnet targeting France:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhAbJvUGr2nq0EQm3OeQUOE-9NEmv65G2M8FeVUwAGpy1wf-bof4OyVr57FSr1GwJjGz-qyYyPZLUy8b9MuJ1JJpSDsoswz5qOZqcoC5G-UFdlHKssWwPnDgV7ekRJm2VTwEpfdOn5Pc14/s1600/22-02-2014+19-48-42.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhAbJvUGr2nq0EQm3OeQUOE-9NEmv65G2M8FeVUwAGpy1wf-bof4OyVr57FSr1GwJjGz-qyYyPZLUy8b9MuJ1JJpSDsoswz5qOZqcoC5G-UFdlHKssWwPnDgV7ekRJm2VTwEpfdOn5Pc14/s1600/22-02-2014+19-48-42.png)

  
Lame webinject:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh3mkFDqTVuLhH_E1dp_e-NgHV5kEVxXJBKZqXJl_EfHlKgzgTows7NnQ2LnspvIUATsfaGdEhjyveFaLCmnFvYev5A-jV78TW0B6N0DZErTVY9YoAfymyiHbAMvfgYTYSUrM1gUzQHU3c/s1600/22-02-2014+20-23-03.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh3mkFDqTVuLhH_E1dp_e-NgHV5kEVxXJBKZqXJl_EfHlKgzgTows7NnQ2LnspvIUATsfaGdEhjyveFaLCmnFvYev5A-jV78TW0B6N0DZErTVY9YoAfymyiHbAMvfgYTYSUrM1gUzQHU3c/s1600/22-02-2014+20-23-03.png)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhEsdArIvxQ2RC8Pst6qHJyDJAAw5-CuTgkkmRkmC0txyBX4NPHZiFqMYHKGnNK1gxENGBS8BCc9qdEKm_GqA2I2gkLDM3c4zVL3onyx40fEJWgSy5DDqYpjxqadWWS8zB8OSkwENjq69Q/s1600/22-02-2014+21-05-56.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhEsdArIvxQ2RC8Pst6qHJyDJAAw5-CuTgkkmRkmC0txyBX4NPHZiFqMYHKGnNK1gxENGBS8BCc9qdEKm_GqA2I2gkLDM3c4zVL3onyx40fEJWgSy5DDqYpjxqadWWS8zB8OSkwENjq69Q/s1600/22-02-2014+21-05-56.png)

  
CCGRAB:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg1OkLl09qUgF5PLNjaVJymctBudjRF388Q-bIT6jvVzaj9PLr4-TYVSo8lM4JNjH4RsLGxAON4Cx9Wi_JMgOOHWmcP5ons47axSc97Rbxm1HFOF8-3YbSWd9brmwA9YeFLzVAq2gaZPXI/s1600/22-02-2014+18-37-52.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg1OkLl09qUgF5PLNjaVJymctBudjRF388Q-bIT6jvVzaj9PLr4-TYVSo8lM4JNjH4RsLGxAON4Cx9Wi_JMgOOHWmcP5ons47axSc97Rbxm1HFOF8-3YbSWd9brmwA9YeFLzVAq2gaZPXI/s1600/22-02-2014+18-37-52.png)

ATSEngine:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgi6Hv5opCXmteDRIkuO0z4wAP3zDUywDqAiaYIgTg_GQfi6YOl9_pOXqDbsu83sriM-ANNWpgd1FNdJQVsXZ6BjcVdNzcmAm_uRhrrRsttNkHAlhSL4CeVv7GMQFjK_RdcJ1kNcn3Vm5o/s1600/20-02-2014+14-21-35.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgi6Hv5opCXmteDRIkuO0z4wAP3zDUywDqAiaYIgTg_GQfi6YOl9_pOXqDbsu83sriM-ANNWpgd1FNdJQVsXZ6BjcVdNzcmAm_uRhrrRsttNkHAlhSL4CeVv7GMQFjK_RdcJ1kNcn3Vm5o/s1600/20-02-2014+14-21-35.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgLkpWEmHBCYkWHdb9vV_4XqdmF0XKprq9pNQWcV3kLABR0o1QmPI8oCnq_p-D0AKvYEH8lCmak7E3lKMdH_gP90EzjD5DHNN1LRJ7iHYYO2lUIuFGj6HmM3sq1PfjDQ5IRj2dcFikmFow/s1600/20-02-2014+14-22-05.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgLkpWEmHBCYkWHdb9vV_4XqdmF0XKprq9pNQWcV3kLABR0o1QmPI8oCnq_p-D0AKvYEH8lCmak7E3lKMdH_gP90EzjD5DHNN1LRJ7iHYYO2lUIuFGj6HmM3sq1PfjDQ5IRj2dcFikmFow/s1600/20-02-2014+14-22-05.png)

  
Nowadays more actors start to use ZeusVM, like the group who was using the 'private' version of [Citadel 3.1.0.0](http://securityblog.s21sec.com/2014/02/citadel-involution.html) and the group who was targeting [Japan](http://www.kernelmode.info/forum/viewtopic.php?f=16&t=1465&start=70#p20700).  
Both switched on ZeusVM as alternative of Citadel.  
  
You can find the samples related to 212.44.64.202 with config and decoded here:  
[http://temari.fr/vx/ZeusVMs\_212.44.64.202.7z](http://temari.fr/vx/ZeusVMs_212.44.64.202.7z)  
  
Some other ZeusVM samples (not related to 212.44.64.202):  
[http://temari.fr/vx/ZeusVMs\_v1.0.0.2\_v1.0.0.5.7z](http://temari.fr/vx/ZeusVMs_v1.0.0.2_v1.0.0.5.7z)  
  
  
  
  
  
root/root  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgMOSdBaaFZXsbeDTaHdkvbAMesQ9uor30prUwiiPM35XW7to99apJt3aEaOzOQTE75dA2BiIF6gX1IADv1d1IhuFXyDjDCeSSw_kA47tfv_AzqX1_-Rj4MT_EFuz-siIzaWI2vP63gxwQ/s1600/mfw_kins.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgMOSdBaaFZXsbeDTaHdkvbAMesQ9uor30prUwiiPM35XW7to99apJt3aEaOzOQTE75dA2BiIF6gX1IADv1d1IhuFXyDjDCeSSw_kA47tfv_AzqX1_-Rj4MT_EFuz-siIzaWI2vP63gxwQ/s1600/mfw_kins.png)

#### [Source](https://www.xylibox.com/2014/04/zeusvm-and-steganography.html)

<br/>
---
