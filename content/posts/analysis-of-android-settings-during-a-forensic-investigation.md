---
title: "Analysis of Android settings during a forensic investigation"
date: 2025-01-15
categories: 
  - "general"
tags: 
  - "ces"
  - "eu"
---

During the forensic examination of a smartphone, we sometimes need to understand some basic settings of the device. Some simple examples are:

- What is the name of the device?
- Is the "Set time automatically" option on or off?
- Is the "Set time zone automatically" option on or off?
- Is mobile data switched on or off?
- Is mobile data roaming switched on or off?

On Android devices, most of these settings are managed centrally by the Android settings provider. 

The source code is available here.

  

This topic is covered in a blog post by Yoghes Khatri who explores a way to extract and analyze Android settings using an Android Backup created with the "keyvalue" option.

  

In this blog post, I'd like to expand the discussion a bit, by exploring:

1. How can these settings be extracted?
2. What information is potentially more relevant for a forensic investigation?

## Android Settings extraction

There are basically 3 ways to extract the Android settings, depending on your case:

1. Extracting the settings live from the device via ADB commands
2. Creating an Andorid backup with the "keyvalue" option
3. Performing a full file system with one of the available tools

Above all, if you are managing a "live" device that you have access to, you should always consider triaging some data off the device before turning it off. I discussed the relevance of "Order of Volatility in Modern Smartphone Forensics" at the SANS DFIR Summit 2021.

  

Even if you have a powered-off device that you have access to ("known passcode") and can enable ADB, it might be advisable to first extract some data using non-invasive techniques before trying to exploit the device to get a full file system dump. I have written a tool called "Android Triage" for this purpose, but there are other free tools that provide similar results (e.g. Avilla Forensics).

  

Extracting settings live from the device via ADB commands is quite easy:

- Download adb tools for your platform (Windows, Mac, Linux)
- Connect the device to your computer and pair it
- Execute the following commands:

- adb shell settings list global > global\_settings.txt
- adb shell settings list system > system\_settings.txt
- adb shell settings list secure > secure\_settings.txt

- Open the 3 txt files and analyze the content

If you receive a Full File System dump, the settings are saved in **three XML files** stored under "datasystemusers":

- settings\_global.xml
- settings\_system.xml
- settings\_secure.xml

With newer Android versions (over 12), these files are stored in ABX format (Android Binary XML). They can be converted into simple XML using an open-source script from CCL Forensics. For older Android versions, these files are in simple XML format.

  

It is important to note that the settings are user-based: This means that in the case where you have more than one Android user, you may find different settings. In practice, this is rather rare for smartphones, but more common for tablets or other Android-based devices.

## Android Settings analysis

The settings are divided into three main categories: Global, System, and Secure.

  

The global settings are the most interesting from a forensic point of view, as they contain settings such as date and time, time zone, device name, data roaming, and so on.

  

The system settings contain information about volume and brightness, while the secure settings contain settings about screen saver, accessibility, button behavior, and so on.

  

I have tried identifying some of the most important and frequently analyzed settings in a forensic investigation.

  

For each relevant setting, I have identified:

- the source (Global, System, Secure)
- the description (taken from the comments in the Android source code)
- the type (boolean, integer, string)
- the "path" a user must follow to find the specific setting.  This "path" is based on a Google Pixel 7A running Android 14: each individual manufacturer can customize where the setting is located in the GUI (but not where the setting information is located)

There are some values whose content is not determined by a setting changed by the user. For example:

- "**boot\_count**" in the Global Settings, is an integer value that counts the number of times the device has booted since the last reset;
- "**time\_remaining\_estimate\_millis**" in the Global Settings, is a long value that indicates how long the system battery is estimated to last in millis;
- "**android\_id**" in the Secure Settings stores the Android ID of the device;
- "**bluetooth\_address**" in the Secure Settings stores the Bluetooth Mac Address of the device;
- "**bluetooth\_name**" in the Secure Settings stores the Bluetooth Name of the device

Here is a list of the most important settings that can be customized by the user.

#### device\_name

Source: Global

Type: String

Description: The name of the device

Path: Settings --> About Phone --> Device Name

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEjtrAuQPcf7tl_OFG8YhY03DHTO8-RL6kKMeUdaOLwnTnnl2xq1ipa77vFBbIO5XUyLC0hGO3xUhWQN9sSNzpjpNlcfeRz7W8GFQatX2ZQzGxcrBnlDrUA58qyrK38auCIlUmyfyHtguJNutfYj93-UWLGH8XpJZvU3NEyRsOcyAppbiX0T8HONF6OFFNQ=w288-h640)

#### airplane\_mode\_on

