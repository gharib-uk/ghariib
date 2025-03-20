---
title: "ESP32 Weather Dashboard with a WiFi Menu"
date: 2025-01-08
---

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fp9e6x9qho300nj7u2alr.png)  
I first made this project using Raspberry Pi Zero and Python (link), and then I re-made it using ESP32 and Arduino. My weather dashboard shows the current weather and a bar chart of 5-day high and low temperatures. There's also a menu interface for switching Wi-Fi connections.

Hardware setup: ESP32 paired with a Nextion NX8048T050 HMI touchscreen display on UART2.

Features:

- Weather data fetching from Open-Meteo API
- Utilizes WiFi library for network connection management
- Utilizes Nextion GUI designing commands to draw a 5-day weather bar chart
- Automatic location detection via IPInfo.io API
- Geocoder for location name resolution via Nominatim API

The next step is to set up all the binaries/elf and RTOS from scratch without using the Arduino framework. I don't know how hard would it be. I hope I'll make it in a few weeks.

Demo video: https://www.youtube.com/watch?v=S042fLQz42w

GitHub: https://github.com/blueskyson/esp32-weather

Go to Source
