---
title: "BSidesPDX CTF SeaQuell"
date: Sat, 21 Oct 2017 21:37:00 +0000
draft: false
type: posts
categories: 
- 
---
# BSidesPDX CTF SeaQuell

<br/>

<br/>
**Category:** Web

**Points:** 200

**Description:**

```
Our competitors at SeaQuell have uploaded their latest proprietary data to their employee area. We have already compromised their web developer and obtained the source code to their site, here: seaquell.py

Host: a32fcd6eab2d811e784db0a6f99bb55a-829124630.eu-central-1.elb.amazonaws.com

Port: 1589

Seaquell.py

```

Note
====

The BSidesPDX organizers have made the source code for all of their challenges freely available so that you can run them at home and follow along. You can find more information [here](https://github.com/BSidesPDX/CTF-2017).

Investigation
=============

At the beginning of the challenge you are given the source for the website, and you know ou have to attempt to login to the employees-only area. Since we’ve got the source, we can see how the SQL queries are created for the login and craft an attack against that query.

```
seaquell.py:31: cur.execute('SELECT password FROM employees WHERE username == "'+user+'"')
```

By injecting `" or 1=1 --` into the username field and supplying a random password, we got a result in one of the HTTP headers that gave us a descriptive error. `Invalid user: "" or 1=1 --" (got [(u'pirat3',), (u'drillBabyDrill',)])`

Now we’ve got passwords, since the SQL query was selecting passwords where a username matched and we inserted a `1=1` which always evaluates true. Next we need to get the usernames associated with these passwords. Nothing a little union select can’t handle. User `" union select username from employees--` with a random password returns us a list of passwords in the same HTTP header error message. `Invalid user: "" union select username from employees--" (got [(u'sparrowj',), (u'tillersonr',)])`

Once we’ve got the username password pairs, we login and there’s a link to the flag. Yes! We’ve got it!

```
<h1>403</h1>Not Authorized
```

I guess not. Let’s take another look at the source and see what’s causing this.

```
        if ('..' in self.path or 
           not (self.path.endswith('.html') or
                self.path.endswith('/') or
                self.path.endswith('.jpg'))):
           self.send_response(403)
           self.end_headers()
           self.wfile.write('<h1>403</h1>Not Authorized')
```

So it looks like if we try to do any directory traversals we’ll get a 403, and if we don’t end with `.html`, `/`, or `.jpg` we’ll also get a 403. Luckily this is not how you properly evaluate the extension of a file in a URL, so we can abuse it to get the flag.

Solution
========

Send your get request but append `?.html` to the end of the flag url so that it looks like `/employees-only/flag?.html`. Now you’ll receive the flag because `self.path.endswith('.html')` evaluates to `True`

Flag
====

**BSidesPDX{7h3\_se4w33d\_i5\_alw4s\_gr33n3r\_wh3n\_y0u\_hav3\_cr3ds}**

#### [Source](http://b0tchsec.com/2017/bsidespdx/seaquell)

<br/>
---
