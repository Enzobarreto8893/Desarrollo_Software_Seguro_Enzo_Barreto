# Universidad Católica del Uruguay

## Facultad de Ingeniería y Tecnologías

### Desarrollo de Software Seguro

#### Unidad Temática 1: Trabajo Obligatorio

#### Integrantes:

- Enzo Barreto
- Guillermo RIvero

<br/>
<br/>

## Consigna

> El propósito de este trabajo obligatorio es crear un Write up en formato Markdown 9 que detalle el proceso de implementación y configuración de un ambiente destinado a la prueba de aplicaciones. Este ambiente será empleado en diversas actividades a desarrollar a lo largo del curso.
> El mismo debe contener por lo menos la siguiente información:
> * Instalación de una máquina virtual de Kali Linux en VirtualBox o VMware.
> * Instalación (si es necesario) de un proxy de interceptació (ZAP o BURP).
> * Instalación de Docker en la máquina virtual de Kali Linux.
> * Ejecución de un contenedor con OWASP Juice Shop.
> * Prueba de la visualización del tráfico en el proxy de interceptación seleccionado.
<br/>

--- 
## Introducción

En este informe se detallará el proceso de instalación y configuración de un ambiente destinado a la prueba de aplicaciones. Así como también la instalación de una máquina virtual de Kali Linux en VirtualBox, la instalación de un proxy de interceptación (ZAP), la instalación de Docker en la máquina virtual de Kali Linux, la ejecución de un contenedor con OWASP Juice Shop y la prueba de la visualización del tráfico en el proxy de interceptación seleccionado.
<br/>

## Preparación del ambiente de trabajo

