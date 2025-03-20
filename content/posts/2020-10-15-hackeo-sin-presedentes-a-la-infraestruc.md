---
title: "Hackeo sin presedentes a la infraestructura TI en Chile Clave nica Gobierno Digital Comisara Virtual Segpres y ms han sido vulnerados"
date: Thu, 15 Oct 2020 03:19:00 -0300
draft: false
type: posts
categories: 
- Chilean Way,Hacking,Leaks,Noticias
---
# Hackeo sin presedentes a la infraestructura TI en Chile Clave nica Gobierno Digital Comisara Virtual Segpres y ms han sido vulnerados

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi2KX0TVSj5khjkbtQV4d18qQzMv4jO4S-bt5WCgpeIWoRuSlhumZqJZVfMOzhm_HMyyA_ylVOIjNoEoNCFrsvvJvxnQOzZG2Gdm9OIppz-vqQHr668K_CHOsUxOlXWGHUTGeMxZoNaCs6g/w640-h288/header.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi2KX0TVSj5khjkbtQV4d18qQzMv4jO4S-bt5WCgpeIWoRuSlhumZqJZVfMOzhm_HMyyA_ylVOIjNoEoNCFrsvvJvxnQOzZG2Gdm9OIppz-vqQHr668K_CHOsUxOlXWGHUTGeMxZoNaCs6g/s796/header.png)

  

 Ayer en la mañana nos desayunamos la noticia de que existió "una posible brecha" en el sistema de clave única de Chile (es una forma centralizada en la cual un ciudadano chileno puede acceder a más de 900 trámites online con una sola contraseña), en la misma mañana salió un comunicado de Gobierno Digital apagando el incendio, diciendo que usaban un robusto sistema de hasheo en sus sistemas y aunque haya existido la brecha no se podía acceder a la clave única, pero igual de todas formas ya habían activado el protocolo de cambio de contraseñas de forma paulatina para evitar problemas.

 

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh8PKpbbey44PuYBZDr02SYHd-AvOsRAar5O0JZelJt41so2EB-CXcEypU7XEqq9PFZWgE1YSvBMsaqcJGgdr55VYxrABwgUP4ww0oaaSXZ_niyZ0MNzVX71BNTD2k4ei4XQ09WzDbiG2YG/w640-h392/Declaraci%25C3%25B3n+Gobierno+Digital.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh8PKpbbey44PuYBZDr02SYHd-AvOsRAar5O0JZelJt41so2EB-CXcEypU7XEqq9PFZWgE1YSvBMsaqcJGgdr55VYxrABwgUP4ww0oaaSXZ_niyZ0MNzVX71BNTD2k4ei4XQ09WzDbiG2YG/s965/Declaraci%25C3%25B3n+Gobierno+Digital.png)

  
 

Ningún sistema es 100% seguro y estas cosas pueden pasar, el protocolo aplicado es el correcto en este tipo de casos, todo "normal" y en el peor de los casos se filtró tu clave única, pero nadie pudo realizar trámites a tu nombre ya que tu contraseña debe ser cambiada en el siguiente logeo (esto le ha pasado a grandes empresas como Dropbox, Linkedin, etc...).

