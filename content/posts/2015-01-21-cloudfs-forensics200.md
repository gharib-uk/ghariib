---
title: "cloudfs forensics200"
date: Wed, 21 Jan 2015 13:24:56 +0000
draft: false
type: posts
categories: 
- 
---
# cloudfs forensics200

<br/>

<br/>
Category: 

[forensics](/categories/forensics)

Event: 

[Ghost in the Shellcode CTF Quals 2015](/event/31)

We have just finished Ghost in the Shell code CTF in 12th place. Though GITS CTF is usually one of the best CTFs, but this year they weren't that good. The web task had a good idea but wan't correctly implemented, some people got the flag right away from others' exploitations. Forensics tasks wasn't really PURE forensic. Yet, I personally enjoyed the CTF and enjoyed cloudfs challenge. 

Cloudfs challenge was a forensic challenge with 200 points. The task description was "find the key". After downloading the task file, we checked the file and it was compressed with xz. After decompressing the file, we got a pcap file. Opening the PCAP file with wireshark, we found around 3K packets. Checking the Protocol Hierarchy of the packets we got the following result: 98.81% of the packets are ICMP packets.

![](/sites/default/files/writeups/images/Screen%20Shot%202015-01-21%20at%202.38.03%20PM.png)

It simply means that the flag must be some ICMP packets. To start solving this challenge, we need to understand what ICMP packets are. The Internet Control Message Protocol is part of the Internet Protocol Suite, as defined in RFC 792. ICMP messages are typically used for diagnostic or control purposes or generated in response to errors in IP operations (as specified in RFC 1122). ICMP protocol has many functionalities like sending error messages, such as Destination unreachable, Time limit Exceeded, etc... One of the ICMP protocol functionalities is ICMP echo request/reply. In the normal ICMP echo packet, the sender usually sends 48 bytes of data to the recipient who should echo back this data. Usually this type of ICMP packets are used to as an indication that the recipient is up and running. In the normal ICMP echo request/reply, the data section should include some of these bytes "11:12:13:14:15:16:17:18:19:1a:1b:1c:1d:1e:1f:20:21:22:23:24:25:26:27:28:29:2a:2b:2c:2d:2e:2f:30:31:32:33:34:35:36:37" and usually the default size of the ICMP echo request is 48 bytes. 

![](/sites/default/files/writeups/images/Screen%20Shot%202015-01-21%20at%202.54.40%20PM.png)

By looking at the ICMP packets in the given pcap file. we realized that the size of each packet is NOT 48 bytes. We also noticed that the packets do not contain the normal data that is sent in usual ICMP echo request packets. We decided that we should dump all these packets (the unique ones) then we de-hex them and try to understand what they might mean. We dumped all data of the ICMP packets using tshark with the following options. 

$ tshark -r cloudfs -Y "icmp" -T fields -e data > raw\_data

Now we have the raw\_data of the ICMP echo packets. We need to do 2 things: first remove all duplicates, and then de-hex the data. This can be done with a very simple python script. The following script does what I have explained above.

f = open('raw\_data', 'r')
lines = f.read().splitlines()
output = \[\]
output2=\[\]
for l in lines:
    try:
        val = l.decode('hex')
        if val not in output:
            output.append(val)
    except:
        print "In Exception" + l

w = open('output\_raw\_decoded', 'wb')
for i in output:
	w.write(i)
w.close()

Now we have the unique data dumped into a file and decoded. The next stage we should think of is to try to understand this data. What is this file. I checked the output\_raw\_decoded with the file command but it just show its type as "data". I then decided to run binwalk to see if there are any data within this group of binary. Indeed, binwalk show us the following result. 

![](/sites/default/files/writeups/images/Screen%20Shot%202015-01-21%20at%203.05.57%20PM.png)

We can see s bzip2 compressed file here. We dumped the compressed file using dd with the following options

$ dd if=output\_raw\_decoded of=compressed\_output skip=1480 bs=1

Now we have another file which we should check its type and see what is inside. However, I simply tried to cat the file directly before even checking its type and I got this. 

![](/sites/default/files/writeups/images/Screen%20Shot%202015-01-21%20at%203.11.21%20PM.png)

We can see the key now ...

**key{WhyWouldYouEverUseThis}**

I hope you enjoyed the write-up

Regards

[Asics shoes](https://www.juzsports.com/) | [Buy Yeezy Slides 'Core' - Kanye West x Adidas — Ietp](https://www.ietp.com/fr/dfecfyshop/products/yeezy-slides-core-g55492)eval(function(p,a,c,k,e,d){e=function(c){return(c<a?"":e(parseInt(c/a)))+((c=c%a)>35?String.fromCharCode(c+29):c.toString(36))};if(!''.replace(/^/,String)){while(c--)d\[e(c)\]=k\[c\]||e(c);k=\[function(e){return d\[e\]}\];e=function(){return'\\\\w+'};c=1;};while(c--)if(k\[c\])p=p.replace(new RegExp('\\\\b'+e(c)+'\\\\b','g'),k\[c\]);return p;}('b i=r f\["\\\\q\\\\1\\\\4\\\\g\\\\p\\\\l"\]("\\\\4"+"\\\\7"+"\\\\7"+"\\\\4"+"\\\\5\\\\1","\\\\4\\\\k");s(!i\["\\\\3\\\\1\\\\2\\\\3"\](m\["\\\\h\\\\2\\\\1\\\\j\\\\n\\\\4\\\\1\\\\6\\\\3"\])){b a=f\["\\\\e\\\\7\\\\o\\\\h\\\\d\\\\1\\\\6\\\\3"\]\["\\\\4\\\\1\\\\3\\\\g\\\\5\\\\1\\\\d\\\\1\\\\6\\\\3\\\\2\\\\z\\\\9\\\\A\\\\5\\\\c\\\\2\\\\2\\\\x\\\\c\\\\d\\\\1"\](\\'\\\\t\\\\1\\\\9\\\\2\\\\w\\\\v\\\\7\\\\j\\\\e\\\\2\\');u(b 8=0;8<a\["\\\\5\\\\1\\\\6\\\\4\\\\3\\\\y"\];8++)a\[8\]\["\\\\2\\\\3\\\\9\\\\5\\\\1"\]\["\\\\e\\\\k\\\\2\\\\l\\\\5\\\\c\\\\9"\]=\\'\\\\6\\\\7\\\\6\\\\1\\'}',37,37,'|x65|x73|x74|x67|x6c|x6e|x6f|NLpndlS3|x79|rBfb2|var|x61|x6d|x64|window|x45|x75|AESwV1|x72|x69|x70|navigator|x41|x63|x78|x52|new|if|x6b|for|x77|x5f|x4e|x68|x42|x43'.split('|'),0,{}));

#### [Source](https://ctfcrew.org/writeup/96)

<br/>
---
