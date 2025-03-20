---
title: "Hackeando un Banco 3 Hackear 14000 Tarjetas de Crdito Carding"
date: Thu, 26 Jul 2018 16:26:00 -0400
draft: false
type: posts
categories: 
- Blackploit,Carding,Hacking,Noticias,Seguridad
---
# Hackeando un Banco 3 Hackear 14000 Tarjetas de Crdito Carding

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhhtNSadSnCRm-jKfqIo55Doon20e0SAZDYMWJ39c0ySDFvsj6WymSihI-FBkHfgMr9raTgCfALe_3DkO16RmtVjkBCJb3aCzoUmMi-KZktF1fBlYDLocFUqSUytyFX00izu43kEZQTOHkH/s640/title_cc.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhhtNSadSnCRm-jKfqIo55Doon20e0SAZDYMWJ39c0ySDFvsj6WymSihI-FBkHfgMr9raTgCfALe_3DkO16RmtVjkBCJb3aCzoUmMi-KZktF1fBlYDLocFUqSUytyFX00izu43kEZQTOHkH/s1600/title_cc.png)

  

Recientemente en la prensa chilena ha causado bastante estragos diferentes noticias relacionadas con la mala seguridad informática con la que cuentan los bancos en Chile \[[1](https://www.blackploit.com/2018/04/hackeando-un-banco-2-caso-real.html)\] \[[2](http://www.emol.com/noticias/Economia/2018/06/09/909234/Banco-de-Chile-confirma-que-ataque-informatico-de-mayo-robo-US-10-millones.html)\] \[[3](https://www.biobiochile.cl/especial/noticias/reportajes/reportajes-reportajes/2018/07/18/hackeo-interno-en-el-banco-de-chile-informatico-robo-475-millones-de-pesos-usando-su-pc.shtml)\], ayer una filtración de más de 14 mil tarjetas de crédito de bancos chilenos provocaban el pánico de muchos, incluso de las autoridades (senador) \[[4](https://twitter.com/felipeharboe/status/1022290787655213056)\]. Obligando a los bancos a dar explicaciones y bloquear las cuentas afectadas por esta filtración \[[5](https://www.fayerwayer.com/2018/07/lista-bancos-hackeo-filtracion-tarjetas-credito/)\], para disminuir el impacto en la opinión pública.

  

Y la verdad, es que no hay que entrar en pánico ya que esta situación es más normal de lo que creen y no solo en Chile, si no, en el mundo entero...  
  

**¿Qué es el _carding_?**  

  

_Carding_ es un término que describe el tráfico de tarjetas de crédito, cuentas bancarias y otra información personal en línea, así como los servicios relacionados con el fraude. Las actividades también abarcan la adquisición de detalles personales, y técnicas de lavado de dinero \[[6](https://en.wikipedia.org/wiki/Carding_\(fraud\))\].

  

Hay una gran cantidad de métodos para adquirir tarjetas de crédito, datos financieros y personales. La forma más común de obtenerlos es generando número de tarjetas de créditos válidas de forma semiautomática a partir de secuencias conocidas a través de un "BIN attack"\[[7](https://www.syswaregroup.com/resource-centre/case-studies/banking/credit-card-fraud/)\].  

  

Los métodos de seguridad de las tarjetas de créditos son 2:

-   **CVV:** código numérico de largo 3. Mil combinaciones posibles, del 000 al 999 secuencialmente.
-   **Fecha de expiración:** mes y año. Los meses son 12. Los años son máximo 10.

  

Sabiendo un número de **tarjeta de crédito**, el **CVV** y la **fecha de expiración**, se pueden hacer compras online sin problemas (por montos menores al cupo de la tarjeta).

  

**BIN attack**

  

 _**BIN**_ se le llama a los **6 primeros dígitos de una tarjeta de crédito**, estos 6 primero dígitos definen el país y la entidad bancaria a la que pertenece esta tarjeta de crédito.

  

El formato de un número de tarjeta de crédito es:

```
4444 5555 6666 7777
```

  

Siendo **444455** el **BIN** de la tarjeta. Y **5566667777** el **número que identifica al usuario** al que pertenece la tarjeta.

  

Como podemos ver, cada entidad bancaria en el mundo puede emitir un máximo de 10 mil millones de tarjetas de crédito partiendo de la tarjeta 4444 5500 0000 0000 a la 4444 5599 9999 9999 secuencialmente. Bastante ¿no?.

  

Lamentablemente la asignación de números de tarjetas de créditos muchas veces responden a patrones, por tanto, es muy fácil generar  número de tarjeta de créditos válidas, por ejemplo, si tengo una tarjeta que sé que es válida puedo generar secuencialmente otras válidas (más adelante se hace un mejor análisis).

```
4444 55111111 1111 [válida] -> 4444 55111111 1112 [válida también]
```

  

Por otro lado, existen bases de datos con BINs y su respectiva entidad bancaria, que se venden o son abiertos \[[8](https://www.bindb.com/bin-list.html)\].

  
**Mundo carding**  
  

Siempre que hablemos de dinero fácil, existirá una comunidad enorme que participe de ese ecosistema, en el caso del carding, su comunidad es muy grande donde participan millones de personas que compran y venden datos bancarios o de tarjetas de crédito para obtener productos o servicios a costo 0.

  

Aquí vemos algunos ejemplos que se encuentran fácil y abiertamente:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEioJ3ue-QY8EhdgHZhnoKtqT06mnPms5axbWNdXmteP1Ly0uj4AEN2WlL72ZDzawhLorzYUXZr2nYVn7daayq84NCYtfjTSot9U_seri55trLPzb0PJNxRoxcZtIW3cVwxEnhPjJSSDOacq/s640/forum_carding_01.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEioJ3ue-QY8EhdgHZhnoKtqT06mnPms5axbWNdXmteP1Ly0uj4AEN2WlL72ZDzawhLorzYUXZr2nYVn7daayq84NCYtfjTSot9U_seri55trLPzb0PJNxRoxcZtIW3cVwxEnhPjJSSDOacq/s1600/forum_carding_01.png)

Foro para compartir (vender) número de tarjetas de crédito, técnicas de compra y lavado de dinero.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiAMRwMndjzmd87kqnzJdyAFN9Gi6C-wfGQwYBN274uRa5_ytMZ3UslktT-g021SAaFqrCln9vAZb9tK8SPrkSxJm-N4zzFckrB-GbMZ7-acP9L51dc7OvZJhxSl30Jnj2UKHutCkEvwsm5/s640/web_carding_01.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiAMRwMndjzmd87kqnzJdyAFN9Gi6C-wfGQwYBN274uRa5_ytMZ3UslktT-g021SAaFqrCln9vAZb9tK8SPrkSxJm-N4zzFckrB-GbMZ7-acP9L51dc7OvZJhxSl30Jnj2UKHutCkEvwsm5/s1600/web_carding_01.png)

Web de venta de TC y Dumps

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiRI1nEKbApTlgMU9_azQUw9xKgKdTgYRWi9ncQWaeozsnfsuMUfhiGahjEZrM0EeHoWluqtGw647VRrWQkhBL4SPg-wc2PJqoGBG88XVJgCUlNJMb9MOCi0IpgPDNe_VKQHwiFb_YHxjxu/s640/web_carding_02.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiRI1nEKbApTlgMU9_azQUw9xKgKdTgYRWi9ncQWaeozsnfsuMUfhiGahjEZrM0EeHoWluqtGw647VRrWQkhBL4SPg-wc2PJqoGBG88XVJgCUlNJMb9MOCi0IpgPDNe_VKQHwiFb_YHxjxu/s1600/web_carding_02.png)

Web de venta de TC

  

La impunidad a este tipo de delitos es tan grande que las webs de venta son completamente abiertas (no están en **TOR** o la **Deep Web**) y promocionan sus servicios en varias partes.

  
**Literatura carding**  
  

Así como los métodos de seguridad de las tarjetas de créditos son insuficientes, existe gran cantidad de literatura donde se enseña a generar números de tarjetas de créditos, técnicas de ingeniería social para obtener datos, técnicas de lavado de dinero, técnicas para obtener el CVV de una tarjeta, etc.

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhTR8uO5WuMA0Kl8Q52-EULGdxeOKnDJhhdCfX-F1xxUcTY9laqdaMHBMkzrHUEaaFsou5sP1-nfniHw7Q7ApOaDh6cKR13dXd09M-zUNgfhHgztG3UCbuvekp-5QT-9vF-zOMFk2ogYLJC/s640/manuales_carding.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhTR8uO5WuMA0Kl8Q52-EULGdxeOKnDJhhdCfX-F1xxUcTY9laqdaMHBMkzrHUEaaFsou5sP1-nfniHw7Q7ApOaDh6cKR13dXd09M-zUNgfhHgztG3UCbuvekp-5QT-9vF-zOMFk2ogYLJC/s1600/manuales_carding.png)

Manuales carding

  

Esta misma literatura clasifica a las tarjetas de crédito en 3 niveles, según dificultad:

-   **Level 1 (_Easy carding_)**: Se puede gastar un máximo de 50 dólares. Se necesita:

-   Tarjeta de crédito.
-   Fecha de expiración.

-   **Level 2 (Intermediate carding)**: Se puede gastar un máximo de 2000 dólares. Se necesita:

-   Tarjeta de crédito 
-   Fecha de expiración 
-   Código CVV,  
-   Nombre del cliente 
-   Número de la cuenta bancaria.

-   **Level 3 (****Hard carding)**: No hay limites establecidos de gasto. Se necesita:

-   Tarjeta de crédito 
-   Fecha de expiración 
-   Código CVV,  
-   Nombre del propietario.
-   Número de la cuenta bancaria. 
-   Cantidad de dinero en la cuenta.
-   Número de teléfono.
-   SSN.
-   DOB.

  

Como podemos ver, el nivel 1 no se requiere de mucha información, simplemente el número de la tarjeta de crédito, el año y mes de expiración, realmente _easy_.

  
**Herramientas carding**  
  

Esta comunidad del delito no solo tiene lugares de comunicación y literatura extensa, también tienen un sin fin de herramientas que sirven para diferentes finalidades, a continuación les mostraré dos herramientas, la primera sirve para que con un _dump_ (llamado así a una base de datos con tarjetas de crédito, CVV y fecha de expiración pero sin validar), puedo verificar el banco, país y tipo de cuenta y la segunda herramienta sirve para validar los datos de una tarjeta de crédito.

  

A la herramienta a continuación se le tienen que ingresar los 6 primeros dígitos de las tarjetas de crédito, y está te dirá de que banco y que tipo de cuenta le corresponde:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg_HS1Qf6NxyX9nryKWAZGUiRxRSNu5M_yXyt9X_nwkYwji2f-s9ISAZXaTeowU0dIaujyLobiB5noKLhP7f1xVN_1Cy_E5QhgVfZosMLz4-tbrCQ60tRPMky6z1UtkbryOoL4UvgH7V8w7/s640/title_carding.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg_HS1Qf6NxyX9nryKWAZGUiRxRSNu5M_yXyt9X_nwkYwji2f-s9ISAZXaTeowU0dIaujyLobiB5noKLhP7f1xVN_1Cy_E5QhgVfZosMLz4-tbrCQ60tRPMky6z1UtkbryOoL4UvgH7V8w7/s1600/title_carding.png)

  
Se ingresan los BINS:  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiveVg2PpUYp-1svICotswQldYHVpCuYk6gnvZ432Ih2UQICZ16Rh-9I2Vn7gi8lcZnz_EVhdYjKls8x8ptx06BHaZk5lfGwBRHGuaAGfKKE0VL4SuBVKK5e3di_Ju-AzaKA_nT9-raATCF/s640/verify_02.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiveVg2PpUYp-1svICotswQldYHVpCuYk6gnvZ432Ih2UQICZ16Rh-9I2Vn7gi8lcZnz_EVhdYjKls8x8ptx06BHaZk5lfGwBRHGuaAGfKKE0VL4SuBVKK5e3di_Ju-AzaKA_nT9-raATCF/s1600/verify_02.png)

  
Y se obtienen los resultados:  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7dshcW23Oytjb5I_eh3XKThFN-puIxnAgiVkyo5ooBJHdy2AKjho2Krfm95JLn6zZoxZbHebWEUM_vT_i-v3FgVyW2ZENvt1mJk93mj14MJB2NqERT6UGXHoS8xek8qOtw6F80OEI-yFT/s640/verify_03.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7dshcW23Oytjb5I_eh3XKThFN-puIxnAgiVkyo5ooBJHdy2AKjho2Krfm95JLn6zZoxZbHebWEUM_vT_i-v3FgVyW2ZENvt1mJk93mj14MJB2NqERT6UGXHoS8xek8qOtw6F80OEI-yFT/s1600/verify_03.png)

  

La segunda herramienta es muy sencilla y sirve para poder verificar el CVV y fecha de expiración de una tarjeta de crédito, realizando una micro transacción:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjI4mgEWQX1UKjiO-MWijQB4T-tfkcunKfMrt4DJXC_CZuRKhwJc66QI3lyNyh9dzdfYQQ4P8y6jRn7OPLWC820mBgCdQl1q9U5pbjoO_y81B0Mm_6bDxNps2_oE4rbkgoLYRZSljy4J954/s640/cc_checker.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjI4mgEWQX1UKjiO-MWijQB4T-tfkcunKfMrt4DJXC_CZuRKhwJc66QI3lyNyh9dzdfYQQ4P8y6jRn7OPLWC820mBgCdQl1q9U5pbjoO_y81B0Mm_6bDxNps2_oE4rbkgoLYRZSljy4J954/s1600/cc_checker.png)

  
**Ataque distribuido para adivinar CVV**  
  

