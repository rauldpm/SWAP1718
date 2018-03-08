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

Antes de empezar esta practica se ha de decir que los hostnames de las maquinas referenciadas en la practica 1 como sigue:

- ubuntu1 como produccion
- ubuntu2 como desarrollo



### Crear tar en con contenido local en equipo remoto <a name="id2"></a>

En este apartado crearemos un fichero tar con el contenido de "/var/www" de desarrollo al home del servidor de produccion, para ello ejecutaremos la siguiente orden:

- tar czf - /var/www | ssh 192.168.56.105 'cat > ~/tar.tgz'

![Imagen CreacionYEnvioTarEnRemoto](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/enviandoTar.png "Imagen CreacionYEnvioTarEnRemoto")

Una vez ejecutado el comando anterior, en el home del servidor de produccion podemos ver que ha aparecido un archivo con el nombre que se le ha dado al enviar.

![Imagen ComprobandoTar](https://github.com/rauldpm/SWAP1718/blob/master/Practica2/Imagenes/archivoRecibido.png "Imagen ComprobandoTar")


### Instalar la herramienta rsync <a name="id3"></a>



### Acceso sin contraseña para ssh <a name="id4"></a>

### Programar tareas con crontab <a name="id5"></a>


