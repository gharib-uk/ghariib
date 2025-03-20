---
title: "Saltar la confirmacin de redireccionamiento en Google y Youtube Bypass Redirect Check Google Youtube"
date: Mon, 13 May 2019 09:30:00 -0400
draft: false
type: posts
categories: 
- Bug,Bypass Redirect,Google,Youtube
---
# Saltar la confirmacin de redireccionamiento en Google y Youtube Bypass Redirect Check Google Youtube

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhwrwrE-8mhUQaFftA_NOVguBISNSMSxEmrEpL-xZWftrZ5cZRdVcYzXA27jS1Xi24N22lm_oX5EQhIykfcPrrlnxImly9cw2LwVL5kadHsJ1d7c5-2x9yhWHiDPW_WxJusJ1llCVSGNoeE/s640/redirect_yes.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhwrwrE-8mhUQaFftA_NOVguBISNSMSxEmrEpL-xZWftrZ5cZRdVcYzXA27jS1Xi24N22lm_oX5EQhIykfcPrrlnxImly9cw2LwVL5kadHsJ1d7c5-2x9yhWHiDPW_WxJusJ1llCVSGNoeE/s1600/redirect_yes.png)

  

El uso de redirectores en páginas webs es relativamente normal, y sus usos principales son:

1.  Avisar al usuario que está saliendo del dominio principal.
2.  Recolectar datos de navegación de los usuarios.
3.  Llevar al usuario a una página llena de publicidad para que hagas click en un entre que está entre dos banners de publicidad.

  

El caso 1 en general lo usan grandes sitios webs para evitar que nos redirijan a sitios webs maliciosos o con phishing sin previo aviso, como lo hace Google en este caso:

```
https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=15&url=https%3A%2F%2Fwww.blackploit.com
```

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjQ9AaItz7T7PY3zKNolmdGJDpTDHKSYUKOyyyoXzraZQGuf33WPzjRhbryfQLQ0ecQ6K10UlwDNBbgKCf2G3NyQg1YTsVLnDLZAI70c9E9izx9-OwQS5lL78VXVLzmaCbmHMPZHwY1dayd/s400/redirect_google.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjQ9AaItz7T7PY3zKNolmdGJDpTDHKSYUKOyyyoXzraZQGuf33WPzjRhbryfQLQ0ecQ6K10UlwDNBbgKCf2G3NyQg1YTsVLnDLZAI70c9E9izx9-OwQS5lL78VXVLzmaCbmHMPZHwY1dayd/s1600/redirect_google.png)

Imagen 1: caso 1 de redireccionamiento

El caso número 2 es usado por grades plataformas para poder separar las **operaciones** con la **analítica** del sistema. El caso emblema es Twitter que usa su acortador **t.co** para redirigir todo el tráfico y poder analizar el comportamiento de sus usuarios, ejemplo:

```
http://t.co/J3LuvnLZ3X --> https://www.blackploit.com
```

  

Usar un acortador es la mejor forma de hacer un redirector ya que así te aseguras de que nadie pueda ingresar en la URL un parámetro malicioso, no como en el ejemplo del caso 2.

  
En el caso 3 existen muchos ejemplos de acortadores y redirectores que usan esta estrategia para obtener dinero por clicks en publicidad.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjvCpiavjpFuU1s2Re5jG7L0Qw2UbNhVZ4_mNtsTdcegs3euIr1EC-b7JksbaBmOYYJmlb0ZxowLkSqaCjKhUbIPwuOcEOVqPr_vbmOMJaV0ZA1QZGPDYrv12M65XPmZPexkdyBQ87OuW_q/s400/ouo.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjvCpiavjpFuU1s2Re5jG7L0Qw2UbNhVZ4_mNtsTdcegs3euIr1EC-b7JksbaBmOYYJmlb0ZxowLkSqaCjKhUbIPwuOcEOVqPr_vbmOMJaV0ZA1QZGPDYrv12M65XPmZPexkdyBQ87OuW_q/s1600/ouo.png)

Imagen 2: ejemplo caso 3

Hoy nos vamos a centrar en el caso 1, específicamente en los redirectores de Google y Youtube, y ver que tan difícil es poder saltar la confirmación de redirección (imágen 1).

  
**Potenciales usos maliciosos de un redirector**  
  

Existen diferentes formas de mal utilizar un redirector, algunas son:

1.  Redirigir a una web con contenido malicioso o phishing, haciendo creer al usuario final que está en el dominio del redirector.
2.  Ocultar el Referer original de un enlace.
3.  Incrementar artificialmente las estadísticas de un sitio web.