Hasta aquí estamos hablando de un problema grave en la seguridad en **un sistema en particular**, lo cual afecta de forma seria la funcionalidad de un sistema del Gobierno de Chile que es usado por prácticamente todos los chilenos, más ahora que estamos en pandemia... Pero el problema parecía haber sido amagado de forma correcta y esperábamos que no hubieran más incidencias, eso hasta que un twittero empieza a liberar los leaks de a poco...  

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjBbtm98Sk-hItQ5L3WXu9exQt4aGCxRr2yxUoeH9SwE0EYPkNsTU5ogqs_PLRV2t7E8e5-TYfnVmItWydj3MjxudxNBYyP8ifYrPwzr-CrmveX7lSF8j1-qZ47-z8C7zDXcTs01NMFbdFf/w464-h640/data_leak_chile_1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjBbtm98Sk-hItQ5L3WXu9exQt4aGCxRr2yxUoeH9SwE0EYPkNsTU5ogqs_PLRV2t7E8e5-TYfnVmItWydj3MjxudxNBYyP8ifYrPwzr-CrmveX7lSF8j1-qZ47-z8C7zDXcTs01NMFbdFf/s855/data_leak_chile_1.png)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7QoHjtFlDaJ7SJZl_TrOZ6mTWiRhP8E4Eh29n-O0sM0Cs1kv3XJaODeVDE4aK7nICUDlGaqBEAsyWuu6VaRnPVuDXap_gJDmUCIcZdP30ODeocayQFEbl40iVPt1SvM-6G9MKoDV40E-J/w506-h640/data_leak_chile_3.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7QoHjtFlDaJ7SJZl_TrOZ6mTWiRhP8E4Eh29n-O0sM0Cs1kv3XJaODeVDE4aK7nICUDlGaqBEAsyWuu6VaRnPVuDXap_gJDmUCIcZdP30ODeocayQFEbl40iVPt1SvM-6G9MKoDV40E-J/s807/data_leak_chile_3.png)

  

Personalmente esperaba que las filtraciones fueran falsas o de bajo impacto, pero cuando empiezo a ver el contenido de los archivos mi peor temor fue confirmado.

No solamente habían vulnerado el sistema de Clave única, **habían comprometido toda la infraestructura de Gobierno Digital**, infraestructura en la cual se cuenta con proyectos como Clave única, Comisaría Virtual, plataforma de tramites online del Ministerio de Bienes Nacionales, y muchos otros.  
  
La infraestructura está montada en **Amazon Web Services** ya que uno de los archivos filtrados contiene todas las configuraciones en formato JSON de AWS, donde se encuentran los usuarios, grupos de acceso, políticas de seguridad, además de las funciones lamdas con sus respectivas variables de entorno:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjeum8djz6UiyvYHhhSn7so0D72LRWaWDL1sWjNLzDOjY6JYelPO807XnitKLdXiY3vwP0yhH9H5CQ69Dco_LRRBDI5x-M7dE1bMEDi66ECj4dYbJ3LxTu2a46KeSXfZ6Iof_uvf9m-EjOV/w640-h516/users_aws.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjeum8djz6UiyvYHhhSn7so0D72LRWaWDL1sWjNLzDOjY6JYelPO807XnitKLdXiY3vwP0yhH9H5CQ69Dco_LRRBDI5x-M7dE1bMEDi66ECj4dYbJ3LxTu2a46KeSXfZ6Iof_uvf9m-EjOV/s863/users_aws.png)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhAt4Q_uavUfmznihXVLNmM1I83n3mIfW9dO4Wnwh9a9_4mdtnmaLluSWLrVFbTHS4Lr3umFSLJkuPrcRk4k93QKwDIGMf3sHlgtntVwcDk8iUV1rod-wHpCss-rAnB499_ITw5r_jtUWZX/w640-h394/lambda_env.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhAt4Q_uavUfmznihXVLNmM1I83n3mIfW9dO4Wnwh9a9_4mdtnmaLluSWLrVFbTHS4Lr3umFSLJkuPrcRk4k93QKwDIGMf3sHlgtntVwcDk8iUV1rod-wHpCss-rAnB499_ITw5r_jtUWZX/s1063/lambda_env.png)

Dentro de los archivos filtrados hay archivos de **configuración para VPNs**, **llaves privadas SSH**, muchos **dumps de bases de datos SQL** y **código de aplicaciones**, entre otros, en conjunto por el momento son más de **9 GB** de archivos, en los cuales hay datos sensibles, contraseñas, nombres, trámites virtuales, API keys, endpoints e información de infraestructura. Al momento de escribir este artículo el usuario en twitter que está filtrando los archivos ya va por el número 16 y pareciera ser que son muchos más dado el nivel de compromiso que se logró en este ataque.

Sin duda un día gris para la democracia digital en Chile. Durante el día es esperable que los medio amplíen la información, pero como no son expertos en la materia quizás no vean el grave impacto que hay producto de este ataque.  

  

Saludos!

#### [Source](http://www.blackploit.com/2020/10/hackeo-sin-presedentes-la.html)

<br/>
---
