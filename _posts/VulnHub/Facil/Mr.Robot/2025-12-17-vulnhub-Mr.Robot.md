---
layout: post
title: "VulnHub - Mr. Robot"
date: 2025-12-16 10:00:00 +0100
description: >-
  Writeup completo de la máquina Mr. Robot de VulnHub, catalogada como de dificultad fácil.
categories: [VulnHub, Fácil, Linux]
tags: [VulnHub, writeup, Brute Force, WordPress]
author: D4rkh45h
pin: false
show_image_post: true
image: /assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/maquina.jpg
---

# VulnHub – Mr. Robot

## 🧠 Overview

**Mr. Robot** es una máquina de **dificultad fácil** disponible en **VulnHub**.  
Este laboratorio es ideal para practicar técnicas de **enumeración, análisis web, fuerza bruta, explotación de WordPress y escalada de privilegios en sistemas Linux**.

---

## 🔎 Escaneo de puertos

Comenzamos realizando un escaneo de puertos para identificar los servicios expuestos por la máquina objetivo.

Los puertos abiertos identificados son:

- **22** (SSH)
- **80** (HTTP)
- **443** (HTTPS)

![Cap1](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap1.png)

---

## 🕵️ Enumeración web

### 🌐 Página principal

Accedemos al servicio web que corre por el puerto **80** y observamos una página interactiva con estética de la serie *Mr. Robot*.

![Cap2](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap2.png)

---

### 🤖 Análisis de robots.txt

Revisamos el archivo `robots.txt` en busca de información sensible.

![Cap3](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap3.png)

En este archivo encontramos:

- Una **key**
- Un **diccionario de palabras**

Descargamos el diccionario para utilizarlo en fases posteriores.

---

### 📂 Fuzzing de directorios

Realizamos fuzzing para descubrir rutas ocultas dentro del sitio web.

![Cap4](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap4.png)

Gracias a este proceso identificamos el panel de login de WordPress:

**http://mrobot.local/wp-login.php**

![Cap5](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap5.png)

---

### ⚠️ Identificación de vulnerabilidad

Detectamos que el sitio utiliza **WordPress 4.3.1**, una versión con vulnerabilidades conocidas.

![Cap6](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap6.png)

Probamos usuarios comunes de WordPress y, teniendo en cuenta la temática de la máquina, utilizamos:

- **elliot**

Confirmamos que el usuario es válido.

---

### 🔓 Ataque de fuerza bruta

Utilizamos el diccionario obtenido desde `robots.txt` para realizar un ataque de fuerza bruta contra el panel de login.

Tras un tiempo, conseguimos las credenciales correctas y accedemos al panel de administración de WordPress.

![Cap7](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap7.png)

---

## 🚪 Acceso inicial

### 🐚 Obtención de Reverse Shell

Desde el panel de WordPress, editamos el archivo `404.php` del tema activo e insertamos una **reverse shell**.

Accedemos a la página modificada y recibimos la conexión en nuestra máquina atacante.

![Cap8](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap8.png)

---

## 👤 Enumeración de usuarios

Una vez dentro del sistema, exploramos el directorio:

**/home/robot/**

Aquí encontramos un archivo que contiene un hash de contraseña:

**robot:c3fcd3d76192e4007dfb496cca67e13b**

![Cap9](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap9.png)

Identificamos el hash como **MD5** y, tras crackearlo, obtenemos la contraseña en texto claro:

**abcdefghijklmnopqrstuvwxyz**

Accedemos al sistema como el usuario **robot**.

---

## ⬆️ Escalada de privilegios

Buscamos binarios con permisos especiales utilizando `find`.

![Cap10](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap10.png)

Identificamos que el binario **nmap** puede ser explotado para escalar privilegios.

---

### 💥 Explotación de Nmap

Ejecutamos **nmap** en modo interactivo para escalar privilegios:

```bash
/usr/local/bin/nmap --interactive
sudo install -m =xs $(which nmap) .
LFILE=file_to_write
./nmap -oG=$LFILE DATA
```

🏁 Conclusión

Tras explotar una mala configuración del binario nmap, conseguimos acceso como root y control total del sistema.

✅ Máquina pwnd!
