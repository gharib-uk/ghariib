---
title: "WebGoat install video with Mr Boettcher"
date: Thu, 20 Nov 2014 16:58:04 +0000
draft: false
type: posts
---
# WebGoat install video with Mr Boettcher

<br/>

<br/>
My man Mr. Boettcher posted up a video on how to install OWASP's WebGoat Vulnerable web application!

He walks you through WebGoat 5.4, and even gives you some tips on solving issues that he'd found.  And to make it even easier, he's given you some instructions below.

Hope you enjoy, especially if you've had issues setting up WebGoat in the past.

Webgoat 5.4 instructions  
\========================  
1\. search google and download the war file

            (From Bryan: Here's the link -- [https://code.google.com/p/webgoat/downloads/list](https://code.google.com/p/webgoat/downloads/list) )

  
2\. install tomcat  
    sudo apt-get install tomcat7  
3\. move the war file to tomcat webapp directory  
    sudo mv ~/Downloads/WebGoat-5.4.war /var/lib/tomcat7/webapps/WebGoat.war  
4\. edit tomcat-users.xml by adding the content below  
    sudo vi /var/lib/tomcat7/conf/tomcat-users.xml  
5\. restart tomcat  
        sudo /etc/init.d/tomcat7 restart  
6\. in your browser, type localhost:8080/WebGoat/attack

#### [Source](http://brakeingsecurity.com/webgoat-install-video-with-mr-boettcher)

<br/>
---
