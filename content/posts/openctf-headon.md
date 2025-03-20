---
title: "OpenCTF : HeadOn"
date: 2025-01-15
categories: 
  - "general"
---

**Category:** Forensics

**Points:** 50

**Description:**

```
Apply directly to the console
https://scoreboard.openctf.com/HeadOn-ac8890852965d787f7591bc10add61bb01efb5eb
```

**File Download:** HeadOn-ac8890852965d787f7591bc10add61bb01efb5eb

```
$ file blob 
blob: data
```

I also try running `strings` on the file, but I don’t find anything interesting.

Let’s try binwalk. If you have never heard of binwalk, it is a fantastic tool for solving forensics challenges. It goes through files of any type, looking for known magic cookies to help identify when one file type is embedded in another. For example, you may be given an ELF file, that has a jpeg embedded in it, binwalk will be able to find it, assuming the file is not encoded or encrypted in some way.

```
$ binwalk blob 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
9719          0x25F7          End of Zip archive
```

So the file contains the end of the zip, not the beginning. I then think about the challenge name and descrition, and realize `HeadOn` is referring to the file header. So while I could have pulled up the zip file specification documentation, this is a CTF, and solving the challenge quickly is most important. So I create a quick zip file so that I can compare the header of the the valid zip file with the invalid zip file header.

```
$ zip tmp.zip blob
```

Valid Zip Header (`tmp.zip`):

```
00000000: 504b 0304 1400 0000 0800 195e 044d ea3e  PK.........^.M.>
```

Invalid Zip Header (`blob`):

```
00000000: 0000 0000 1400 0000 0800 345b 044d 4921  ..........4[.MI!
```

Ok, so immediately I notice that the valid zip file starts with the bytes, `504b 0304` while the invalid zip file starts with `0000 0000`. So simply use your favorite hex-editor, to change the first 4 bytes of `blob` to match with the first 4 bytes of `tmp.zip`. I used sublime-text, but I’ll also include a few lines of Python that can take care of this.

```
with open('blob', 'rb') as f:
    f.read(4)
    data = f.read()
patched_data = 'x50x4bx03x04' + data
with open('blob_patched.zip', 'wb') as f:
    f.write(patched_data)
```

Now extract the zip file.

```
$ unzip blob_patched.zip 
Archive:  blob_patched.zip
  inflating: flag.pdf
```

And open flag.pdf in your preferred pdf viewer.

![](https://i.imgur.com/GWjZU9n.png)

# Flag

**Flag{SDG7qJ734rIw6f3f90832r}**

Go to Source