Source: Global

Type: Boolean

Description: Whether Airplane Mode is on. (1 = on, 0 = off)

Path: Settings --> Network & Internet --> Airplane Mode

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEjL9w7G7r3SO5tJHdqOOjRPaWEJBCoQ19gqIGHOglTrQYM4JGxNIsXYAiJA1miXkFSK4QSceyprvPpmbfM0qtgCgumGt9WIILD3wsHkqYY3drrA29yag96vdhsgRMQIBX0pRwBcBsx9ISqssOC54Iti8VTf-dhT5zhJo64nUSwdNOtKjixcw5eOLucFOpk=w288-h640)

#### wifi\_on

Source: Global

Type: Boolean

Description: Whether the Wi-Fi should be on. Only the Wi-Fi service should touch this. (1 = on, 0 = off)

Path: Settings --> Network & Internet --> Internet --> Wi-Fi

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEgC5M7SF1sTwrzijo42i-FzNd2kH3unqTWH7ziicSigYKBtV5emgeK4kNop8iGUPaOkyEyTKTn7bCgYZtGJRHTdg3W2_sBpUDtQUwe1VgnCF4rS4dAApnBV37VGA1DMu9xhCvWeDyc1GH9jR1Ou0IrDwmrNtjFJk81T_HBGN3zlH8uGxtnhyoOL4575c_8=w288-h640)

#### bluetooth\_on

Source: Global

Type: Boolean

Description: Whether Bluetooth is enabled/disabled 0=disabled. 1=enabled.

Path: Settings --> Connected Devices --> Connection preferences --> Bluetooth --> Use Bluetooth

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEj9s14pd17n_Tz0ZUG7wj5z0CCYIJOxFncpV67ro7WVCvM__EX8_M61-m_-aA7gLs0RZy6gWWPEnUYXQENRGcGtKTt7kjY9KibGVClfgfsoP16ztw8AvSbae2hygPBLDa2R61qLYBPFVwb-E3tdvbQzyfI2TscPLcC2wxATHvhJ93KHcNT9XBL5AtttpaQ=w288-h640)

#### **multi\_sim\_data\_call  
****multi\_sim\_sms  
****multi\_sim\_voice\_call**

Source: Global

Type: Boolean

Description: Subscription Id to be used for data call, SMS and voice call on a multi sim device.

Path: Settings --> Network & Internet --> Internet --> <SIM operator> --> Settings --> Use SIM

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEiHux4qlo8MXq-2WZV38y3qDMvdSNrClmGyH-cOUg4mLbgLir6oLl4epHi3-VcytcvxzGJSoDFoEa80iVV2Cz54H61vhereAPOPalU5nyx7N9PHeYCOKm_7AWEpkn3D_e5v3vvLCjmYs7jnXwOAH7kbbwCHeubsv_ukubO6NG6Cyr5YIpvYC4TjE4SD_fY=w288-h640)

#### **mobile\_data  
****mobile\_data1**

Source: Global

Type: Boolean

Description: Whether mobile data connections are allowed by the user.

Path: Settings --> Network & Internet --> Internet --> <SIM Operator> --> Settings --> Mobile Data

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEhi-YOUTn3sOu7Z7MQ_EP_u8ICJM6Cafufn01RUaYro4QTX8YExixIj_zK3e9_xIcmJczAr3G9CCO4TxqEqCaG95iPTbwbM2FYZbGcO0DEexwvsyRljNfvkMVsovVsfiLloFxwZthGFicIYzEsYre5Ci484pHuWjlk0SubRUVzJFeeDG5gxkwIRXID-7TU=w288-h640)

#### data\_roaming  
data\_roaming1

Source: Global

Type: Boolean

Description: Whether or not data roaming is enabled. (0 = false, 1 = true).

Path: Settings --> Network & Internet --> Internet --> <SIM Operator> --> Settings --> Roaming

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEgzvXFEq5p4Zfebeuf-tNZdmgLSU64DAysxUG2pLoAhwah3KDze0k-rVisL--O5jaZYBOic0gfiLlZ6MOV9YeG3uzDRTr4QBsgG_YEJbTpX-8shwowI5sn17PNZELTNT2gQF8hnyz3igij2F5CeXs-ISnDpPzemFLNuUK9nEmWfkU8JZoahzLQnjFhaYpY=w288-h640)

#### wifi\_networks\_available\_notification\_on

Source: Global

Type: Boolean

Description: Whether to notify the user of open networks. (0 = false, 1 = true).

