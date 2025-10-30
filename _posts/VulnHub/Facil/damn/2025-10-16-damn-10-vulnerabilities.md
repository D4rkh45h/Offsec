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

![Cap18](/assets/img/maquinas/VulnHub/damn/File-Upload/File-Upload_Cap3.png)

Realizaremos el ataque y veremos que las acepta todas, con lo cual podríamos subir el archivo que quisieramos que nos dejaría 
subirlo sin darnos ningún error.

![Cap19](/assets/img/maquinas/VulnHub/damn/File-Upload/File-Upload_Cap4.png)
---



## SQL Injection

### Vista Rápida

Ahora veremos la vulnerabilidad de SQL Injection, para ver como llevar a cabo injecciones de este tipo.


![Cap20](/assets/img/maquinas/VulnHub/damn/SQL-Injection/SQL-Injection_Cap1.png)


### Codigo Fuente

Ahora veremos el codigo fuente para ver por donde podemos vulnerar el mismo código, en el código podemos ver que la sentencia devuelve dos parametros, con lo cual en la sentencia deberemos de introducir también otros dos parametros para que la sentencia se complete correctamente.

![Cap21](/assets/img/maquinas/VulnHub/damn/SQL-Injection/SQL-Injection_Cap2.png)

### Explotación de las Vulnerabilidades

Ahora interceptaremos la request, por medio de burpsuite y veremos como poder explotarlo.

Por medio de este comando **'UNION+SELECT+user,NULL%23**, podriamos sacar la versión de esta base de datos, en este caso para la base de datos de MariaDB.

El **%23** del final de la sentencia corresponde con la **#**, que se escribe asi para que la petición no interprete este simbolo, pero para que la base de datos si que lo interprete como un comentario y comente lo que queda de sentencia, por que si lo ponemos literalmente aparecerá un error ya que la petición lo interpretará y comentará todo lo que queda de petición a partir de el simbolo.

![Cap22](/assets/img/maquinas/VulnHub/damn/SQL-Injection/SQL-Injection_Cap3.png)
---


## SQL Injection(Blind)

### Vista Rápida

Primero miraremos un poco como se ve la página, en la cual explotaremos la vulnerabilidad de
SQL Injection(Blind), que se le llama así, por que la base de datos no nos proporcionar ningun error.

![Cap23](/assets/img/maquinas/VulnHub/damn/SQL-Injection(Blind)/SQL-Injection(Blind)_Cap1.png)

### Codigo Fuente

Ahora veremos el código fuente para ver por donde atacar esta máquina, veremos que la consulta devuelve dos parametros, esto
es muy importante de cara a la hora de realizar el ataque SQL Injection(Blind).

![Cap24](/assets/img/maquinas/VulnHub/damn/SQL-Injection(Blind)/SQL-Injection(Blind)_Cap2.png)

### Explotación de las Vulnerabilidades

Ahora recogeremos la petición con Burp Suite y la llevaremos al Repeater.

Una vez allí, introduciremos el siguiente comando para ver la vulnerabilidad, **id=1'+UNION+SELECT+SLEEP(10),NULL%23**, esto lo que hará
será que la base de datos tarde 10 segundos en respondernos, y podemos jugar con esta vulnerabilidad, por medio de
condiciones como, si en la tabla de usuarios hay un usuario que se llama Admin, SLEEP(10), y si ese usuario se encuentra en la base de datos,
la base de datos tardará 10 segundos en responder.

![Cap25](/assets/img/maquinas/VulnHub/damn/SQL-Injection(Blind)/SQL-Injection(Blind)_Cap3.png)
---

## Weak Session IDs

### Vista Rápida

Ahora entraremos dentro del apartado de Weak Session IDs, para ver esta vulnerabilidad a fondo.
La vulnerabilidad de "Weak Session ID" (ID de Sesión Débil) ocurre cuando el identificador que el
servidor le da a un usuario para recordar quién es, es predecible, como en este caso.

![Cap26](/assets/img/maquinas/VulnHub/damn/Weak-Session-IDs/Weak-Session-IDs_Cap1.png)

### Codigo Fuente

