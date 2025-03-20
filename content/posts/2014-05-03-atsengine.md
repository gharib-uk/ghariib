---
title: "ATSEngine"
date: Sat, 03 May 2014 23:12:00 +0000
draft: false
type: posts
categories: 
- [object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object]
---
# ATSEngine

<br/>

<br/>
ATSEngine injects can be found oftenly inside Zeus configs, it makes the webinjects more dynamic because most of the content is located remotely and can be updated much easily instead of sending new config to all the bots.  
It's the main difference with this, and a standard web inject inside Zeus.  
One just allows you to do a static change in the page while the other gives you much more options, for example, customized webinjects, pop-ups, online requests for token etc...  
ATSEngine have also a jabber alert feature, it let the fraudster know when the victim is logged to his bank account so it would be a god time to backconnect him (with the VNC feature of Zeus) and do the transaction.  
Most of ATSEngine panels are also hosted on SSL because banks use SSL.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiPhLmFFYYiVauEai3wB2OJKKdX3U24LaqWezrJvqxVt5XUsvsCTJ4JZnlXAwLNlF8u8-HYJBiWH2lEYmnBP7uf0wtFRGLcna-izJmsr9AoR6IwdsJwJSy8b1W7Yv4UMuJ_uJapljqTZ1M/s1600/2014-05-04_12-19-42.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiPhLmFFYYiVauEai3wB2OJKKdX3U24LaqWezrJvqxVt5XUsvsCTJ4JZnlXAwLNlF8u8-HYJBiWH2lEYmnBP7uf0wtFRGLcna-izJmsr9AoR6IwdsJwJSy8b1W7Yv4UMuJ_uJapljqTZ1M/s1600/2014-05-04_12-19-42.png)

ATSEngine on a ZeusVM config.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh0OSwWoloh7S6BPTKULZq51ZxklEkBpr9rviGatBabWcVvdbovbTTofdttIP6gXYoZ0Eu0HLA-dCnnk3AGxP4lhhIxQU2W-YH3p08EOU76WL-XXIEs4lgCsnHABEYSNsAsMCDkyFH_W5A/s1600/2014-05-04_12-25-03.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh0OSwWoloh7S6BPTKULZq51ZxklEkBpr9rviGatBabWcVvdbovbTTofdttIP6gXYoZ0Eu0HLA-dCnnk3AGxP4lhhIxQU2W-YH3p08EOU76WL-XXIEs4lgCsnHABEYSNsAsMCDkyFH_W5A/s1600/2014-05-04_12-25-03.png)

ATSEngine on a Citadel config.  
Example of [figrabber.js](http://pastebin.com/MBw8HcCB) from an ATSEngine panel.  
  
Some guys do also a business with this type of web injects, for example:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgF0ILpLY9wIP5U7B5I5XjOu5CExSw8JbZM72h9vty_TqkCN4Saz6Wat2ZzotvN67jR4pSjYOprJbDVk4tbgDexmbIyzwUdyjP8rMhBpfndBFRs6LaNLUqJlJsOrazkgJQkTy-yn2kAiPc/s400/02-01-2014+21-24-42.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgF0ILpLY9wIP5U7B5I5XjOu5CExSw8JbZM72h9vty_TqkCN4Saz6Wat2ZzotvN67jR4pSjYOprJbDVk4tbgDexmbIyzwUdyjP8rMhBpfndBFRs6LaNLUqJlJsOrazkgJQkTy-yn2kAiPc/s1600/02-01-2014+21-24-42.png)

He's offering a service for writing injects.  
The title says "Auto-uploads and Injects from professionals for professionals"  
The rest of the text explains how the service works, it's more a terms and conditions post rather than a technical description of the product, about moneyback, privacy, guarantees and other stuff.  
They dont write mobile botnets, trojan horses, traffic direction systems or other malware software except injects, also they dont guarantee bypass of protection (like Rapport).  
yummba is know anyway for writing injects for ATSEngine.  
  
