# Practica 2. Clonar la información de un sitio web #
 
## Universidad de Granada - ETSIIT ##
### Servidores Web de Altas Prestaciones ###

### Participantes ###

- Raul Del Pozo Moreno
- Juan Carlos Hermoso Quesada

### Indice ###

1. [Descripcion](#id1)
2. [Crear tar en con contenido local en equipo remoto](#id2)
3. [Instalar la herramienta rsync](#id3)
4. [Acceso sin contraseña para ssh](#id4)
5. [Programar tareas con crontab](#id5)

### Descripcion <a name="id1"></a>



### Crear tar en con contenido local en equipo remoto <a name="id2"></a>

En este apartado crearemos un fichero tar con el contenido de "/var/www" de la maquina 2 al home de la maquina 1, para ello ejecutaremos la siguiente orden:

- tar czf - /var/www | ssh 192.168.56.105 'cat > ~/tar.tgz'

![Imagen CreacionYEnvioTarEnRemoto](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/enviandoArchivo.png "Imagen CreacionYEnvioTarEnRemoto")

Una vez ejecutado el comando anterior, en el home de la maquina 1 podemos ver que ha aparecido un archivo con el nombre que se le ha dado al enviar.

![Imagen ComprobandoTar](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/archivoRecibido.png "Imagen ComprobandoTar")


### Instalar la herramienta rsync <a name="id3"></a>

Para instalar la herramienta rsync usamos el siguiente comando:

- sudo apt-get install rsync

Ahora vamos a proceder a clonar una carpeta de la maquina 1 en la maquina 2, primero debemos dar permisos a la carpeta a clonar mediante el comando:

- sudo chown rauldpm:rauldpm -R /var/www

** Importante ** 

Para un punto posterior, especificamente el de programar tareas, este comando hay que ejecutarlo dentro del root y no desde la cuenta de usuario mediante el comando sudo

- sudo su
- chown rauldpm:rauldpm -R /var/www (como root)

![Imagen chown maquina1](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/chown1.png "Imagen chown maquina 1")

![Imagen chown maquina2](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/chown2.png "Imagen chown maquina 2")

Una vez que el usuario tiene permisos sobre la carpeta a clonar podemos ejecutar la herrmienta rsync desde la maquina 2 poniendo la ip de la maquina 1 como destino:

- rsync -avz -e ssh 192.168.56.105:/var/www/ /var/www/

![Imagen rsync](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/rsyncMaquina1a2.png "Imagen rsync")

Como vemos en la imagen, se han ejecutado una serie de comandos a modo de prueba:

1. ifconfig enp0s8 -> (en maquina2) para ver su ip, la maquina 2 tiene la ip **192.168.56.115** y la maquina 1 tiene la **192.168.56.105**
2. less /var/www/html/hola.html -> para ver el contenido del archivo original de la maquina 2, como vemos, tiene el texto: **"Esto funciona en la maquina2"**
3. Ejecutamos el rsync
4. less /var/www/html/hola.html -> revisamos el archivo y vemos que el contenido ha cambiado, ahora pone: **"Esto funciona en maquina1"**


### Acceso sin contraseña para ssh <a name="id4"></a>




### Programar tareas con crontab <a name="id5"></a>


