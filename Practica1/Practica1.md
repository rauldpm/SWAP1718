# Practica 1 #

## UGR - ETSIIT - SWAP ##

Participantes:

- Raul Del Pozo Moreno
- Juan Carlos Hermoso Quesada

||-------------------------------------------------||

### Descripcion ###

En esta practica se realizara la instalacion de dos maquinas virtuales con Ubuntu Server 16.04.3. Durante la instalacion se instalaran los servicios siguientes, tal y como indica la practica:

- OpenSSH server
- LAMP server

### Configuracion ###

Para permitir la conexion entre ambas maquinas virtuales, se ha creado una red Solo-Anfitrion en VirtualBox con las siguientes caracteristicas:

**Nombre Red Solo-Anfitrion**
 
![Imagen Red-Anfitrion Nombre](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/RedSoloAnfitrion.png "Imagen Red-Anfitrion Nombre")

**Configuracion Adaptador**

![Imagen Adaptador Solo-Afitrion](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/RedSoloAnfitrionAdaptador.png "Imagen Configuracion Adaptador")

**Configuracion DHCP**

![Imagen DHCP Solo-Anfitrion](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/RedSoloAnfitrionDHCP.png "Imagen Configuracion DHCP")

**Configuracion Red Maquina Virtual**

Una vez configurado Virtual Box, hay que crear el adaptador en las maquinas virtuales, este proceso es el mismo para las dos maquinas asi que solo se mostrara una de ellas, para ello, vamos a la **Configuracion** de la maquina virtual y en la opcion de **Red**, vamos a la pestaña **Adaptador 2**, aqui lo configuramos de la siguiente manera:

![Imagen Configuracion Red Maquina Virtual](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/Configuracion%20Red%20Maquina%20Virtual.png "Imagen Configuracion Red Maquina Virtual")

El siguiente paso es crear la interfaz de red nueva en Ubuntu Server, para ello, iniciamos la maquina virtual y una vez logueados en el sistema escribimos el siguiente comando:

- sudo cp /etc/network/interfaces /etc/network/interfaces.old (para tener un respaldo de seguridad)

Despues de crear la copia de seguridad, editamos el archivo para crear la interfaz:

- sudo vim /etc/network/interfaces

![Imagen Interfaz Maquina 1](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/Configuracion%20interfaces%201.png "Imagen Interfaz 1")

![Imagen Interfaz Maquina 2](FALTA "Imagen Interfaz 2")

Como podemos ver, se le ha asignado la direccion ip 192.168.56.105 a la maquina ubuntu1 (SWAP1), y 192.168.56.115 a la maquina ubuntu2 (SWAP2), mediante estas direcciones las maquinas seran capaces de conectarse entre si como veremos mas adelante.

Para ver si funciona esto, hacemos ping a cada maquina

**Ping maquina 1 (ubuntu1) a maquina 2 (ubuntu2)**

- ping 192.168.56.115

![Imagen ping maquina 1 a 2](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/Ping%20maquina%201.png "Imagen Ping maquina 1")

**Ping maquina 2 (ubuntu2) a maquina 1 (ubuntu1)** 

- ping 192.168.56.105

![Imagen ping maquina 2 a 1](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/Ping%20maquina%202.png "Imagen Ping maquina 2")

Como vemos, hay conexion entre ellas, lo siguiente es comprobar si hay conexion hacia fuera, hacia internet, para ello ejecutamos el siguiente comando:

**Ping a google**

- ping 8.8.8.8 

![Ping fallido](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/ping%20google%20fallido.png "Ping fallido")

En algunas ocasiones, es posible que de error y no haya salida (se quedara pensando durante mucho tiempo), para arreglar esto, ejecutamos los siguiente comandos:

- sudo ifconfig enp0s8 down
- sudo ip addr
- sudo ifconfig enp0s8 up
- sudo dhclient -v

![Reset de red](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/reset%20red.png "Reset red")

Con esto, hemos reseteado la red, y como vemos, ya funciona:

![Ping exito](https://github.com/rauldpm/SWAP1718/blob/master/Practica1/Imagenes/ping%20google%20exito.png "Ping exito")

Este proceso se realiza para ambas maquinas.























