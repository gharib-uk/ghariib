---
title: "Cmo hackearon los correos del Congreso y Senado de Chile"
date: Fri, 31 Aug 2018 18:48:00 -0300
draft: false
type: posts
categories: 
- Chilean Way,Hacked,Noticias
---
# Cmo hackearon los correos del Congreso y Senado de Chile

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhuVy7ggZRSQcBuIJ88iENACZ9GHJE3MbxbnKc_RXu_Vxz3ciBDRBb9g90ofoqhRqu4mFeZfTNpWsHY3cTuAyUExLd_BMp-PYLedczuvvXHy1qKyATLlVKwe-MVcv-bf9pTFDrhosGg3lai/s640/hacker-boy.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhuVy7ggZRSQcBuIJ88iENACZ9GHJE3MbxbnKc_RXu_Vxz3ciBDRBb9g90ofoqhRqu4mFeZfTNpWsHY3cTuAyUExLd_BMp-PYLedczuvvXHy1qKyATLlVKwe-MVcv-bf9pTFDrhosGg3lai/s1600/hacker-boy.png)

  

Una vez más el grupo "TheShadowBrokers" genera alarma pública en Chile y esta vez se debe a que supuestamente "hackearon" los correos del Congreso y Senado de ese país.

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEio_niVJNJq7-oC4GSLAPR7ST4-eKiFVNV33MItY3cZI0Wu9iLA0RCLm5vyib8FuSUUTEyGksIhbSPk0VqecguuZpOcZ6XtncqXDkPQToZj8uXZ_a3VNh9RwYLzlYGHw43u8e-416WwsQsU/s640/fake_hacked.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEio_niVJNJq7-oC4GSLAPR7ST4-eKiFVNV33MItY3cZI0Wu9iLA0RCLm5vyib8FuSUUTEyGksIhbSPk0VqecguuZpOcZ6XtncqXDkPQToZj8uXZ_a3VNh9RwYLzlYGHw43u8e-416WwsQsU/s1600/fake_hacked.jpg)

  

Al hacer un análisis de los datos "hackeados" se nota rápidamente que son muy pocos los correos involucrados, por tanto no fue una intrusión a los servidores de correos del Congreso o Senado.

  

Como ya es común, los "TheShadowBrokers" solo recopilan datos, así que al buscar los correos en [Have I Been Pwned](https://haveibeenpwned.com/), todos los correos había sido filtrados en otras ocasiones:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhqsVZQHwl4HAkEJ_YN8KDfMUYFg2O1XnDiU5nLDT851Wf4UMJQGEROM8RzBddPinozp5QiQVyShv1SHiY_hl23jNR1W2Oj0bvDiUGXB2K3mf_h61RPQw-jEnG5lprzUlqYZMRfAsJqV2mx/s640/pwneds.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhqsVZQHwl4HAkEJ_YN8KDfMUYFg2O1XnDiU5nLDT851Wf4UMJQGEROM8RzBddPinozp5QiQVyShv1SHiY_hl23jNR1W2Oj0bvDiUGXB2K3mf_h61RPQw-jEnG5lprzUlqYZMRfAsJqV2mx/s1600/pwneds.png)

  

De estas filtraciones la más interesante es la de _**Exploit.in**_, ya que esta es una recopilación tipo combolist (email:contraseña) de muchas otras filtraciones, la particularidad es que todas las contraseñas están en texto plano (fueron crackeadas por algún ruso erudito en delitos informáticos). Esa base de datos está en la red y es cosa de buscarla, al comparar los datos de esa bases de datos con los "hackeados" por los "TheShadowBrokers", BINGO! son exactamente las mismas cuentas y contraseñas.

  

Por tanto lo único que tuvieron que hacer es:

  

```
grep -rnw '/ruta/a/Exploit.in_database/' -e 'congreso.cl'
grep -rnw '/ruta/a/Exploit.in_database/' -e 'senado.cl'
```

  

Así de simple, probablemente ni si quiera hayan servido para acceder a las cuentas reales, pero si para generar alarma pública.

  
**Fuentes:**  

-   [https://www.fayerwayer.com/2018/08/chile-correos-congreso-hackeo/](https://www.fayerwayer.com/2018/08/chile-correos-congreso-hackeo/)
-   [https://www.antronio.cl/threads/the-shadowbrokers-filtra-claves-de-senadores.1292187/](https://www.antronio.cl/threads/the-shadowbrokers-filtra-claves-de-senadores.1292187/)

  
\[+\] Salu2

#### [Source](http://www.blackploit.com/2018/08/como-hackearon-los-correos-del-congreso.html)

<br/>
---
