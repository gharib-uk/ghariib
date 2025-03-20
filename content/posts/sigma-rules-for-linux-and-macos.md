---
title: "Sigma rules for Linux and MacOS"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "virus"
---

**TLDR**: VT Crowdsourced Sigma rules will now also match suspicious activity for macOS and Linux binaries, in addition to Windows.

We recently discussed how to maximize the value of Sigma rules by easily converting them to YARA Livehunts. Unfortunately, at that time Sigma rules were only matched against Windows binaries.

Since then, our engineering team worked hard to provide a better experience to Sigma lovers, increasing Crowdsourced Sigma rules value by extending matches to macOS and Linux samples.

## Welcome macOS and Linux

Although we are still working to implement Sysmon in our Linux and macOS sandboxes, we implemented new features that allow Sigma rule matching by extracting samples’ runtime behavior.

For example, a process created in our sandbox that ends in “/crontab” and contains the "-l" parameter in the command line would match the following Sigma rule:

logsource:

  product: linux

  category: process\_creation

detection:

  selection:

    Image|endswith: '/crontab'

    CommandLine|contains: ' -l'

  condition: selection

We have mapped all the fields used by Sigma rules with the information offered by our sandboxes, which allowed us to map rules for image\_load, process\_creation and registry\_set, among others.

This approach has limitations. However, about 54% of Crowdsourced Sigma rules for Linux and 96% for macOS are related to process creation, meaning we already have enough information to match all these with our sandboxes’ output. The same happens for rules based on file creation.

Let’s look at some examples!

## Linux, MacOS and Windows examples

The following shell script sample matches 11 Crowdsourced Sigma Rule matches.

