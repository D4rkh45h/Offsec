---
layout: post
title: "VulnHub - Empire"
date: 2026-01-05 10:00:00 +0100
description: >-
  Writeup completo de la máquina Empire de VulnHub, catalogada como de dificultad fácil.
categories: [VulnHub, Fácil, Linux]
tags: [VulnHub, writeup]
author: D4rkh45h
pin: false
show_image_post: true
image: /assets/img/maquinas/VulnHub/empire/empire_Cap/maquina.jpg
---

# VulnHub – Empire

## 🧠 Overview

**Empire** es una máquina de **dificultad fácil** disponible en **VulnHub**.  
Este laboratorio es ideal para practicar **enumeración web, análisis de contenido oculto, uso de claves SSH y escalada de privilegios en sistemas Linux**.

---

## 🔎 Escaneo de puertos

Comenzamos realizando un escaneo de puertos para identificar los servicios expuestos por la máquina objetivo.

Los puertos abiertos identificados son:

- **22** (SSH)
- **80** (HTTP)

![Cap1](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap1.png)

---

## 🕵️ Enumeración web

### 🌐 Página principal

Accedemos al servicio web que corre por el puerto **80** y observamos una página con temática de *Mr. Robot*.

![Cap2](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap2.png)

A simple vista la página no nos proporciona información relevante, por lo que continuamos con técnicas de enumeración.

---

### 📂 Fuzzing de directorios

Realizamos fuzzing para descubrir rutas ocultas dentro del sitio web.

![Cap3](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap3.png)

Durante el fuzzing encontramos una ruta interesante llamada:

/~secret

![Cap4](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap4.png)

---

### 🔑 Página secreta

Al acceder a esta página, observamos una pista importante:  
se menciona la existencia de una **clave privada SSH**, junto con información adicional útil.

![Cap5](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap5.png)

Continuamos realizando un segundo fuzzing dentro de esta ruta para localizar dicha clave.

![Cap6](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap6.png)

---

### 🗝️ Obtención de la private key

Accedemos a la ruta descubierta y encontramos la **clave privada SSH**, aunque se encuentra **ofuscada**.

![Cap7](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap7.png)

Para desofuscarla utilizamos **CyberChef**, probando distintos métodos hasta determinar que el cifrado utilizado es **Base58**.

![Cap8](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap8.png)

Una vez decodificada, guardamos la clave privada en nuestra máquina atacante.

---

## 🔓 Crackeo de la clave SSH

Utilizamos `ssh2john` para extraer el hash de la clave privada y posteriormente empleamos **John the Ripper** para obtener la contraseña.

![Cap9](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap9.png)

La contraseña de la clave privada es:

P@55w0rd!

---

## 🚪 Acceso inicial

Con la **clave privada**, su **contraseña** y el **usuario indicado en la página web**, accedemos correctamente al servicio SSH.

![Cap10](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap10.png)

Una vez dentro del sistema, establecemos una **reverse shell** para trabajar de forma más cómoda.

![Cap11](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap11.png)

---

## ⬆️ Escalada de privilegios

Comenzamos la enumeración local para buscar posibles vías de escalada.

### 🔍 Revisión de permisos sudo

Al ejecutar `sudo -l`, observamos que tenemos permisos sobre una **librería modificable**, lo cual representa un posible vector de ataque.

![Cap12](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap12.png)

---

### 🧰 Uso de linPEAS

Descargamos y ejecutamos **linPEAS** para identificar configuraciones inseguras dentro del sistema.

![Cap13](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap13.png)

Una vez localizado el archivo vulnerable, procedemos a modificarlo.

![Cap14](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap14.png)

---

### 👤 Cambio de usuario

Modificamos el archivo y cambiamos de usuario.  
Observamos que este usuario **no requiere contraseña** para iniciar sesión.

![Cap15](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap15.png)

---

### 💥 Explotación con pip

Al ejecutar nuevamente `sudo -l`, detectamos que podemos usar **pip como root sin contraseña**, lo cual es una **grave mala configuración**.

![Cap16](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap16.png)

Ejecutamos los comandos necesarios para abusar de esta configuración y escalar privilegios.

![Cap17](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap17.png)

---

## 🏁 Root y flag final

Tras la explotación, conseguimos acceso como **root**.

![Cap18](/assets/img/maquinas/VulnHub/empire/empire_Cap/empire_Cap18.png)

Finalmente, leemos el archivo **root.txt** y completamos la máquina.

✅ **Máquina pwnd!**