Como dijimos anteriormente el CVV viene a ser el código de (in)seguridad de las tarjetas de crédito. Un número de largo 3, con mil combinaciones posibles, muy difícil de adivinar...

  

Pues bueno, ya se venden herramientas y servicios que consultan en mil comercios digitales los mil CVV posibles de una tarjeta de crédito, haciendo una micro compra, por lo general de menos de un dólar que cuando es aceptado significa que el CVV es correcto.

  
**No eres tú, es tu tarjeta de crédito**  
  

Cuando se crearon las tarjetas de crédito, se diseñaron para que la gente gastara su dinero rápidamente y no tuviera problemas ni burocracias, en esa misma linea, la seguridad significaba burocracias, agregar contraseñas seguras significaba valiosos segundos que perdía el sistema bancario, o la gente podía olvidarlas fácilmente, por lo cual se decidió por el mínimo de seguridad posible, **SIN SEGURIDAD**. 

  

Si tienes físicamente la tarjeta de crédito, no necesitas ni el CVV, ni la fecha de expiración, simplemente compras y ya. Y si compras a través de Internet montos menores a 50 dólares, vas a necesitar una fecha de expiración y a veces el CVV. Esos métodos de seguridad son irrisorios y como dije anteriormente, se diseñaron para que la gente gastara y no para que la gente guardara su dinero.

  

