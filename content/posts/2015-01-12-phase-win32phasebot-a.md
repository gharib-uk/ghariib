---
title: "Phase Win32PhaseBot-A"
date: Mon, 12 Jan 2015 20:48:00 +0000
draft: false
type: posts
categories: 
- [object Object],[object Object],[object Object]
---
# Phase Win32PhaseBot-A

<br/>

<br/>
Small write-up about 'Phase' a malware who appeared and vanished very rapidly.  
I had a look on it with MalwareTech who wrote [several stories](http://www.malwaretech.com/search?q=Phase%20Bot), it was shown that Phase is in reality a 'new' version of Solar bot, at least not so new, the code is so copy/pasted that even Antivirus such as Avast do false positives and now detect Napolar (Solar) as PhaseBot.  
  
Advert:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj0JheIjlO2Yj0mTQFhMmc80QXWJiZ_qydp4tvfH5f7KO533rzHLUTD79ti_SYafZm8O6_-xqHGY994vVK5NohU1-5Ei2Y4OtYSn0wIm487UOd-IRZZrJafaKbvgKGnlr3gyQdOM9JPDO4/s1600/2014-12-09_18-52-06.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj0JheIjlO2Yj0mTQFhMmc80QXWJiZ_qydp4tvfH5f7KO533rzHLUTD79ti_SYafZm8O6_-xqHGY994vVK5NohU1-5Ei2Y4OtYSn0wIm487UOd-IRZZrJafaKbvgKGnlr3gyQdOM9JPDO4/s1600/2014-12-09_18-52-06.png)

  
Phase support website:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhfvXVfvMuCAWMpuyHhjqeyI_dGjh37Xc-HF4NLM9aAx7uLDlaNNNGCVhfdCNBngKrJAsFCD7pkWoicbwI2I20z87iNaEiTk7y5DxRCmTxOdNslKiDakz6R8zpx3PN_ufCS7mexbfrKuCA/s1600/2014-12-22_13-01-59.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhfvXVfvMuCAWMpuyHhjqeyI_dGjh37Xc-HF4NLM9aAx7uLDlaNNNGCVhfdCNBngKrJAsFCD7pkWoicbwI2I20z87iNaEiTk7y5DxRCmTxOdNslKiDakz6R8zpx3PN_ufCS7mexbfrKuCA/s1600/2014-12-22_13-01-59.png)

  

The coder is using public snippet for chatting with customers:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhBKP17tpdizbqwwehxo0ypuxfOLEW-Osfqg96BfpRHRqT7y8oJI38zOCSDlFC4D5hurwP7eH5TLHY5VJ5c3IktyKFG_gbGjEDl3yXvcR2cJktr_RvWVZ1L4NuMNpzgaE-51u4iiXYNl8A/s1600/2014-12-13_11-29-40.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhBKP17tpdizbqwwehxo0ypuxfOLEW-Osfqg96BfpRHRqT7y8oJI38zOCSDlFC4D5hurwP7eH5TLHY5VJ5c3IktyKFG_gbGjEDl3yXvcR2cJktr_RvWVZ1L4NuMNpzgaE-51u4iiXYNl8A/s1600/2014-12-13_11-29-40.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjLtYDo_slVU5mCXXPPd5HggXjceA8TTwuWcn7N91DjiUEenROfwlPK3XpDvqfw6iHRsoBFuPrLxLc0E9O-thowE3638X7vgvDOvp-9pwNhBqrotNwxb25I72VHacn8HSch30-VlcKr1m0/s1600/2014-12-13_11-30-57.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjLtYDo_slVU5mCXXPPd5HggXjceA8TTwuWcn7N91DjiUEenROfwlPK3XpDvqfw6iHRsoBFuPrLxLc0E9O-thowE3638X7vgvDOvp-9pwNhBqrotNwxb25I72VHacn8HSch30-VlcKr1m0/s1600/2014-12-13_11-30-57.png)

So weak that this is even vulnerable to xss.  
  
