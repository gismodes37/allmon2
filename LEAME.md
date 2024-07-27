# Allmon2-2024
Instalación de Allmon2 paso a paso 2024
Machine Translated by Google

Léame de Allmon
Actualizado y traducido el 20 de julio de 2024

Allmon es un sitio web para administrar uno o más nodos app_rpt (también conocido como Allstarlink). Cada nodo local administrado muestra una lista de nodos conectados. La lista está ordenada en orden inverso al nodo recibido más recientemente. Entonces, el último nodo en hablar siempre está al principio de la lista. Cualquier nodo que se esté recibiendo actualmente se resaltará con un fondo verde y se moverá 
a la parte superior de la lista. La lista de nodos se actualiza una vez por segundo, lo que proporciona un estado casi en tiempo real.

Los nodos se pueden agrupar al configurar Allmon. Los grupos permiten mostrar dos nodos de modo o dos en la misma página. Esto puede resultar útil si utiliza dispositivos con pantallas más pequeñas o si desea ver un par de nodos al mismo tiempo. No intente mostrar más de unos pocos por grupo ya que el rendimiento se verá afectado.

Los usuarios que hayan iniciado sesión pueden realizar conexiones y desconexiones. Los usuarios se mantienen con la utilidad Apache htpasswd. Sólo hay un nivel de inicio de sesión, administrador.

Allmon también monitoreará a los clientes VOTANTES. Cada instancia de VOTER muestra una lista de módulos de cliente ligero de radio (RTCM) adjuntos. El RSSI para cada RTCM se muestra en estilo degráfico de barras junto con un color para indicar el RTCM votado actualmente.

Instrucciones de instalación    -   (Acá ira un link a un tutorial personalizado... pronto !)

­*Si instaló su(s) nodo(s) desde el portal web Allstarlink y nunca a tocado la línea de comando, tienes una curva de aprendizaje bastante pronunciada por delante. Por otro lado, si conoce Apache y Linux, la 
instalación debería ser pan comido.

­*Es necesario instalar Apache y PHP en su servidor. La contraseña de Apache La utilidad htpasswd debe estar disponible si desea instalar un administrador.

­*Allmon 2 requiere PHP 5.2 o superior. Si ejecuta Allmon o su nodo ACID o su servidor web es RedHat 5, tendrá que actualizar su repositorio yum para obtener un php lo suficientemente actualizado con 
wget -­q ­-O - ­http://www.atomicorp.com/installers/atomic | sh 
y luego yum actualizan php. Eso debería llevarte a php 5.4 al momento de escribir este artículo. Alternativamente, el paquete pecl json puede funcionar pero no lo he probado.

­*El servidor web puede ser local para su nodo o en un servidor independiente. Si le preocupa el rendimiento de su nodo, utilice un servidor independiente. Además, podrá ver dos o más modos de sus nodos (incluso si se encuentran en servidores app_rpt separados). El puerto 5038 de Asterisk Manager (u otro de su elección) debe estar abierto hacia su servidor web.

­*Coloque estos archivos en algún lugar del árbol de documentos de su servidor web. Puede ser el docroot o cualquier subdirectorio que desee.

­*Copie allmon.ini.txt a allmon.ini.php y luego edítelo para su(s) nodo(s)
información. El ID de usuario y las contraseñas que ingrese aquí son los que usará en manager.conf en su(s) servidor(es) de nodo.

­*Opcionalmente, copie voter.ini.txt a voter.ini.php y edítelo con su información de votante.

­*Cree su archivo .htpasswd para los usuarios administradores. En el directorio de su servidor web, en la línea de comando haga: htpasswd ­c .htpasswd nombre de usuario Algunos sistemas necesitarán la opción ­d para forzar el cifrado cryptr() Machine Translated by Google necesario por php

­*Asegúrate de tener la última fuente Allstar (recomiendo la imagen Iso beta para PC es la que e utilizado con buenos resultados) de svn ya que hay versiones especiales de comandos Allmon en versiones posteriores de rpt.c.

*Editar /etc/asterisk/manager.conf ­ 
*Marcar astdb.php como ejecutable y agregarlo a cron. Corre solo una vez al día, por favor. es decir, 
01 03   * * *  cd /var/www/html/allmon2; ./astdb.php

­*Si tiene nodos privados, cambie el nombre de privatenodes.sample.txt a privatenodes.txt y edítelo con su información.
La línea con NODE|CALL|INFO|LOCATION se puede eliminar. Está ahí para mostrar el formato únicamente.

­*Edite controlpanel.ini.php para los comandos que desee. Asegúrese de mantener el etiquetas[] y las etiquetas cmds[] en pares asociados.

Base de datos Allstarlink

­*Si no tienes la "base de datos" Allstarlink (en realidad solo un archivo de texto), el La columna Información del nodo solo mostrará la dirección IP de los nodos remotos. 

­*Los usuarios de ACID y Limey deben ejecutar el script astdb.php para obtener una copia inicial de la base de datos. Debe actualizarse periódicamente con el script astdb.php. Configure su trabajo cron en un tiempo razonable, como una vez al día, y ejecútelo manualmente cuando necesite una actualización ad hoc ocasional.