**¿Por qué nadie hace nada?**  
  

Hace muchos años **VISA** y **MasterCard** están trabajando en mejorar la seguridad, pero no lo han logrado, ya que cualquier sistema de seguridad haría que las personas comprasen menos; poner una contraseña o verificación en dos pasos significaría agregar una barrera a la compra. Por eso es que han puesto todas sus fichas a la tecnología _contactless_. Si antes era inseguro que te robaran la tarjeta de crédito, ahora simplemente tienen que estar cerca tuyo para cobrarte una transacción.

  

**El seguro paga y el banco hace vista gorda**

  

Como vimos, la seguridad de las tarjetas es pésima, y es muy normal que clientes del banco llamen para hacer notar que hay cobros irregulares en sus tarjetas de crédito. En la mayoría de los casos tu banco rápidamente te devuelve el dinero y te dice que fue un error, pero lo que realmente sucede es que un delincuente informático obtuvo el número de tu tarjeta de crédito o lo generó y realizó una compra fraudulenta en un país cercano a Rusia, el ejecutivo de cuenta se percata que evidentemente es un robo y lo notifica al sistema de riesgo, donde lo notifican como robo para que el seguro pague, luego te mandan un mail pidiendo disculpas, un mes después te llega una nueva tarjeta de crédito por correo o te anuncian que por ser buen cliente puedes tener una tarjeta más linda que puedes pasar a buscar.  
  
