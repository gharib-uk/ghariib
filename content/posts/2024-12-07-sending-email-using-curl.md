---
title: "Sending Email Using Curl"
date: Sat, 07 Dec 2024 17:00:35 GMT
draft: false
type: posts
---
# Sending Email Using Curl

<br/>

<br/>
I have a Gitlab runner which runs every day, and sometimes builds and pushes a new docker image if a new version of some software has been released. I wanted to update it to send me an email after pushing an image. I didn’t find any options in Dockerhub or
<br/>
I have a Gitlab runner which runs every day, and sometimes builds and pushes a new docker image if a new version of some software has been released. I wanted to update it to send me an email after pushing an image. I didn’t find any options in Dockerhub or Gitlab to do this. Dockerhub lets you call out to a webhook, so I guess I could do something with IFTTT or similar but I really didn’t want to have to sign up to another service to handle this.

Turns out curl can be used to send an email, and it’s pretty trivial. Curl is already installed inside my gitlab runner because I needed it as part of the build. I already have an authenticated SMTP server:

```
curl --ssl-reqd \
     --url       "smtps://mail.example.com:465" \
     --user      "username:password" \
     --mail-from "postmaster@example.com" \
     --mail-rcpt "postmaster@example.com" \
     --upload-file <(echo -e "Subject: New $IMAGE_NAME:$IMAGE_VERSION tag pushed\n\nSee subject. Sent from gitlab runner")
```

I used environment variables configured in gitlab for the creds, sender and rcpt email rather than hardcoding them in a public git repo, for obvious reasons.

Then I waited. I wasn’t sure if this would work initially as for all I knew gitlab would block outgoing port 465 from it’s runners. It doesn’t though. The next time a new version of sqlite3 was released I received the email alerting me to that fact.

#### [Source](https://www.grepular.com/Sending_Email_Using_Curl)

<br/>
---
