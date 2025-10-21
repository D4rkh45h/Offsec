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

Ahora explotaremos esta vulnerabilidad por medio de este comando **120.0.0.1 && whoami**,
con el cual veremos que nos dirá que usuario esta
lanzando este comando.

![Cap6](/assets/img/maquinas/VulnHub/damn/Command-Injection/Command-Injection_Cap3.png)

No debemos de confundir esta vulnerabilidad que es Command Injection, con la vulnerabilidad de HTML Injection, ya que la 
vulnerabilidad de Command Injection es la que hemos visto, ejecutando codigo que la máquina entiende, pero en el HTML Injection,
solo podriamos introducir comandos de HTML, que sería los unicos interpretados por la máquina. 
---


## CSRF (Cross-Site Request Forgery)

### Vista Rápida

Ahora entraremos dentro de el apartado de CSRF, para ver esta vulnerabilidad, y lo que podemos hacer con esta.

![Cap7](/assets/img/maquinas/VulnHub/damn/CSRF/CSRF_Cap1.png)

### Codigo Fuente

Ahora veremos el código fuente para ver por donde podemos atacar esta máquina, dentro del código podemos ver que,
la contraseña se cambia por medio del método get, y esto es incorrecto, ya que debería de ser una petición POST,
esto tiene un vulnerabilidad, la cual es CSRF, con la cual le podremos cambiar la contraseña a el usuario que pinche en este
enlace sin que el lo sepa.

Un atacante puede construir una URL maliciosa que, al ser visitada por un usuario autenticado en la aplicación vulnerable, ejecutará automáticamente un cambio de contraseña sin su consentimiento.

![Cap8](/assets/img/maquinas/VulnHub/damn/CSRF/CSRF_Cap2.png)

### Explotación de las Vulnerabilidades

Ahora cambiaremos la contraseña del usuario, le pondremos por ejemplo 123, y copiaremos la url que nos genera.

![Cap9](/assets/img/maquinas/VulnHub/damn/CSRF/CSRF_Cap3.png)

Tras esto, copiaremos la url y la mandaremos por correo a otro usuario que este registrado en esta página.

**http://90.74.118.215:9292/vulnerabilities/csrf/?password_new=123&password_conf=123&Change=Change#**

![Cap10](/assets/img/maquinas/VulnHub/damn/CSRF/CSRF_Cap4.png)

Le mándaremos un correo al usuario.

![Cap11](/assets/img/maquinas/VulnHub/damn/CSRF/CSRF_Cap5.png)

Ahora este usuario entrará dentro de esta url, de la cual le hemos cambiado el nombre para que parezca real, y veremos la mágia de esto, lo que hará será cambiar la contraseña por la que le hemos puesto nosotros.

![Cap12](/assets/img/maquinas/VulnHub/damn/CSRF/CSRF_Cap6.png)

Veremos como se le ha cambiado la contraseña al usuario por la que nosotros hemos puesto. También podemos hacerlo de una forma 
un poco más profesional, para que después de pasar por aqui nos lleve a instagram, añadiendole algo a la url http://90.74.118.215:9292/vulnerabilities/csrf/?password_new=123&password_conf=123&Change=Change#&redirect=https://www.instagram.com, al añadirle esto **&redirect=https://www.instagram.com**, después de pasar por esta url se dirigirá a instagram, y parecerá que el enlace no ha hecho nada raro.
---



## Local File Inclusion (LFI)

### Vista Rápida

Ahora entraremos dentro del apartado de Local File Inclusion, para ver en que consiste esta vulnerabilidad.

![Cap13](/assets/img/maquinas/VulnHub/damn/LFI/LFI_Cap1.png)

### Codigo Fuente

Ahora veremos el código el cual veremos que no tiene nada en especial.

![Cap14](/assets/img/maquinas/VulnHub/damn/LFI/LFI_Cap2.png)

### Explotación de las Vulnerabilidades

Veremos que desde esta url podremos acceder a fichero locales de la máquina a los cuales no deberiamos de poder acceder, y veríamos contenido sensible, como por ejemplo en el siguiente caso.

![Cap15](/assets/img/maquinas/VulnHub/damn/LFI/LFI_Cap3.png)

Veremos que si en la url, a partir del page=, ponemos estos comandos para llegar a la raíz de la máquina y luego entrar dentro de la carpeta de usuarios **../../../../../etc/passwd**, podremos entrar dentro de la carpeta de usuarios y ver todos los usuarios que tiene la máquina. Para verlos como en la segunda captura haremos Ctrl + u.

![Cap15](/assets/img/maquinas/VulnHub/damn/LFI/LFI_Cap4.png)
---




## File Upload

### Vista Rápida

Ahora veremos la vulnerabilidad de File upload, para ello entraremos dentro del apartado de File Upload, para ver la misma.

![Cap16](/assets/img/maquinas/VulnHub/damn/File-Upload/File-Upload_Cap1.png)

### Codigo Fuente

Ahora veremos el codigo fuente para ver si podemos ver alguna vulnerabilidad a través de esta. Por medio de este codigo fuente podemos ver
que no tiene ningún control en cuanto a los archivos que se suben por lo cual podriamos subir una reverse shell para obtener el control
de la máquina o incluso cosas peores. Pero vamos a comprobarlo con Burp Suite.

![Cap17](/assets/img/maquinas/VulnHub/damn/File-Upload/File-Upload_Cap2.png)

### Explotación de las Vulnerabilidades

Ahora capturaremos la petición y la llevaremos al intrududer para probar distintas extensiones y ver si las acepta.

![Cap17](/assets/img/maquinas/VulnHub/damn/File-Upload/File-Upload_Cap3.png)

Realizaremos el ataque y veremos que las acepta todas, con lo cual podríamos subir el archivo que quisieramos que nos dejaría 
subirlo sin darnos ningún error.

![Cap17](/assets/img/maquinas/VulnHub/damn/File-Upload/File-Upload_Cap4.png)