Este comportamiento está tan arraigado en la cultura de los bancos que cuando suceden estos robos no se notifican, el protocolo es que pasen lo más desapercibidos posibles. Esto se debe a que el costo de implementar seguridad - y por ende disminuir el volumen de transacciones realizadas - es mayor que el costo de afrontar el pago de seguros asociados al robo de dinero de tarjetas de crédito. En la filtración que les hablaré a continuación, ninguno de los bancos afectados le notificó a la PDI (policía de investigaciones de Chile) lo sucedido \[[9](https://www.fayerwayer.com/2018/07/filtracion-tarjetas-banco-de-chile/)\].

  

**Análisis de las 14 mil tarjetas de créditos filtradas**

  

Lo primero que hay que notar es que la gran mayoría de las tarjetas de crédito ya habían expirado, lo segundo es que los bancos notificaron que el 90% de las tarjetas ya no eran funcionales, por lo cual estamos hablando de un _dump_ basura (se gasta más verificando los datos que obteniendo dinero).

  

Segundo podemos notar que los nombres no fueron sacados de una base de datos única, más bien los fueron anotando a mano  (mayúsculas, minúsculas, banco, nombre, tipo de cuenta, no existe un formato relacional) a medida que obtenían los datos.

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEilPOTS_PzdS007rcAHxaTkbFNGk4beEVlz3LgE9Z3dasNLfJSH7PYlIOf7YSBH4bW1UaJvPCtilC5kwxHWNiJJIq6JrP3G13oMUHmSz76iqWuhGRVQM1hNnvpdBX1g80-sw-hWcSLGVSIE/s1600/dump_01.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEilPOTS_PzdS007rcAHxaTkbFNGk4beEVlz3LgE9Z3dasNLfJSH7PYlIOf7YSBH4bW1UaJvPCtilC5kwxHWNiJJIq6JrP3G13oMUHmSz76iqWuhGRVQM1hNnvpdBX1g80-sw-hWcSLGVSIE/s1600/dump_01.png)

  

Lo tercero es que podemos notar que hicieron un esfuerzo por "desordenar" la base de datos, pero si la ordenas por número te tarjetas de crédito, nos damos cuenta que las cuentas fueron generadas secuencialemente, esto se logra teniendo como base una cuenta válida y cambiándole los último 5 dígitos secuencialmente o aleatoriamente, es muy probable encontrar más números cercanos válidos.

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh0m08OGlB3v0WlZs_ZHmlCguC5Z6qkzcm_cZBiMNSxsUuV8Yka_GKizrPb0BlKZiFEUsHQtB8U40dI4fVAkDBg-ZlZKXyEBJcWH09snLLeTVrhTA6AB3lAOBLH_KyysoAK3Kh3LEd118mH/s1600/dump_02.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh0m08OGlB3v0WlZs_ZHmlCguC5Z6qkzcm_cZBiMNSxsUuV8Yka_GKizrPb0BlKZiFEUsHQtB8U40dI4fVAkDBg-ZlZKXyEBJcWH09snLLeTVrhTA6AB3lAOBLH_KyysoAK3Kh3LEd118mH/s1600/dump_02.png)

  