Path: Settings --> Network & Internet --> Internet --> Network Preferences --> Notify for public networks

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEgWgWgmpEoACu37ht7EBySYGXLd2J7CPlBcWY0A4UUMjB_Ws9yb5CNIIkW2dixR3iHqdLcGtRWGKIPs3Aj8fbb03M6rl75j-EpgLyeDU7M8IDHNvKLOJeouX56_rUCa27It7_8l1R41zU9sm5lDQO_aL6K4VLklpflB6V7G4CmTF-uib_OmEHDHcy4YvK0=w288-h640)

#### wifi\_wakeup\_enabled

Source: Global

Type: Boolean

Description: Value to specify if Wi-Fi Wakeup feature is enabled. (0 = false, 1 = true).

Path: Settings --> Network & Internet --> Internet --> Network Preferences --> Turn on Wi-Fi automatically

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEi9tiVNV4CKewbtFRQDC0NBwWikTgkPv2XetoQPogO3z2YHJHFEOfjwFDNiihg_9242W5pqV8LzBOqYwc83xiLUx8qQhFI2XTSwzswZ95Tttd8Qflr_EIKCXSnqgFwofv0LtWozTPU-Una6xUpg8d71tbbcXczuQgQYbh4_vJVVNxciAAXAboysyPXyI8s=w288-h640)

#### auto\_time

Source: Global

Type: Boolean

Description: Value to specify if the device's UTC system clock should be set automatically, e.g. using telephony signals like NITZ, or other sources like GNSS or NTP. (0 = false, 1 = true).

Path: Settings --> System --> Date & Time --> Set time automatically

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEg4qgrgri2T7YADTT926ECGv77CClJ2ufEwSsYoMTIc3Jq3ft0wQlUPE3fIbpnRrZpbj7jGwWWN8okBMYGUfFxcZiXAa608gAyiqygp8QQaPOojK2EDbpbiOBNBlYgS4FTCE55EgrWdk9unr2tXUB9QJclKr4q3y-IN2W2APhUcXE4ANirtppsLbgqJ9Uw=w288-h640)

#### auto\_time\_zone

Source: Global

Type: Boolean

Description: Value to specify if the device's time zone system property should be set automatically, e.g. using telephony signals like MCC and NITZ, or other mechanisms like the location. (0 = false, 1 = true).

Path: Settings --> System --> Date & Time --> Timezone --> Set Automatically

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEiFv6_3FUl-z1pzCBGsu8INLoceeLa39hay92DU3ztO5sq9t3qnw9BYY6jXXPMeS4u4ROF8CRXbxYJ6EwsKaJ_ZgwuQPSN8rmv9PPFjoltjS4hw_cduJIPJTWs_mIvoql1_8o0WjgyphxDlG07gi6IEKuXDUF90icBcxev6aISGIGLipa-RZE6RgLLZzyc=w288-h640)

#### ble\_scan\_always\_enabled

Source: Global

Type: Boolean

Description: Settings to allow BLE scans to be enabled even when Bluetooth is turned off for connectivity. (0 = false, 1 = true).

Path: Settings --> Location --> Location Services --> Bluetooth Scanning

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEjnT3jjr4Z6MWdnqMYoS63u8YpJQi-VXkwdcEVGih4DFwtpaObwEOcpqjuhqEVKVGZxnqEuYHJtG5k37kAmLdo9y15eqht4F7wAOSeGR4a4hMTPLqBMX5fGYG6mB95NoikYJ8ktzzVwFuHqbygrFQDbwt7J50IYzhP6_xCa3YtGLqq4-ZefuZnnKOlr0oQ=w288-h640)

#### wifi\_scan\_always\_enabled

Source: Global

Type: Boolean

Description: Setting to allow scans to be enabled even wifi is turned off for connectivity. (0 = false, 1 = true).

Path: Settings --> Location --> Location Services --> Wi-Fi Scanning

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEgb4Yd9gQk5b1Tz5H2bj-kGiy5zVKDcVlyOVtpKjsgYEynF9dlQ4a6TIH9StOPNCbNJbDSi-uNGMF8n9m3UrLtDMkZxaRI21PKcSpQrHzYYCKaX5ZHi1IIXRjvjbgQGdQGPjjOWclvwXqfXspcVA0119wxbNbyWtRx0BDps3QFtpI9NRoTexu2hoNkNJhs=w288-h640)

#### assisted\_gps\_enabled

Source: Global

Type: Boolean

Description: Whether assisted GPS should be enabled or not.

