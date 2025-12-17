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

## Descripción General

**Mr. Robot** es una máquina de **dificultad fácil** disponible en la plataforma **VulnHub**.  
En este laboratorio se practican técnicas como:

- Enumeración de servicios
- Análisis web
- Fuerza bruta
- Explotación de WordPress
- Escalada de privilegios en Linux

---

## Enumeración

### Escaneo de Puertos

Comenzamos realizando un escaneo de puertos para identificar los servicios expuestos por la máquina objetivo.

Los puertos detectados fueron:

- **22** (SSH)
- **80** (HTTP)
- **443** (HTTPS)

![Cap1](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap1.png)

---

## Enumeración Web

### Acceso a la Página Principal

Accedemos al servicio web que corre por el puerto **80**.

![Cap2](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap2.png)

---

### Análisis de `robots.txt`

Revisamos el archivo `robots.txt` para descubrir posibles rutas interesantes.

![Cap3](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap3.png)

En este archivo encontramos:

- Una **key**
- Un **diccionario de palabras**

Procedemos a descargar el diccionario para usarlo posteriormente.

---

### Fuzzing de Directorios

Realizamos un fuzzing para identificar directorios y archivos ocultos en la web.

![Cap4](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap4.png)

Gracias a esto, descubrimos la ruta del panel de login de WordPress:

**http://mrobot.local/wp-login.php**


![Cap5](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap5.png)

---

### Identificación de Vulnerabilidad

Detectamos que el sitio utiliza **WordPress 4.3.1**, versión que contiene vulnerabilidades conocidas.

![Cap6](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap6.png)

Probamos usuarios comunes de WordPress y, dado el contexto de la máquina, utilizamos el nombre del protagonista de la serie:

- **elliot**

Confirmamos que el usuario es válido.

---

### Ataque de Fuerza Bruta

Utilizamos el diccionario obtenido previamente para realizar un ataque de fuerza bruta contra el panel de login.

Tras un tiempo, conseguimos la contraseña y logramos acceso al panel de WordPress.

![Cap7](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap7.png)

---

## Explotación

### Obtención de Reverse Shell

Editamos el archivo `404.php` del tema activo e insertamos una reverse shell.  
Posteriormente, accedemos a dicha página desde el navegador para ejecutar el código.

![Cap8](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap8.png)

Con esto obtenemos acceso a la máquina.

---

## Acceso Inicial

Dentro del sistema, encontramos en el directorio:

**/home/robot/**

Una contraseña en formato hash:

**robot:c3fcd3d76192e4007dfb496cca67e13b**

Identificamos el tipo de hash utilizando `hash-identifier`.

![Cap9](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap9.png)

El hash corresponde a **MD5**.  
Tras crackearlo, obtenemos la contraseña en texto claro:

**abcdefghijklmnopqrstuvwxyz**

Accedemos al sistema como el usuario **robot**.

---

## Escalada de Privilegios

Ejecutamos el comando `find` para localizar binarios con permisos especiales.

![Cap10](/assets/img/maquinas/VulnHub/Mr.Robot/Mr.Robot_Cap/Mr.Robot_Cap10.png)

Detectamos que **nmap** puede ser explotado para escalar privilegios.

---

### Explotación de Nmap

Ejecutamos nmap en modo interactivo:

```bash
/usr/local/bin/nmap --interactive
sudo install -m =xs $(which nmap) .
LFILE=file_to_write
./nmap -oG=$LFILE DATA
```

Con privilegios de **root** obtenidos, la máquina queda completamente comprometida.

✅ **Machine pwnd!**