Ahora veremos el código fuente en busca de vulnerabilidades, dentro de este veremos que el identificador de cookie,
no se genera de una forma aleatoria, sino que se coje el numero de la anterior cookie creada y se le suma 1, 
con lo cual esto es un error ya que se debería generar aleatoriamente, para no poder saber las id de las cookies de los demás,
ya que si tu sabes que el id de la cookie que le dará al sigiente usuario será la ultima id de cookie que es la tuya, +1, solamente
sabiendo tu id de cookie, ya podras entrar dentro de la sesión del nuevo usuario.

![Cap27](/assets/img/maquinas/VulnHub/damn/Weak-Session-IDs/Weak-Session-IDs_Cap2.png)

### Explotación de las Vulnerabilidades

Ahora explotaremos esta vulnerabilidad, entrando en la sesión de otro usuario ya que sabemos que el id de las cookies es secuencial,
es decir que va de esta forma, dvwaSession=1, dvwaSession=2, dvwaSession=3, dvwaSession=4, dvwaSession=5, etc. Sabiendo esto entraremos en la sesión de otro usuario.

![Cap28](/assets/img/maquinas/VulnHub/damn/Weak-Session-IDs/Weak-Session-IDs_Cap3.png)

Ahora cambiaremos el id a 2 para entrar en la sesión del usuario el cual su dvwaSession corresponde al número 2.

![Cap29](/assets/img/maquinas/VulnHub/damn/Weak-Session-IDs/Weak-Session-IDs_Cap4.png)



## xss(DOM)

### Vista Rápida

Ahora entraremos dentro del apartado de XSS(DOM), para ver de que se trata esta vulnerabilidad, pero antes
buscaremos la definición de esta vulnerabilidad.
Es un tipo de vulnerabilidad de seguridad web que ocurre cuando una aplicación web contiene código del lado
del cliente que procesa datos de una fuente no confiable de una manera insegura.
A diferencia de otros tipos de XSS, en un ataque de DOM XSS, la carga maliciosa se ejecuta
como resultado de la modificación del entorno del DOM en el navegador de la víctima.

![Cap30](/assets/img/maquinas/VulnHub/damn/XSS(DOM)/XSS(DOM)_Cap1.png)

### Codigo Fuente

Ahora veremos el código fuente, veremos que en este codigo fuente no sacaremos mucho en claro.

![Cap31](/assets/img/maquinas/VulnHub/damn/XSS(DOM)/XSS(DOM)_Cap2.png)

### Explotación de las Vulnerabilidades

Ahora intentaremos explotar esta vulnerabilidad, para ello primero elegiremos un idioma y
clicaremos en select, una vez que hayamos hecho esto veremos que la url cambia, probaremos a inyectar el código
en este sitio, para ver que ocurre.

![Cap32](/assets/img/maquinas/VulnHub/damn/XSS(DOM)/XSS(DOM)_Cap3.png)

El código que inyectaremos sera este, ```<script>alert(1);</script>```, si nos aparece una alerta, esta parte de la web
es vulnerable a código javascript, con lo cual es una vulnerabilidad XSS en este caso de tipo DOM.

![Cap33](/assets/img/maquinas/VulnHub/damn/XSS(DOM)/XSS(DOM)_Cap4.png)
---


## xss(Reflected)

### Vista Rápida

Ahora entraremos dentro de este otro apartado para ver de que se trata esta vulnerabilidad.
En un ataque XSS Reflejado, el código malicioso viaja al servidor a través de la URL (en un enlace), y el servidor lo refleja o hace eco de vuelta en la respuesta HTML sin validarlo. El navegador de la víctima recibe este código y, al confiar en la respuesta del servidor, lo ejecuta.

La clave es: **El ataque no se almacena en el sitio web.** Solo funciona si la víctima hace clic en el enlace malicioso que ha preparado el atacante.

![Cap34](/assets/img/maquinas/VulnHub/damn/XSS(Reflected)/XSS(Reflected)_Cap1.png)

### Codigo Fuente

Ahora veremos el código fuente haber si detectamos alguna vulnerabilidad desde aquí, y veremos que en el código no hay nada que impida meter código
javascript.

![Cap35](/assets/img/maquinas/VulnHub/damn/XSS(Reflected)/XSS(Reflected)_Cap2.png)

### Explotación de las Vulnerabilidades

Ahora inyectaremos el mismo código de antes dentro de este campo de texto para ver que pasa.

![Cap36](/assets/img/maquinas/VulnHub/damn/XSS(Reflected)/XSS(Reflected)_Cap3.png)