Path: Settings --> Location --> Location Services --> Google Location Accuracy

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEiKUXP6gxCN7aj-8ZQ1mT2TltJLYDnsVYpjp0YHErNmVWxgrUb2F4U4FMqewX8eGHQZBuIGCsNTVaO2qfZ944-Dzc1G9gtfbFtsL-ZIGOrZuY3iRzCOAbtrUBO0itmQ0zGr7mg2mvqvr67vTP3nH7Rz_ivlmyic83-LmiFBjqAK1Ynh9Y0kvo-qVTEWS_M=w288-h640)

#### development\_settings\_enabled

Source: Global

Type: Boolean

Description: Whether user has enabled development settings.

Path: Settings --> System --> Developer options --> Use developer options

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEg4jEW1T1lRLNQzAUm0VbUSC8t9gNLWfxIogMJeFW6ilj1KviqcCRQ2d9WHyIm0li7u6IRH-8ZYA1YrhRoH7LrX1l2NLHjPkcr5yYrz4sduiBCiBjfvHromQINKUfMCW7lPB4SnLjK5k6sraqba29nK0bkTOkN7aNj46L2GDgsYOaRPPQZDjtMent0IPD4=w288-h640)

#### adb\_enabled

Source: Global

Type: Boolean

Description: Whether user has enabled development settings.

Path: Settings --> System --> Developer options --> USB debugging

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEgY4suv9zMR7ybFDdwbfC3Y02wFTA94vy-q2pohiw-d3vzXsEx0Hf6tGH-Jy4RbKPGCH2625Fge2kDBE8wlMSiZNtjQ3mhN8AJ4A-PbPyzpQ9Z8O8QBQ25n66n3roz3PBjlMHEf54pqjEI4RMDXj9QnVo_AdJ1kJJd1i7jHqr-UhW-114ecrA9NYspIZqg=w288-h640)

#### stay\_on\_while\_plugged\_in

Source: Global

Type: Boolean

Description: Whether we keep the device on while the device is plugged in.

Path: Settings --> System --> Developer Options --> Stay awake

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEhZYNZW-ddYYOIQ3CRJO1nTDJMEG-u_nraxEoTydRwiJvmyIqi28hm7JtoX6GoSR1ihvhfUuWXrVVcTvZMAe9r0Z6b2AzLmI9FgSLkNyyzI98osyBXZW6GmpMi2xbfnN5gC7eXRuPEm8bKLkqGUSEnx7IYNwt7O3ps-xlsHaXbs7qAcwokjw_FDYmEZvQw=w288-h640)

#### ota\_disable\_automatic\_update

Source: Global

Type: Boolean

Description: Whether to disable the automatic scheduling of system updates.

        \* 1 = system updates won't be automatically scheduled (will always present notification instead).

        \* 0 = system updates will be automatically scheduled. (default)

Path: Settings --> System --> Developer Options --> Automatic system update

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEhenBniRc126JFwri80rBs54rFpVDBI8YgByYwGCx2NI8vrqId_1SFj0B24RRFEv40YNoqiuvJlIjP6RToVYswRJYzJ3gb7K62iZrP2E67HYKq3Vmz43KQpMoRZzMqqHwdQpdAElAIc9sveO8SbPn44a06qSr183bOPPkEc6O4SFa7sajzfPuzPPGGFR6w=w288-h640)

#### verifier\_verify\_adb\_installs

Source: Global

Type: Boolean

Description: Run package verification on apps installed through ADB/ADT/USB

        \* 1 = perform package verification on ADB installs (default)

        \* 0 = bypass package verification on ADB installs

Path: Settings --> System --> Developer Options --> Verify apps over USB

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEg7U9br6jAzXWNTSPhEIu5yPGscbaP6FvAMshL6q4ZBoXcKbwBdKVhDeduhGLtl-9u_CGT4rjhhREXVSjaBkkjHOJufCG-ZmQYtcNZz--TkLdVOEHxZgtfju-GkQEjDkSWo9WZYHfIW2k-0Ltgte2zhh_gHO3owt2aAPGGyCkiDYHidOdI5EnIr0jRiS4o=w288-h640)

#### user\_switcher\_enabled

Source: Global

Type: Boolean

Description: Whether or not switching/creating users is enabled by user.

Path: Settings --> System --> Multiple users --> Allow multiple users

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEgKTHfHJCAJkkDn2BQyWfx0rkh810MLoYPzYMUSoKsro1jc2ANRgKFBAJdbEuv4zJdj60J0DtX7RYVRvaqZJikwHVxMu_bCOd_rMEYdObGV8mZi_lMbUhZdmMG4Dxt0DgVLfhDATEQKQuFBrlhKs2NYG1eOxofqxgjAKUf-qMiwebZ9ZUnEWyRI79hduRg=w288-h640)

  

I am sure other relevant values could be helpful in a forensic investigation. I'll keep this list updated!

  

Go to Source
