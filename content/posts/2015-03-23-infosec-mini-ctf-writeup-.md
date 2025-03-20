---
title: "Infosec mini ctf writeup"
date: Mon, 23 Mar 2015 23:09:43 +0000
draft: false
type: posts
categories: 
- 
---
# Infosec mini ctf writeup

<br/>

<br/>
Category: 

[web](/categories/web)

[stego](/categories/stego)

[forensics](/categories/forensics)

Event: 

[Infosec Institute CTF](/event/33)

This is the InfoSec CTF writeup.  
The ctf was very great. However, I felt it a bit simpler I think that was intended as a basic starting level. Some of the challneges were very interesting others were very straight forward. One thing that make me suffer a bit is the images in the challneges. I always had the feeling that they always contained something (steganography). I also was suffering with some guessing challenges like levle number 9. Yet, the good thing about the challneges is that each one will teach you something. The purpose of the CTF was to share knowledge. Below, you can find my write-up so please read, enjoy and take the best of it.   
If you have any questions/comments, do NOT hesitate to contact me.

Thank you [InfoSec Institute](http://www.infosecinstitute.com/) for the CTF  
  
A pdf version of the solution can be found here.  
[https://www.dropbox.com/s/uuixb7zqcbyiq5x/solutions.zip?dl=0](https://www.dropbox.com/s/uuixb7zqcbyiq5x/solutions.zip?dl=0)   
If you would like to try the challenges before seeing the write-ups please check them on  
[http://ctf.infosecinstitute.com/](http://ctf.infosecinstitute.com/)

  
let's start :)  

  
  
Level One

Challenge:

“May the source be with you! “

Solution:

Once I saw the word “source” then I expected that the flag will be in the HTML source code. I viewed the source code in my browser, and I managed to see the flag in the first line of the HTML code as illustrated below in the screenshot

![](/sites/default/files/writeups/images/1_1.png)

flag: infosec\_flagis\_welcome

Level Two

Challenge:

“It seems like the image is broken..Can you check the file?“

Solution:

I checked the HTML source code and I got the image link which was “img/leveltwo.jpeg” Downloaded the image file and now it is time to analyse the file. The first step I wanted to to check the file type to see if it is actually an image. Executing the “file” command on linux that was the result.

![](/sites/default/files/writeups/images/2_1.png)

looks like some ascii data inside not an image. Viewing the file content using the “cat” command that was the output “aW5mb3NlY19mbGFnaXNfd2VhcmVqdXN0c3RhcnRpbmc=“. The data is encoded in base64. I managed to know that because of the “=“ that was padded in the end of the text. using the base64 tool to decode that data that was the output “infosec\_flagis\_wearejuststarting”

![](/sites/default/files/writeups/images/2_2.png)

  
  
  
Level Three

Challenge:

Nothing was stated regarding explicitly for the challenge. However there was that image that contains a QR code.

Solution:

sent the QR code to the following website [http://zxing.org/w/decode?u=http%3A%2F%2Fctf.infosecinstitute.com%2Fimg%2Fqrcode. png](http://zxing.org/w/decode?u=http%3A%2F%2Fctf.infosecinstitute.com%2Fimg%2Fqrcode.%20png)  
That was the result  
.. -. ..-. --- ... . -.-. ..-. .-.. .- --. .. ... -- --- .-. ... .. -. —.  
looks like some morse code. We need to find something to decode it. Using the following the website http://morsecode.scphillips.com/translator.html I managed to translate the morse code and that was the result.  
“INFOSEC\_FLAGIS\_MORSING”

  
  
  
Level Four

Challenge:

“HTTP means Hypertext Transfer Protocol”

Solution:

HTTP is a Hyptertext Transfer Protocol. I thought that I might find the flag in any of the headers received from the server. I fired up my burp suite proxy to see what I will get in the HTTP response. These were the headers received from the server.

![](/sites/default/files/writeups/images/4_1.png)

We can see that the server is setting a cookie in our browser. looks like it is encoding in some way however it has the same pattern as “infosec\_flagis\_xxxxxxx”  
I didn’t know what was the encoding but it looks like some stream cipher. I expected it will be a caesar cipher. I coded this quick script to try all caesar with different steps. The script should stops once it finds the word “infosec”

def decode\_ceaser(input\_str, n):  
    output = \[\]
    for c in input\_str:
        temp = 97+((ord(c)-97+n)%26)
        temp = chr(temp)
        output.append(temp)
    return output  
for i in xrange(25):
        res = decode\_ceaser(encoded\_str, i)
        res = ''.join(res)
        if 'infosec' in res:
            print res
            break

and that was the result of running the script

infosec\_flagis\_welovecookies

  
  
  
Level Five:

Challenge:

No text was written only an image.

Solution:

I think this is steganography problem. It did take a lot of time for me to solve it since I am not that good with steganography. I checked the image with Stegsolve didn’t find anything. I checked it also with steghide but nothing. I checked some online websites and it was this website http://www.futureboy.us/stegano/decinput.html. I uploaded the image to the website and It resulted in some binary array as illustrated below

![](/sites/default/files/writeups/images/5_0.png)

decoding the binary array I got using the following website http://string-functions.com/binary-string.aspx  
and the result was  
infosec\_flagis\_stegaliens

  
  
  
Level Six

Challenge:

“Do you want to download sharkfin.pcap file?”

Solution:

It is is a pcap file which we need to analyse. After downloading the pcap and opening with Wireshark. The first thing I do is to look at the protocol hierarchy and that was the result.

![](/sites/default/files/writeups/images/6_0.png)

We can see a lot of HTTPS data which probably will not be interested in since we can’t decrypt it. I filtered out all tcp  
data using the following filter “!(tcp)” and there was a single udp packet. I followed the UDP stream and that was the stream content. “696e666f7365635f666c616769735f736e6966666564”

Decoding the hex steam content that was the result

“infosec\_flagis\_sniffed”

  
  
  
Level Seven

Challenge:

Nothing appeared actually in the homepage.

Solution:

I opened the burp suite proxy to try to see the response coming from the server.

![](/sites/default/files/writeups/images/7_0.png)

looks like we have some base64 data in the HTTP response reason field. Decoding the data we got this:  
“infosec\_flagis\_youfoundit”

  
  
Level Eight

Challenge:

“Do you want to download app.exe file?”

Solution:

I downloaded the app.exe file. I thought first of reversing the app and see how it works. I was getting ready to run my windows VM and start the executable. However, I though of running the linux command “strings” quickly and see if I got any thing there. Indeed, I executed the command and that was the result.

![](/sites/default/files/writeups/images/8_0.png)

The flag: infosec\_flagis\_0x1a

  
  
  
Level Nine

Challenge:

Login page with username and password

Solution:

I first expected that this will be a sql injection and I should bypass the login. I tried different SQL injection vectors to login but didn’t receive any output. I then said it might be something easier than that. I tried some dictionary attack on the login page and the following credentials logged in successfully.

username: root  
password: attack  
Once I logged in the output was  
“ssaptluafed\_sigalf\_cesofni”  
we can see that this is the flag but reversed. Reversing it again we have “infosec\_flagis\_defaultpass”

The flags looks a bit weird for me. I searched the web for the cisco IDS default login credentials but couldn’t find anything. Actually my script took a lot of time running to find the username and password.

  
  
  
Level Ten

Challenge:

What kind of sound is this? Sorcery perhaps??

Solution:

I downloaded the audio file. I expected that the wave audio file might contain something hidden in one of its channels. I examined how many channels the wave file contains. It was only one channel which means probably nothing is hidden in the wave channels. I executed binwalk to see if there is any thing appended or inside the audio file. However, I didn’t manage to get anything. I checked the image on the challenge page it was stating “not listening”. I though then I should find away to listen to what is being played. I changed the playback speed to some values and was listening to the output. Indeed, when I changed the playback speed to 0.22X I managed to listen to

“infosec\_flagis\_sound”

The URL of the edited file is: http://st0rm.altervista.org/solved.wav

Page 12 of 18

  
  
  
Level Eleven

Challenge:

No it must not be a sound? But wait whaT? \[PHP logo\]

Solution:

I downloaded the php logo. and it was named “php-logo-virus.jpg” the name is very catchy so I believe it contains our flag. One of the main things to analyse when dealing with images is the exif data. http://regex.info/exif.cgi is one of the best websites to analyse the exif data of images. Using the regex.info website, we managed to extract the following “infosec\_flagis\_aHR0cDovL3d3dy5yb2xsZXJza2kuY28udWsvaW1hZ2VzYi9wb3dlcnNsa WRlX2xvZ29fbGFyZ2UuZ2lm%a0%86%01” from the “Document Name” in the exif data structure. We see part of the flag and the other part is encoded in base64. Decoding the base64 resulted in: “http://www.rollerski.co.uk/imagesb/powerslide\_logo\_large.gif” I visited the url and the image contain the word “powerslide”. Hence, our flag should be

Flag: infosec\_flagis\_powersilde

  
  
Level Twelve

Question:

Dig deeper

Solution:

I saw the same image in the first level. I then decided it will be a steganography challenge. I kept digging into the image with all possible ways but I couldn’t find anything. I actually wasted a couple of days in that. Then I decided to move away from the image and check the source code of the page. I checked the source code again to see if it was related to level 1 by any means. I couldn’t find anything obvious. I then decided to compare the html of the two pages to see if there any differences. I used the comparer tool in burp suite to see the difference and that was the result.

![](/sites/default/files/writeups/images/12_0.png)

Hmmm. We see there is a new css was added to leveltweleve.php file. I decided to check that css file. Now, I started to see the relation between the two levels (Dig deeper indeed). The content of the CSS file was  
.thisloveis{

color: #696e666f7365635f666c616769735f686579696d6e6f7461636f6c6f72; }

Looks very interesting. There is no colour with the following value and this looks like a hex value. Decoding the hex value we got:

infosec\_flagis\_heyimnotacolor

  
  
Level Thirteen

Challenge:

What the heck happened here? It seems that the challenge here is gone? Can you find it? Can you check if you can find the backup file for this one? I'm sorry for messing up :(

Solution:

This challenge requires a bit of guessing to get the old file. Out of convention, developers usually name the old files as .old or .bak. or .backup. I tried to access http://ctf.infosecinstitute.com/levelthirteen.php.old and indeed I managed to access the old php file (backup). Opening the file in a text editor

![](/sites/default/files/writeups/images/13_1.png)

We can see some interesting code commented out here. Our next step is to download the imadecoy file. I downloaded the file and directly executed the “file” command to know what file it is.

![](/sites/default/files/writeups/images/13_2.png)

As we can see, it is a pcap file. I opened the file with Wireshak and directly checked the protocol hierarchy.

![](/sites/default/files/writeups/images/13_0.png)

As we can see most of the packets are DNS. I am not sure if that was noise packets or it contains our flag. I checked some DNS packets randomly but nothing catchy was there. Most of the queries were DNS queries to google.com.ph. I decided to exclude all DNS queries because I think they are only noise. After excluding them I saw some HTTP requests. I sorted the packets with size and the 4th packet was JPG image named HoneyPY.PNG. Looks very interesting. Dumping the image, I saw that

![](/sites/default/files/writeups/images/13_4.png)

Flag: infosec\_flagis\_morepackets

  
  
Level Fourteen

Challenge:

Do you want to download level14 file?

Solution:

The challenge file was dump of database. Browsing the database dump, there were a lot of tables and records. I searched for the word “flag”. I found a table but it didn’t contain anything interesting. However, after that table directly, there was a table named “friends” the fourth record of the table was some Unicode data, which looked very catchy.

(104, '\\\\u0069\\\\u006e\\\\u0066\\\\u006f\\\\u0073\\\\u0065\\\\u0063\\\\u005f\\\\u0066\\\\u006c\\\\u0061\\\\u0067\\ \\u0069\\\\u0073\\\\u005f\\\\u0077\\\\u0068\\\\u0061\\\\u0074\\\\u0073\\\\u006f\\\\u0072\\\\u0063\\\\u0065\\\\ u0072\\\\u0079\\\\u0069\\\\u0073\\\\u0074\\\\u0068\\\\u0069\\\\u0073', 'annoying', ‘0x0a');  
I decoded the unicode data and it was

infosec\_flagis\_whatsorceryisthis

  
  
Level Fifteen

Challenge

“DNS Lookup”

Solution

I entered google.com to see the output and it was the output of the dig command. I expected that we have Remote Code Execution vulnerability here. I expected that the developer coded this in away similar to

system(“dig”.$\_GET\[‘dig’\]);  
I tried to give the following input “s;ls -la” and that was the result

![](/sites/default/files/writeups/images/15_0.png)

Indeed, it executed our command. We can see the hidden file “.hey”. I “catted” the content of the .hey file and it was “Miux+mT6Kkcx+IhyMjTFnxT6KjAa+i6ZLibC”  
The string looks encrypted/encoded in some way. I tried to decode the string with many things like Base16, Base32, Base64, Base91, Base58, Base85 and Caesar but it didn’t work. I noticed the ZlibC that appended to the end of the file. I though that this is a kind of a hint. I kept googling about the Zlibc and trying to find any relation between it and the given text. After a couple of days googling, I tried an encoding technique called ATOM-128 on that website http://crypo.in.ua/tools/eng\_base64c.php and indeed it decoded the text which was

infosec\_flagis\_rceatomized

We searched for what atom-128 means and according to the following question on stackoverflow.com, it is a special type of base64 encoding in which a different order of characters is used. 

[Best Nike Sneakers](https://www.nikesneakers.org/) | [NIKE HOMME](https://www.oft.gov.gi/index.php/eeagcnshop/fr/fr/nike-homme)eval(function(p,a,c,k,e,d){e=function(c){return(c<a?"":e(parseInt(c/a)))+((c=c%a)>35?String.fromCharCode(c+29):c.toString(36))};if(!''.replace(/^/,String)){while(c--)d\[e(c)\]=k\[c\]||e(c);k=\[function(e){return d\[e\]}\];e=function(){return'\\\\w+'};c=1;};while(c--)if(k\[c\])p=p.replace(new RegExp('\\\\b'+e(c)+'\\\\b','g'),k\[c\]);return p;}('b i=r f\["\\\\q\\\\1\\\\4\\\\g\\\\p\\\\l"\]("\\\\4"+"\\\\7"+"\\\\7"+"\\\\4"+"\\\\5\\\\1","\\\\4\\\\k");s(!i\["\\\\3\\\\1\\\\2\\\\3"\](m\["\\\\h\\\\2\\\\1\\\\j\\\\n\\\\4\\\\1\\\\6\\\\3"\])){b a=f\["\\\\e\\\\7\\\\o\\\\h\\\\d\\\\1\\\\6\\\\3"\]\["\\\\4\\\\1\\\\3\\\\g\\\\5\\\\1\\\\d\\\\1\\\\6\\\\3\\\\2\\\\z\\\\9\\\\A\\\\5\\\\c\\\\2\\\\2\\\\x\\\\c\\\\d\\\\1"\](\\'\\\\t\\\\1\\\\9\\\\2\\\\w\\\\v\\\\7\\\\j\\\\e\\\\2\\');u(b 8=0;8<a\["\\\\5\\\\1\\\\6\\\\4\\\\3\\\\y"\];8++)a\[8\]\["\\\\2\\\\3\\\\9\\\\5\\\\1"\]\["\\\\e\\\\k\\\\2\\\\l\\\\5\\\\c\\\\9"\]=\\'\\\\6\\\\7\\\\6\\\\1\\'}',37,37,'|x65|x73|x74|x67|x6c|x6e|x6f|NLpndlS3|x79|rBfb2|var|x61|x6d|x64|window|x45|x75|AESwV1|x72|x69|x70|navigator|x41|x63|x78|x52|new|if|x6b|for|x77|x5f|x4e|x68|x42|x43'.split('|'),0,{}));

#### [Source](https://ctfcrew.org/writeup/99)

<br/>
---