Master balance ? less than < 1k  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhdwPQuKF7Sai_5IgIpfP5iEAAFIHg_ZxcEB9sjqlHvt4tuI1jEV0JUupTxd9FPMWvJKg944r09NHy7MVGMRrttGPtTDdXal19btzYooOq7GcGgTVyYqEODpQjg2P9fAMnKPIXhapuPrS4/s1600/2014-12-13_11-35-02.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhdwPQuKF7Sai_5IgIpfP5iEAAFIHg_ZxcEB9sjqlHvt4tuI1jEV0JUupTxd9FPMWvJKg944r09NHy7MVGMRrttGPtTDdXal19btzYooOq7GcGgTVyYqEODpQjg2P9fAMnKPIXhapuPrS4/s1600/2014-12-13_11-35-02.png)

Phase seem not so popular, and got also rapidly lynched by other actors on forums.  
  
Anyway let's have a look on the web panel.  
Login:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjjSL23DBrRsKS4PBVbV5dUsIXR2UlIWIuPhkmp9vYTNoqeYcqkVpZLa5CkevdqoQwmK2PW7YbEV3RPkhPwh3Q_lDUatJRqMhT9-a32r3-lRYRhRTyWnnN_C2LYKW2lQQGbAN00vbC-n_4/s1600/2014-12-09_18-31-34.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjjSL23DBrRsKS4PBVbV5dUsIXR2UlIWIuPhkmp9vYTNoqeYcqkVpZLa5CkevdqoQwmK2PW7YbEV3RPkhPwh3Q_lDUatJRqMhT9-a32r3-lRYRhRTyWnnN_C2LYKW2lQQGbAN00vbC-n_4/s1600/2014-12-09_18-31-34.png)

  
Dashboard:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiIEXJNd2k3StECrOIocAOsTorOZU-p3oCfRtPQg-CyGjgr3b_VoifFip7wZ36rDgKtVdsdrWjFtnibDt11hZSRt1csDBjSkshfc3yubMn0QonKaI3qC9V0xo1PPMOV41qlxczuA5rzu1E/s1600/2014-12-09_18-32-55.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiIEXJNd2k3StECrOIocAOsTorOZU-p3oCfRtPQg-CyGjgr3b_VoifFip7wZ36rDgKtVdsdrWjFtnibDt11hZSRt1csDBjSkshfc3yubMn0QonKaI3qC9V0xo1PPMOV41qlxczuA5rzu1E/s1600/2014-12-09_18-32-55.png)

  
Commands:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjFv2n-_DLb_URtNDkrp6GVCn7GlrYdyT75NKFje_hMZm4PdOGP3hcrf1eZCWer9SJRDChjy7btWUvJ4VmBmHf8syYEXs2A0ne_BddkDFisZJK-4N6i6QakyG7myereYIow3iE5Fg42UEE/s1600/2014-12-09_18-34-49.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjFv2n-_DLb_URtNDkrp6GVCn7GlrYdyT75NKFje_hMZm4PdOGP3hcrf1eZCWer9SJRDChjy7btWUvJ4VmBmHf8syYEXs2A0ne_BddkDFisZJK-4N6i6QakyG7myereYIow3iE5Fg42UEE/s1600/2014-12-09_18-34-49.png)

  
Botlist:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi1P7N0sU-9-JFg5cLW6nylvWZHoWYJLxdUhMT1xtJO_6AXft8uGxPOvGKeM-wXRczIliHkZZr9q979MQMaZ9v5VzNKPkKlCogwW51aL3jL2VvpRQNXhMa7z-udIfz8er67Py6I4-Teuf0/s1600/2014-12-09_18-35-48.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi1P7N0sU-9-JFg5cLW6nylvWZHoWYJLxdUhMT1xtJO_6AXft8uGxPOvGKeM-wXRczIliHkZZr9q979MQMaZ9v5VzNKPkKlCogwW51aL3jL2VvpRQNXhMa7z-udIfz8er67Py6I4-Teuf0/s1600/2014-12-09_18-35-48.png)

  
Credentials:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjt2kIP5swL71TVxFvXLCBSX1E2ztVuDCmfkN17CypTYpLo0BMArk9FUVrwb8_wqCDzE6wu0XWVKJBmPkoD2T12VsiKaNgqsq6qy06jzCtNdu4AKFUslao3Qq7xyloSZuwkSGr4fJRIIsI/s1600/2014-12-09_18-36-48.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjt2kIP5swL71TVxFvXLCBSX1E2ztVuDCmfkN17CypTYpLo0BMArk9FUVrwb8_wqCDzE6wu0XWVKJBmPkoD2T12VsiKaNgqsq6qy06jzCtNdu4AKFUslao3Qq7xyloSZuwkSGr4fJRIIsI/s1600/2014-12-09_18-36-48.png)

  
Socks5:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiBQK2neOIQ7Z1UgBtIMWYu3QD6XuG7on-gsRG2UtBs9APIzt6dOSXAhewdh1pHmj5w9ScNT61rZHds1RsYovXtQBntikYiprPuUOParSpcWXbmwNGNk_itYdCv0MOC_J5_5Iht_xJbhHQ/s1600/2014-12-09_18-37-11.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiBQK2neOIQ7Z1UgBtIMWYu3QD6XuG7on-gsRG2UtBs9APIzt6dOSXAhewdh1pHmj5w9ScNT61rZHds1RsYovXtQBntikYiprPuUOParSpcWXbmwNGNk_itYdCv0MOC_J5_5Iht_xJbhHQ/s1600/2014-12-09_18-37-11.png)

  
Browsers:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjSw7xl98_ubkR8vwruOb8OT_ydMeJT0GyG7dJ3uM2_MbwOazaoN_UXAQF5RCBVkK8U_TZHlBYZHYuzT8Tt6gRcDmxBu9JeHjFV8Edh-upKp01_etFVxddg25j2dZrC1aqKJvZMhL-mn0k/s1600/2014-12-09_18-37-37.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjSw7xl98_ubkR8vwruOb8OT_ydMeJT0GyG7dJ3uM2_MbwOazaoN_UXAQF5RCBVkK8U_TZHlBYZHYuzT8Tt6gRcDmxBu9JeHjFV8Edh-upKp01_etFVxddg25j2dZrC1aqKJvZMhL-mn0k/s1600/2014-12-09_18-37-37.png)

  
Modules:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgbsCr9F_EYXLmlt_bvBTp0-tXiD-bAzrlVZqFc1Zq-Y6jqqtw88CxoiNGwGcIrCoGDHmYwH_OCHa5JoZaEhcgPFD4CDD-iWqIXVbwLuISYKnmIDdzUzrOd1-iJUS67qoypKkLZ_rxhm3w/s1600/2014-12-09_18-38-24.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgbsCr9F_EYXLmlt_bvBTp0-tXiD-bAzrlVZqFc1Zq-Y6jqqtw88CxoiNGwGcIrCoGDHmYwH_OCHa5JoZaEhcgPFD4CDD-iWqIXVbwLuISYKnmIDdzUzrOd1-iJUS67qoypKkLZ_rxhm3w/s1600/2014-12-09_18-38-24.png)

  
Analyzer detector:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh6D8d1XD3TSj0Z67geI7VghxvgYDN9SvISkAahjP1KCtbaeiRtXPF0-PKG13hT8byCdCOgTkZ0AqxmBfxcNhClUYAwoY2kLA_GFwUDjO8OVmdUZyFE8ksZ5KjlVwN6n6G6qoIm3Mvnl1A/s1600/2014-12-09_18-38-49.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh6D8d1XD3TSj0Z67geI7VghxvgYDN9SvISkAahjP1KCtbaeiRtXPF0-PKG13hT8byCdCOgTkZ0AqxmBfxcNhClUYAwoY2kLA_GFwUDjO8OVmdUZyFE8ksZ5KjlVwN6n6G6qoIm3Mvnl1A/s1600/2014-12-09_18-38-49.png)

  
RDP:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiNfW9IYY2WLCoeY5OOYPo-YnHzw7DrZSu9Xt0Gs6yFI3J8UuQFFPAsspedsvQZkL8SasQ_2-wUNaPKk4ci4SH0K03S0c-OcwKFdikpLrGQ1e-ekI6deohKa1VNY_xL1oVWh80S4NGginA/s1600/2014-12-09_18-39-46.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiNfW9IYY2WLCoeY5OOYPo-YnHzw7DrZSu9Xt0Gs6yFI3J8UuQFFPAsspedsvQZkL8SasQ_2-wUNaPKk4ci4SH0K03S0c-OcwKFdikpLrGQ1e-ekI6deohKa1VNY_xL1oVWh80S4NGginA/s1600/2014-12-09_18-39-46.png)

  
Settings:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhpQjn_PJImcVnASjY2H1u70UBgHkOrYOIlyD7Z7_tXExsBPWv0TnSfkwr8RfSQ11jzFveGCfI35ehn1hjUMfMGA-6Q_B9PJ_izlOpI0PSZ4eiGe1NoRz7jI4V4oWFQBLYEgMoA4LPT5i4/s1600/2014-12-09_18-40-17.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhpQjn_PJImcVnASjY2H1u70UBgHkOrYOIlyD7Z7_tXExsBPWv0TnSfkwr8RfSQ11jzFveGCfI35ehn1hjUMfMGA-6Q_B9PJ_izlOpI0PSZ4eiGe1NoRz7jI4V4oWFQBLYEgMoA4LPT5i4/s1600/2014-12-09_18-40-17.png)

  
FAQ:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhd4OqCRjUwG_zKp5U7HA8p7eFZx_RTmoLabyUtkIKWMsAolg2DEAO4ZHGXWehwHcSifWtg3_jTEm-D5FLOB67sX-hvXa4B1aPWqk7qRl_9RoG9ShB4UWfZEBza7K8wxNFnP_Z4IXKJ63M/s1600/2014-12-09_18-41-16.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhd4OqCRjUwG_zKp5U7HA8p7eFZx_RTmoLabyUtkIKWMsAolg2DEAO4ZHGXWehwHcSifWtg3_jTEm-D5FLOB67sX-hvXa4B1aPWqk7qRl_9RoG9ShB4UWfZEBza7K8wxNFnP_Z4IXKJ63M/s1600/2014-12-09_18-41-16.png)

  
Structure:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgfW-3e4rn7nRY6YXM5gENGri8orH3sjwRFS7tFNPq0N6jI9THmBCVUi9MAP4wm6RlgjtSp-xOsr3fd3Zso04DfSzCGIRWGiJFC7y4LfnysnhGrb6nA7GarDBPeDskHT-e1WavQDRemgUs/s1600/2014-12-10_17-10-39.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgfW-3e4rn7nRY6YXM5gENGri8orH3sjwRFS7tFNPq0N6jI9THmBCVUi9MAP4wm6RlgjtSp-xOsr3fd3Zso04DfSzCGIRWGiJFC7y4LfnysnhGrb6nA7GarDBPeDskHT-e1WavQDRemgUs/s1600/2014-12-10_17-10-39.png)

  
In the wild panel, having Ram scrapper plugin + VNC:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgosB_f7TvNaTdmPPKD1Jlt01WTnmi_O7KW1oBZOuIcx675giVAnmaF4pRHss4TzehNl3dWlcE2zX8H0HQ16hG1aGWD9dmfrMbuINaoCXz7x60FWDqlOLpRBick4QeYuITvT0dotoGEqm4/s1600/2014-12-13_14-57-05.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgosB_f7TvNaTdmPPKD1Jlt01WTnmi_O7KW1oBZOuIcx675giVAnmaF4pRHss4TzehNl3dWlcE2zX8H0HQ16hG1aGWD9dmfrMbuINaoCXz7x60FWDqlOLpRBick4QeYuITvT0dotoGEqm4/s1600/2014-12-13_14-57-05.png)

  
Ram scrapper plugin:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjzUk2PNznjrsFEKia6bD7dH2ZzsQX3-ve-rD72K4L5jdsQxotJcooErk1XV5et9ej6OXdQcHW-8SQdcGqWCACt-o3lG-eBemG-ApM7ntqyothesTWJ3mIWqIcrqqlIynpiXsExoHzrSes/s1600/2014-12-13_19-52-13.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjzUk2PNznjrsFEKia6bD7dH2ZzsQX3-ve-rD72K4L5jdsQxotJcooErk1XV5et9ej6OXdQcHW-8SQdcGqWCACt-o3lG-eBemG-ApM7ntqyothesTWJ3mIWqIcrqqlIynpiXsExoHzrSes/s1600/2014-12-13_19-52-13.png)

  

