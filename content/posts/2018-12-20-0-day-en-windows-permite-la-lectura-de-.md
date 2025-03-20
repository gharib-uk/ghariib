---
title: "0-Day en Windows permite la lectura de cualquier archivo sin privilegios"
date: Thu, 20 Dec 2018 16:29:00 -0300
draft: false
type: posts
categories: 
- 0-day,Exploit,Windows OS
---
# 0-Day en Windows permite la lectura de cualquier archivo sin privilegios

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEicMJI6jgz-Ywg4zZHDuls4hsg4rLR5aYfN2xp-xGnFxMVLRazK_Wmuj-i5Mi907PLZLu4fi5WGImUCEpQ9po7tNCtTywFR0fHE4_08PguKYutx49TIpTgRtTP3g35_S827Z_V3O8kPMocO/s640/readfile.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEicMJI6jgz-Ywg4zZHDuls4hsg4rLR5aYfN2xp-xGnFxMVLRazK_Wmuj-i5Mi907PLZLu4fi5WGImUCEpQ9po7tNCtTywFR0fHE4_08PguKYutx49TIpTgRtTP3g35_S827Z_V3O8kPMocO/s1600/readfile.png)

  

El investigador de seguridad [SandboxEscaper](https://twitter.com/Evil_Polar_Bear) ha publicado hoy el PoC para una nueva vulnerabilidad 0day que afecta a Windows que permite a un usuario con pocos privilegios o a un programa malicioso leer el contenido de cualquier archivo en un equipo con Windows, que de otro modo sólo sería posible con privilegios de administrador.

  

Según el investigador, debido a una validación incorrecta, se puede abusar de la función "**_MsiAdvertiseProduct_**" de Windows que se encarga de generar "un script publicitario para anunciar un producto y  que permite al instalador escribir en un script la información de registro" forzando al servicio de instalación a hacer una copia de cualquier archivo con privilegios de SYSTEM y leer su contenido, lo que resulta en una **vulnerabilidad de lectura arbitraria de archivos**.

  

SandboxEscaper nos comparte un vídeo demostrativo donde podemos ver el funcionamiento de su exploit readfile.exe para leer un archivo con privilegios de SYSTEM:

  

  

Además de compartir el vídeo de demostración de la vulnerabilidad, SandboxEscaper también publicó su exploit en Github, pero debido a que **Github hoy es de Microsoft**, decidieron cerrar su cuenta sin mayores explicaciones.

  

Si desean descargar este exploit 0day (aún), lo pueden hacer con alguno de los enlaces que hay en el **[blog de SandboxEscaper](https://sandboxescaper.blogspot.com/2018/12/readfile-0day.html)**, lamentablemente no está disponible en Github.

  
  
**Fuentes:**  

-   [https://twitter.com/Evil\_Polar\_Bear](https://twitter.com/Evil_Polar_Bear/status/107560501110576742)
-   [https://sandboxescaper.blogspot.com](https://sandboxescaper.blogspot.com/)
-   [https://thehackernews.com](https://thehackernews.com/2018/12/windows-zero-day-exploit.html)

#### [Source](http://www.blackploit.com/2018/12/0day-en-windows-permite-la-lectura-de.html)

<br/>
---
