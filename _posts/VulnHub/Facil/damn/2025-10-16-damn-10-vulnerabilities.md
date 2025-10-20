---
layout: post
title: "VulnHub - damn 10 Vulnerabilities"
date: 2025-10-16 10:00:00 +0100 # Fecha y hora de publicación (YYYY-MM-DD HH:MM:SS +ZonaHoraria)
description: >-
  Este es un writeup de la máquina de VulnHub, damn 10 Vulnerabilities, la 1.10.
categories: [VulnHub, Facil, Linux]
tags: [Vulnhub, writeup, damn 10 Vulnerabilities, Brute Force]
author: D4rkh45h
pin: false
show_image_post: true
image: /assets/img/maquinas/VulnHub/damn/maquina.png # Asegúrate de que esta imagen exista
---

## 💻 VulnHub - DAMN 10 Vulnerabilities

### Overview

**damn 10 Vulnerabilities** fue una máquina de dificultad **Fácil** que me permitió explorar las 10 vulnerabilidades más comunes en páginas web.

La Máquina se compone de diversos apartados con diversas vulnerabilidades, iremos resolviendo uno a uno cada uno de estos apartados.

## Brute Force

### Vista Rápida

Una vez dentro de la página, veremos que tenemos un panel de login.

![Cap1](/assets/img/maquinas/VulnHub/damn/Brute-Force/fuerza_bruta-1Captura.png)

### Codigo Fuente

Ahora veremos el codigo fuente para ver las vulnerabilidades del mismo.

![Cap2](/assets/img/maquinas/VulnHub/damn/Brute-Force/fuerza_bruta-2Captura.png)

### Explotación de las Vulnerabilidades

Ahora veremos que dentro del codigo fuente tiene una vulnerabilidad, la cual se basa en que gracias a que podemos ver la consulta sql, sabemos que podemos insertar una sentencia la cual nos dará el acceso a la base de datos user = 'admin' OR '1'='1--, esta se colocaría en el campo de admin y ya tendriamos acceso a la base de datos.

![Cap3](/assets/img/maquinas/VulnHub/damn/Brute-Force/fuerza_bruta-3Captura.png)
---

## Command Injection

### Vista Rápida

Ahora entraremos al apartado de **Command Injection**, para ver esta vulnerabilidad.

![Cap4](/assets/img/maquinas/VulnHub/damn/Command-Injection/Command-Injection_Cap1.png)

### Codigo Fuente

Ahora veremos un poco el codigo fuente para ver las vulnerabilidades que tiene en este apartado. Podemos ver que en este codigo nos permitiría
inyectar código malicioso para poder ver fichero o incluso una reverse shell, que nos podría dar el control de la máquina.

![Cap5](/assets/img/maquinas/VulnHub/damn/Command-Injection/Command-Injection_Cap2.png)

### Explotación de las Vulnerabilidades

Ahora explotaremos esta vulnerabilidad por medio de este comando 120.0.0.1 && whoami,
con el cual veremos que nos dirá que usuario esta
lanzando este comando.

![Cap6](/assets/img/maquinas/VulnHub/damn/Command-Injection/Command-Injection_Cap3.png)