### Instalación de VirtualBox
Nuestro equipo decidió instalar VirtualBox por un tema de performance y de disponibilidad de recursos. Para la instalación de VirtualBox se siguió el siguiente [procedimiento](https://www.youtube.com/watch?v=QVqfKjXxKzg) que figura en el sitio oficial de VirtualBox. 
La configuración utilizada fue la recomendada por el instalador, con la excepción de la memoria RAM que se le asignó a la máquina virtual, la cual fue de 4GB.
<br/>

### Instalación de Kali Linux

**¿Que es Kali Linux?**

Kali Linux es una distribución de Linux basada en Debian enfocada en la seguridad informática. Es un sistema operativo que se ejecuta en la máquina virtual y que permite realizar pruebas de seguridad en aplicaciones web.

El procedimiento adoptado para la instalación de Kali Linux fue el siguiente:

1. Descargar la imagen de Kali Linux desde el [sitio oficial](https://www.kali.org/get-kali/#kali-virtual-machines).
2. Elegimos el programa VirtualBox para la instalación de Kali Linux en la versión 64 bits.
3. Una vez que se descargó la imagen, se procedió a descomprimir el archivo. El cual contenía un archivo `.vbox` y un archivo `.vdi`. 
El archivo `.vbox` es el archivo de configuración de la máquina virtual y el archivo `.vdi` es el disco duro virtual.
4. Como el archivo `.vbox` es el archivo de configuración de la máquina virtual, se procedió a importar la máquina virtual en VirtualBox. Para ello, se seleccionó la opción `Archivo -> Importar servicio virtualizado` y se seleccionó el archivo `.vbox` que se había descargado.
5. Una vez importada la máquina virtual, se procedió a configurarla. Para ello, se seleccionó la opción `Configuración` y se configuró la memoria RAM en 4GB y la cantidad de procesadores en 2. Lo que ofrece a la máquina virtual un rendimiento aceptable para el desarrollo de las actividades del curso.
6. Una vez configurada la máquina virtual, se procedió a iniciarla. Para ello, se seleccionó la opción `Iniciar` y se esperó a que se inicie el sistema operativo.
7. Una vez iniciado el SO, se ingresaron las credenciales por defecto que son `kali` como usuario y `kali` como contraseña. Y desde una terminal se procedió a cambiar el password del usuario `root` con el comando `passwd root` y el propio de `kali` con el comando `passwd kali`. 

> `passwd` es un comando que permite cambiar la contraseña de un usuario. Si se ejecuta sin argumentos, cambia la contraseña del usuario actual. Si se ejecuta con el nombre de un usuario como argumento, cambia la contraseña de ese usuario. 

```bash
sudo passwd root

sudo passwd kali
```
> `sudo` es un comando que permite ejecutar comandos con privilegios de superusuario.

8. Una vez cambiadas las contraseñas, se procedió a actualizar el sistema operativo con el comando `sudo apt-get update && sudo apt-get upgrade -y`.

```bash
sudo apt-get update && sudo apt-get upgrade -y
```
> `apt-get` es un comando que permite instalar, actualizar y eliminar paquetes de software en el sistema operativo.
> `update` es un comando que permite actualizar la lista de paquetes disponibles.
> `upgrade` es un comando que permite actualizar los paquetes instalados.

Una vez finalizada la actualización, se procedió a reiniciar la máquina virtual. 

Al iniciar nuevamente con las credenciales actualizadas y el equipo con las ultimas actualizaciones ya tenemos el sistema operativo listo para comenzar a trabajar.
<br/>

### Instalación de Burp

---
**¿Que es Burp?**

Burp Suite es una herramienta para realizar en análisis de seguridad de aplicaciones Web, la podemos llamar de fácil uso, ya que cuenta con la integración de componentes que permiten realizar el análisis de forma automatizado.

La distribución de Kali ya cuenta con Burp Suite instalado, por lo que no fue necesario instalarlo. Para ejecutarlo, se debe abrir una terminal y ejecutar el comando `burpsuite`.


<img src="/Users/enzobarreto/Desktop/UCU/2doSEM25/Desarrollo Seguro/Desarrollo_Software_Seguro_Enzo_Barreto/UT1/imagenes/burp-inicio.png" alt="BurpSuite en menú" width=400 height=auto>

```bash
kali@kali:~$ burpsuite
``` 
O simplemente buscar la aplicación en el menú de aplicaciones.


### Configuración de TLS en Burp

Para poder interceptar el tráfico HTTPS, se debe instalar el certificado de Burp en el navegador. Para ello, se debe abrir el navegador y acceder a la URL `http://burp` y se debe descargar el certificado.

Para interceptar el tráfico desde Burp vamos a la pestaña `Proxy` y seleccionamos la opción `Intercept is on`.



Desde el navegador de Firefox, abrimos la pestaña `Settings` y buscamos la opción `Proxy`.

<img src="/Users/enzobarreto/Desktop/UCU/2doSEM25/Desarrollo Seguro/Desarrollo_Software_Seguro_Enzo_Barreto/UT1/imagenes/cargar-proxy-manual-para-burp.png" alt="Proxy http en 127.0.0.1:8080" width=auto height=auto>

Con el proxy configurado, abrimos la URL `http://burp` y descargamos el certificado CA de Burp.

<img src="/Users/enzobarreto/Desktop/UCU/2doSEM25/Desarrollo Seguro/Desarrollo_Software_Seguro_Enzo_Barreto/UT1/imagenes/pagina-burpsuite.png" alt="Descarga de certificados" width=auto height=auto>

Estos certificados se utilizan para autenticar la identidad de entidades en línea, como sitios web, y garantizar la seguridad de las comunicaciones cifradas mediante tecnologías como SSL/TLS. La CA verifica la identidad de la entidad solicitante y firma digitalmente el certificado, lo que garantiza la integridad y autenticidad de la información.

Para configurar el certificado en Firefox, se debe ir a la pestaña `Settings` y  desde la pestaña de `Privacy & Security` se debe seleccionar la opción `View Certificates`.

<img src="/Users/enzobarreto/Desktop/UCU/2doSEM25/Desarrollo Seguro/Desarrollo_Software_Seguro_Enzo_Barreto/UT1/imagenes/import-CA-firefox.png" alt="Acceso a Certificados" width=auto height=auto>

Una vez que se abre la ventana de certificados, se debe seleccionar la pestaña `Authorities` y se debe importar el certificado de Burp que habíamos descargado anteriormente. Y se debe marcar la opción `Trust this CA to identify websites` y se debe presionar el botón `OK`.

<img src="/Users/enzobarreto/Desktop/UCU/2doSEM25/Desarrollo Seguro/Desarrollo_Software_Seguro_Enzo_Barreto/UT1/imagenes/creer-en-certificado-firefox.png" alt="Importar certificados" width=auto height=auto>

Ahora que ya tenemos configurado el certificado de Burp en Firefox, podemos acceder a la URL que deseemos y ver el tráfico que se genera en Burp. Recordar que para que se pueda interceptar el tráfico, se debe tener activada la opción `Intercept is on` en Burp, en este caso aparecerá un cuadro de dialogo en el cual se debe presionar el botón `Forward` para que se pueda visualizar el tráfico en Burp.

<img src="/Users/enzobarreto/Desktop/UCU/2doSEM25/Desarrollo Seguro/Desarrollo_Software_Seguro_Enzo_Barreto/UT1/imagenes/historial-proxy-III.png" alt="Probar burp" width=auto height=auto>
<br/>

---
### Instalación de Docker

**¿Que es Docker?**

Docker es un proyecto de código abierto que automatiza el despliegue de aplicaciones dentro de contenedores de software, proporcionando una capa adicional de abstracción y automatización de virtualización de aplicaciones en múltiples sistemas operativos.

Para instalar Docker en Kali Linux, se debe abrir una terminal y ejecutar el siguiente comando:

```bash
sudo apt-get install -y docker.io
```

`install` es un comando que permite instalar paquetes de software en el sistema operativo.

`-y` es un argumento que permite que el comando se ejecute sin interacción del usuario.

Una vez finalizada la instalación, se debe realizar la siguiente configuración: 

```bash
sudo systemctl enable docker --now
```
<img src="/Users/enzobarreto/Desktop/UCU/2doSEM25/Desarrollo Seguro/Desarrollo_Software_Seguro_Enzo_Barreto/UT1/imagenes/docker-status.png" alt="Docker" width=auto height=auto>

`systemctl` es un comando que permite controlar el sistema init de Linux.

`enable` es un argumento que permite habilitar un servicio.

`--now` es un argumento que permite iniciar un servicio.

Una vez finalizada la configuración, se debe reiniciar la máquina virtual para asegurarnos que los cambios se apliquen correctamente.

Como forma de comprobar que la instalación se realizó correctamente, se debe ejecutar el siguiente comando: `docker version` que nos muestra que efectivamente la instalación se realizó correctamente y algunos datos de la versión instalada.

```bash
docker version
```

<img src="/Users/enzobarreto/Desktop/UCU/2doSEM25/Desarrollo Seguro/Desarrollo_Software_Seguro_Enzo_Barreto/UT1/imagenes/Captura de pantalla 2025-08-25 a la(s) 11.13.07 p. m..png" alt="Ejecución de 'docker version'" width=auto height=auto>

Por último para evitar tener que escribir `sudo` cada vez que se ejecuta un comando de Docker, se debe agregar el usuario al grupo `docker` con el siguiente comando:

```bash
sudo usermod -aG docker $USER
```

`usermod` es un comando que permite modificar un usuario.

`-aG` es un argumento que permite agregar un usuario a un grupo.

`$USER` es una variable de entorno que contiene el nombre del usuario actual.
<br/>

### Instalación de OWASP Juice Shop en un contenedor Docker

**¿Que es OWASP Juice Shop?**

OWASP Juice Shop es una aplicación web vulnerable escrita en Node.js. El proyecto es un esfuerzo de código abierto patrocinado por la Fundación OWASP y operado por una comunidad internacional de voluntarios. El proyecto OWASP Juice Shop es el buque insignia de OWASP y el proyecto más maduro y sofisticado de la cartera de OWASP.

Para instalar OWASP Juice Shop en un contenedor Docker, se debe ejecutar el siguiente comando:

```bash
docker run --rm -p 3000:3000 bkimminich/juice-shop
```

`run` es un comando que permite ejecutar un contenedor.

`--rm` es un argumento que permite eliminar el contenedor una vez que se detiene.

`-p` es un argumento que permite mapear un puerto del contenedor a un puerto del host.

`3000:3000` es el mapeo del puerto del contenedor al puerto del host.

`bkimminich/juice-shop` es el nombre de la `imagen` que se va a utilizar para crear el contenedor, la misma es extraída del repositorio de [Docker Hub](https://hub.docker.com/).

En términos generales el ejecutar este comando permite validar la existencia de la imagen en el repositorio local, si no existe la descarga y luego crea el contenedor con la imagen descargada. Una vez que el contenedor se encuentra en ejecución, se puede acceder a la aplicación desde el navegador ingresando la URL `http://localhost:3000`.

**Comprobando que funcionen los contenedores**

Es necesario abrir el Firefox e ingresar a la URL `http://localhost:3000` para comprobar que el contenedor se encuentra en ejecución. Esto nos debería mostrar la siguiente pantalla:

<img src="/Users/enzobarreto/Desktop/UCU/2doSEM25/Desarrollo Seguro/Desarrollo_Software_Seguro_Enzo_Barreto/UT1/imagenes/Juice-corriendo .png" alt="Imagen de Juice Shop en localhost:3000" width=auto height=auto>

Para comprobar que el contenedor se encuentra en ejecución, se debe abrir una terminal y ejecutar el siguiente comando:

```bash
docker ps
```

`ps` es un comando que permite listar los contenedores que se encuentran en ejecución.


<br/>

### Prueba de la visualización del tráfico en Burp

Una buena forma de evitar estar configurando el proxy periódicamente es instalando la extensión de Firefox llamada `Foxyproxy`, para ellos debemos abrir un navegador y realizar la busqueda de esta extensión en _www.google.com_.

A continuación se observan los pasos para la configuración de esta extensión:
1. Una vez realizada encontrada la dirección de mozilla para descargar la extensión aparecerá la opción de `Agregar a Firefox`.
<img src="/Users/enzobarreto/Desktop/UCU/2doSEM25/Desarrollo Seguro/Desarrollo_Software_Seguro_Enzo_Barreto/UT1/imagenes/foxyproxy.png" alt="Agregar Foxyproxy" width=auto height=auto>

2. Se instala la extensión y nos redirige al menú de configuración.
<img src="h/Users/enzobarreto/Desktop/UCU/2doSEM25/Desarrollo Seguro/Desarrollo_Software_Seguro_Enzo_Barreto/UT1/imagenes/opcion-foxyproxy.png" alt="Extensión" width=500 height=auto>

3. El siguiente paso es clickear el botón `añadir` (add), para crear ua nueva configuración.
<img src="/Users/enzobarreto/Desktop/UCU/2doSEM25/Desarrollo Seguro/Desarrollo_Software_Seguro_Enzo_Barreto/UT1/imagenes/agregar-foxyproxy.png" alt="Agregar configuración" width=400>

4. A continuación se realizaran los pasos de configuración del proxy.
* 1- Colocar un nombre a la configuración, a forma de poder distinguir entre diferentes configuraciones.
* 2- Agregar el `URL` (localhost o 127.0.0.1), desde donde se va conectar.
* 3- Agregar el `Puerto` escucha.
* 4- `Guardar` la configuración.

<img src="/Users/enzobarreto/Desktop/UCU/2doSEM25/Desarrollo Seguro/Desarrollo_Software_Seguro_Enzo_Barreto/UT1/imagenes/guardar-foxyproxy.png" alt="Pasos de configuración">

5. Desde el icono de la extensión seleccionar la configuración que deseamos activar, en nuestro caso `Burp`.
<img src="h/Users/enzobarreto/Desktop/UCU/2doSEM25/Desarrollo Seguro/Desarrollo_Software_Seguro_Enzo_Barreto/UT1/imagenes/ok-foxyproxy.png" alt="Activar la configuración en la extensión">
<br/>

Ahora que ya tenemos el `Foxyproxy` configurado, lo siguiente es ir al Burp Suite y habilitar la opción en la pestaña `PROXY` llamada _Interceptar_ (esta debería quedar en modo ON).

Desde una nueva pestaña del navegador, colocamos en la ruta de acceso el URL `http://localhost:3000` que es donde tenemos corriendo nuestra imagen de Docker, una vez listo pulsamos `Enter` y el BurpSuite comenzará a capturar el tráfico.

<img src="/Users/enzobarreto/Desktop/UCU/2doSEM25/Desarrollo Seguro/Desarrollo_Software_Seguro_Enzo_Barreto/UT1/imagenes/historial-proxy-III.png" alt="Intercepción de data">

Como se puede apreciar en la imagen, el método `GET` llama al **HTTP/1.1**, que se encuentra en el `host` _localhost:3000_. Para continuar cargando la página debemos presionar el botón `Forward` del BurpSuite.

Ahí se comenzará a cargar los recursos de la página, en cada `Forward` se procederá a descargar otro recurso. 
<img src="/Users/enzobarreto/Desktop/UCU/2doSEM25/Desarrollo Seguro/Desarrollo_Software_Seguro_Enzo_Barreto/UT1/imagenes/historial-proxy.png" alt="Siguiente recurso">

Como se puede apreciar se comienza a descargar un nuevo recurso, este se puede ver en `GET`.
<br/>

---
# Conclusiones

Este trabajo nos permitió conocer y aprender a utilizar herramientas que nos serán de gran utilidad para el desarrollo de las actividades del curso. Generando un ambiente de trabajo que permite realizar las diferentes operaciones relacionadas a la seguridad, en un ambiente controlado.

Algo a destacar de este trabajo es que no todos conocíamos la versión Kali/Linux, pero descubrir la existencia de este tipo de recursos que contienen herramientas de código libre que podemos utilizar para llevar a cabo distintas actividades, es para nosotros muy aprovechable y rico en conocimiento.

<br/>
<br/>

---
# Fuentes consultadas

**Instalación de Kali Linux** Gamb1t. (13 de Marzo de 2023). _Installing Kali Linux_. Recuperado de https://www.kali.org/docs/installation/hard-disk-install/

**Instalación de FoxyProxy** Eric H. Jung. (s.f.). _FoxyProxy Standard_. Recuperado de https://addons.mozilla.org/es/firefox/addon/foxyproxy-standard/ 

**Burp Suite** s.n. (9 de agosto de 2023). _Getting started with Burp Suite_. Recuperado de https://portswigger.net/burp/documentation/desktop/getting-started 

**Docker** Docker. (2023). _Overview of the get started guide_. Recuperado de https://docs.docker.com/?_gl=1*jv3iee*_ga*MzgzNTM4NDUzLjE2ODk3MDk4MDg.*_ga_XJWPQMJYHQ*MTY5Mjg0NTEyNy40LjEuMTY5Mjg0NTEyOC41OS4wLjA.

**Instructivo de OWAPS** OWASP Uruguay. (s.f.) _Introducción a la seguridad de aplicaciones_. Recuperado de https://docs.google.com/presentation/d/14oCaDqbFJmKry1sLu52F05zn_VbXAuCq/edit#slide=id.p1 

