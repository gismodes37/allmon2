**Instalación de Allmon2  -  Paso a Paso**


**Allmon**  
>Allmon es un sitio web para administrar uno o más nodos app_rpt ( también conocido como All Star Link ). Cada nodo local administrado muestra una lista de nodos conectados. La lista está ordenada en orden inverso al nodo recibido más recientemente. Entonces, el último nodo en hablar siempre está al principio de la lista. Cualquier nodo que se esté recibiendo actualmente se resaltará con un fondo verde y se moverá a la parte superior de la lista. La lista de nodos se actualiza una vez por segundo y proporciona un estado casi en tiempo real. 


**Notas Previas**

>Este proyecto lo realice en una máquina virtual montada y administrada con el **Software VMWare Worhstation**, esta máquina virtual está sobre **Windows 11**, de esta forma el nodo All Star Link es totalmente funcional y fue configurado para operar con un simple micrófono de PC y la aplicación iaxRpt.

>Cabe señalar que esta instalación requiere conocimientos de manejo de terminal y comandos Linux en Debian

>Para realizar este tutorial, tenemos que tener ya instalado un **Nodo All Star Link configurado y funcionando*, queda sobreentendido que Allmon es una Aplicación Web de administración de Nodos All Star Link. (enlace al tutorial de instalación de un Nodo All Star Link) y explicaremos la instalación de Allmon en su versión N° 2

>Al final de este tutorial dejaré la Zona de Descargas con todos los programas necesarios para realizar este tutorial.


>Lo primero que hay que hacer es entrar al nodo con la cuenta de usuario 'repeater' y su contraseña: xxxxx,  desde la línea de comando, ya sea utilizando un teclado y un monitor o por conexión SSH mediante red de forma similar a la que se configuró el nodo.

```sh
repeater@repeater:~$  
``` 
>Una vez ahí, debes escribir los siguientes comandos. Algunos de ellos tardarán bastante en ejecutarse dependiendo de las prestaciones de la máquina o equipo que estés usando y la velocidad de tu servicio de Internet.

**Primeros pasos:**

**1.- Instalar Git:**

```sh
repeater@repeater:~$ sudo apt install git -y
```

**2.- Clonar el repositorio de Allmon2:**

```sh
repeater@repeater:~$ cd /var/www/html
repeater@repeater:/var/www/html$ sudo git clone https://github.com/gismodes37/Allmon2-2024.git allmon2
```

**3.- Crear el archivo allmon.ini.php:**

>Ve al repositorio de Github: <a href="https://github.com/gismodes37/Allmon2-2024/blob/main/allmon.ini.txt" style="background-color: white;" target="_blank"><span style="color: black;">https://github.com/gismodes37/Allmon2-2024/blob/main/allmon.ini.txt</span></a>, copia el contenido del archivo y pégalo en un archivo con el mismo nombre en tu instalación.

```sh
repeater@repeater:~$ cd /var/www/html/allmon2
repeater@repeater:/var/www/html/allmon2$ sudo nano allmon.ini.txt
```

>Pega el contenido copiado en el archivo allmon.ini.txt que creaste y guarda el archivo.

```sh
repeater@repeater:/var/www/html/allmon2$ sudo mv allmon.ini.txt allmon.ini.php
```

**4.- Renombrar controlpanel.ini.txt:**

```sh
repeater@repeater:/var/www/html/allmon2$ sudo mv controlpanel.ini.txt controlpanel.ini.php
```

**5.- Actualizar referencias en los archivos:**

```sh
repeater@repeater:/var/www/html/allmon2$ sudo sed -i 's/allstarlink.org/pttlink.org/g' astdb.php
repeater@repeater:/var/www/html/allmon2$ sudo sed -i 's/Allstar /PTTLink /g' header.inc
repeater@repeater:/var/www/html/allmon2$ sudo sed -i 's/allstarlink.org/pttlink.org/g' link.php
```

**6.- Hacer astdb.php ejecutable:**

```sh
repeater@repeater:/var/www/html/allmon2$ sudo chmod +x astdb.php
```

**7.- Configurar la autenticación HTTP:**

```sh
repeater@repeater:/var/www/html/allmon2$ sudo htpasswd -cB /var/www/html/allmon2/.htpasswd admin
```

>Se te pedirá que ingreses una contraseña para el usuario admin.

**8.- Ejecutar manualmente el script astdb.php para configurar la base de datos:**

```sh
repeater@repeater:/var/www/html/allmon2$ sudo ./astdb.php
```

**9.- Agregar una tarea a crontab:**

```sh
repeater@repeater:/var/www/html/allmon2$ sudo nano /etc/crontab
```

-Al final del archivo agrega la siguiente línea:

```sh
01 03   * * *   root cd /var/www/html/allmon2; ./astdb.php
```

**10.- Reiniciar el nodo:**

```sh
repeater@repeater:~$ sudo reboot
```



