---
layout: post
title: "VulnHub - Breakout"
date: 2025-12-16 10:00:00 +0100
description: >-
  Writeup completo de la máquina Breakout de VulnHub, catalogada como de dificultad fácil.
categories: [VulnHub, Fácil, Linux]
tags: [VulnHub, writeup, Breakout, Brute Force]
author: D4rkh45h
pin: false
show_image_post: true
image: /assets/img/maquinas/VulnHub/breakout/Breakout_Cap/maquina.jpg
---

# VulnHub - Breakout

## 🧠 Overview

**Breakout** es una máquina de **dificultad fácil** disponible en VulnHub.  
En este laboratorio se trabajan técnicas básicas de **enumeración, análisis web, obtención de credenciales, reverse shell y escalada de privilegios**, ideal para practicar fundamentos de pentesting en entornos Linux.

---

## 🔎 Escaneo de puertos

Comenzamos realizando un escaneo de puertos para identificar los servicios expuestos por la máquina objetivo.

En la siguiente captura podemos observar que los puertos abiertos son:

- 139
- 445
- 80
- 10000
- 20000

![Cap1](/assets/img/maquinas/VulnHub/breakout/Breakout_Cap/Breakout_Cap1.png)

---

## 🕵️ Búsqueda de información

### 🌐 Página web

Accedemos al servicio web que corre por el puerto 80.  
Una vez dentro, inspeccionamos el **código fuente** en busca de información interesante y encontramos unas **credenciales codificadas**.

![Cap2](/assets/img/maquinas/VulnHub/breakout/Breakout_Cap/Breakout_Cap2.png)

Copiamos estas credenciales y las llevamos a una página de decodificación para obtener la contraseña en texto claro.

![Cap3](/assets/img/maquinas/VulnHub/breakout/Breakout_Cap/Breakout_Cap3.png)

Guardamos la contraseña obtenida en nuestro equipo para utilizarla más adelante.

![Cap4](/assets/img/maquinas/VulnHub/breakout/Breakout_Cap/Breakout_Cap4.png)

---

## 🔐 Servicios adicionales

A continuación, investigamos los otros puertos abiertos que resultaban sospechosos, concretamente el **10000** y el **20000**.

![Cap5](/assets/img/maquinas/VulnHub/breakout/Breakout_Cap/Breakout_Cap5.png)

Detrás de estos puertos encontramos un **panel de autenticación**.

![Cap6](/assets/img/maquinas/VulnHub/breakout/Breakout_Cap/Breakout_Cap6.png)

Probamos las credenciales obtenidas anteriormente y conseguimos autenticarnos correctamente.

![Cap7](/assets/img/maquinas/VulnHub/breakout/Breakout_Cap/Breakout_Cap7.png)

---

## 👤 Enumeración de usuarios

Utilizamos la herramienta **enum4linux** para enumerar usuarios del sistema a través del servicio SMB.

![Cap8](/assets/img/maquinas/VulnHub/breakout/Breakout_Cap/Breakout_Cap8.png)

En la siguiente captura podemos ver que se identifica un usuario válido.

![Cap9](/assets/img/maquinas/VulnHub/breakout/Breakout_Cap/Breakout_Cap9.png)

---

## 🚪 Acceso inicial

### Autenticación web y Reverse Shell

Nos autenticamos en la aplicación web utilizando el usuario y la contraseña obtenidos.  
Dentro de la aplicación encontramos una funcionalidad que permite ejecutar comandos del sistema, lo que aprovechamos para lanzarnos una **reverse shell**.

También aprovechamos este acceso para obtener la **flag de usuario**.

![Cap10](/assets/img/maquinas/VulnHub/breakout/Breakout_Cap/Breakout_Cap10.png)

En la siguiente captura vemos cómo recibimos la conexión en nuestra máquina atacante.  
Además, comprobamos que el usuario tiene permiso para ejecutar el comando `tar`, lo cual será clave más adelante.

![Cap11](/assets/img/maquinas/VulnHub/breakout/Breakout_Cap/Breakout_Cap11.png)

---

## ⬆️ Escalada de privilegios

Una vez dentro del sistema, comenzamos a buscar archivos interesantes.  
En el directorio **backups** encontramos un archivo llamado `old_pass`.

![Cap12](/assets/img/maquinas/VulnHub/breakout/Breakout_Cap/Breakout_Cap12.png)

Utilizamos el comando `tar` para copiar este archivo a nuestro directorio personal.

![Cap13](/assets/img/maquinas/VulnHub/breakout/Breakout_Cap/Breakout_Cap13.png)

Al leer su contenido, descubrimos que contiene la **contraseña del usuario root**.  
Nos autenticamos como root y obtenemos control total del sistema.

![Cap14](/assets/img/maquinas/VulnHub/breakout/Breakout_Cap/Breakout_Cap14.png)

Finalmente, la máquina queda completamente comprometida (**pwnd**) y podemos capturar la **flag de root**.

![Cap15](/assets/img/maquinas/VulnHub/breakout/Breakout_Cap/Breakout_Cap15.png)
