# Instalación de Allmon2  -  Paso a Paso - CA2IIG


## Allmon
>Allmon es un sitio web para administrar uno o más nodos app_rpt ( también conocido como All Star Link ). Cada nodo local administrado muestra una lista de nodos conectados. La lista está ordenada en orden inverso al nodo recibido más recientemente. Entonces, el último nodo en hablar siempre está al principio de la lista. Cualquier nodo que se esté recibiendo actualmente se resaltará con un fondo verde y se moverá a la parte superior de la lista. La lista de nodos se actualiza una vez por segundo y proporciona un estado casi en tiempo real.

<div align="center" class="separator" style="clear: both;"><a><img alt="" data-original-height="372" data-original-width="783" height="219" src="https://blogger.googleusercontent.com/img/a/AVvXsEicxmZ8G34OajNSWp74Io1Ce04mbFsPqpmQWt8n3tl52N1iA7fufEDQuhuArlANLYOk5Int8_6iCeejkRa8PXpq4h5hhkN83pMRxVJu0_84ItxM64fT4eGp91cmmy2U8hR7UUf1i52UajSc42sbnNPWCx_M0O3_ApQ-fddguVDNQQdHlRgZm3Y7rDWgL7GE=w461-h219" width="461" /></a></div>

## Notas Previas

>Este proyecto lo realice en una máquina virtual montada y administrada con el Software VMWare Worhstation, esta máquina virtual está sobre Windows 11, de esta forma el nodo All Star Link es totalmente funcional y fue configurado para operar con un simple micrófono de PC y la aplicación iaxRpt.

>Cabe señalar que esta instalación requiere conocimientos de manejo de terminal y comandos Linux en Debian

>Para realizar este tutorial, tenemos que tener ya instalado un nodo All Star Link configurado y funcionando, queda sobreentendido que Allmon es una Aplicación Web de administración de Nodos All Star Link. (enlace al tutorial de instalación de un Nodo All Star Link) y explicaremos la instalación de Allmon en su versión N° 2

>Al final de este tutorial dejaré la **Zona de Descargas** con todos los programas necesarios para realizar este tutorial.


>Lo primero que hay que hacer es entrar al nodo con la cuenta de usuario 'repeater' y su contraseña: xxxxx,  desde la línea de comando, ya sea utilizando un teclado y un monitor o por conexión SSH mediante red de forma similar a la que se configuró el nodo.

```sh
repeater@repeater:~$  
``` 
>Una vez ahí, debes escribir los siguientes comandos. Algunos de ellos tardarán bastante en ejecutarse dependiendo de las prestaciones de la máquina o equipo que estés usando y la velocidad de tu servicio de Internet.

## Primeros pasos:

```sh
sudo apt update
sudo apt install apache2 -y
sudo apt install php libapache2-mod-php -y
```
<br>
**1.- Instalar Git:**

```sh
sudo apt install git -y
```

<br>
**2.- Clonar el repositorio de Allmon2:**

```sh
cd /var/www/html/allmon2
sudo git clone https://github.com/gismodes37/Allmon2-2024.git
```
<br>
**3.- Crear el archivo allmon.ini.php:**

>Ve al repositorio de Github: <a href="https://github.com/gismodes37/Allmon2-2024/blob/main/allmon.ini.txt" style="background-color: white;" target="_blank"><span style="color: black;">https://github.com/gismodes37/Allmon2-2024/blob/main/allmon.ini.txt</span></a>, copia el contenido del archivo y pégalo en un archivo con el mismo nombre en tu instalación.

```sh
cd /var/www/html/allmon2
sudo nano allmon.ini.txt
```
<br>
>Pega el contenido copiado en el archivo allmon.ini.txt que creaste y guarda el archivo.

```sh
sudo mv allmon.ini.txt allmon.ini.php
```

<br>
**4.- Renombrar controlpanel.ini.txt:**

```sh
sudo mv controlpanel.ini.txt controlpanel.ini.php
```

<br>
**5.- Actualizar referencias en los archivos: dentro de /var/www/html/allmon2$ **

```sh
sudo sed -i 's/allstarlink.org/pttlink.org/g' /var/www/html/allmon2/astdb.php
```
```sh
sudo sed -i 's/Allstar /PTTLink / g' /var/www/html/allmon2/header.inc
```
```sh
sudo sed -i 's/allstarlink.org/pttlink.org/g' /var/www/html/allmon2/link.php
```

<br>
**6.- Hacer astdb.php ejecutable: dentro de /var/www/html/allmon2$**

```sh
sudo chmod +x /var/www/html/allmon2/astdb.php
```

<br>
**7.- Configurar la autenticación HTTP: dentro de /var/www/html/allmon2$**

