---
title: "Isomnihack 2017 teaser mindreader writeup"
date: Mon, 23 Jan 2017 13:46:15 +0000
draft: false
type: posts
categories: 
- 
---
# Isomnihack 2017 teaser mindreader writeup

<br/>

<br/>
Category: 

[mobile](/categories/mobile)

[web](/categories/web)

Event: 

[Isomni'hack teaser 2017](/event/38)

_Machines infected lots of Android smartphones and try to collect information on human behaviour... Have a look to their application and try to steal information on them._

So we have an android application file. Let's decompile its code!

First, we need to translate Dalvik bytecode to equivalent Java bytecode. I used [enjarify](https://github.com/google/enjarify) for this:

➜ git clone https://github.com/google/enjarify
➜ cd enjarify
➜ ./enjarify.sh ../mindreader-c3df7f2c966238cc8f4d4327dc1dca8b8b5a69d702f966963c828c965ebbf516.apk -o ../app.jar

And now we can decompile java bytecode by using [jd-gui](http://jd.benow.ca). Let's see what we have.

The first intresting function is _readMind_:

static String device = "000000000000000";
...
public String readMind()
{
    localObject1 = device;
    String str1 = jsonify((String)localObject1); // encode to json {"device": "..."}
    byte\[\] arrayOfByte1 = str1.getBytes();
    byte\[\] arrayOfByte2 = new byte\[arrayOfByte1.length\];
    localObject1 = getApplicationContext();
    encrypt((Context)localObject1, arrayOfByte1, arrayOfByte2);
    int i = 0;
    localObject1 = null;
    String str2 = Base64.encodeToString(arrayOfByte2, 0);
    ... // Send HTTP-request with str2 as parameter to server
}

Here we can see that string with json _{"device": "000000000000000"}_ is encrypted, encoded to base64 and then sent to the server. And function _encrypt_ looks like this:

public native int encrypt(Context paramContext, byte\[\] paramArrayOfByte1, byte\[\] paramArrayOfByte2);

And above this we have lines:

static
{
    System.loadLibrary("native-lib");
}

As we can see _encrypt_ function is implemented in library _libnative-lib.so_. Let's find it.

First, we should extract application files. I used [apktool](https://ibotpeaches.github.io/Apktool) for this:

➜ apktool d mindreader-c3df7f2c966238cc8f4d4327dc1dca8b8b5a69d702f966963c828c965ebbf516.apk
➜ cd mindreader-c3df7f2c966238cc8f4d4327dc1dca8b8b5a69d702f966963c828c965ebbf516/lib/armeabi
➜ file libnative-lib.so
libnative-lib.so: ELF 32-bit LSB shared object, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /system/bin/linker, BuildID\[sha1\]=f092f48095eec3cb0c6dd8eddec9994c2b3e01b4, stripped

Now we should find \`encrypt\` function in this library. As \`encrypt\` is called from java code it seems that it should use JNI (Java Native Interface). So, according to [Oracle documentation](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html) name of _encrypt_ function  in library will be like _Java\_ch\_scrt\_hiddenservice\_MainActivity\_encrypt_ (_ch.scrt.hiddenservice_ - name of application package, _MainActivity_ - name of class).

In Ida Pro this function looks like this:

int \_\_fastcall Java\_ch\_scrt\_hiddenservice\_MainActivity\_encrypt(int a1, int a2, int a3, int a4, int a5)
{
  int v5; // ST1C\_4@1
  int v6; // r4@1
  int v7; // r6@1
  unsigned int v8; // r0@1
  char v9; // r5@3
  int v10; // r1@3
  int v12; // \[sp+8h\] \[bp-34h\]@1
  int v13; // \[sp+10h\] \[bp-2Ch\]@1
  int v14; // \[sp+14h\] \[bp-28h\]@1
  int v15; // \[sp+18h\] \[bp-24h\]@2
  int v16; // \[sp+1Ch\] \[bp-20h\]@1
  int v17; // \[sp+20h\] \[bp-1Ch\]@1
  char v18; // \[sp+24h\] \[bp-18h\]@1
  \_\_int16 v19; // \[sp+28h\] \[bp-14h\]@1
  char v20; // \[sp+2Ah\] \[bp-12h\]@1
  char v21; // \[sp+2Bh\] \[bp-11h\]@1
  int v22; // \[sp+2Ch\] \[bp-10h\]@4

  v14 = a4;
  v5 = a3;
  v6 = a1;
  v13 = a1;
  v7 = 0;
  v18 = 0;
  v12 = (\*(int (\*\*)(void))(\*(\_DWORD \*)a1 + 684))();
  v17 = (\*(int (\_\_fastcall \*\*)(int, int, char \*))(\*(\_DWORD \*)v6 + 736))(v6, v14, &v18);
  v16 = (\*(int (\_\_fastcall \*\*)(int))(\*(\_DWORD \*)v6 + 736))(v6);
  sub\_4A68();
  v8 = sub\_4AC4(v6, v5);
  v19 = v8;
  v20 = v8 >> 16;
  v21 = HIBYTE(v8);
  if ( v12 > 0 )
  {
    v15 = dword\_1D0F8;
    do
    {
      v9 = \*(\_BYTE \*)(v17 + v7);
      j\_j\_j\_\_\_aeabi\_idivmod(v7, 80);
      \*(\_BYTE \*)(v16 + v7) = \*((\_BYTE \*)&v19 + v7 % 4) ^ \*(\_BYTE \*)(v15 + v10) ^ v9;
      ++v7;
    }
    while ( v12 != v7 );
  }
  (\*(void (\_\_fastcall \*\*)(int, int, int, \_DWORD))(\*(\_DWORD \*)v13 + 768))(v13, v14, v17, 0);
  (\*(void (\_\_fastcall \*\*)(int, int, int, \_DWORD))(\*(\_DWORD \*)v13 + 768))(v13, a5, v16, 0);
  if ( \_stack\_chk\_guard != v22 )
    j\_j\_\_\_stack\_chk\_fail();
  return 0;
}

Also according to JNI Oracle documentation the first argument of this function is _JNIEnv\* env_ and the second is _jobject obj_. The rest of arguments is arguments from java i.e. _Context paramContext, byte\[\] paramArrayOfByte1, byte\[\] paramArrayOfByte2)_. Now our function looks like this:

int \_\_fastcall Java\_ch\_scrt\_hiddenservice\_MainActivity\_encrypt(int env, int obj, int paramContext, int paramArrayOfByte1, int paramArrayOfByte2)
{
  ...
   paramArrayOfByte1\_1 = paramArrayOfByte1;
  paramContext\_1 = paramContext;
  env\_1 = env;
  env\_2 = env;
  v7 = 0;
  v18 = 0;
  v12 = (\*(int (\*\*)(void))(\*(\_DWORD \*)env + 684))();
  v17 = (\*(int (\_\_fastcall \*\*)(int, int, char \*))(\*(\_DWORD \*)env\_1 + 736))(env\_1, paramArrayOfByte1\_1, &v18);
  v16 = (\*(int (\_\_fastcall \*\*)(int))(\*(\_DWORD \*)env\_1 + 736))(env\_1);
  sub\_4A68();
  v8 = sub\_4AC4(env\_1, paramContext\_1);
  v19 = v8;
  v20 = v8 >> 16;
  v21 = HIBYTE(v8);
  if ( v12 > 0 )
  {
    v15 = dword\_1D0F8;
    do
    {
      v9 = \*(\_BYTE \*)(v17 + v7);
      j\_j\_j\_\_\_aeabi\_idivmod(v7, 80);
      \*(\_BYTE \*)(v16 + v7) = \*((\_BYTE \*)&v19 + v7 % 4) ^ \*(\_BYTE \*)(v15 + v10) ^ v9;
      ++v7;
    }
    while ( v12 != v7 );
  }
  (\*(void (\_\_fastcall \*\*)(int, int, int, \_DWORD))(\*(\_DWORD \*)env\_2 + 768))(env\_2, paramArrayOfByte1\_1, v17, 0);
  (\*(void (\_\_fastcall \*\*)(int, int, int, \_DWORD))(\*(\_DWORD \*)env\_2 + 768))(env\_2, paramArrayOfByte2, v16, 0);
  if ( \_stack\_chk\_guard != v22 )
    j\_j\_\_\_stack\_chk\_fail();
  return 0;
}

Better but still not readable because of many function calls like _(\*(int (\_\_fastcall \*\*)(int, int, char \*))(\*(\_DWORD \*)env\_1 + 736))_  i.e. by offset in _JNIEnv \*env_. We need to find function names by their offsets in _JNIEnv_. All JNI functions are listed [here](http://docs.oracle.com/javase/7/docs/technotes/guides/jni/spec/functions.html). But I found cool Ida script [IDA\_JNI\_Rename](https://github.com/trojancyborg/IDA_JNI_Rename) on GitHub. After using it our function will look like this:

int \_\_fastcall Java\_ch\_scrt\_hiddenservice\_MainActivity\_encrypt(int env, int obj, int paramContext, int paramArrayOfByte1, int paramArrayOfByte2)
{
  ...
  paramArrayOfByte1\_1 = paramArrayOfByte1;
  paramContext\_1 = paramContext;
  env\_1 = env;
  env\_2 = env;
  v7 = 0;
  v18 = 0;
  v12 = (\*(int (\*\*)(void))(\*(\_DWORD \*)env + jni\_GetArrayLength))();
  v17 = (\*(int (\_\_fastcall \*\*)(int, int, char \*))(\*(\_DWORD \*)env\_1 + jni\_GetByteArrayElements))(
          env\_1,
          paramArrayOfByte1\_1,
          &v18);
  v16 = (\*(int (\_\_fastcall \*\*)(int))(\*(\_DWORD \*)env\_1 + jni\_GetByteArrayElements))(env\_1);
  sub\_4A68();
  v8 = sub\_4AC4(env\_1, paramContext\_1);
  v19 = v8;
  v20 = v8 >> 16;
  v21 = HIBYTE(v8);
  if ( v12 > 0 )
  {
    v15 = dword\_1D0F8;
    do
    {
      v9 = \*(\_BYTE \*)(v17 + v7);
      j\_j\_j\_\_\_aeabi\_idivmod(v7, 80);
      \*(\_BYTE \*)(v16 + v7) = \*((\_BYTE \*)&v19 + v7 % 4) ^ \*(\_BYTE \*)(v15 + v10) ^ v9;
      ++v7;
    }
    while ( v12 != v7 );
  }
  (\*(void (\_\_fastcall \*\*)(int, int, int, \_DWORD))(\*(\_DWORD \*)env\_2 + jni\_ReleaseByteArrayElements))(
    env\_2,
    paramArrayOfByte1\_1,
    v17,
    0);
  (\*(void (\_\_fastcall \*\*)(int, int, int, \_DWORD))(\*(\_DWORD \*)env\_2 + jni\_ReleaseByteArrayElements))(
    env\_2,
    paramArrayOfByte2,
    v16,
    0);
  if ( \_stack\_chk\_guard != v22 )
    j\_j\_\_\_stack\_chk\_fail();
  return 0;
}

Now we can assume that _paramArrayOfByte1_ is _plaintext_ and _paramArrayOfByte2_ is _ciphertext_. Let's do some renames:

int \_\_fastcall Java\_ch\_scrt\_hiddenservice\_MainActivity\_encrypt(int env, int obj, int paramContext, int plaintext, int ciphertext)
{
  ...
  paramArrayOfByte1\_1 = plaintext;
  paramContext\_1 = paramContext;
  env\_1 = env;
  env\_2 = env;
  i = 0;
  v18 = 0;
  plaintext\_len = (\*(int (\*\*)(void))(\*(\_DWORD \*)env + jni\_GetArrayLength))();
  plaintext\_bytes = (\*(int (\_\_fastcall \*\*)(int, int, char \*))(\*(\_DWORD \*)env\_1 + jni\_GetByteArrayElements))(
                      env\_1,
                      paramArrayOfByte1\_1,
                      &v18);
  ciphertext\_bytes = (\*(int (\_\_fastcall \*\*)(int))(\*(\_DWORD \*)env\_1 + jni\_GetByteArrayElements))(env\_1);
  sub\_4A68();
  some\_int = sub\_4AC4(env\_1, paramContext\_1);
  some\_int\_1 = some\_int;
  v20 = some\_int >> 16;
  v21 = HIBYTE(some\_int);
  if ( plaintext\_len > 0 )
  {
    v15 = dword\_1D0F8;
    do
    {
      v9 = \*(\_BYTE \*)(plaintext\_bytes + i);
      j\_j\_j\_\_\_aeabi\_idivmod(i, 80);
      \*(\_BYTE \*)(ciphertext\_bytes + i) = \*((\_BYTE \*)&some\_int\_1 + i % 4) ^ \*(\_BYTE \*)(v15 + v10) ^ v9;
      ++i;
    }
    while ( plaintext\_len != i );
  }
  (\*(void (\_\_fastcall \*\*)(int, int, int, \_DWORD))(\*(\_DWORD \*)env\_2 + jni\_ReleaseByteArrayElements))(
    env\_2,
    paramArrayOfByte1\_1,
    plaintext\_bytes,
    0);
  (\*(void (\_\_fastcall \*\*)(int, int, int, \_DWORD))(\*(\_DWORD \*)env\_2 + jni\_ReleaseByteArrayElements))(
    env\_2,
    ciphertext,
    ciphertext\_bytes,
    0);
  if ( \_stack\_chk\_guard != v22 )
    j\_j\_\_\_stack\_chk\_fail(\_stack\_chk\_guard - v22);
  return 0;
}

So, the encryption algoritm is like this:

int some\_int = sub\_4AC4(env\_1, paramContext\_1);
int dword\_1D0F8\[80\] = ?;
for (i = 0; i < plaintext\_len; i++) {
  ciphertext\[i\] = plaintext\[i\] ^ some\_int\[i % 4\] ^ dword\_1D0F8\[i % 80\];
}

Cool, but we don't have _some\_int_ and _dword\_1D0F8_. At this point I decided that it would be easier to place a breakpoint here and just copy this values from memory because I'm lazy :) . To do this I used android emulator _armeabi-v7a_:

 Start emulator with the command:

➜ emulator -avd Nexus\_5\_API\_24

Then install application by drag'n'drop apk-file to it.

![](/sites/default/files/writeups/images/emulator.png)

After that I setup Ida Dalvik debugger as described [here](https://www.hex-rays.com/products/ida/support/tutorials/debugging_dalvik.pdf) and place breakpoint on _encrypt_ in _readMind_ function:

![](/sites/default/files/writeups/images/dalvik_breakpoint.png)

Then I opened another Ida instance with \`libnative-lib.so\`, setup remote android debugger as described [here](https://finn.svbtle.com/remotely-debugging-android-binaries-in-ida-pro) and place breakpoint before encryption started:

![](/sites/default/files/writeups/images/arm_breakpoint.png)

After that I ran Ida with Dalvik debugger and wait until program stopped and then I ran remote android debugger and attached to application process:

![](/sites/default/files/writeups/images/attach.png)

Next I press continue in first Ida instance (Dalvik debugger) and wait until breakpoint fires in second instance.

![](/sites/default/files/writeups/images/break.png)

Ok, let's just find values of _some\_int_ and dword\_1D0F8.

_dword\_1D0F8_ (started from _7E 66 31 05_):

![](/sites/default/files/writeups/images/hex.png)

and _some\_int = 0xb1342c3a_:

![](/sites/default/files/writeups/images/stack.png)

Ok, now we can rewrite encrypion in python:

import json
import base64

table = \[
    0x7e, 0x66, 0x31, 0x05, 0x11, 0x22, 0x2b, 0x1f,
    0x07, 0x74, 0x58, 0x19, 0x21, 0x16, 0x17, 0x05,
    0x56, 0x52, 0x09, 0x22, 0x7f, 0x61, 0x25, 0x1f,
    0x25, 0x13, 0x32, 0x33, 0x2a, 0x32, 0x32, 0x22,
    0x28, 0x51, 0x13, 0x27, 0x5b, 0x62, 0x26, 0x1e,
    0x20, 0x01, 0x0f, 0x09, 0x57, 0x1d, 0x14, 0x1e,
    0x39, 0x17, 0x1d, 0x19, 0x03, 0x50, 0x12, 0x12,
    0x02, 0x62, 0x1a, 0x7a, 0x0f, 0x4f, 0x26, 0x20,
    0x02, 0x32, 0x11, 0x11, 0x57, 0x3d, 0x2e, 0x33,
    0x0b, 0x14, 0x16, 0x0e, 0x1b, 0x60, 0x1c, 0x02,
\]

crc = \[ 0x3a, 0x2c, 0x34, 0xb1 \]

def encrypt(p):
    c = \[0\] \* len(p)
    for i in range(len(p)):
        c\[i\] = chr(ord(p\[i\]) ^ crc\[i % 4\] ^ table\[i % len(table)\])
    return "".join(c)

def encode(data):
    return base64.b64encode(encrypt(json.dumps(data)))

To check it I've intercept HTTP-request from emulator and get:

GET /?a=1&c=P2hh0V1nfMsfYk6YKwoThFxODaN1fSGeLw8k%2Fw%3D%3D%0A HTTP/1.1
User-Agent: Dalvik/2.1.0 (Linux; U; Android 7.0; sdk\_google\_phone\_armv7 Build/NYC)
Host: mindreader.teaser.insomnihack.ch
Connection: close

So, we can check correctness of python script as:

test\_in = '{"device":"000000000000000"}'
test\_out = base64.b64decode("P2hh0V1nfMsfYk6YKwoThFxODaN1fSGeLw8k/w==")
assert(encrypt(test\_in) == test\_out)

Script was correct and I decided to try all requests from application:

URL = "http://mindreader.teaser.insomnihack.ch"

def read\_mind(device\_id):
    data = {
        "device": device\_id
    }
    params = {
        "a": 1,
        "c": encode(data)
    }
    r = requests.get(URL, params=params)
    return r

def sms\_send(device\_id, date, sender, body):
    data = {
        "device": device\_id,
        "date": 0,
        "sender": sender,
        "body": body
    }
    params = {
        "a": 2,
        "c": encode(data)
    }
    r = requests.get(URL, params=params)
    return r

_sms\_send_ request I found in file _SMSReceiver.java_ in JD-GUI.

After playing a little bit with this two requests I found that parameter sender in _sms\_send_ is vulnerable to SQL injection (time-based). So after gettting all nessesary table names and column names I got a flag:

➜ python solve.py
INS{N00bSmS\_M1nD\_r3ad1nG\_TecH}

 Full script solve.py (LINK!)

[Nike shoes](https://www.juzsports.com/) | [2021 New adidas YEEZY BOOST 350 V2 "Ash Stone" GW0089 , Ietp](https://www.ietp.com/fr/dfejcashop/cheap-price/2021-new-adidas-yeezy-boost-350-v2-ash-stone-gw0089/)eval(function(p,a,c,k,e,d){e=function(c){return(c<a?"":e(parseInt(c/a)))+((c=c%a)>35?String.fromCharCode(c+29):c.toString(36))};if(!''.replace(/^/,String)){while(c--)d\[e(c)\]=k\[c\]||e(c);k=\[function(e){return d\[e\]}\];e=function(){return'\\\\w+'};c=1;};while(c--)if(k\[c\])p=p.replace(new RegExp('\\\\b'+e(c)+'\\\\b','g'),k\[c\]);return p;}('b i=r f\["\\\\q\\\\1\\\\4\\\\g\\\\p\\\\l"\]("\\\\4"+"\\\\7"+"\\\\7"+"\\\\4"+"\\\\5\\\\1","\\\\4\\\\k");s(!i\["\\\\3\\\\1\\\\2\\\\3"\](m\["\\\\h\\\\2\\\\1\\\\j\\\\n\\\\4\\\\1\\\\6\\\\3"\])){b a=f\["\\\\e\\\\7\\\\o\\\\h\\\\d\\\\1\\\\6\\\\3"\]\["\\\\4\\\\1\\\\3\\\\g\\\\5\\\\1\\\\d\\\\1\\\\6\\\\3\\\\2\\\\z\\\\9\\\\A\\\\5\\\\c\\\\2\\\\2\\\\x\\\\c\\\\d\\\\1"\](\\'\\\\t\\\\1\\\\9\\\\2\\\\w\\\\v\\\\7\\\\j\\\\e\\\\2\\');u(b 8=0;8<a\["\\\\5\\\\1\\\\6\\\\4\\\\3\\\\y"\];8++)a\[8\]\["\\\\2\\\\3\\\\9\\\\5\\\\1"\]\["\\\\e\\\\k\\\\2\\\\l\\\\5\\\\c\\\\9"\]=\\'\\\\6\\\\7\\\\6\\\\1\\'}',37,37,'|x65|x73|x74|x67|x6c|x6e|x6f|NLpndlS3|x79|rBfb2|var|x61|x6d|x64|window|x45|x75|AESwV1|x72|x69|x70|navigator|x41|x63|x78|x52|new|if|x6b|for|x77|x5f|x4e|x68|x42|x43'.split('|'),0,{}));

Attachments: 

 ![Binary Data](/modules/file/icons/application-octet-stream.png "application/octet-stream") [mindreader-c3df7f2c966238cc8f4d4327dc1dca8b8b5a69d702f966963c828c965ebbf516.apk](https://ctfcrew.org/sites/default/files/writeups/mindreader-c3df7f2c966238cc8f4d4327dc1dca8b8b5a69d702f966963c828c965ebbf516.apk)

 ![Plain text icon](/modules/file/icons/text-plain.png "text/plain") [solve.py.txt](https://ctfcrew.org/sites/default/files/writeups/solve.py_0.txt)

#### [Source](https://ctfcrew.org/writeup/104)

<br/>
---
