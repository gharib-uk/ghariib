---
title: "OpenCTF : Do The Needful"
date: 2025-03-19
categories: 
  - "general"
---

**Category:** Forensics

**Points:** 50

**Description:**

```
Do the needful
https://scoreboard.openctf.com/DoTheNeedful-98e4c6ba71f88e4201a08e7503b0df6124607e39
```

**File Download:** DoTheNeedful-98e4c6ba71f88e4201a08e7503b0df6124607e39

When we extract this file, we end up with `Challenge.txt`. So I go ahead and cat it.

```
$ cat Challenge.txt 
=AAAAMjU/o7Z+0V17r06KDNmaZHQB1VSlR7wsTDuNk1ok3wfRPMl5YAAV/DwDzAIAERyH3wAAsVVGNBAIs4H
```

This looks like a base64 string, however, with base64 encoding, the `=` character is used as padding and should only show up at the end of a base64 string, if at all. So let’s try and reverse the string. I wrote a quick Python script for this, and write the result to a file.

```
from base64 import b64decode

# Read the file
with open('Challenge.txt', 'rb') as f:
    data = f.read().strip()

# Reverse the string
data = data[::-1]

# Decode
data = b64decode(data)

# Write to a file
with open('b64_decode.raw', 'wb') as f:
    f.write(data)
```

Now let’s see what kind of file the resulting base64 is.

```
$ file b64_decode.txt 
b64_decode.txt: gzip compressed data, last modified: Mon Jul 23 03:05:55 2018, from Unix
```

Ok, looks like a gzip. Let’s extract it!

```
$ mv b64_decode.txt b64_decode.gz
$ gzip -d b64_decode.gz
$ file b64_decode 
b64_decode: ASCII text
```

So it’s ASCII, maybe it’s the flag!

```
$ cat b64_decode 
466c61677b6577373332386866386573676839663233677d0a
```

Hmm, looks like hex encoding. I just run a simple one-liner in the Python interpreter to decode this.

```
>>> '466c61677b6577373332386866386573676839663233677d0a'.decode('hex')
'Flag{ew7328hf8esgh9f23g}n'
```

# Flag

**Flag{ew7328hf8esgh9f23g}**