```sh
sudo htpasswd -cB /var/www/html/allmon2/.htpasswd admin
```

>Se te pedirá que ingreses una contraseña para el usuario admin.

<br>
**8.- Ejecutar manualmente el script astdb.php para configurar la base de datos:**

```sh
cd /var/www/html/allmon2
/var/www/html/allmon2$ sudo astdb.php
```
<br>
**9.- Agregar una tarea a crontab:**

```sh
/var/www/html/allmon2$ sudo nano /etc/crontab
```

>Al final del archivo agrega la siguiente línea:

```sh
01 03   * * *   root cd /var/www/html/allmon2; astdb.php
```

<br>
**10.- Reiniciar el nodo:**

```sh
sudo reboot
```

<br>
# Configuración de Allmon2

>Si llegaste a esta etapa, quiere decir que ya lograste instalar Allmon2. Ahora toca configurar la aplicación web para que funcione correctamente y puedas manejar tu nodo.

<br>
**1.- Editar el archivo allmon.ini.php:**

```sh
cd /var/www/html/allmon2
/var/www/html/allmon2$ sudo nano allmon.ini.php
```

<br>
**2.- Configurar el nodo:**

>Busca los corchetes [500] y cambia el 500 por el número de tu nodo. Por ejemplo, si tu número de nodo es 12345, cambia [500] a [12345].

```sh
[12345]
host=127.0.0.1:5038
user=admin
passwd=yourpassword  # Esta contraseña es la misma que pusiste en la etapa de instalación
nomenu=no
hideNodeURL=no
menu=yes  # Si no existe esta línea, agrégala
```

<br>
**3.- Guardar el archivo:**

>Presiona Ctrl + X, luego Y (o S en algunos sistemas), y finalmente Enter para guardar y salir del editor.

>A continuación, ingresa los siguientes comandos:

```sh
mv -f /var/www/html/allmon2/allmon.ini.php $WEBROOT/allmon2/allmon.ini.php.$DATEEXT 2>/dev/null #backup default
cp -f /usr/local/sbin/allmon.ini.php /var/www/html/allmon2/allmon.ini.php 2>/dev/null #copy what node-setup script created
```
<br>
**4.- Actualizar los permisos:**

```sh
/var/www/html/allmon2$ sudo chmod 755 astdb.php
```

<br>
**5.- Hora de probar Allmon2:**

>Abre un navegador web y accede a la dirección IP de tu nodo seguida de /allmon2. Por ejemplo:

```url
http://192.168.x.x/allmon2
```
<br>
>Inicia sesión con el usuario admin y la contraseña que configuraste durante la instalación.

```plaintext
Usuario: admin
Contraseña: la que pusiste durante la instalación
```

<br>
**6.- Felicidades:**

>Ya tienes funcionando Allmon2 conectado a tu nodo AllStar Link. Felicitaciones. Ahora tenemos que habilitar una aplicación que reconozca nuestra configuración de audio del equipo y nos permita hablar. Esta aplicación se llama iaxRpt.

>EN LA SIGUIENTE DIRECCION PODEMOS COMPROBAR LA PRESENCIA DE NUESTRO NODO ACTIVO :   http://stats.allstarlink.org/maps/allstarUSAMap.html
>
<br>
# Zona de Descargas (Copia y pega en un navegador)
<br>
>All Star Link  Beta  para PC64bit AMD/Intel :  (Repositorio personal en Mega)
```url
https://mega.nz/file/6xE0XR6Q#88XI2cwu1uRFWrvfh0fffG4jlSKwvy9f56jnxpIcARY
```

<br>
>All Star Link  Beta  para PC64bit AMD/Intel :  (Repositorio All Star Link)
```url
https://downloads.allstarlink.org/ASL_Images_Beta/Intel-AMD/asl-2.0.0-beta.6-kc1kcc-20210324-amd64.hybrid.iso
```

<br>
>iaxRpt :  (Repositorio personal en Mega)
```url
https://mega.nz/file/fo8kDa6Z#hIZuuHDJrrzfhtDsskh5Wpq8nCu1AR2LjDAT0un09uY
```

<br>
>iaxRpt :  (Repositorio All Star Link)
```url
https://wiki.allstarlink.org/images/5/56/Setup_iaxrpt_xipar_010146.exe
```

<br>
>Bien, con esto damos por finalizada la instalación de Allmon en su versión N° 2, espero que lo disfrutes para cualquier fallo en la instalación o configuración te recomiendo retroceder a los pasos anteriores y repetir el proceso, si no hay solución me escribes a contacto@pcsupport-stg.com.
<br>
CA2IIG<br>
Guillermo Ismodes<br>
La Serena - Chile