![](https://lh7-us.googleusercontent.com/bTWJ1UA1ZZD4Rp1zveMMQnBj56Ml4m7CNrtWH5WhtyHoHeZ_jQDszTLVAzzMceuWMx6Ca8KuBfZohKHmbBvL4GXs54q5Ioh3BB8S_fpZBraBPB3hTZnQkjwbC6VDTaAjuKQTsaxIlY47PwEq0ShTu6a8UL3Z-JRQbkf4-cXBMvv1LGHiWc57ouscO9y4SK0PZdj0CqzFqZnbvLlZDlNUoKWYoV7KioE16SOr7w)

For every rule, it is possible to check what triggered the match by clicking on "View matches”. In the case of Windows binaries, it would show what Sysmon event matched the behavior described in the Sigma rule, as we can see below:

![](https://lh7-us.googleusercontent.com/nH37fyOS9lB4SH9qzIPAK1yTKLJbS-MCJi7qeWjswE9dhWkogvY5br3uA-J2yPoDF9KYFgupIoA5k5YrG9ZzWwhNIdadZHk-xwJZg_zTUM7DTqeA6nqwMtg2o4sGnL38ZkzhB3Pd8MFf8BI-xN5oTQ0Ol-yMAtkww-xEPK5drozKGfysVmADlLUOuUwer9oGtl0gqz7c-mZXTbez9Rt492v3fTJADgURaO6ydg)

In the case of the shell script mentioned above, it shows the values that are relevant to the logic of the rule as you can see in the following image:

![](https://lh7-us.googleusercontent.com/iBVwMpjLAzwLYgV7s-PHZfaTDmuE2ISh-lBEGrLFcFgNIpQ7-f3B2vm4a8R_cEWM5Uax6v3xWT35reZbqurLjx90n-8asI2l1Pxdp41-xEFj0H3Sz99dHzJYtH6kgd54Gw3NvrKWr2UGCI588Ou4QpCZL5VGJnV5q1SddyyPRK6cPezRfFFCB6DVVT7HsB3xrc_FoRkickpOYVcO5F6z-oO6xHemiuSPM5p-eg)

Interestingly, Sigma rules intended for Linux also produce results in macOS environments, and vice versa. In this case, the shell script can be interpreted by both operating systems. Indeed, one of the matching rules for the sample called Indicator Removal on Host - Clear Mac System Logs was specifically created for macOS:

![](https://lh7-us.googleusercontent.com/IU3OM1Nz14k8Pdkaqpi5RWHGc9NjWAHoPKGCL90esa0QZMQb1ZhWqRTDqnVd6vJgCvNpso8PHNQJT2hwmjOrhJRCencqP93ABVfLas7udwAnUCx7gH-svk8LICqcHhZDhodxd5PyVL-oEcAOCx8j40JYMU8d4Iq7Pe-mUh_3IvxIcm20_ChTS7294F-GdclSfNPL19773yuw75SEvOXRruSBYi6NuHMvCtlW3g)

while a second matching rule, Commands to Clear or Remove the Syslog , was created for Linux:

![](https://lh7-us.googleusercontent.com/I83iS3D0wpyQu8TFf0n4dlf153RAfdvIRl8iMWb-sm1NMY8t3P8wwJ3Ixr2rO8gmzAaYHdz7hXuUsRDGzxi_UYuD6OFu3pKsXMXe8KbbDeaAL0tBWfDgcGICAE-irxXsdNRR9kkLDsWsSaXx-5tcdallNMpmYuc-m1GZbAQ4j6-vDPK9wp0Q9ga_Xs7iILRPWqKPzgQv8Q4DoPlLhf274anMMSi3YngwDPbx2w)

To get more examples of samples with Sigma rules that match sandboxes’ output instead of Sysmon, you can use the following queries:

(have:sigma) and not have:evtx type:mac

(have:sigma) and not have:evtx type:linux

A second interesting example is a dmg matching 8 Sigma rules, 5 of them originally created for Linux OS under the “process\_creation” category and 2 rules created for macOS. The last match… is a Sigma rule created for Windows samples!

![](https://s5.gifyu.com/images/Sjbsg.gif)

The new feature matching Sigma rules with Linux and macOS samples helped us identify some rules that are maybe too generic, which is not necessarily a problem as long as this is the intended behavior.

In this case, the Usage Of Web Request Commands And Cmdlets rule was originally created to detect web request using Windows’ command line:

![](https://lh7-us.googleusercontent.com/M5l7N9cJ_zdbuR-MthKBo19qxAcOfCFc2WsKIX4r7G6BVPRJsnhPrqaNxWtyuXmw2r57sc6lDsTtKi0FehS79GM-dN5E5ZYf9ePC_topnL7foWDDZ80kbnRax1seDc6D1iZeBmhtJMQE3wtFVAAFTUG9xydgYY6pIockMd9fLgn7ZrvbZSf1QcmXyShOdi3Eh-uS5OaACCiRnxXwdXXy8MAAuboqpfdVZJB2fQ)

The rule seems a bit too generic since it only checks for a few strings in the command line, although it can be highly effective for generic detection of suspicious behavior.

To understand why our Macintosh Disk Image sample triggered a detection for this rule, we checked the matches:

![](https://lh7-us.googleusercontent.com/Gzxk4mCCV0uJ4gyiwoQzIPqsscuWfDyu9CCoNhDuviiC-1zJNh4EyM_EJZq4y27OHUeiUMgf35MmPnMTGZwaYPmPC1VDaY2uzH1QHFb3XqXXTb20RY7Zl3RB_hw-GZqtnUfXP63Soq_RaWI8Wjnplq6z5yT8QWuWSg7B5YVVFbSxJx7GvTuT7uAp4UEzxnGkVYNtSXf_Mo5MKb2Ps-VCCpsDF5nvbL9dcKE3Gw)

As we can see, the use of the string “curl” in the command line was enough to match this sample.

This sigma rule had about 9k hits last year only, with more than 300 of the files being Linux or macOS samples. You can obtain the full list using the following query:

sigma\_rule:f92451c8957e89bb4e61e68433faeb8d7c1461c3b90d06b3403c8f3d87c728b8 and (type:linux or type:mac)

## Creating Livehunt rules from Sysmon EVTX outputs

So far we have mainly focused on samples that do not have Sysmon (EVTX) logs. Now let's see how it is possible to create a Livehunt rule based on Sysmon logs. For this, we are going to use the “structure” functionality provided in the Livehunt YARA editor, as we explain in this post.

The sample we will use in this example is associated with CobaltStrike and matches multiple Sigma rules that identify certain behaviors. It is important to note that for every Sigma match, we will see in the file “structure” the context **that matched** but not the full EVTX logs. These can be downloaded from the sample’s VT report behavior section under “Download Artifacts” or using our API (available for public and privately scanned files).

The following image shows the matching raw EVTX generated by our sample:

![](https://lh7-us.googleusercontent.com/04pqw6oXn8r_d-13nYXDumqEUwjtBKraV6WDXV7YV4zD00ZYiUJ5pgwkCrMi00IiIo1aOxmt7mbi3E6xWoJkz-FMlQOHAdA42xCrPoEWLfjFwmOkgz8D5poXj8J5eXXBSYSesDNd7YvxyA0ffV7oA3RbGoiOOSAZ7YY5_ZvXLWBVXvEyErLBcc3XSW0TU4GP7OLnwclWMRERkotJzwSC0iZO-X2SdaGz7yUgFA)

From the sample’s JSON Structure, **Sigma\_analysis\_results** is an array that contains objects with all the relevant information related to the matching Sigma rules, including details about the rule itself and EVTX logs. From the previous image, the first highlighted section is related to process creation and the second one is a registry event (value set).

As explained in our post, by just clicking on the fields that you are interested in you can start building your Livehunt rule, and adjust values accordingly. In this case, our rule will identify files creating registry keys under \\CurrentVersion\\RunOnce\\ with a .bat or .vbs extension:

import "vt"

rule sigma\_example\_registry\_keys {

  meta:

    target\_entity = "file"

  condition:

    for any vt\_behaviour\_sigma\_analysis\_results in vt.behaviour.sigma\_analysis\_results: (

      for any vt\_behaviour\_sigma\_analysis\_results\_match\_context in vt\_behaviour\_sigma\_analysis\_results.match\_context: (

        vt\_behaviour\_sigma\_analysis\_results\_match\_context.values\["TargetObject"\] icontains "\\CurrentVersion\\RunOnce\\" and

        (vt\_behaviour\_sigma\_analysis\_results\_match\_context.values\["Details"\] endswith ".vbs" or vt\_behaviour\_sigma\_analysis\_results\_match\_context.values\["Details"\] endswith ".bat")

      )

    )

}

Running this YARA using a Retrohunt finds multiple files:

daef729493b9061e7048b4df10b71fdba2e11d9147512f48463994a88c834a30 141e87e62c110b86cf7b01a2def60faab6365f6391eb0d4a7cbad8d480dd4706 814b2cab7c5a12ec18f345eb743857e74f5be45c35642dc01330e7a0def6269a 31b0e9b188fe944d58867bbfc827d77c7711c3a690168a417377fe6bf1544408 dd6051509ed8cf3d059b538fa8878f87423c51b297b49a12144d3d2923c89cce 647323f0245da631cef57d9ca1e3327c3242fe1cbbf6582c4d187e9f5fbfb678 40a90dd3b2132a299f725e91a5d0127013b21af24074afb944d8bc5735c1bd53 b44c6d2dd8ad93cecd795cecde83081292ee9949d65b2e98d4a2a3c8a97bd936 710b0cca7e7c17a3dd2a309f5ca417b76429feac1ab5fb60f5502995ebbd1515 50c098119ce41771e7a3b8230a7aa61ebea925e8eda46c33f0dd42b8950b92fe

Here you can see some interesting matches:

![](https://lh7-us.googleusercontent.com/sZLZhAfbKUNhk9RujX-DtJkb9Kg9yFT85fYEcpeyX1fdnglquAV0b8uZv8p4SodmKDpZjUEBp16rEV8ALiwRFYsewYdOBqKuA3b_5dcn9e1fMvHnYZLknfiOPrQ6G81-4408Huvi6-STzDJyC47X5IUuioFoMxljA8CN5dDaz0na3rnpyIM6Stz0aBHIgkJfuVEFe2rZGUCptk5k3KxWyD6DaNAmq6ZV62Efzw)

The next rule focuses on file creation events related to Sysmon (EVID 11) under the “C:WindowsSystem32” directory, with a “.dll” extension and having any “cve” tag (flagging potential CVE exploitation). Remember we can always include any additional details related to the samples we want to hunt, such as positives, metadata, tags, engines, … in addition to EVTX fields:

import "vt"

rule sigma\_rule\_evtx\_cve {

  meta:

    target\_entity = "file"

  condition:

    for any vt\_behaviour\_sigma\_analysis\_results in vt.behaviour.sigma\_analysis\_results: (

      for any vt\_behaviour\_sigma\_analysis\_results\_match\_context in vt\_behaviour\_sigma\_analysis\_results.match\_context: (

        vt\_behaviour\_sigma\_analysis\_results\_match\_context.values\["TargetFilename"\] startswith "C:\\Windows\\System32\\" and

        vt\_behaviour\_sigma\_analysis\_results\_match\_context.values\["TargetFilename"\] endswith ".dll" and

        for any vt\_metadata\_tags in vt.metadata.tags: (

        vt\_metadata\_tags icontains "cve-"

        )

      )

    )

}

## Sysmon EVTX fields - overlaps

Some of the details found in Sysmon EVTX fields (found in the VT JSON samples’ structure) can be redundant with details provided in other more traditional fields that you use for your Livehunt rules through the YARA VT module.

For example, instead of: vt\_behaviour\_sigma\_analysis\_results\_match\_context.values\["TargetFilename"\] from vt.behaviour.sigma\_analysis\_results

you could use: vt.behaviour.files\_written to identify file creation events.

When that’s the case, we recommend using traditional fields found in VT samples’ structure for the following reasons:

- Sysmon information is fully stored/indexed only the part matching the Sigma rule, which will limit any YARA hunting.
- We mapped most Sysmon fields into YARA VT module for simplicity.
- Linux and MacOS samples do not have any Sysmon information related to Sigma rules. Similar details about the match can be found under the “behaviour” JSON structure entry.

The new Sysmon-like details offered in the file “structure” also make VT an excellent platform for researchers and Sigma rule creators, allowing them to leverage this information without the need to create their own lab.

The following table helps mapping VT Intelligence queries, YARA VT module fields, Sigma Categories, and Sigma fields:

   
|   VT Intelligence   |   YARA VT module field   |   Sigma Category   |   Sigma Field   |
| --- | --- | --- | --- |
|   behavior\_created\_processes   |   vt.behaviour.processes\_created   |   process\_creation   |   Image  CommandLine  ParentCommandLine  ParentImage  OriginalFileName   |
|   behavior\_files   |   vt.behaviour.files\_attribute\_changed  vt.behaviour.files\_deleted  vt.behaviour.files\_opened  vt.behaviour.files\_copied  vt.behaviour.files\_copied\[x\].destination  vt.behaviour.files\_copied\[x\].source  vt.behaviour.files\_written  vt.behaviour.files\_dropped  vt.behaviour.files\_dropped\[x\].path  vt.behaviour.files\_dropped\[x\].sha256  vt.behaviour.files\_dropped\[x\].type   |   file\_access  file\_change  file\_delete  file\_rename  file\_event   |   TargetFilename   |
|   behavior\_injected\_processes   |   vt.behaviour.processes\_injected   |   process\_access  create\_remote\_thread  process\_creation   |   CallTrace  GrantedAccess  SourceImage  TargetImage  StartModule  StartFunction  TargetImage  SourceImage   |
|   behavior\_processes   |   vt.behaviour.processes\_terminated  vt.behaviour.processes\_killed  vt.behaviour.processes\_created  vt.behaviour.command\_executions  vt.behaviour.processes\_injected   |   process\_access  create\_remote\_thread  process\_creation   |   CallTrace  GrantedAccess  SourceImage  TargetImage  StartModule  StartFunction  TargetImage  SourceImage  Image  CommandLine  ParentCommandLine  ParentImage  OriginalFileName   |
|   behavior\_registry   |   vt.behaviour.registry\_keys\_deleted  vt.behaviour.registry\_keys\_opened  vt.behaviour.registry\_keys\_set  vt.behaviour.registry\_keys\_set\[x\].key  vt.behaviour.registry\_keys\_set\[x\].value   |   registry\_add  registry\_delete  registry\_event  registry\_rename  registry\_set   |   EventType  TargetObject  Details   |
|   behavior\_services   |   vt.behaviour.services\_bound  vt.behaviour.services\_created  vt.behaviour.services\_opened  vt.behaviour.services\_started  vt.behaviour.services\_stopped  vt.behaviour.services\_deleted   |   registry\_set  process\_creation   |   Image  CommandLine  ParentCommandLine  ParentImage  EventType  TargetObject  Details   |
|   behavior\_network   |   vt.behaviour.dns\_lookups  vt.behaviour.dns\_lookups\[x\].hostname  vt.behaviour.dns\_lookups\[x\].resolved\_ips  vt.behaviour.hosts\_file  vt.behaviour.ip\_traffic  vt.behaviour.ip\_traffic\[x\].destination\_ip  vt.behaviour.ip\_traffic\[x\].destination\_port  vt.behaviour.ip\_traffic\[x\].transport\_layer\_protocol  vt.behaviour.http\_conversations  vt.behaviour.http\_conversations\[x\].url  vt.behaviour.http\_conversations\[x\].request\_method  vt.behaviour.http\_conversations\[x\].request\_headers  vt.behaviour.http\_conversations\[x\].response\_headers  vt.behaviour.http\_conversations\[x\].response\_status\_code  vt.behaviour.http\_conversations\[x\].response\_body\_filetype  vt.behaviour.smtp\_conversations\[x\].hostname  vt.behaviour.smtp\_conversations\[x\].destination\_ip  vt.behaviour.smtp\_conversations\[x\].destination\_port  vt.behaviour.smtp\_conversations\[x\].smtp\_from  vt.behaviour.smtp\_conversations\[x\].smtp\_to  vt.behaviour.smtp\_conversations\[x\].message\_from  vt.behaviour.smtp\_conversations\[x\].message\_to  vt.behaviour.smtp\_conversations\[x\].message\_cc  vt.behaviour.smtp\_conversations\[x\].message\_bcc  vt.behaviour.smtp\_conversations\[x\].timestamp  vt.behaviour.smtp\_conversations\[x\].subject  vt.behaviour.smtp\_conversations\[x\].html\_body  vt.behaviour.smtp\_conversations\[x\].txt\_body  vt.behaviour.smtp\_conversations\[x\].x\_mailer  vt.behaviour.tls   |   network\_connection   |   DestinationHostname  DestinationIp  DestinationIsIpv6  DestinationPort  DestinationPortName  SourceIp  SourceIsIpv6  SourcePort  SourcePortName   |
|   behavior (too generic)   |   vt.behaviour.modules\_loaded   |   image\_load   |   ImageLoaded  Image  OriginalFileName   |

## Wrapping up

At VirusTotal, we believe that the Sigma language is a valuable tool for the community to share information about samples’ behavior. Our objective is to make its use on VT as simple as possible. Our addition of MacOS and Linux is just the start of what we are working on, as we aim to add Sysmon for Linux to obtain more robust results, including the ability to download full generated logs.

Remember that here you have a list of all the Crowdsourced Sigma rules that are currently deployed in VirusTotal and that you can use for threat hunting.

We hope you join our fan club of Sigma and VirusTotal, and as always we are happy to hear your feedback.

Happy Hunting!

**TLDR**: VT Crowdsourced Sigma rules will now also match suspicious activity for macOS and Linux binaries, in addition to Windows.

We recently discussed how to maximize the value of Sigma rules by easily converting them to YARA Livehunts. Unfortunately, at that time Sigma rules were only matched against Windows binaries.

Since then, our engineering team worked hard to provide a better experience to Sigma lovers, increasing Crowdsourced Sigma rules value by extending matches to macOS and Linux samples.

## Welcome macOS and Linux

Although we are still working to implement Sysmon in our Linux and macOS sandboxes, we implemented new features that allow Sigma rule matching by extracting samples’ runtime behavior.

For example, a process created in our sandbox that ends in “/crontab” and contains the "-l" parameter in the command line would match the following Sigma rule:

logsource:

  product: linux

  category: process\_creation

detection:

  selection:

    Image|endswith: '/crontab'

    CommandLine|contains: ' -l'

  condition: selection

We have mapped all the fields used by Sigma rules with the information offered by our sandboxes, which allowed us to map rules for image\_load, process\_creation and registry\_set, among others.

This approach has limitations. However, about 54% of Crowdsourced Sigma rules for Linux and 96% for macOS are related to process creation, meaning we already have enough information to match all these with our sandboxes’ output. The same happens for rules based on file creation.

Let’s look at some examples!

## Linux, MacOS and Windows examples

The following shell script sample matches 11 Crowdsourced Sigma Rule matches.

![](https://lh7-us.googleusercontent.com/bTWJ1UA1ZZD4Rp1zveMMQnBj56Ml4m7CNrtWH5WhtyHoHeZ_jQDszTLVAzzMceuWMx6Ca8KuBfZohKHmbBvL4GXs54q5Ioh3BB8S_fpZBraBPB3hTZnQkjwbC6VDTaAjuKQTsaxIlY47PwEq0ShTu6a8UL3Z-JRQbkf4-cXBMvv1LGHiWc57ouscO9y4SK0PZdj0CqzFqZnbvLlZDlNUoKWYoV7KioE16SOr7w)

For every rule, it is possible to check what triggered the match by clicking on "View matches”. In the case of Windows binaries, it would show what Sysmon event matched the behavior described in the Sigma rule, as we can see below:

![](https://lh7-us.googleusercontent.com/nH37fyOS9lB4SH9qzIPAK1yTKLJbS-MCJi7qeWjswE9dhWkogvY5br3uA-J2yPoDF9KYFgupIoA5k5YrG9ZzWwhNIdadZHk-xwJZg_zTUM7DTqeA6nqwMtg2o4sGnL38ZkzhB3Pd8MFf8BI-xN5oTQ0Ol-yMAtkww-xEPK5drozKGfysVmADlLUOuUwer9oGtl0gqz7c-mZXTbez9Rt492v3fTJADgURaO6ydg)

In the case of the shell script mentioned above, it shows the values that are relevant to the logic of the rule as you can see in the following image:

![](https://lh7-us.googleusercontent.com/iBVwMpjLAzwLYgV7s-PHZfaTDmuE2ISh-lBEGrLFcFgNIpQ7-f3B2vm4a8R_cEWM5Uax6v3xWT35reZbqurLjx90n-8asI2l1Pxdp41-xEFj0H3Sz99dHzJYtH6kgd54Gw3NvrKWr2UGCI588Ou4QpCZL5VGJnV5q1SddyyPRK6cPezRfFFCB6DVVT7HsB3xrc_FoRkickpOYVcO5F6z-oO6xHemiuSPM5p-eg)

Interestingly, Sigma rules intended for Linux also produce results in macOS environments, and vice versa. In this case, the shell script can be interpreted by both operating systems. Indeed, one of the matching rules for the sample called Indicator Removal on Host - Clear Mac System Logs was specifically created for macOS:

![](https://lh7-us.googleusercontent.com/IU3OM1Nz14k8Pdkaqpi5RWHGc9NjWAHoPKGCL90esa0QZMQb1ZhWqRTDqnVd6vJgCvNpso8PHNQJT2hwmjOrhJRCencqP93ABVfLas7udwAnUCx7gH-svk8LICqcHhZDhodxd5PyVL-oEcAOCx8j40JYMU8d4Iq7Pe-mUh_3IvxIcm20_ChTS7294F-GdclSfNPL19773yuw75SEvOXRruSBYi6NuHMvCtlW3g)

while a second matching rule, Commands to Clear or Remove the Syslog , was created for Linux:

![](https://lh7-us.googleusercontent.com/I83iS3D0wpyQu8TFf0n4dlf153RAfdvIRl8iMWb-sm1NMY8t3P8wwJ3Ixr2rO8gmzAaYHdz7hXuUsRDGzxi_UYuD6OFu3pKsXMXe8KbbDeaAL0tBWfDgcGICAE-irxXsdNRR9kkLDsWsSaXx-5tcdallNMpmYuc-m1GZbAQ4j6-vDPK9wp0Q9ga_Xs7iILRPWqKPzgQv8Q4DoPlLhf274anMMSi3YngwDPbx2w)

To get more examples of samples with Sigma rules that match sandboxes’ output instead of Sysmon, you can use the following queries:

(have:sigma) and not have:evtx type:mac

(have:sigma) and not have:evtx type:linux

A second interesting example is a dmg matching 8 Sigma rules, 5 of them originally created for Linux OS under the “process\_creation” category and 2 rules created for macOS. The last match… is a Sigma rule created for Windows samples!

![](https://s5.gifyu.com/images/Sjbsg.gif)

The new feature matching Sigma rules with Linux and macOS samples helped us identify some rules that are maybe too generic, which is not necessarily a problem as long as this is the intended behavior.

In this case, the Usage Of Web Request Commands And Cmdlets rule was originally created to detect web request using Windows’ command line:

![](https://lh7-us.googleusercontent.com/M5l7N9cJ_zdbuR-MthKBo19qxAcOfCFc2WsKIX4r7G6BVPRJsnhPrqaNxWtyuXmw2r57sc6lDsTtKi0FehS79GM-dN5E5ZYf9ePC_topnL7foWDDZ80kbnRax1seDc6D1iZeBmhtJMQE3wtFVAAFTUG9xydgYY6pIockMd9fLgn7ZrvbZSf1QcmXyShOdi3Eh-uS5OaACCiRnxXwdXXy8MAAuboqpfdVZJB2fQ)

The rule seems a bit too generic since it only checks for a few strings in the command line, although it can be highly effective for generic detection of suspicious behavior.

To understand why our Macintosh Disk Image sample triggered a detection for this rule, we checked the matches:

![](https://lh7-us.googleusercontent.com/Gzxk4mCCV0uJ4gyiwoQzIPqsscuWfDyu9CCoNhDuviiC-1zJNh4EyM_EJZq4y27OHUeiUMgf35MmPnMTGZwaYPmPC1VDaY2uzH1QHFb3XqXXTb20RY7Zl3RB_hw-GZqtnUfXP63Soq_RaWI8Wjnplq6z5yT8QWuWSg7B5YVVFbSxJx7GvTuT7uAp4UEzxnGkVYNtSXf_Mo5MKb2Ps-VCCpsDF5nvbL9dcKE3Gw)

As we can see, the use of the string “curl” in the command line was enough to match this sample.

This sigma rule had about 9k hits last year only, with more than 300 of the files being Linux or macOS samples. You can obtain the full list using the following query:

sigma\_rule:f92451c8957e89bb4e61e68433faeb8d7c1461c3b90d06b3403c8f3d87c728b8 and (type:linux or type:mac)

## Creating Livehunt rules from Sysmon EVTX outputs

So far we have mainly focused on samples that do not have Sysmon (EVTX) logs. Now let's see how it is possible to create a Livehunt rule based on Sysmon logs. For this, we are going to use the “structure” functionality provided in the Livehunt YARA editor, as we explain in this post.

The sample we will use in this example is associated with CobaltStrike and matches multiple Sigma rules that identify certain behaviors. It is important to note that for every Sigma match, we will see in the file “structure” the context **that matched** but not the full EVTX logs. These can be downloaded from the sample’s VT report behavior section under “Download Artifacts” or using our API (available for public and privately scanned files).

The following image shows the matching raw EVTX generated by our sample:

![](https://lh7-us.googleusercontent.com/04pqw6oXn8r_d-13nYXDumqEUwjtBKraV6WDXV7YV4zD00ZYiUJ5pgwkCrMi00IiIo1aOxmt7mbi3E6xWoJkz-FMlQOHAdA42xCrPoEWLfjFwmOkgz8D5poXj8J5eXXBSYSesDNd7YvxyA0ffV7oA3RbGoiOOSAZ7YY5_ZvXLWBVXvEyErLBcc3XSW0TU4GP7OLnwclWMRERkotJzwSC0iZO-X2SdaGz7yUgFA)

From the sample’s JSON Structure, **Sigma\_analysis\_results** is an array that contains objects with all the relevant information related to the matching Sigma rules, including details about the rule itself and EVTX logs. From the previous image, the first highlighted section is related to process creation and the second one is a registry event (value set).

As explained in our post, by just clicking on the fields that you are interested in you can start building your Livehunt rule, and adjust values accordingly. In this case, our rule will identify files creating registry keys under \\CurrentVersion\\RunOnce\\ with a .bat or .vbs extension:

import "vt"

rule sigma\_example\_registry\_keys {

  meta:

    target\_entity = "file"

  condition:

    for any vt\_behaviour\_sigma\_analysis\_results in vt.behaviour.sigma\_analysis\_results: (

      for any vt\_behaviour\_sigma\_analysis\_results\_match\_context in vt\_behaviour\_sigma\_analysis\_results.match\_context: (

        vt\_behaviour\_sigma\_analysis\_results\_match\_context.values\["TargetObject"\] icontains "\\CurrentVersion\\RunOnce\\" and

        (vt\_behaviour\_sigma\_analysis\_results\_match\_context.values\["Details"\] endswith ".vbs" or vt\_behaviour\_sigma\_analysis\_results\_match\_context.values\["Details"\] endswith ".bat")

      )

    )

}

Running this YARA using a Retrohunt finds multiple files:

daef729493b9061e7048b4df10b71fdba2e11d9147512f48463994a88c834a30 141e87e62c110b86cf7b01a2def60faab6365f6391eb0d4a7cbad8d480dd4706 814b2cab7c5a12ec18f345eb743857e74f5be45c35642dc01330e7a0def6269a 31b0e9b188fe944d58867bbfc827d77c7711c3a690168a417377fe6bf1544408 dd6051509ed8cf3d059b538fa8878f87423c51b297b49a12144d3d2923c89cce 647323f0245da631cef57d9ca1e3327c3242fe1cbbf6582c4d187e9f5fbfb678 40a90dd3b2132a299f725e91a5d0127013b21af24074afb944d8bc5735c1bd53 b44c6d2dd8ad93cecd795cecde83081292ee9949d65b2e98d4a2a3c8a97bd936 710b0cca7e7c17a3dd2a309f5ca417b76429feac1ab5fb60f5502995ebbd1515 50c098119ce41771e7a3b8230a7aa61ebea925e8eda46c33f0dd42b8950b92fe

Here you can see some interesting matches:

![](https://lh7-us.googleusercontent.com/sZLZhAfbKUNhk9RujX-DtJkb9Kg9yFT85fYEcpeyX1fdnglquAV0b8uZv8p4SodmKDpZjUEBp16rEV8ALiwRFYsewYdOBqKuA3b_5dcn9e1fMvHnYZLknfiOPrQ6G81-4408Huvi6-STzDJyC47X5IUuioFoMxljA8CN5dDaz0na3rnpyIM6Stz0aBHIgkJfuVEFe2rZGUCptk5k3KxWyD6DaNAmq6ZV62Efzw)

The next rule focuses on file creation events related to Sysmon (EVID 11) under the “C:WindowsSystem32” directory, with a “.dll” extension and having any “cve” tag (flagging potential CVE exploitation). Remember we can always include any additional details related to the samples we want to hunt, such as positives, metadata, tags, engines, … in addition to EVTX fields:

import "vt"

rule sigma\_rule\_evtx\_cve {

  meta:

    target\_entity = "file"

  condition:

    for any vt\_behaviour\_sigma\_analysis\_results in vt.behaviour.sigma\_analysis\_results: (

      for any vt\_behaviour\_sigma\_analysis\_results\_match\_context in vt\_behaviour\_sigma\_analysis\_results.match\_context: (

        vt\_behaviour\_sigma\_analysis\_results\_match\_context.values\["TargetFilename"\] startswith "C:\\Windows\\System32\\" and

        vt\_behaviour\_sigma\_analysis\_results\_match\_context.values\["TargetFilename"\] endswith ".dll" and

        for any vt\_metadata\_tags in vt.metadata.tags: (

        vt\_metadata\_tags icontains "cve-"

        )

      )

    )

}

## Sysmon EVTX fields - overlaps

Some of the details found in Sysmon EVTX fields (found in the VT JSON samples’ structure) can be redundant with details provided in other more traditional fields that you use for your Livehunt rules through the YARA VT module.

For example, instead of: vt\_behaviour\_sigma\_analysis\_results\_match\_context.values\["TargetFilename"\] from vt.behaviour.sigma\_analysis\_results

you could use: vt.behaviour.files\_written to identify file creation events.

When that’s the case, we recommend using traditional fields found in VT samples’ structure for the following reasons:

- Sysmon information is fully stored/indexed only the part matching the Sigma rule, which will limit any YARA hunting.
- We mapped most Sysmon fields into YARA VT module for simplicity.
- Linux and MacOS samples do not have any Sysmon information related to Sigma rules. Similar details about the match can be found under the “behaviour” JSON structure entry.

The new Sysmon-like details offered in the file “structure” also make VT an excellent platform for researchers and Sigma rule creators, allowing them to leverage this information without the need to create their own lab.

The following table helps mapping VT Intelligence queries, YARA VT module fields, Sigma Categories, and Sigma fields:

   
|   VT Intelligence   |   YARA VT module field   |   Sigma Category   |   Sigma Field   |
| --- | --- | --- | --- |
|   behavior\_created\_processes   |   vt.behaviour.processes\_created   |   process\_creation   |   Image  CommandLine  ParentCommandLine  ParentImage  OriginalFileName   |
|   behavior\_files   |   vt.behaviour.files\_attribute\_changed  vt.behaviour.files\_deleted  vt.behaviour.files\_opened  vt.behaviour.files\_copied  vt.behaviour.files\_copied\[x\].destination  vt.behaviour.files\_copied\[x\].source  vt.behaviour.files\_written  vt.behaviour.files\_dropped  vt.behaviour.files\_dropped\[x\].path  vt.behaviour.files\_dropped\[x\].sha256  vt.behaviour.files\_dropped\[x\].type   |   file\_access  file\_change  file\_delete  file\_rename  file\_event   |   TargetFilename   |
|   behavior\_injected\_processes   |   vt.behaviour.processes\_injected   |   process\_access  create\_remote\_thread  process\_creation   |   CallTrace  GrantedAccess  SourceImage  TargetImage  StartModule  StartFunction  TargetImage  SourceImage   |
|   behavior\_processes   |   vt.behaviour.processes\_terminated  vt.behaviour.processes\_killed  vt.behaviour.processes\_created  vt.behaviour.command\_executions  vt.behaviour.processes\_injected   |   process\_access  create\_remote\_thread  process\_creation   |   CallTrace  GrantedAccess  SourceImage  TargetImage  StartModule  StartFunction  TargetImage  SourceImage  Image  CommandLine  ParentCommandLine  ParentImage  OriginalFileName   |
|   behavior\_registry   |   vt.behaviour.registry\_keys\_deleted  vt.behaviour.registry\_keys\_opened  vt.behaviour.registry\_keys\_set  vt.behaviour.registry\_keys\_set\[x\].key  vt.behaviour.registry\_keys\_set\[x\].value   |   registry\_add  registry\_delete  registry\_event  registry\_rename  registry\_set   |   EventType  TargetObject  Details   |
|   behavior\_services   |   vt.behaviour.services\_bound  vt.behaviour.services\_created  vt.behaviour.services\_opened  vt.behaviour.services\_started  vt.behaviour.services\_stopped  vt.behaviour.services\_deleted   |   registry\_set  process\_creation   |   Image  CommandLine  ParentCommandLine  ParentImage  EventType  TargetObject  Details   |
|   behavior\_network   |   vt.behaviour.dns\_lookups  vt.behaviour.dns\_lookups\[x\].hostname  vt.behaviour.dns\_lookups\[x\].resolved\_ips  vt.behaviour.hosts\_file  vt.behaviour.ip\_traffic  vt.behaviour.ip\_traffic\[x\].destination\_ip  vt.behaviour.ip\_traffic\[x\].destination\_port  vt.behaviour.ip\_traffic\[x\].transport\_layer\_protocol  vt.behaviour.http\_conversations  vt.behaviour.http\_conversations\[x\].url  vt.behaviour.http\_conversations\[x\].request\_method  vt.behaviour.http\_conversations\[x\].request\_headers  vt.behaviour.http\_conversations\[x\].response\_headers  vt.behaviour.http\_conversations\[x\].response\_status\_code  vt.behaviour.http\_conversations\[x\].response\_body\_filetype  vt.behaviour.smtp\_conversations\[x\].hostname  vt.behaviour.smtp\_conversations\[x\].destination\_ip  vt.behaviour.smtp\_conversations\[x\].destination\_port  vt.behaviour.smtp\_conversations\[x\].smtp\_from  vt.behaviour.smtp\_conversations\[x\].smtp\_to  vt.behaviour.smtp\_conversations\[x\].message\_from  vt.behaviour.smtp\_conversations\[x\].message\_to  vt.behaviour.smtp\_conversations\[x\].message\_cc  vt.behaviour.smtp\_conversations\[x\].message\_bcc  vt.behaviour.smtp\_conversations\[x\].timestamp  vt.behaviour.smtp\_conversations\[x\].subject  vt.behaviour.smtp\_conversations\[x\].html\_body  vt.behaviour.smtp\_conversations\[x\].txt\_body  vt.behaviour.smtp\_conversations\[x\].x\_mailer  vt.behaviour.tls   |   network\_connection   |   DestinationHostname  DestinationIp  DestinationIsIpv6  DestinationPort  DestinationPortName  SourceIp  SourceIsIpv6  SourcePort  SourcePortName   |
|   behavior (too generic)   |   vt.behaviour.modules\_loaded   |   image\_load   |   ImageLoaded  Image  OriginalFileName   |

## Wrapping up

At VirusTotal, we believe that the Sigma language is a valuable tool for the community to share information about samples’ behavior. Our objective is to make its use on VT as simple as possible. Our addition of MacOS and Linux is just the start of what we are working on, as we aim to add Sysmon for Linux to obtain more robust results, including the ability to download full generated logs.

Remember that here you have a list of all the Crowdsourced Sigma rules that are currently deployed in VirusTotal and that you can use for threat hunting.

We hope you join our fan club of Sigma and VirusTotal, and as always we are happy to hear your feedback.

Happy Hunting!

Go to Source