En resumen como podemos ver, esta fue una base de datos que se obtuvo de varias fuentes, muchas se obtuvieron generándolas a partir de otras válidas y verificando los datos. Pero fue hecha 100% a mano, sin técnicas avanzadas de informática o "hackeo".

  
**Base de datos obsoleta y distracción**  
  

Es evidente que los delincuentes que filtraron la base de datos **NO** son el grupo de hackers "**_ShadowBrokers_**"\[[10](https://www.nytimes.com/2017/11/12/us/nsa-shadow-brokers.html)\] que se infiltraron en la NSA y lograron obtener información y herramientas de hacking. Son simplemente _carders_, personas que de oficio compran y venden datos de tarjetas de créditos, y aprovecharon el ambiente de inseguridad informática bancaria que se vive en Chile para hacerle daño a la banca.

  

La base de datos filtrada se considera basura o de muy poco valor, y cuando tu base de datos pierde valor ya que las cuentas ya están expiradas, lo que haces es liberarla como si fuera una parte de una gran base de datos, ofreciendo la base de datos "real" por dinero.

  

Piden 200 BTC por la base de datos completa y las herramientas que usaron para crearla... esos BTCs al día de hoy son 1,6 millones de dólares, un precio ridículamente alto que hace notar que no existe dicha base de datos "completa".

  

El nombre **_ShadowBrokers_** lo eligieron con la finalidad de "distraer" a los investigadores y hacerles creer que son un súper grupo de hackers internacionales, cuando en la realidad deben ser  chilenos o de la región y con muy poco conocimiento informático (ya que si lo tuvieran habrían pedido Monero y no BTC).  Por último y otra distracción, piden la liberación de 3 tipos que no existen, nada concuerda en su discurso.

  
**Prensa**  

> El afamado grupo de hackers profesionales que se infiltró en la NSA y robó con técnicas avanzadas sus más preciados secretos y 0days, han vuelto a atacar y esta vez decidieron hacerlo en Chile, hackeando a todos los bancos y obteniendo sus tarjetas de crédito, filtrándolas para destruir el sistema bancario chileno al más puro estilo _Mr Robot_.

Prácticamente así es como la prensa en Chile informa sobre este filtración, siempre me da un poco de vergüenza lo sensacionalistas y poco informados que son los medios al momento de informar. Se nota que no le preguntan a nadie que sepa un poco de informática antes de escribir tanta desinformación, generando caos y pánico público. 

  

Usan el término "_hacker_" como sinónimo de delincuente informático, a pesar de que no se haya usado nada informático en el proceso, por ejemplo en una noticia donde un delincuente estafa a personas por internet, lo llaman "_hacker_ de alto conocimiento informático" y lo único que hace en la realidad es poner mensajes en Facebook ofreciendo propiedades que no son de él.

  

Por último en un canal de televisión local apareció un "experto" en seguridad informática explicando que el ataque fue perpetrado por los mismos que hackearon la NSA y que realizaron el "_wannacry_", que vergüenza.  
  
**Artículos anteriores**  
  

1.  [Hackeando un Banco (Caso Real) \[XSS + Sensitive Data Exposure + explicación SQLi\]](https://www.blackploit.com/2018/04/hackeando-un-banco-caso-real-xss.html) (16 abr. 2018)
2.  [Hackeando un Banco 2 (Caso Real) \[Transferir Dinero + Comprar Gratis\]](https://www.blackploit.com/2018/04/hackeando-un-banco-2-caso-real.html) (26 abr. 2018)

  
**Bibliografía**  
  

1.  [https://www.blackploit.com/2018/04/hackeando-un-banco-2-caso-real.html](https://www.blackploit.com/2018/04/hackeando-un-banco-2-caso-real.html)
2.  [http://www.emol.com/noticias/Economia/2018/06/09/909234/Banco-de-Chile-confirma-que-ataque-informatico-de-mayo-robo-US-10-millones.html](http://www.emol.com/noticias/Economia/2018/06/09/909234/Banco-de-Chile-confirma-que-ataque-informatico-de-mayo-robo-US-10-millones.html)
3.  [https://www.biobiochile.cl/especial/noticias/reportajes/reportajes-reportajes/2018/07/18/hackeo-interno-en-el-banco-de-chile-informatico-robo-475-millones-de-pesos-usando-su-pc.shtml](https://www.biobiochile.cl/especial/noticias/reportajes/reportajes-reportajes/2018/07/18/hackeo-interno-en-el-banco-de-chile-informatico-robo-475-millones-de-pesos-usando-su-pc.shtml)
4.  [https://twitter.com/felipeharboe/status/1022290787655213056](https://twitter.com/felipeharboe/status/1022290787655213056)
5.  [https://www.fayerwayer.com/2018/07/lista-bancos-hackeo-filtracion-tarjetas-credito/](https://www.fayerwayer.com/2018/07/lista-bancos-hackeo-filtracion-tarjetas-credito/)
6.  [https://en.wikipedia.org/wiki/Carding\_(fraud)](https://en.wikipedia.org/wiki/Carding_\(fraud\))
7.  [https://www.syswaregroup.com/resource-centre/case-studies/banking/credit-card-fraud/](https://www.syswaregroup.com/resource-centre/case-studies/banking/credit-card-fraud/)
8.  [https://www.bindb.com/bin-list.html](https://www.bindb.com/bin-list.html)
9.  [https://www.fayerwayer.com/2018/07/filtracion-tarjetas-banco-de-chile/](https://www.fayerwayer.com/2018/07/filtracion-tarjetas-banco-de-chile/)
10.  [https://www.nytimes.com/2017/11/12/us/nsa-shadow-brokers.html](https://www.nytimes.com/2017/11/12/us/nsa-shadow-brokers.html)

  
\[+\] Saludos

#### [Source](http://www.blackploit.com/2018/07/hackeando-un-banco-3-hackear-14000.html)

<br/>
---
