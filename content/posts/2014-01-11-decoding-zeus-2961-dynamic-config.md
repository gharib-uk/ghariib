---
title: "Decoding Zeus 2961 dynamic config"
date: Sat, 11 Jan 2014 17:18:00 +0000
draft: false
type: posts
categories: 
- [object Object],[object Object],[object Object],[object Object],[object Object]
---
# Decoding Zeus 2961 dynamic config

<br/>

<br/>
I got a look on the zeus builder who was released by the MMBB guy on exploit.in, finally i'm decided to write something about it, so let's talk about the change in the config encryption.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj-XfLKAVlZNV2wypPwWNQ2mNl57CEKvEfxb67CdkXKcKm-ngjohPADb2zSHU5EjnJ0k8ur6HokiF99t8ATvmiL1E0lcOQ6CdsnfcaM1jdg6cYLlyKVhHN1dknRHOXzDMWhhU7Y7OmQemM/s1600/11-01-2014+14-53-52.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj-XfLKAVlZNV2wypPwWNQ2mNl57CEKvEfxb67CdkXKcKm-ngjohPADb2zSHU5EjnJ0k8ur6HokiF99t8ATvmiL1E0lcOQ6CdsnfcaM1jdg6cYLlyKVhHN1dknRHOXzDMWhhU7Y7OmQemM/s1600/11-01-2014+14-53-52.png)

MD5: [0a05783316e7f765e731aadf5098564f](https://www.virustotal.com/en/file/382e34d713572348c76eb81313ead3066da307b5b7a3cede83484b7fb235b1dc/analysis/1388760471/)  
  
This version use AES instead of RC4 and can interact with the latest version of Firefox.  
Anyway it's nothing more than a basic Zeus v2.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhk8a2t8dQmhyHvjo4d9f-VvhRVfkbJCJQQsIN9cq9AfDjVdJ4oT6I2yF41TQMC1Ix2kdGDRCFmnuD7lnNWxOHe4RY2AZcvXXHG8waclvVYfcwlV60OzKWKgJkX6Hy4jF2qEH4eUJwQNx0/s1600/11-01-2014+15-02-10.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhk8a2t8dQmhyHvjo4d9f-VvhRVfkbJCJQQsIN9cq9AfDjVdJ4oT6I2yF41TQMC1Ix2kdGDRCFmnuD7lnNWxOHe4RY2AZcvXXHG8waclvVYfcwlV60OzKWKgJkX6Hy4jF2qEH4eUJwQNx0/s1600/11-01-2014+15-02-10.png)

  
iBank parser on the panel, monitoring of process:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi3qxT14Kjzd9oiAPFxcur2t5e8O_pxrn7v51Q6CRkwpiBEu8OdzJSAXNGq3TgFkfbuaZXCLrTUXXCcPtudliT6ZR46V_wwTsYE9S7W64abXKOG9WPd51ADDSYcv_oMplqESDPn9Ei-eAM/s1600/11-01-2014+15-13-33.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi3qxT14Kjzd9oiAPFxcur2t5e8O_pxrn7v51Q6CRkwpiBEu8OdzJSAXNGq3TgFkfbuaZXCLrTUXXCcPtudliT6ZR46V_wwTsYE9S7W64abXKOG9WPd51ADDSYcv_oMplqESDPn9Ei-eAM/s1600/11-01-2014+15-13-33.png)

About the panel, the released version require Ioncube loader (nvm, the gate code can be recovered easily)  
  
Now let's view an example of report from modules, keylog+screenshot:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj-8MCW6RCPLKtiX5wu2d_YGj9VDvid3Dawph5pBGv4jeVIF1sDmqqj0RVSsgY-r4WWtFg3VZ-MlAyKSF72J5pPNP4yhw7R6kH8wuYJufypRpAvhbEvs14oVQY02EcJ0q-4dokgTMLGdpc/s1600/11-01-2014+16-26-41.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj-8MCW6RCPLKtiX5wu2d_YGj9VDvid3Dawph5pBGv4jeVIF1sDmqqj0RVSsgY-r4WWtFg3VZ-MlAyKSF72J5pPNP4yhw7R6kH8wuYJufypRpAvhbEvs14oVQY02EcJ0q-4dokgTMLGdpc/s1600/11-01-2014+16-26-41.png)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh6Liz6o95NVOJFmlsuLtUo_jQq6vsGgKot4bEgs6q6WGiyx7rNEObOS0263h1AbpvxbMHkqSSd3L3HExm1Nz1mW9dtT5SMzX9sTk1TdLF_SoqODa690NXnv5EJYNhxtbTK55dD_YZTXWA/s1600/KScn_f.jpeg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh6Liz6o95NVOJFmlsuLtUo_jQq6vsGgKot4bEgs6q6WGiyx7rNEObOS0263h1AbpvxbMHkqSSd3L3HExm1Nz1mW9dtT5SMzX9sTk1TdLF_SoqODa690NXnv5EJYNhxtbTK55dD_YZTXWA/s1600/KScn_f.jpeg)

  
Part of the static config (in plain on generated bot):  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg-9zxnDuTzKeB_LlmCYddaA_WrOeZ0dUKcw_4MJvKQ3IC4haoS5OGqc_qZQAwjjt1FJW5Y5Zg1Mw-dX9yGLhMk2n6GIFcNLeOxo3Kr54gJPIFtyH4yaMz0E3dcYuZ5CDNXN9xu_cJ-okA/s1600/11-01-2014+16-48-22.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg-9zxnDuTzKeB_LlmCYddaA_WrOeZ0dUKcw_4MJvKQ3IC4haoS5OGqc_qZQAwjjt1FJW5Y5Zg1Mw-dX9yGLhMk2n6GIFcNLeOxo3Kr54gJPIFtyH4yaMz0E3dcYuZ5CDNXN9xu_cJ-okA/s1600/11-01-2014+16-48-22.png)

  
Installation process/dynamic config decoding (beware, dubstep):  

  
And a small code because it's easier to understand:  

