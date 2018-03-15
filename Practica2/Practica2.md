# Práctica 2. Clonar la información de un sitio web #

## Universidad de Granada - ETSIIT ##
### Servidores Web de Altas Prestaciones ###

### Participantes ###

- Raúl Del Pozo Moreno
- Manuel Mesas Gutiérrez

### Índice ###

1. [Descripción](#id1)
2. [Crear tar en con contenido local en equipo remoto](#id2)
3. [Instalar la herramienta rsync](#id3)
4. [Acceso sin contraseña para ssh](#id4)
5. [Programar tareas con crontab](#id5)

### 1 - Descripción <a name="id1"></a>

En esta práctica veremos cómo sincronizar carpetas entre máquinas de forma automática mediante la herramienta **rsync** y el programador de tareas **crontab**, también veremos como conectar las máquinas sin requerir una contraseña mediante **ssh**.

### 2 - Crear tar en con contenido local en equipo remoto <a name="id2"></a>

En este apartado crearemos un fichero tar con el contenido de "/var/www" de la máquina 2 al home de la máquina 1, para ello ejecutaremos la siguiente orden:

- tar czf - /var/www | ssh 192.168.56.105 'cat > ~/tar.tgz'

![Imagen CreacionYEnvioTarEnRemoto](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/enviandoArchivo.png "Imagen CreacionYEnvioTarEnRemoto")

Una vez ejecutado el comando anterior, en el home de la máquina 1 podemos ver que ha aparecido un archivo con el nombre que se le ha dado al enviar.

![Imagen ComprobandoTar](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/archivoRecibido.png "Imagen ComprobandoTar")


### 3 - Instalar la herramienta rsync <a name="id3"></a>

Para instalar la herramienta rsync usamos el siguiente comando:

- sudo apt-get install rsync

Ahora vamos a proceder a clonar una carpeta de la máquina 1 en la máquina 2, primero debemos dar permisos a la carpeta a clonar mediante el comando:

- sudo chown rauldpm:rauldpm -R /var/www

** Importante **

Para un punto posterior, específicamente el de programar tareas, este comando hay que ejecutarlo dentro del root y no desde la cuenta de usuario mediante el comando sudo

- sudo su
- chown rauldpm:rauldpm -R /var/www (como root)

![Imagen chown maquina1](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/chown1.png "Imagen chown maquina 1")

![Imagen chown maquina2](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/chown2.png "Imagen chown maquina 2")

Una vez que el usuario tiene permisos sobre la carpeta a clonar podemos ejecutar la herramienta rsync desde la máquina 2 poniendo la ip de la máquina 1 como destino:

- rsync -avz -e ssh 192.168.56.105:/var/www/ /var/www/

![Imagen rsync](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/rsyncMaquina1a2.png "Imagen rsync")

Como vemos en la imagen, se han ejecutado una serie de comandos a modo de prueba:

1. ifconfig enp0s8 -> (en máquina 2) para ver su ip (la máquina 2 tiene la ip **192.168.56.115** y la máquina 1 tiene la **192.168.56.105**)
2. less /var/www/html/hola.html -> para ver el contenido del archivo original de la máquina 2, como vemos, tiene el texto: **"Esto funciona en máquina 2"**
3. Ejecutamos el rsync
4. less /var/www/html/hola.html -> revisamos el archivo y vemos que el contenido ha cambiado, ahora pone: **"Esto funciona en máquina 1"**


### 4 - Acceso sin contraseña para ssh <a name="id4"></a>

Ahora haremos que la máquina 2 pueda ejecutarse mediante ssh a la máquina 1 sin contraseña.

En la máquina 2 ejecutamos:

- ssh-keygen -b 4096 -t rsa

Con este comando crearemos una clave nueva (ya existía una de la práctica 1, por eso pregunta que si queremos sobreescribir)

![Imagen ssh-keygen](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/ssh-keygen.png "Imagen ssh-keygen")

Para que esto funcione, debemos dar permisos al fichero que contiene las claves autorizadas

- chmod 600 ~/.ssh/authorized_keys

![Imagen chmod](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/chmod.png "Imagen chmod")

Acto seguido enviamos la nueva clave a la otra máquina mediante el siguiente comando:

- ssh-copy-id 192.168.56.105

![Imagen ssh-copy](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/ssh-copy.png "Imagen ssh-copy")

Para probar que esto ha funcionado ejecutamos en la máquina 2 un comando sobre la máquina 1

- ssh 192.168.56.105 uname -a

![Imagen ssh-command](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/sshCommand.png "Imagen ssh-command")

Como vemos, ha ejecutado el comando sin contraseña, para asegurarnos, ejecutamos una conexión sobre la máquina 1

![Imagen ssh-connect](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/sshConnect.png "Imagen ssh-command")

Y como volvemos a ver, conecta directamente

### 5 - Programar tareas con crontab <a name="id5"></a>

Finalmente, vamos a programar una tarea que nos sincronice la carpeta "/var/www/" cada cierto tiempo.

Para ello, hay que editar el fichero "/etc/crontab" y añadimos la nueva regla:

- sudo vim /etc/crontab

En el archivo añadimos la siguiente línea:

- \*  \*  \* \* \* rauldpm rsync -avz -e ssh 192.168.56.105:/var/www/ /var/www/

![Imagen crontab](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/crontab.png "Imagen crontab")

Esta línea lo que hace es ejecutar el comando rsync mediante el usuario rauldpm, cada minuto.

Para comprobar su funcionamiento, editamos el archivo "hola.html" en la máquina 1 y vemos si al pasar un minuto ha cambiado el archivo "hola.html" de la máquina 2.

Primero cambiamos el contenido de la máquina 1

![Imagen hola11](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/hola11.png "Imagen hola11")

Y vemos que la máquina 2 tiene el mismo contenido

![Imagen hola21](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/hola21.png "Imagen hola21")

Acto seguido cambiamos el contenido del fichero de la máquina 1

![Imagen hola12](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/hola12.png "Imagen hola12")

Y al pasar el minuto comprobamos el archivo de la máquina 2

![Imagen hola22](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/hola22.png "Imagen hola22")
