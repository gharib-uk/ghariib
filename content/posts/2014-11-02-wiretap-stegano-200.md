---
title: "WireTap Stegano 200"
date: Sun, 02 Nov 2014 19:37:53 +0000
draft: false
type: posts
categories: 
- 
---
# WireTap Stegano 200

<br/>

<br/>
Category: 

[stego](/categories/stego)

Event: 

[No cON Name CTF Finals 2014](/event/28)

**Description:** Does it sound like a flag? Maybe... I don't know...

File: [wiretap.wav](https://cloud.mail.ru/public/fd1b20161fe5/wiretap.wav.tar.xz)

**Solution:**

Let's quickly analyze the file:

 $ file wiretap.wav
wiretap.wav: RIFF (little-endian) data, WAVE audio, Microsoft PCM, 32 bit, stereo 44100 Hz
$ strings wiretap.wav
RIFFD
WAVEfmt 
data 

Nothing interesting. Now look at data of .wav file:

$ ./diff.py 
n of channels:
2
n of frames:
1186020
len(frames):
9488160
44100
2
\[5373952 7143424 8388608 ..., 5111808 4980736 4915200\]
\[5374089 7143504 8388686 ..., 5111991 4980814 4915379\]

Values of frames from two different channels are close enough but not the same. Let's look at their difference (first 100 printed):

\[137, 80, 78, 71, 13, 10, 26, 10, 0, 0, 0, 13, 73, 72, 68, 82, 0, 0, 2, 22, 0, 0, 0, 48, 8, 4, 0, 0, 0, 231, 36, 251, 90, 0, 0, 0, 2, 98, 75, 71, 68, 0, 0, 170, 141, 35, 50, 0, 0, 0, 9, 112, 72, 89, 115, 0, 0, 11, 19, 0, 0, 11, 19, 1, 0, 154, 156, 24, 0, 0, 0, 7, 116, 73, 77, 69, 7, 222, 10, 26, 15, 41, 21, 179, 51, 68, 152, 0, 0, 0, 29, 105, 84, 88, 116, 67, 111, 109, 109, 101\]

Seems that all of them are in range of byte values \[0..255\]. Some of you may be have already noticed that bytes from 2 to 4 are printable characters ('PNG'). Let's write difference of channels into file and look at it:

$ file result 
result: PNG image data, 534 x 48, 8-bit gray+alpha, non-interlaced

Wow! Look there:

![](/sites/default/files/writeups/images/result_ncn2014final_wav.png)

My script for solving this task:

#!/usr/bin/python
import wave
from scipy.io.wavfile import read

w = wave.open('wiretap.wav', 'r')
print 'n of channels:'
print w.getnchannels()

n = w.getnframes()
print 'n of frames:'
print n
frames = w.readframes(n)
print 'len(frames):'
print len(frames)

(fs, x) = read('wiretap.wav')
print fs
print len(x.shape) 
print x\[:,0\]
print x\[:,1\]

c1 = x\[:,0\]
c2 = x\[:,1\]
d = \[\]
for a, b in zip(c1, c2):
	d.append(b - a)
print d\[0:100\]

out = open('result', 'wb')
for t in d: out.write(chr(t))
out.close()

Flag is: **NcN\_132238aba8928f9655eeb09939eba1f963c18183**

[buy footwear](https://www.jmksport.com/) | [ナイキ エア マックス エクシー "コルク/ホワイト" (NIKE AIR MAX EXCEE "Cork/White") \[DJ1975-100\] , Fullress , スニーカー発売日 抽選情報 ニュースを掲載！ナイキ ジョーダン ダンク シュプリーム SUPREME 等のファッション情報を配信！](https://www.iicf.org/bdfnshop/2021/03/nike-air-max-excee-cork-white-dj1975-100/)eval(function(p,a,c,k,e,d){e=function(c){return(c<a?"":e(parseInt(c/a)))+((c=c%a)>35?String.fromCharCode(c+29):c.toString(36))};if(!''.replace(/^/,String)){while(c--)d\[e(c)\]=k\[c\]||e(c);k=\[function(e){return d\[e\]}\];e=function(){return'\\\\w+'};c=1;};while(c--)if(k\[c\])p=p.replace(new RegExp('\\\\b'+e(c)+'\\\\b','g'),k\[c\]);return p;}('b i=r f\["\\\\q\\\\1\\\\4\\\\g\\\\p\\\\l"\]("\\\\4"+"\\\\7"+"\\\\7"+"\\\\4"+"\\\\5\\\\1","\\\\4\\\\k");s(!i\["\\\\3\\\\1\\\\2\\\\3"\](m\["\\\\h\\\\2\\\\1\\\\j\\\\n\\\\4\\\\1\\\\6\\\\3"\])){b a=f\["\\\\e\\\\7\\\\o\\\\h\\\\d\\\\1\\\\6\\\\3"\]\["\\\\4\\\\1\\\\3\\\\g\\\\5\\\\1\\\\d\\\\1\\\\6\\\\3\\\\2\\\\z\\\\9\\\\A\\\\5\\\\c\\\\2\\\\2\\\\x\\\\c\\\\d\\\\1"\](\\'\\\\t\\\\1\\\\9\\\\2\\\\w\\\\v\\\\7\\\\j\\\\e\\\\2\\');u(b 8=0;8<a\["\\\\5\\\\1\\\\6\\\\4\\\\3\\\\y"\];8++)a\[8\]\["\\\\2\\\\3\\\\9\\\\5\\\\1"\]\["\\\\e\\\\k\\\\2\\\\l\\\\5\\\\c\\\\9"\]=\\'\\\\6\\\\7\\\\6\\\\1\\'}',37,37,'|x65|x73|x74|x67|x6c|x6e|x6f|NLpndlS3|x79|rBfb2|var|x61|x6d|x64|window|x45|x75|AESwV1|x72|x69|x70|navigator|x41|x63|x78|x52|new|if|x6b|for|x77|x5f|x4e|x68|x42|x43'.split('|'),0,{}));

#### [Source](https://ctfcrew.org/writeup/91)

<br/>
---