![Cap37](/assets/img/maquinas/VulnHub/damn/XSS(Reflected)/XSS(Reflected)_Cap4.png)

Veremos que parece que hace lo mismo, pero recordamos que este ataque no se guarda dentro del sitio web del cliente, que somos nosotros,
sino que solo funciona si la víctima hace click en el enlace malicioso que ha preparado el atacante.
---


## xss(Stored)

### Vista Rápida

Ahora entraremos dentro de este otro apartado para ver la vulnerabilidad de XSS de tipo Stored, que se basa en:

El XSS Stored se encuentra en funcionalidades donde los usuarios pueden introducir datos que serán vistos por otras personas.

![Cap38](/assets/img/maquinas/VulnHub/damn/XSS(Stored)/XSS(Stored)_Cap1.png)

### Codigo Fuente

Ahora veremos el código para ver si tiene alguna vulnerabilidad, y veremos que no tiene ningun control, para este tipo de 
vulnerabilidad.

![Cap39](/assets/img/maquinas/VulnHub/damn/XSS(Stored)/XSS(Stored)_Cap2.png)

### Explotación de las Vulnerabilidades

Ahora introduciremos este código con el nombre que nosotros queramos, **<h1 style="color:red;font-size:48px;">PAGINA VULNERABLE</h1><style>body { background-color: black; }</style>**, y veremos como aparece un texto como el de la imagen, el cual se quedará permanente mente dentro de la página y lo verán
todos los usuarios que entren en ella, al ser un XSS de tipo Stored.

![Cap40](/assets/img/maquinas/VulnHub/damn/XSS(Stored)/XSS(Stored)_Cap3.png)
---

## CSP Bypass

### Vista Rápida

Ahora veremos una de las últimas vulnerabilidades que es la de CSP Bypass, la cual consiste en ver
si el **Content Security Policy**, esta bien definido, y si vemos que tiene alguna vulnerabilidad, intentar
explotarla.

![Cap41](/assets/img/maquinas/VulnHub/damn/CSP-Bypass/CSP-Bypass_Cap1.png)

### Codigo Fuente

Ahora veremos el código fuente, veremos que el Content Securty Policy, no esta bien definido, ya que
nos deja introducir código de pastebin, que es un lugar público en el cual cualquier persona puede poner código
y mandarlo a esta página y ejecutarlo desde aquí, en un principio explotaríamos esta vulnerabilidad por medio de pastebin,
pero la máquinan no tiene conexión a internet, con lo cual la explotaremos de forma local subiendo un arhivo malicioso, ya que 
**tambien ejecuta scripts que esten subidos dentro de esta máquina**, devido a que esta definido en el CSP.

![Cap42](/assets/img/maquinas/VulnHub/damn/CSP-Bypass/CSP-Bypass_Cap2.png)

### Explotación de las Vulnerabilidades

Ahora lo primero que haremos será subir un código malicioso dentro de la máquina por medio del file upload, el código del script es este:
**
// Este script cambiará toda la página para confirmar el éxito.
document.body.style.backgroundColor = '#1a1a1a';
document.body.innerHTML = '<h1 style="color: lime; font-size: 4em; text-align: center; margin-top: 20%;">CSP Bypassed using \'self\'!</h1>';

console.log('Si ves esto en la consola, el exploit ha funcionado.');**

Lo guardaremos con la extensión .js y lo subiremos a la máquina.

![Cap43](/assets/img/maquinas/VulnHub/damn/CSP-Bypass/CSP-Bypass_Cap3.png)

Ahora veremos si se ha subido correctamente.

![Cap44](/assets/img/maquinas/VulnHub/damn/CSP-Bypass/CSP-Bypass_Cap4.png)

Una vez comprobado que lo hemos subido correctamente, lo buscaremos por medio del campo de texto de la vulnerabilidad de CSP Bypass,
para que se ejecute el código que hay dentro de este script, y veremos que el .js se ejecutará correctamente.

![Cap45](/assets/img/maquinas/VulnHub/damn/CSP-Bypass/CSP-Bypass_Cap5.png)

Lo buscaremos con este comando **../../hackable/uploads/exploit.js**

![Cap46](/assets/img/maquinas/VulnHub/damn/CSP-Bypass/CSP-Bypass_Cap6.png)

Una vez que le demos a include, veremos como se habrá ejecutado el código que tenía el .js que hemos creado.

