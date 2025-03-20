---
title: "Docker y docker-compose para proyecto con Selenium y Python"
date: Wed, 22 Nov 2023 21:54:00 -0300
draft: false
type: posts
categories: 
- Dev
---
# Docker y docker-compose para proyecto con Selenium y Python

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgAk-HkuxaEqnpdaw6-67TJ7ihyphenhyphen8l0n9YJCVtwRqDogrbQN62ecU6TPNGJ2J2PcUAWYCdGlWg_lf9AKJOnccTIQC4amEberjZ7KB_F4ec0zRFzlAyuZrZ1Xk6YX0YetVK_EWgtbfUK_u8Cu4_HV9_UtB97ntCI7Yr5EecjySAgF2at7RwrUzd9rUanfsLW6/w640-h366/docker-selenium-python.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgAk-HkuxaEqnpdaw6-67TJ7ihyphenhyphen8l0n9YJCVtwRqDogrbQN62ecU6TPNGJ2J2PcUAWYCdGlWg_lf9AKJOnccTIQC4amEberjZ7KB_F4ec0zRFzlAyuZrZ1Xk6YX0YetVK_EWgtbfUK_u8Cu4_HV9_UtB97ntCI7Yr5EecjySAgF2at7RwrUzd9rUanfsLW6/s1792/docker-selenium-python.png)

  

Hace un tiempo estaba desarrollando un bot que interactuaba con una web. El bot estaba hecho en python y Selenium. Quería dockerizar la aplicación para correrla con un solo comando en cualquier parte y no me encontré con ninguna solución u opción que funcionara o que no tuviera que usar una imagen custom de docker, por lo que hize mi propia versión que corre con un docker seguro no custom en el cual se descargan e instalan todo lo necesario para poder correr tu script en python con selenium. También cree su docker-compose para hacerlo aún más sencillo y poder correrlo en modo gráfico y ver el navegador por pantalla si estoy testeando.

  

Aquí dejo lo archivos:

  

_**requeriments.txt**_

```
selenium==4.12.0
```

Se puede usar con otras versiones de Selenium.

  

_**main.py**_

```
from selenium.common.exceptions import NoSuchElementException
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium import webdriver
from selenium.webdriver import ActionChains
...

def main():
	tu_codigo.....

if __name__ == '__main__':
    main()
```

  

**_Dockerfile_**

```
FROM python:3.9

# install google chrome
RUN echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" | \
    tee -a /etc/apt/sources.list.d/google.list && \
    wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | \
    apt-key add - && \
    apt-get update && \
    apt-get install -y google-chrome-stable libxss1


RUN BROWSER_MAJOR=$(google-chrome --version | sed 's/Google Chrome \([0-9]*\).*/\1/g') && \
    wget https://googlechromelabs.github.io/chrome-for-testing/LATEST_RELEASE_${BROWSER_MAJOR} -O chrome_version && \
    wget https://storage.googleapis.com/chrome-for-testing-public/`cat chrome_version`/linux64/chromedriver-linux64.zip && \
    unzip chromedriver-linux64.zip && \
    mv chromedriver-linux64/chromedriver /usr/local/bin/ && \
    DRIVER_MAJOR=$(chromedriver --version | sed 's/ChromeDriver \([0-9]*\).*/\1/g') && \
    echo "chrome version: $BROWSER_MAJOR" && \
    echo "chromedriver version: $DRIVER_MAJOR" && \
    if [ $BROWSER_MAJOR != $DRIVER_MAJOR ]; then echo "VERSION MISMATCH"; exit 1; fi


COPY requirements.txt /app/
WORKDIR /app
RUN pip install --no-cache-dir -r requirements.txt

COPY main.py .

ENTRYPOINT ["python", "main.py"]
```

  

**_docker-compose.yml_**

```
version: '3.8'

services:
  selenium-script:
    build: .
    environment:
      - DISPLAY=unix$DISPLAY
    volumes:
      - /tmp/.X11-unix:/tmp/.X11-unix
      - .:/app
    privileged: true
    stdin_open: true
    tty: true
```

  

### **Modo de uso**

  

Para correrlo debes ejecutar:

```
docker-compose run selenium-script
```

  

Si quieres poder ver el navegador cuando corras el docker debes darle permiso al usuario docker para que acceda al servidor x11, que es la base de la interfaz gráfica de usuario en estos sistemas Unix.

```
xhost +local:docker
```

  
  
  
Espero les sirva  
Saludos!!!

#### [Source](http://www.blackploit.com/2023/11/docker-y-docker-compose-para-proyecto.html)

<br/>
---