­*El Beagle Bone Black ya cuenta con un archivo astdb.txt actualizado diariamente. BBB los usuarios deben agregar un enlace simbólico a ese archivo con "ln -­s /var/log/asterisk/astdb.txt astdb.txt".

Errores conocidos
*Si solo se muestra un grupo y no se muestran nodos individuales (menú=sí) index.php no redireccionará al grupo.

Actualizaciones ­ 
* 2012/12/06 Se pueden agregar nodos privados a astdb.txt ­
* 2012/12/10 Grupos fijos donde se usa más de un grupo ­
* 2012/12/17 Se pueden agregar elementos de menú a allmon.ini ­
* 2013/04/19 Cambios en el menú de votantes, consulte github para obtener más detalles.
* 05/04/2014 Sucursal Allmon II
  -­Internet Explorer ya no es compatible. Pero cualquier otro navegador moderno del mundo funciona bien.
  -­Utiliza eventos HTML 5 enviados por servidor. SSE reemplaza el polling largo de JavaScript para aumentar la frecuencia de actualización y reducir la carga en Asterisk. IE no es compatible con SSE.
­  -La forma en que se actualiza la lista de nodos ha cambiado porque a veces Tomó 5 o 6 clics antes de una respuesta. Esto se resolvió actualizando partes de la tabla bajo demanda cuando cambian los datos.
­  -Las filas del encabezado (con fondo gris) solo se actualizan cuando la página está primero cargado.
­  -Las columnas de tiempo se actualizan aproximadamente cada segundo.
­  -Todo lo demás se actualiza solo cuando cambia algo más que las columnas de tiempo.
­  -Los clics ahora funcionan a la primera, siempre.
­  -Otras mejoras en la interfaz de usuario.
­  -Los grupos ahora se manejan con la clave/valor nodos=x,x,x en el nodo estrofa de allmon.ini.php. Groups.ini ya no se utiliza.
­  -La visualización de votantes es un nodo seleccionado en lugar de todos los nodos del servidor.

­* 2024/07/20 README actualizado 
* 2014/04/22 Muchos cambios de código para que sucedan estas pocas cosas:
  -Machine Translated by Google
  -Un inicio de sesión incorrecto en un servidor no debería impedir el inicio de sesión exitoso visualización de nodos
  -Conexión a AMI, estado de conexión fallida y error de inicio de sesión
los mensajes ahora se muestran
  -Mostrar "Sin conexiones". cuando no los hay.
  -astdb.php no se repetirá para siempre cuando no se encuentre el archivo
* 24/04/2014 Añadido el Panel de Control
  -Consulte controlpanel.ini.txt para configurar.
* 30/11/2014 Actualización del repositorio de git.
  -Se agregaron cambios de astdb.txt a README. -
astdb.txt ya no se distribuye con Allmon para BBB compatibilidad. Los usuarios de ACID/Limey simplemente ejecutan astdb.php como antes.
  - Archivos de muestra .ini.txt actualizados.
  - menu.php tiene un poco más de flexibilidad.
  - Mejor compatibilidad con sistemas privados.
  - Se agregaron más comandos de ejemplo del panel de control.
* 07/07/2016 Se agregó favicon. Gracias KC9ONA
* 27/07/2016 Versión etiquetada 2.1 - Cambia la barra de navegación al menú de estilo desplegable CSS
  - !!!¡¡¡AVISO!!! !!!¡¡¡ADVERTENCIA!!! Los cambios de usuario se realizan en allmon.ini.php. Es necesario seguir ocultando menús ocultos. El comportamiento predeterminado es mostrar elementos de menú a menos que estén ocultos con la nueva directiva INI nomenu=yes.
  - Aparte de los menús ocultos, los elementos existentes deberían seguir funcionando como antes.
  - La estrofa INI [Break] ya no se utiliza. No es necesario en el contexto desplegable.
  - Los marcadores deberían funcionar como antes 2.1. Es posible que sea necesario actualizar el navegador (debido al cambio de CSS) para mostrar correctamente el menú la primera vez.
  - El enlace de inicio de sesión/cierre de sesión se ha movido encima (y fuera del camino) de la barra de navegación.
  - La nueva directiva INI "system=<nombre>" crea y/o agrega un elemento al menú desplegable nombrado.

Ver ejemplo allmon.ini.txt
  - La página about.php ha sido eliminada y su texto se ha movido a la nueva página de inicio. página.
  - El título del sitio ahora es un enlace a la nueva página de inicio.

* 2016/12/18
  - Puede mostrar más de un RTCM a la vez, es decir, link.php?nodes=1111,1112 ver ejemplo en allmon.ini.txt
  - Se corrigió la configuración INI hideNodeURL=yes.
* 29/08/2017
  - Consulte los comentarios de git para obtener actualizaciones de ahora en adelante.
* 2018/02/02
  - Actualización de seguridad. La sesión PHP reemplaza las cookies de inicio de sesión.
  - Consulte las notas de la versión del 27/07/2016 con respecto a nomenu=
  - Gracias a KB4FXC, M0NFI, KN2R y WA3DSP
* 2018/02/05
  - Gracias a coolacid por el proceso X-Accel-Buffering
  - También se agregó X-Accel-Buffering a votanteserver.php


