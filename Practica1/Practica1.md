# Práctica 1. Presentación de las prácticas y preparación de las herramientas #

## Universidad de Granada - ETSIIT ##
### Servidores Web de Altas Prestaciones ###

### Participantes ###

- Raúl Del Pozo Moreno
- Juan Carlos Hermoso Quesada

### Índice ###

1. [Descripción](#id1)
2. [Configuración de red](#id2)
3. [Conexión por ssh entre máquinas](#id3)
4. [Creación y prueba de fichero en HTML](#id4)
5. [Conexión por curl entre máquinas](#id5)

### Descripción <a name="id1"></a>

En esta práctica se realizará la instalación de dos máquinas virtuales con Ubuntu Server 16.04.3. Durante la instalación se instalarán los servicios siguientes, tal y como indica la práctica:

- OpenSSH server
- LAMP server

Como el proceso de alguna configuraciones es exactamente el mismo en ambas máquinas, en algunos pasos solo se mostrará la configuración de una de ellas, indicando que hay que repetir para la otra máquina.

### Configuración de red <a name="id2"></a>

Para permitir la conexión entre ambas máquinas virtuales, se ha creado una red Sólo-Anfitrión en VirtualBox con las siguientes características:

**Nombre Red Sólo-Anfitrión**

![Imagen Red-Anfitrión Nombre](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/RedSoloAnfitrion.png "Imagen Red-Anfitrión Nombre")

**Configuración Adaptador**

![Imagen Adaptador Solo-Anfitrión](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/RedSoloAnfitrionAdaptador.png "Imagen Configuración Adaptador")

**Configuración DHCP**

![Imagen DHCP Solo-Anfitrión](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/RedSoloAnfitrionDHCP.png "Imagen Configuracion DHCP")

**Configuración Red Maquina Virtual**

Una vez configurado VirtualBox, hay que crear el adaptador en las máquinas virtuales, este proceso es el mismo para las dos máquinas así que solo se mostrará una de ellas, para ello, vamos a la **Configuración** de la máquina virtual y en la opción de **Red**, vamos a la pestaña **Adaptador 2**, aquí lo configuramos de la siguiente manera:

![Imagen Configuración Red Maquina Virtual](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/Configuracion%20Red%20Maquina%20Virtual.png "Imagen Configuración Red Maquina Virtual")

El siguiente paso es crear la interfaz de red nueva en Ubuntu Server, para ello, iniciamos la maquina virtual y una vez logueados en el sistema escribimos el siguiente comando:

- sudo cp /etc/network/interfaces /etc/network/interfaces.old (para tener un respaldo de seguridad)

Después de crear la copia de seguridad, editamos el archivo para crear la interfaz:

- sudo vim /etc/network/interfaces

![Imagen Interfaz Maquina 1](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/Configuracion%20interfaces%201.png "Imagen Interfaz 1")

![Imagen Interfaz Maquina 2](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/Configuracion%20interfaces%202.png "Imagen Interfaz 2")

Como podemos ver, se le ha asignado la dirección ip 192.168.56.105 a la maquina ubuntu1, y 192.168.56.115 a la maquina ubuntu2, mediante estas direcciones las máquinas serán capaces de conectarse entre sí como veremos más adelante.

Para ver si funciona esto, hacemos ping a cada máquina

**Ping maquina 1 (ubuntu1) a máquina 2 (ubuntu2)**

- ping 192.168.56.115
- ping 192.168.56.105

![Imagen ping maquina 1 a 2](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/Ping%20maquina%201.png "Imagen Ping maquina 1")

**Ping maquina 2 (ubuntu2) a maquina 1 (ubuntu1)**

![Imagen ping maquina 2 a 1](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/Ping%20maquina%202.png "Imagen Ping maquina 2")

Como vemos, hay conexión entre ellas, lo siguiente es comprobar si hay conexión hacia fuera, hacia Internet, para ello ejecutamos el siguiente comando:

**Ping a Google**

- ping 8.8.8.8

![Ping fallido](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/ping%20google%20fallido.png "Ping fallido")

En algunas ocasiones, es posible que de error y no haya salida (se quedará pensando durante mucho tiempo), para arreglar esto, ejecutamos los siguiente comandos:

- sudo ifconfig enp0s8 down
- sudo ip addr
- sudo ifconfig enp0s8 up
- sudo dhclient -v

![Reset de red](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/reset%20red.png "Reset red")

Con esto, hemos reseteado la red, y como vemos, ya funciona:

![Ping éxito](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/ping%20google%20exito.png "Ping éxito")

Este proceso se realiza para ambas máquinas.

### Conexión por ssh entre máquinas <a name="id3"></a>

Para la conexión mediante ssh, tenemos que seguir una serie de pasos, los cuales consisten en:

1. Crear clave ssh
2. Importar clave a la otra máquina

**Creación de clave ssh en máquina 2 (ubuntu2)**

Para la creación de la clave ssh tenemos que acudir al siguiente comando, el cual nos creará una clave a la cual podremos configurar con un nombre (opcional) y darle una contraseña para su uso.

- ssh-keygen

![Clave ssh ubuntu2](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/ssh-keygen2.png "Clave ssh en maquina 2")

Una vez creada la clave, la importamos de la máquina 2 a la otra máquina con el siguiente comando.

- ssh-copy-id rauldpm@192.168.56.105

**Envío de clave ssh de maquina 2 (ubuntu2) a maquina 1 (ubuntu1)**

![Envío clave ssh ubuntu2 a ubuntu1](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/ssh-copy2.png "Envío clave ssh ubuntu2 a ubuntu1")

Una vez que ambas máquinas tienen la clave de la otra, es hora de comprobar que se puede realizar la conexión entre ellas, esto se hará con el comando:

- ssh rauldpm@192.168.56.105 (en máquina 2, para conectar con la máquina 1)

**Conexión de máquina 2 (ubuntu2) a máquina 1 (ubuntu1)**

![Conexión ssh ubuntu2 a ubuntu1](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/ssh-connect2.png "Conexión ssh ubuntu2 a ubuntu1")

Como podemos ver, antes de la conexión se ha realizado un ifconfig de la interfaz **enp0s8**, y si nos fijamos, una vez realizada la conexión, la dirección ip de la interfaz **enp0s8** cambia de valor, coincidiendo con la ip a la cual se le ha realizado la conexión.

También podemos saber que ha sido un éxito porque el nombre de la máquina cambia.


### Creación y prueba de fichero en HTML <a name="id4"></a>

Ahora vamos a comprobar que el Apache2 está en funcionamiento, para ello se va a crear un archivo HTML llamado hola.html en la ruta "/var/www/html/", el cual contendrá el siguiente código:

< HTML>  
< BODY>  
Esto funciona  :)  
< /BODY>  
< /HTML>  


Esto lo vamos a mostrar solo en una máquina ya que es el mismo proceso en ambas máquinas.

- sudo vim /var/www/html/hola.html

Y para ver el contenido más fácilmente:

- sudo less /var/www/html/hola.html

![Archivo HTML](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/holaMaquina1.png "Archivo HTML")

Y en el navegador de la máquina anfitrión comprobamos que se interpreta el archivo.

![Comprobación HTML](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/HTML.png "Comprobación HTML")


### Conexión por curl entre máquinas <a name="id5"></a>

Para comprobar que tenemos conectividad mediante curl, realizaremos una petición del archivo hola.html de la otra máquina.

- curl 192.168.56.115/hola.html (desde ubuntu1 a ubuntu2)
- curl 192.168.56.105/hola.html (desde ubuntu2 a ubuntu1)

**Curl desde la máquina 1 (ubuntu1) a la máquina 2 (ubuntu2)**

![Curl ubuntu1 a ubuntu2](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/curl1.png "Curl ubuntu1 a ubuntu2")

**Curl desde la máquina 2 (ubuntu2) a la máquina 1 (ubuntu1)**

![Curl ubuntu2 a ubuntu1](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/curl2.png "Curl ubuntu2 a ubuntu1")