Let's have a look on a C&C now..  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhMUAdNIgrEQNpAYxpa3gjcJkqgYVtzFnSuty85BL8EyUQDqskRyfHIFgedC5cwQ-VSqqF2MNTfK0iZvwnGroUd_KuSF6Zl7K2PjsUYazcM1ClEWnVoC2EEpUeKxgxQpb8pqjT6VkUMpaI/s1600/R9e1D23.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhMUAdNIgrEQNpAYxpa3gjcJkqgYVtzFnSuty85BL8EyUQDqskRyfHIFgedC5cwQ-VSqqF2MNTfK0iZvwnGroUd_KuSF6Zl7K2PjsUYazcM1ClEWnVoC2EEpUeKxgxQpb8pqjT6VkUMpaI/s1600/R9e1D23.png)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhq6Sqk0w7-6dJbl7AG6U55skaiu92qoaKoDFT8Pfw6aAcfz0XpoF_HzY5398ddmC-6H-U4YwcfXfeh17vCNKYWw1u9zNX6zr_0zXW0bzwVKZj-CCydlXJW_0F_K2IndEzqjtoRZSaNvfQ/s1600/BqHPLni.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhq6Sqk0w7-6dJbl7AG6U55skaiu92qoaKoDFT8Pfw6aAcfz0XpoF_HzY5398ddmC-6H-U4YwcfXfeh17vCNKYWw1u9zNX6zr_0zXW0bzwVKZj-CCydlXJW_0F_K2IndEzqjtoRZSaNvfQ/s1600/BqHPLni.png)

  
Accounts:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgZVpC6UYtorJqLLG1icTy-OByWabQZo10EIcvmbK63rqtZsCmJKwWRdssGeqnz1DNgUzKozkAvwN6sDsGBBKrB7h3Wju98jcz759hM56khxe1PcI27V7kU23ZlozTq0M898VHrSVAMlng/s400/02-01-2014+21-03-17.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgZVpC6UYtorJqLLG1icTy-OByWabQZo10EIcvmbK63rqtZsCmJKwWRdssGeqnz1DNgUzKozkAvwN6sDsGBBKrB7h3Wju98jcz759hM56khxe1PcI27V7kU23ZlozTq0M898VHrSVAMlng/s1600/02-01-2014+21-03-17.png)

  
Reports:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhjpestNbojwrxba_9ZnuecB3yQ40rBTEvaSX0wz1UJU27FXO7WeFT7DR9_kcoJ2eSLBKp67tEakO2pyYxIlV591K6-TGHWE9kbBYaTQZFd9PvecBpxdlvXDDehdrXXKyaab3MrRiCPkdc/s400/02-01-2014+21-13-52.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhjpestNbojwrxba_9ZnuecB3yQ40rBTEvaSX0wz1UJU27FXO7WeFT7DR9_kcoJ2eSLBKp67tEakO2pyYxIlV591K6-TGHWE9kbBYaTQZFd9PvecBpxdlvXDDehdrXXKyaab3MrRiCPkdc/s1600/02-01-2014+21-13-52.png)

  
Options main:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgic_Fv_6zr7gaI10mRq4ymqi8kLNMyE51WH8S7jPL3mt6H1dkllzAvykJBCE6jDboNdMaKjhxCiAucl4vpb5CGhxE6C3KNrpkrEuXKSoV5HDxhdhL2wemZn0wWkkozzASG3LFu1A46le8/s400/02-01-2014+21-21-25.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgic_Fv_6zr7gaI10mRq4ymqi8kLNMyE51WH8S7jPL3mt6H1dkllzAvykJBCE6jDboNdMaKjhxCiAucl4vpb5CGhxE6C3KNrpkrEuXKSoV5HDxhdhL2wemZn0wWkkozzASG3LFu1A46le8/s1600/02-01-2014+21-21-25.png)

  
Options Jabber:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjQjyTE8DX0Jdnqf5VSU3ah1NUj9IKBgTtycNlbkmttRwaWBDq8UhH4QepfE4OJjDo6lkHeXOcd4z2agrT9ITTvoqhA1ZTcTqRqP9hiw4TCCdTOzLPwl33klaquCOwkgYrD0_5TL5JaYX4/s400/02-01-2014+21-22-25.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjQjyTE8DX0Jdnqf5VSU3ah1NUj9IKBgTtycNlbkmttRwaWBDq8UhH4QepfE4OJjDo6lkHeXOcd4z2agrT9ITTvoqhA1ZTcTqRqP9hiw4TCCdTOzLPwl33klaquCOwkgYrD0_5TL5JaYX4/s1600/02-01-2014+21-22-25.png)

  
Another panel, on SSL:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjNF8-ZsfhEOpn6irpZ3hUDTeNfN7T_zZrUBqKoSpxfvu8ywr3uH0vx4H81_WZwoP1UJJy1nBGn4gStUTkdtwKWAvQWyBF2RU5EzZEvR54Nhmga1PNEr2oMMOn0njZvWt70GPciDtrLeIk/s1600/09-01-2014+00-33-21.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjNF8-ZsfhEOpn6irpZ3hUDTeNfN7T_zZrUBqKoSpxfvu8ywr3uH0vx4H81_WZwoP1UJJy1nBGn4gStUTkdtwKWAvQWyBF2RU5EzZEvR54Nhmga1PNEr2oMMOn0njZvWt70GPciDtrLeIk/s1600/09-01-2014+00-33-21.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjkkhUoLv2SZI9AH3e7aSe0NaV8QThFhbFRIyO_XfG-Rv_kNGRWj6HbpmxZSML12utTdKZaEC0wON3QZNOCw4kUbZo0OoTRbYFKFFKB1uqyUYFxNb8XlwfRyq0JZYmOoNxeTj3WV4zUOKU/s1600/09-01-2014+00-32-44.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjkkhUoLv2SZI9AH3e7aSe0NaV8QThFhbFRIyO_XfG-Rv_kNGRWj6HbpmxZSML12utTdKZaEC0wON3QZNOCw4kUbZo0OoTRbYFKFFKB1uqyUYFxNb8XlwfRyq0JZYmOoNxeTj3WV4zUOKU/s1600/09-01-2014+00-32-44.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh1ocJ8q363R_IqZuzA_49hYRZ9ufZe6ZFhSWQNUMx8llrxMr_0xfeteva5JrPFk3ap0MD2Xi1ppbijFDQ5rA-MiSgyRjjtR714WVSrZpvr_sgdTc7cpdldJeHV9KXhyphenhyphengq30-G5IQb3HyA/s1600/09-01-2014+00-33-49.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh1ocJ8q363R_IqZuzA_49hYRZ9ufZe6ZFhSWQNUMx8llrxMr_0xfeteva5JrPFk3ap0MD2Xi1ppbijFDQ5rA-MiSgyRjjtR714WVSrZpvr_sgdTc7cpdldJeHV9KXhyphenhyphengq30-G5IQb3HyA/s1600/09-01-2014+00-33-49.png)

  
Another panel, on SSL:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjKdo-q2MdiT1QmJW7OKWvokCo9oUH5XCdDZhruzUDxJ2ugT9he18KIsbQNZpZ08j9vXNm1vU3CYUz1HOcQMNmY_h-fgVfks1l9_10aCS_2dZu7hvRhJqVrMTN-HpBA_d8F-POHxwfS-2s/s1600/22-01-2014+14-18-52.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjKdo-q2MdiT1QmJW7OKWvokCo9oUH5XCdDZhruzUDxJ2ugT9he18KIsbQNZpZ08j9vXNm1vU3CYUz1HOcQMNmY_h-fgVfks1l9_10aCS_2dZu7hvRhJqVrMTN-HpBA_d8F-POHxwfS-2s/s1600/22-01-2014+14-18-52.png)

  
Another panel, still on SSL:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgwre1W9j3zRjrEa1h1FVAY016IWMcb8GO98T3du3H8JtZCM-E-srmol1u-PcQ_Zza0_GspBtI538tlXL5kj_5gS40zCjp2_SLHdmtJCSgJpgWjudEDTCJZFyOj4MYC_4BhtyqhB0qsaIQ/s1600/20-02-2014+15-02-46.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgwre1W9j3zRjrEa1h1FVAY016IWMcb8GO98T3du3H8JtZCM-E-srmol1u-PcQ_Zza0_GspBtI538tlXL5kj_5gS40zCjp2_SLHdmtJCSgJpgWjudEDTCJZFyOj4MYC_4BhtyqhB0qsaIQ/s1600/20-02-2014+15-02-46.png)

  
Details:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjDRQj1bykRPEsp1R6DkxBzMc4k-MjEf3WGMTE3VMulyiZ1Gvga5nxX-_wqlVx0CDDuErlyU-7wKeUt7BHk3Flp0EtmPC8tbgjWNSFq7zUd8lBCU-wVwG1WLzL7s7kAQHwP_GCgz4ZTFgo/s1600/20-02-2014+15-28-51.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjDRQj1bykRPEsp1R6DkxBzMc4k-MjEf3WGMTE3VMulyiZ1Gvga5nxX-_wqlVx0CDDuErlyU-7wKeUt7BHk3Flp0EtmPC8tbgjWNSFq7zUd8lBCU-wVwG1WLzL7s7kAQHwP_GCgz4ZTFgo/s1600/20-02-2014+15-28-51.png)

  
Additional fields rules:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiVg8dizbvsDCs4T7Ihvgrvcu-L29pqM1mntbFhf5VbFLi090_gkITNh683QN_xol0lFa1w9oRABdG3LvwXgDKckfW_ssMmpu32bc3rwMmas1ry_GgKtm0S4Q_y4-ULVGV3XaszggJaaRQ/s1600/20-02-2014+15-06-26.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiVg8dizbvsDCs4T7Ihvgrvcu-L29pqM1mntbFhf5VbFLi090_gkITNh683QN_xol0lFa1w9oRABdG3LvwXgDKckfW_ssMmpu32bc3rwMmas1ry_GgKtm0S4Q_y4-ULVGV3XaszggJaaRQ/s1600/20-02-2014+15-06-26.png)

  
Additionnal fields rules (texts):  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhfxtwWV0ngx54pgUU0ujehZhxT2De6KNubcrbo_GlDcpYDK0AdGVOp2kLnkRIMfx7AWogyuglUoTqMHIWiigeGcF2JxPoikDch7uMXw2e7Q0z_r_lz0AWsfHuorTCk_zJekXbUNi1xMgM/s1600/20-02-2014+15-07-25.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhfxtwWV0ngx54pgUU0ujehZhxT2De6KNubcrbo_GlDcpYDK0AdGVOp2kLnkRIMfx7AWogyuglUoTqMHIWiigeGcF2JxPoikDch7uMXw2e7Q0z_r_lz0AWsfHuorTCk_zJekXbUNi1xMgM/s1600/20-02-2014+15-07-25.png)

  
Edit rule:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj5AafSGYMJS4tZWPLWHIz9IK-36Ig3WQCOj0nPYSuE7S6Xv-pET-6Me-x0-tp7B7-V0bK-GO3yzkKDqKu1_MVGTalomrl9R9M-sVBL0Zsl21FhAV3LXF3dQJEfQDU0yp4udZ4xkZHwWrM/s1600/20-02-2014+15-20-15.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj5AafSGYMJS4tZWPLWHIz9IK-36Ig3WQCOj0nPYSuE7S6Xv-pET-6Me-x0-tp7B7-V0bK-GO3yzkKDqKu1_MVGTalomrl9R9M-sVBL0Zsl21FhAV3LXF3dQJEfQDU0yp4udZ4xkZHwWrM/s1600/20-02-2014+15-20-15.png)

  
Edit text:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjD0PftmHUUdEOdOm2vM-l7gXWnqQb0jhIVM5juuwUOnwx6_7Ajq-n3IrNGYbueoiPPN14UPV0pXN8pRCojomZnL50Yqskq0Odcl30gmhzBE_nTUm0itIKTI4p1kyvIVWKZ4nOBo-5i77s/s1600/20-02-2014+15-18-21.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjD0PftmHUUdEOdOm2vM-l7gXWnqQb0jhIVM5juuwUOnwx6_7Ajq-n3IrNGYbueoiPPN14UPV0pXN8pRCojomZnL50Yqskq0Odcl30gmhzBE_nTUm0itIKTI4p1kyvIVWKZ4nOBo-5i77s/s1600/20-02-2014+15-18-21.png)

  
VBV/MCSC rules:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgsOBxcQD4K787WVt25rfs1bcigdo08NUVwa6XbzqeM58_z9jpDbZXnxvi2_Bh32Y0c7ivdgx-6vXUQ3xOycJn3IjRhkmU3j88vdn00NXId1dexQH1OqmslCQVY9sb0FwxK0A1B9JXUSic/s1600/20-02-2014+15-08-30.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgsOBxcQD4K787WVt25rfs1bcigdo08NUVwa6XbzqeM58_z9jpDbZXnxvi2_Bh32Y0c7ivdgx-6vXUQ3xOycJn3IjRhkmU3j88vdn00NXId1dexQH1OqmslCQVY9sb0FwxK0A1B9JXUSic/s1600/20-02-2014+15-08-30.png)

  
Add a rule:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi_CaZPobkMaxCFwtrH6ks9kBe_aCn3pJDjdTpyqlwZrZzajwAwALtdonse6GuNOdRUT-kButWpF7UB62ZvsgHPiyyKHKUxV-GInRNqFptEc6jELdkGTstoWV2BJ660ACHCAcnFw6nx0O4/s1600/20-02-2014+15-16-42.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi_CaZPobkMaxCFwtrH6ks9kBe_aCn3pJDjdTpyqlwZrZzajwAwALtdonse6GuNOdRUT-kButWpF7UB62ZvsgHPiyyKHKUxV-GInRNqFptEc6jELdkGTstoWV2BJ660ACHCAcnFw6nx0O4/s1600/20-02-2014+15-16-42.png)

  
Options:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjPc8Hp1rzRE_Xelpu0EfhnDV7XUtj1oFC09B9O2FKZ5rK6cdu97v2loNk-bSM1wGcgic_UXOObjIqk0UmeVC_LkIMb3PVEPMgDlEkaXlr_E_gVLJSTuKhrPK9armGH9_YpEb-gkGpGNlc/s1600/20-02-2014+15-11-02.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjPc8Hp1rzRE_Xelpu0EfhnDV7XUtj1oFC09B9O2FKZ5rK6cdu97v2loNk-bSM1wGcgic_UXOObjIqk0UmeVC_LkIMb3PVEPMgDlEkaXlr_E_gVLJSTuKhrPK9armGH9_YpEb-gkGpGNlc/s1600/20-02-2014+15-11-02.png)

  
Options (CC Checker):  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhEP8945GEDUnizLdpQYMqdkbUeTeliWYN3-wMcd4yCQHiQL3EBX10ALKUGtyFVcDMUBfQMWobE_8ZTpCKIMonYa79948_lZzAVK7ou70NqQL22TV56fH4e-jtWuXYxdtHkBE8CfmaF0FE/s1600/20-02-2014+15-12-23.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhEP8945GEDUnizLdpQYMqdkbUeTeliWYN3-wMcd4yCQHiQL3EBX10ALKUGtyFVcDMUBfQMWobE_8ZTpCKIMonYa79948_lZzAVK7ou70NqQL22TV56fH4e-jtWuXYxdtHkBE8CfmaF0FE/s1600/20-02-2014+15-12-23.png)

  
Files, dumped from another panel, targeting La banque Postal (a French bank):  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjF3rrEh_OaN1F6grGTp8eCP0DjAwkUtt4By311QRKfMe-cSOLRnba38YTcAggt3eSPFNOIECRp3NVN8wFfqZtnHvE5gkRgy94hyphenhyphen2Geoy6X0SSfv1n27NvsRsNQ5VVFD_p7Kl9gKSMNHSA/s1600/02-01-2014+21-36-38.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjF3rrEh_OaN1F6grGTp8eCP0DjAwkUtt4By311QRKfMe-cSOLRnba38YTcAggt3eSPFNOIECRp3NVN8wFfqZtnHvE5gkRgy94hyphenhyphen2Geoy6X0SSfv1n27NvsRsNQ5VVFD_p7Kl9gKSMNHSA/s1600/02-01-2014+21-36-38.png)

#### [Source](https://www.xylibox.com/2014/05/atsengine.html)

<br/>
---