<?php  
    function decode($data, $key) {  
        $td \= [mcrypt\_module\_open](http://www.php.net/mcrypt_module_open)(MCRYPT\_RIJNDAEL\_128, '', MCRYPT\_MODE\_ECB, '');  
        $iv \= [mcrypt\_create\_iv](http://www.php.net/mcrypt_create_iv)([mcrypt\_enc\_get\_iv\_size](http://www.php.net/mcrypt_enc_get_iv_size)($td), MCRYPT\_RAND);  
         
        [mcrypt\_generic\_init](http://www.php.net/mcrypt_generic_init)($td, $key, $iv);  
        [mcrypt\_generic](http://www.php.net/mcrypt_generic)($td, $data);  
         
        $data \= [mdecrypt\_generic](http://www.php.net/mdecrypt_generic)($td, $data);  
         
        [mcrypt\_generic\_deinit](http://www.php.net/mcrypt_generic_deinit)($td);  
        [mcrypt\_module\_close](http://www.php.net/mcrypt_module_close)($td);  
         
        return $data;  
    }  
     
    function visualDecrypt(&$data) {  
        $len \= [strlen](http://www.php.net/strlen)($data);  
         
        if ($len \> 0)  
            for ($i \= $len \- 1; $i \> 0; $i\--)  
                $data\[$i\] \= [chr](http://www.php.net/chr)([ord](http://www.php.net/ord)($data\[$i\]) ^ [ord](http://www.php.net/ord)($data\[$i \- 1\]));  
    }  
     
    $data    \= [file\_get\_contents](http://www.php.net/file_get_contents)('config.bin');  
    $key     \= [md5](http://www.php.net/md5)('hasd7h12g1', true);  
    $decoded \= decode($data, $key);  
     
    visualDecrypt($decoded);  
     
    $size \= [strlen](http://www.php.net/strlen)($decoded);  
     
    [header](http://www.php.net/header)('Content-Type: application/octet-stream;');  
    [header](http://www.php.net/header)('Content-Transfer-Encoding: binary');  
    [header](http://www.php.net/header)('Content-Length: ' . $size);  
    [header](http://www.php.net/header)('Content-Disposition: attachment; filename=config\_decrypted.dll');  
    [header](http://www.php.net/header)('Expires: 0');  
    [header](http://www.php.net/header)('Cache-Control: no-cache, must-revalidate');  
    [header](http://www.php.net/header)('Pragma: no-cache');  
     
    echo($decoded);  
     
    [exit](http://www.php.net/exit);  
?>

  
You can find the decoded modules here:  
JAVA: [7d7ae6ffbd9f3c7673b339f9b94493e5](https://www.virustotal.com/en/file/d8e3cad3a831ef3325c6fdf1640fa09b9f9ae9c1caff7f0aff5196057a98fd09/analysis/1389194044/)  
BSS: [cc98dabebe047c6115a6cd9d13ed3122](https://www.virustotal.com/en/file/2b7e5567984217e9c484896baed2df083ceea98d45d003c4a96775cf7d7d8694/analysis/1389194046/)  
KEYLOG: [8ac1c7c019d16ff3b8a9543d46ae5e0e](https://www.virustotal.com/en/file/5ca8eaf746881093764c4ace675e2c935a35c87eb9411b95b53d78f09f93d7cb/analysis/1389194047/)  
  
And if you want to test yourself the WebInject, i usually use this code:  

set\_url http://requesttests.appspot.com\* GP  
data\_before  
</body>  
data\_end  
  
data\_inject  
<center><img src="http://temari.fr/webinject.png" alt="Injected!"></center>  
data\_end  
  
data\_after  
data\_end

  
  
  
  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhnZnX5v7RW6O0yMHcjUlPtcPi9cPc5IC6sG_AcHR8kJnQFhXumhSO2TU-SXT7agRTiBCQW8CplaKIjjJXk9pqrzslkRY-P05SdoqfvQKc6wABDsTqoIpuz04ERGw30C1NSOsnsIvfaUSE/s1600/11-01-2014+18-12-53.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhnZnX5v7RW6O0yMHcjUlPtcPi9cPc5IC6sG_AcHR8kJnQFhXumhSO2TU-SXT7agRTiBCQW8CplaKIjjJXk9pqrzslkRY-P05SdoqfvQKc6wABDsTqoIpuz04ERGw30C1NSOsnsIvfaUSE/s1600/11-01-2014+18-12-53.png)

/facepalm

#### [Source](https://www.xylibox.com/2014/01/decoding-zeus-2961-config.html)

<br/>
---