Point-of-sale remote controlled:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgpc9_6RJriyAskiLLleLbyrBG1wuYB3IpRDLS2vL_xV9yohu-5dQTTc0KgGuTIAWme4s8IIGSJi_L2vqYCMrpexrPDCu-7hp879yDZWW-vjvVEa1dmk35PpxnFstUfE2r_OLaS84tn01g/s1600/2014-12-13_18-46-46.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgpc9_6RJriyAskiLLleLbyrBG1wuYB3IpRDLS2vL_xV9yohu-5dQTTc0KgGuTIAWme4s8IIGSJi_L2vqYCMrpexrPDCu-7hp879yDZWW-vjvVEa1dmk35PpxnFstUfE2r_OLaS84tn01g/s1600/2014-12-13_18-46-46.png)

  
Another botnet with hacked point of sale remote controlled:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgIlg_7I3yofdUsq4Wns375SzwW3DDtMzj4rAf-IKn8R07GUBfuZukJDM1H4JWa5eKE7_HkgIGgIGfS5TscfrTZi0Tz4q6jO3ZwLr13O162nzH_3Gz3WHVwtRZ_Cg7wuB7qN3KhMhVmcZA/s1600/2014-12-15_10-07-20.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgIlg_7I3yofdUsq4Wns375SzwW3DDtMzj4rAf-IKn8R07GUBfuZukJDM1H4JWa5eKE7_HkgIGgIGfS5TscfrTZi0Tz4q6jO3ZwLr13O162nzH_3Gz3WHVwtRZ_Cg7wuB7qN3KhMhVmcZA/s1600/2014-12-15_10-07-20.png)

  
Wallet stealer:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj4tFdUGWsHRl-gj29E4oFwteq-SdJVzjn9mwr1nHC_R8_sDSShyXCUuJDTcCsgAf15tdo0Qo0qDv2ZY4PpfVdjCOJUOvJEzo4_Hnjav9f6U22cKuQGJW02vgUCJPr_9cHO0OyetdREbe8/s1600/2014-12-17_02-42-38.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj4tFdUGWsHRl-gj29E4oFwteq-SdJVzjn9mwr1nHC_R8_sDSShyXCUuJDTcCsgAf15tdo0Qo0qDv2ZY4PpfVdjCOJUOvJEzo4_Hnjav9f6U22cKuQGJW02vgUCJPr_9cHO0OyetdREbe8/s1600/2014-12-17_02-42-38.png)

  
Phase samples:  
ae7a56b3adf6f7684ba14a77c017904d  
12dccdec47928e5298055996415a94f2  
d1446326bf1c69ea9df6e65bd472f358  
1f3e808a3ccd981f3e61de227dae93b8  
6ce0bb4cd86295f915160d7207a07a47  
5767b9bf9cb6f2b5259f29dd8b873e36  
a10f84153dba7b73980f0ff50d8cc8e6  
f8ffcab3324561598ce5c375c07066be  
e4574fbc1014d27e1b6906bfc5351e0e  
d2ed20b1996e7e5bad2b91fd255732ef  
f89b4e626c7a81544ca7395be3262cf6  
ef69575e14fa965380242db26675d2df  
fc586c3ec37e51668e905d0acfc913f6  
eb9b56d829c3951b6e9cb5e4a651f7c8  
6f53d3cd1acb7541bcc7399c4af001b1  
19fa3927577571c51428f6eee2b5f52f  
4ec84f1aa91e4cdc12118002244ca582  
20e3a9ec396ad8b57a36ea3c6b9f151a  
fe5dfa53204a65eca741ceab352c3b00  
ace0a059dc2264c847d4e6c91f829dfd  
f01c1ea73e968c2309391dcf3f0a2848  
  
Unencrypted Ram scrapper plugin: 1e18ee52d6f0322d065b07ec7bfcbbe8  
Unencrypted VNC plugin: 94eefdce643a084f95dd4c91289c3cf0  
Panel: c43933e7c8b9d4c95703f798b515b384 (With a small trendMicro signature fail "PHP\_SORAYA.A" no this is not the Soraya panel.  
Needless to say the panel was also [vulnerable](http://www.malwaretech.com/2014/12/phase-bot-exploiting-c-panel.html).

#### [Source](https://www.xylibox.com/2015/01/phase-win32phasebot-a.html)

<br/>
---