En el caso 3 uno puede hacer un script que visite un enlace con redirector, y la página web objetivo va empezar a tener un incremento de visitas del sitio web del redirector, aumentando de forma artificial la fuente de tráfico en un sitio web.

  
**Análisis**  
  

Google en su redirector principal, cada vez que haces click en un enlace del buscador, crea un redirector con la URL objetivo (en este ejemplo **https://www.blackploit.com/**) y crea 2 tokens (**ved** y **usg**) que cambian para evitar que cualquiera pueda aumentar de forma artificial los clicks (que son usados entre otras cosas para posicionar los resultados de búsquedas), en el caso de que los parámetros **ved** y **usg** no sean válidos, te mandarán a una página con un mensaje de redirección (imagen 1):

```
https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&
ved=2ahUKEwiAkoTo8pbiAhUBA9QKHYSBAPkQFjAAegQIBhAC& 
url=https%3A%2F%2Fwww.blackploit.com%2F& 
usg=AOvVaw2YsoR0eglfSf7B9Vk2ZRzS
```

  

**¿Cómo podemos predecir estos tokens?**, No tengo la menor idea, pero no es necesario ya que Google tiene un _whitelist_ de dominios que no requieren los tokens de verificación.

  

**¿Cuáles son estos dominios?**, Obviamente google.com y todos sus TLDs (google.es, cl, net, org, etc.) y todos sus subdominios, por tanto la regla es _**\*.google.{com, cl, es, ... ,TLDs}**_.

  

Por ejemplo:  

```
https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=15&
url=https%3A%2F%2Fdrive.google.com
```

En este ejemplo nos redirige a _**drive.google.com**_ sin mensajes de redireccionamiento.

  

```
https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=15&
url=https%3A%2F%2Ffake.google.com
```

En este ejemplo nos redirige  a _**fake.google.com**_ sin mensajes de redireccionamiento, siendo que _**fake.google.com**_ no existe, lo que confirma la regla vista anteriormente.

  

Por lo tanto debemos simplemente encontrar un solo redirector en _**\*.google.com**_ que no tenga reglas o mensajes de redireccionamiento y podemos saltar todos los redirectores de Google, es así como encontramos el redirector de Hangouts:

```
https://hangouts.google.com/linkredirect?dest=https%3A%2F%2Fwww.blackploit.com%2F
```

Este redirector simplemente nos envía a la URL que queramos sin mensajes ni avisos.

  

Por lo tanto ya tenemos todo lo que necesitamos, simplemente necesitamos codificar la URL paso a paso, donde se pueden ayudar de [esta web](https://meyerweb.com/eric/tools/dencoder/).

  
Codificamos la web objetivo:  

```
https://www.blackploit.com
 |
 v
https%3A%2F%2Fwww.blackploit.com
```

  
La agregamos al redirector de Hangouts y la volvemos a codificar:  

```
https://hangouts.google.com/linkredirect?dest=https%3A%2F%2Fwww.blackploit.com
 |
 v
https%3A%2F%2Fhangouts.google.com%2Flinkredirect%3Fdest%3Dhttps%253A%252F%252Fwww.blackploit.com
```

  
Por último la agregamos al redirector de Google:  

```
https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=15&url=https%3A%2F%2Fhangouts.google.com%2Flinkredirect%3Fdest%3Dhttps%3A%2F%2Fwww.blackploit.com
```

  

Listo! tenemos un enlace [**https://www.google.com/...**](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=15&url=https%3A%2F%2Fhangouts.google.com%2Flinkredirect%3Fdest%3Dhttps%3A%2F%2Fwww.blackploit.com) que redirige a **https://www.blackploit.com/** saltándose el filtro de Google.

  

Este mismo método se puede aplicar en otros redirectores, acá les mostraré dos ejemplos, uno en **_accounts.google.com_** y otro en _**accounts.youtube.com**_, ambos tienen el mismo comportamiento:

```
https://accounts.youtube.com/accounts/SetSID?ilo=1&ils=a4cc1b7ed445598%20f16cef403bb3b0311&ilc=0
&continue=https%3A%2F%2Fhangouts.google.com%2Flinkredirect%3Fdest%3Dhttps%3A%2F%2Fwww.blackploit.com
```

Y:  

```
https://accounts.google.com/signin/v2/identifier?uilel=3&service=youtube&passive=true&hl=es-419
&continue=https%3A%2F%2Fhangouts.google.com%2Flinkredirect%3Fdest%3Dhttps%3A%2F%2Fwww.blackploit.com
&feature=sign_in_button&app=desktop&hl=es-419&next=%2F&flowName=GlifWebSignIn&flowEntry=ServiceLogin
```

  

Estos dos casos son más interesantes ya que cuando el usuario está logeado en una cuenta Google redirige a la web objetivo sin mostrar mensajes, en el caso de que no esté logeado en Google, le pide usuario y contraseña y luego lo redirige a la web objetivo sin problemas ni avisos.

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhlZ_md5pFUMfbUw-9m0A5ibVFMJe1ubQgcrGhvb1_oF3ZlqC6F9oobAmOY9GwvbepJySWfKt7gY8B17o9PELSjVb2ES07PrfXPS_rQyDnFum_RhrmiTis9NenxvIfuMnT-fzmEvTGuNuCW/s640/redirect_google_user_pass.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhlZ_md5pFUMfbUw-9m0A5ibVFMJe1ubQgcrGhvb1_oF3ZlqC6F9oobAmOY9GwvbepJySWfKt7gY8B17o9PELSjVb2ES07PrfXPS_rQyDnFum_RhrmiTis9NenxvIfuMnT-fzmEvTGuNuCW/s1600/redirect_google_user_pass.png)

Imágen 3: redirección cuando no estamos logeado en Google.

  
**Resumen**  
  

```
(1)
https://hangouts.google.com/linkredirect?dest=https%3A%2F%2Fwww.blackploit.com

(2)
https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=15
&url=https%3A%2F%2Fhangouts.google.com%2Flinkredirect%3Fdest%3Dhttps%3A%2F%2Fwww.blackploit.com

(3)
https://accounts.youtube.com/accounts/SetSID?ilo=1&ils=a4cc1b7ed445598%20f16cef403bb3b0311&ilc=0
&continue=https%3A%2F%2Fhangouts.google.com%2Flinkredirect%3Fdest%3Dhttps%3A%2F%2Fwww.blackploit.com

(4)
https://accounts.google.com/signin/v2/identifier?uilel=3&service=youtube&passive=true&hl=es-419
&continue=https%3A%2F%2Fhangouts.google.com%2Flinkredirect%3Fdest%3Dhttps%3A%2F%2Fwww.blackploit.com
&feature=sign_in_button&app=desktop&hl=es-419&next=%2F&flowName=GlifWebSignIn&flowEntry=ServiceLogin
```

  
**Reporte**  
  

Se envió el reporte a Google y el bot que responde dijo que era un comportamiento completamente previsto y que incluso no era un defecto, si no, una característica.

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhwaARiuFNyl_5-PkEEi39vhKj1LKJq0AJXmA0TTXaVo9ctMWdALOLkcxkFWRaWiL8xTgZh-WgfBTeWnYYzmgjrh6rPCAQ9FMRWNa7bNIispGewYckOah4I7AoHc5W00V3uT0gNyUGufWjD/s320/google_report.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhwaARiuFNyl_5-PkEEi39vhKj1LKJq0AJXmA0TTXaVo9ctMWdALOLkcxkFWRaWiL8xTgZh-WgfBTeWnYYzmgjrh6rPCAQ9FMRWNa7bNIispGewYckOah4I7AoHc5W00V3uT0gNyUGufWjD/s1600/google_report.png)

Reporte a Google

Incluso mandan un enlace para informarnos más: [https://sites.google.com/site/bughunteruniversity/nonvuln/open-redirect](https://sites.google.com/site/bughunteruniversity/nonvuln/open-redirect)

  

En ese enlace dice claramente que en los redirectores  se considera vulneravilidad:

-   Content Security Policy bypass
-   Referrer check bypass
-   URL whitelist bypass

Yo no sé si entiendo mal o el bot necesita un arreglo, pero ya se reportó y el bot tiene la última palabra.

  

**Conclusión**

  

Sin duda se pueden encontrar muchos más casos, aquí presento 3 que no me costó mucho trabajo en encontrar.

  

Puede parecer que esta no es una vulnerabilidad, pero se supone que cuando uno accede a un enlace de un dominio importante, este no debería redirigir a un sitio web malicioso.

  

Por otro lado existen redirectores que tienen en su whitelist de sitios de confianza a google.com, por lo cual esos redirectores también son bypasseables.

  

Personalmente ahora tengo más ojo y no me fio 100% de enlaces de Google.

  
  
\[+\] Salu2!

#### [Source](http://www.blackploit.com/2019/05/bypass-redirect-check-google-youtube.html)

<br/>
---
