# Tarea3 - Docker 

> Adrián García de la Cera
>
> Enlace reopositorio GitHub:
>
> https://github.com/fuejomusta/dockerAdri-n.git
>
> Enlace vídeo presentación:
>
> https://www.loom.com/share/31ab362060de4f6cbafd8e188eb7d6d5?sid=1b899e87-8821-4900-b2a8-a2c2dc6f8b3b



[TOC]



## Pasos previos

Lo primero que vamos a realizar es crear el repositorio GitHub, con la rama principal llamada "main".

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250410185722551-1744960018778-1.png" alt="image-20250410185722551" style="zoom:67%;" />

Una vez creado el repositorio, lo clonamos en soucetree para poder trabajar con él en local.

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250410192438513-1744960018779-2.png" alt="image-20250410192438513" style="zoom:67%;" />

Con el repositorio ya creada y clonado en local, procedemos a crear las ramas, una por ejercicio:

```
git branch ejercicio-1
git branch ejercicio-2
git branch ejercicio-3
```

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250418092348069.png" alt="image-20250418092348069" style="zoom:67%;" />

Con las ramas ya creadas, procedemos a guardar en cada una de ellas el ejercicio correspondiente,  realizando el guardado y el commit.

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250418093438348.png" alt="image-20250418093438348" style="zoom:67%;" />

Para fusionar las ramas desde `sourcetree` seleccionamos la rama principal `main` , hacemos clic con botón derecho sobre la rama a fusionar y seleccionamos `Merge ejercicio-*`.

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250418094949558.png" alt="image-20250418094949558" style="zoom:67%;" />

Como vemos en la siguiente imagen de `Sourcetree` todas las ramas se han fusionado con la rama principal `main`

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250418095207358.png" alt="image-20250418095207358" style="zoom:67%;" />

# Ejercicio 1 - Contenedores en red y Docker Desktop

> Adrián García de la Cera



[TOC]

## Enunciado

>Este ejercicio se resolverá con Docker Desktop, si se soluciona con
>comandos la nota será 0
>Si necesitas usar comandos, usa la Terminal integrada en Desktop
>SUGERENCIA: Utiliza la extensión PortNavigator para gestionar las
>redes en Docker Desktop

1.Crea una red bridge `redej1`

2.Crea un contenedor con una imagen de `mariaDB` que estará en la red `redej1` .Este contenedor se ejecutará en segundo plano, y será accesible a través del puerto 3306.

- Definir una contraseña para el usuario `root` , y un usuario con tu nombre de pila y con contraseña. La BD por defecto será `DAW`.
- Genera un script SQL que cree una tabla `módulos` con algunos registros con los nombres de los módulos que estás estudiando.

3.Crear un contenedor con `Adminer` o con `phpMyAdmin` que se pueda conectar al contenedor de la BD.

4.Desde la interfaz gráfica elegida, conéctate a la BD con tu usuario personal, ejecuta el script con los datos de los módulos y muestra la BD y la tabla creados.

5.Elige entre estas dos opciones:

- Instala la extensión `Disk Usage` , muestra el espacio ocupado, borra algo...
- Instala la extensión `Resource usage` y muestra su salida cuando estén activos los contenedores.

## 1.Creación de red bridge redej1

Para crear la red usaremos como aconseja el título la extensión `PortNavigator` , para ello lo primero que debemos realizar es instalarla, en la parte central superior buscamos la extensión y pulsamos sobre el botón `Install`.

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417184709141.png" alt="image-20250417184709141" style="zoom:67%;" />

Una vez instalada, nos aparecerá en la parte izquierda, en la sección de extensiones, pulsamos sobre ella y en el botón central `"Add Network"`

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417184930518.png" alt="image-20250417184930518" style="zoom:67%;" />

La creamos con nombre `redej1` como solicita el enunciado:

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417185040821.png" alt="image-20250417185040821" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417185159770.png" alt="image-20250417185159770" style="zoom:67%;" />

## 2. Crea un contenedor con una imagen de `mariaDB`

Para crear el contenedor, el primer paso es usar el buscador de Docker Desktop para buscar la imagen oficial de `mariadb` (Descargaremos la imagen oficial, que se india con un símbolo de una medalla junto al nombre), la descargamos con el botón `Pull` y creamos el contenedor con el botón `Run`.

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417185900459.png" alt="image-20250417185900459" style="zoom:67%;" />

Para crear el contenedor con los datos requeridos en el enunciado, accedemos a la documentación de la imagen la cual podemos revisar o desde la web de Docker Hub o debajo de los propios botones de `Pull` y `Run`

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417191228887.png" alt="image-20250417191228887" style="zoom:67%;" />

### Definir credenciales 

Al pulsar el botón `Run` desplegamos `Optional settings` y procedemos a definir las opciones:

- Definir la contraseña para el usuario root: Revisando la documentación, observamos que la variable se llama `MARIADB_ROOT_PASSWORD`, le estableceremos de contraseña `root`.

- Definir usuario con mi nombre y contraseña: En la documentación encontramos un enlace ([MariaDB's Knowledge Base : MariaDB Server Docker Official Image Environment Variables ](https://mariadb.com/kb/en/mariadb-server-docker-official-image-environment-variables/)) a la web oficial de `mariadb` en esta observamos el nombre de las variables para asignar valores que deseamos.

  - Nombre de usuario: La variable de entorno según documentación es `MARIADB_USER`, a la cual le daremos como valor mi nombre y apellido, `AdrianGarcia`.
  - Contraseña: La variable de entorno se llama `MARIADB_PASSWORD`, a la que le daremos como valor `laboral`.

- Nombre de la base de datos `DAW`, para ello le asignaremos a la variable de entorno `MARIADB_DATABASE` el valor `DAW`.

  <img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417192839781.png" alt="image-20250417192839781" style="zoom:67%;" />

  

Una vez finalizada la configuración, pulsamos sobre `Run` y comienza a descargarse y configurarse.

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417193104887.png" alt="image-20250417193104887" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417193215356.png" alt="image-20250417193215356" style="zoom:67%;" />

## 3. Crear un contenedor con `Adminer` 

Al igual que se hizo anteriormente con mariadb en el buscador buscamos `Adminer`, lo descargamos con el botón `Pull` y creamos el contenedor con `Run`.

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417194721730.png" alt="image-20250417194721730" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417194803521.png" alt="image-20250417194803521" style="zoom:67%;" />

## 4. Conectar contenedores a redej1

### Configurar redes

Como podemos ver en la siguiente imagen, actualmente tenemos ambos contenedores conectados a la red `bridge` por defecto, debemos desconectarlos y conectarlos a la red creada `redej1`

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417202445909.png" alt="image-20250417202445909" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417203054122.png" alt="image-20250417203054122" style="zoom:67%;" />

Una vez conectados comprobamos su ip con `ifconfig` y realizamos un ping para comprobar la conexión:

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417203344637.png" alt="image-20250417203344637" style="zoom:67%;" />

### Conexión a interfaz grafica

Para conectarnos a través del navegador indicamos:

```
http://localhost:8081/
```

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417204448529.png" alt="image-20250417204448529" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417204516168.png" alt="image-20250417204516168" style="zoom:67%;" />

### Ejecución del script SQL 

Una vez dentro  seleccionaremos Comando SQL y ejecutaremos el script:

```sql
CREATE TABLE modulos (
	id INT AUTO_INCREMENT PRIMARY KEY,
	nombre VARCHAR(75) NOT NULL
);

INSERT INTO modulos (nombre) VALUES
('cliente'),
('interfaces'),
('despliegue'),
('ingles'),
('programación');
```

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417205042713.png" alt="image-20250417205042713" style="zoom:67%;" />

Con el script ya ejecutado, podemos comprobar en registros que realmente se han añadido:

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417205213578.png" alt="image-20250417205213578" style="zoom:67%;" />

## 5.Instalación y comprobaciones con `Disk Usage` 

Lo primero que debemos hacer al igual que con `mariadb` y `adminer` es buscarlo en el buscador, nos vamos a la pestaña `Extensions` y pulsamos `Install` 

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417205659282.png" alt="image-20250417205659282" style="zoom:67%;" />

Una vez instalado, en el menú de la izquierda nos vamos a `Extensions` y seleccionamos `Disk usage`, nos mostrará un grafico con el espacio utilizado por las imágenes y los volúmenes y un listado de los mismos:

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417205927928.png" alt="image-20250417205927928" style="zoom:67%;" />

Borramos algún contenedor y alguna imagen

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417210117482.png" alt="image-20250417210117482" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417210217587.png" alt="image-20250417210217587" style="zoom: 67%;" />

Vemos que tenemos mucho almacenamiento usado en cache, pero usando la herramienta `Reclaim space` (botón azul a la derecha de la imagén) la cual nos indica que tenemos 1.03GB para liberar,  lo liberamos:

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417210514101.png" alt="image-20250417210514101" style="zoom:67%;" />

## Borrado de volúmenes, contenedores y la red

Para eliminar los contenedores, en el menú contenedores usamos el botón `delete`  con forma de cubo de basura:

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417210915015.png" alt="image-20250417210915015" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417210954310.png" alt="image-20250417210954310" style="zoom:67%;" />

Para borrar la red, nos dirigimos a `Extensions`, y en `PortNavigator` nos mostrará todas las redes, y en la red `redej1` pulsamos en la `X` de arriba a la derecha para borrarla.



![image-20250417211152584](C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417211152584.png)





# Ejercicio 2 - Docker Compose

> Adrián García de la Cera



[TOC]

## Enunciado

Explorar la imagen de la aplicación FileBrowser en este repositorio en GitHub: https://hub.docker.com/r/hurlenko/filebrowser

Escribir un fichero compose.yaml para desplegarla. Los datos se pueden guardar utilizando volúmenes o utilizando bind-mount.

Entregar, al menos, las siguientes capturas de pantalla y los comandos y/ooperaciones con Docker Desktop empleados para resolver el ejercicio:

- Captura de pantalla y documento donde se vea el fichero docker-compose.yaml que has creado.

- Captura de pantalla donde se vean los volúmenes/carpetas donde se han almacenado los datos.
- Captura de pantalla donde se vea la aplicación funcionando, sube algún fichero, cambia el lenguaje a español...
- Explicar brevemente cómo funciona esta aplicación y qué hace.

## Desarrollo

El desarrollo del ejercicio se va a realizar en una maquina virtual y lo dividiremos en partes, en la primera crearemos el fichero compose.yaml, y explicaremos sus partes, en la segunda iniciaremos el servicio  accediendo a la web y mostrando su funcionamiento realizando cambio de idioma, etc y por último eliminaremos el contenedor.

Para entrar en contexto, filebrowser es un explorador de archivos web que como su web indica: 

"*filebrowser proporciona una interfaz de gestión de archivos dentro de un directorio especificado y se puede utilizar para cargar, eliminar,  previsualizar, renombrar y editar sus archivos. Permite la creación de  múltiples usuarios y cada usuario puede tener su propio directorio. Se  puede utilizar como una aplicación independiente o como middleware."*

### Creación archivo

Lo primero que vamos a hacer es crear el directorio donde alojar el archivo compose.yaml y el propio archivo con los siguientes comandos:

Creamos el directorio, accedemos al mismo y creamos el archivo compose.yaml:

```
mkdir ejercicioDocker2
cd ejercicioDocker2
touch compose.yaml
```

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417100306398.png" alt="image-20250417100306398" style="zoom:67%;" />

Una vez creado el archivo accedemos al mismo

```
nano compose.yaml
```

Una vez dentro completamos con el siguiente codigo:

```
version: '3.8'

services:
  filebrowser:
    image: hurlenko/filebrowser
    container_name: filebrowser
    restart: unless-stopped
    ports:
      - "8080:8080"
    volumes:
      - ./filebrowser/config:/config
      - ./filebrowser/data:/srv
    environment:
      - FB_BASEURL=/filebrowser
```

El archivo compose.yaml se divide en varias partes:

1. La versión en este caso 3.8
2. Se definen los servicios (contenedores) que se van a desplegar en este caso filebrowser.
3. Se especifica la imagen del contenedor a utilizar.
4. Asignamos un nombre al contenedor.
5. Definimos la política de reinicio, para que se reinicie automáticamente si se detiene excepto si el usuario lo detiene manualmente.
6. Mapeamos el puerto 8080:8080  para acceder a la interfaz.
7. Montamos los volúmenes de configuración y de archivos, para que cuando se elimine o reinicie el servicio no se pierda la información.
8. Configuramos la URL base la aplicación. 

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417102731108.png" alt="image-20250417102731108" style="zoom:67%;" />

Una vez configurado el archivo compose.yaml y guardado ejecutamos el servicio en segundo plano.

### Inicio de servicio

```
docker compose up -d
```

Esto descargará la imagen,  creará la red, creará y arrancara el contendor y montará los volúmenes.

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417103554239.png" alt="image-20250417103554239" style="zoom:67%;" />



Ya creado el archivo compose.yaml y montado comprobamos si accedemos correctamente al servicio.

```
http://localhost:8080
```

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417103756827.png" alt="image-20250417103756827" style="zoom:67%;" />

### Pruebas con filebrowser

Nos logamos con las credenciales que se indican en el enunciado (user: admin  password: admin) y procedemos a cambiar el idioma en Settings

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417104000404.png" alt="image-20250417104000404" style="zoom:67%;" />

Como vemos a continuación, de momento no tenemos ningún archivo subido a filebrowser:

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417104331982.png" alt="image-20250417104331982" style="zoom:67%;" />

Vamos a crear dentro de filebrowser una carpeta llamada "Ejercicio docker" , para ello, en la esquina superior derecha hacemos clic en las 3 barras para que nos despliegue el menú y seleccionamos nueva carpeta, lo que nos abrirá un cuadro para indicar el nombre de la misma, una vez creada directamente accede a la misma, ahora en la parte superior derecha damo a los 3 puntos y seleccionamos subir, nos abrirá el gestor de archivos de linux y seleccionamos el fichero a subir en nuestro caso hemos seleccionado el propio archivo compose.yaml.

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417104831458.png" alt="image-20250417104831458" style="zoom:67%;" />

### Finalización del servicio 

Para finalizar vamos a detener el servicio y eliminar usamos el comando

```
docker compose down
```

En la siguiente imagen vemos como tras detener el servicio, como se han usado volúmenes a persistido la información:

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417111243142.png" alt="image-20250417111243142" style="zoom:67%;" />





# Ejercicio 3 - imagen con Dockerfile - Aplicación web

> Adrián García de la Cera



[TOC]

## Enunciado

Necesitamos un fichero Dockerfile que automatice las siguientes operaciones para crear una imagen que contenga un servidor con un sitio web y un script php. Características de la imagen:

- Usa un contenedor que ejecute una instancia de la imagen `php:7.4- apache` , que se llame ejercicio3 y que sea accesible desde un navegador en el puerto 8000.
- Coloca en el directorio raíz del servicio web ( `/var/www/html` ) un "sitio web" donde figure tu nombre - el sitio deberá tener al menos un archivo `index.html` sencillo y un `archivo .css`.
- Coloca en ese mismo directorio raíz el siguiente script php , llámalo `fecha.php`.

```php
<?php
setlocale(LC_TIME, "es_ES.UTF-8");
$mes_actual = strftime("%B");
$fecha_actual = date("d/m/Y");
$hora_actual = date("H:i:s");
echo "<h1>Información</h1>";
echo "<p>Hoy es $fecha_actual</p>";
echo "<p>El mes es: <strong>$mes_actual</strong></p>";
echo "<p>Hora: $hora_actual</p>";
?>
```

- Ver la salida del script `fecha.php` y de la página `index.html` en el navegador.

Una vez creada la imagen, súbela a tu cuenta de Docker Hub

- Borra la imagen de tu Docker local
- Baja ('pull') de tu cuenta la imagen que acabas de subir
- Muestra las imágenes que tienes
- Ejecuta un contenedor usando esa imagen

Deberás entregar, al menos, las siguientes capturas de pantalla, los comandos empleados y/o operaciones con Docker Desktop para resolver cada apartado:

- creación inicial del contenedor - documenta los pasos hasta el borrado del mismo.

- bloque de código con el Dockerfile.

- creación de la nueva imagen.

- subida de la imagen a tu cuenta de Docker Hub.

- operación de 'pull' de la imagen de Docker Hub

- creación de un nuevo contenedor con esa imagen y su ejecución.

  Cambia el puerto del contenedor, por ejemplo, `- p 1234:80`.

- el acceso al navegador con la página html y con el script `php`.

## Desarrollo

### Creación de archivos

Lo primero que vamos a realizar para la ejecución del ejercicio es crear los ficheros indes.html, estilos.css y fecha.php en lacarpeta web.

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417163858278.png" alt="image-20250417163858278" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417143200203.png" alt="image-20250417143200203" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417143214528.png" alt="image-20250417143214528" style="zoom:67%;" />

Una vez creados estos, creamos el archivo Dockerfile en el directorio raíz, el cual se compone de 3 elementos:

1. FROM: Indica la imagen base para construir la imagen Docker.
2. COPY: Copia el contenido de la carpeta web al directorio `/var/www/html/`.
3. RUN: Ejecuta los comandos `chown -R www-data:www-data /var/www/html` cambia el grupo y el propietario de los archivos al directorio que usa el servidor apache y `chmod -R 755 /var/www/html` modifica los permisos para que el propietario pueda leer, escribir y ejecutar y el grupo solo leer y ejecutar.

```dockerfile
# Usar la imagen base de PHP con Apache
FROM php:7.4-apache

# Copia el contenido de la carpeta ./web/
COPY ./web/ /var/www/html/

# Ejecuta dos comandos dentro del contenedor
RUN chown -R www-data:www-data /var/www/html && chmod -R 755 /var/www/html
```

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417143354493.png" alt="image-20250417143354493" style="zoom:67%;" />

### Creación de la imagen

Para crear la imagen usamos el siguiente comando:

```dockerfile
docker build -t adriangarciaejerciciodokerfile/ejercicio3:v1 .
```

El comando se compone de las siguientes partes:

- docker build: Es el comando para crear la imagen.
- -t: Es la etiqueta que usamos para dar un nombre a la imagen.
- adriangarciaejerciciodokerfile: Es el nombre de la imagen, en este caso comienda con mi nombre y primer apellida (`adrian garcia`) al que se le a agregado el nombre del ejercicio.
- ejercicio3: Se trata del nombre de la imagen.
- v1: Es la etiqueta de la imagen.
- . : Indica la ruta en la que están los archivos necesario como es la actual se indica con ".".

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417165220794.png" alt="image-20250417165220794" style="zoom:67%;" />

Comprobamos que ha creado la imagen:

```
docker images
```

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417165912392.png" alt="image-20250417165912392" style="zoom:67%;" />

Ejecutamos el contenedor en modo desatendido (`-d`) en el puerto 8080:

```dockerfile
docker run -d -p 8080:80 adriangarciaejerciciodokerfile/ejercicio3:v1
```

Como podemos ver en Docker Desktop ya vemos la imagen creada:

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417170551229.png" alt="image-20250417170551229" style="zoom:67%;" />

Comprobamos si se accede a través del navegador:

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417171939437.png" alt="image-20250417171939437" style="zoom:67%;" />

### Comprobaciones 

#### Subida de imagenes

Para subir una imagen a Docker Hub lo primero es logarse con el comando `docker login`

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417172953028.png" alt="image-20250417172953028" style="zoom: 67%;" />

En este caso al intentar subir la imagen a Docker hub da error por que el nombre indicado en la imagen no coincide con el usuario de Docker hub, por lo que procedemos a modificar la etiqueta de la imagen de `adriangarciaejerciciodokerfile/ejercicio3:v1` a `fuejomusta/ejercicio3:v1`, creando una nueva etiqueta para esa imagen y permitiendo subirla:

```
docker tag adriangarciaejerciciodokerfile/ejercicio3:v1 fuejomusta/ejercicio3:v1
```

Una vez solucionado el problema de la etiqueta tenemos 2 opciones para subir la imagen a Docker Hub, a través de Docker Desktop, de manera muy sencilla, junto a la etiqueta aparecen 3 simbolos, un botón de play, un cubo de basura y 3 puntos verticales que al pulsar sobre ellos nos dejara indicar `Push to Docker Hub`, lo que iniciará la subida:

# <img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417174623200.png" alt="image-20250417174623200" style="zoom:67%;" />

Otra manera es a través de la terminal con el código:

```
docker push fuejomusta/ejercicio3:v1
```

Una vez confirmado que ha subido la imagen, accedemos a nuestro repositorio en la web de Docker Hub y confirmamos que está la imagen creada.

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417174826890.png" alt="image-20250417174826890" style="zoom:67%;" />

#### Borrado y descarga de imagen

Una vez subida la imagen procedemos a borrar la imagen local bien con el botón con símbolo de basura de Docker Desktop o con el comando: 

```
docker rmi fuejomusta/ejercicio3:v1
```

Es posible que nos dé error por estar trabajando el contenedor, en tal caso será necesario comprobar el contenedor (`docker ps -a`), parar la ejecución del contendor (`docker stop nombre contenedor`) y luego borrarlo (`docker rm -f nombre contenedor`).

Una ver borrado comprobamos que ya no existe la imagen en Docker Desktop o con comando:

```
docker images
```

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417175945155.png" alt="image-20250417175945155" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417180006659.png" alt="image-20250417180006659" style="zoom:67%;" />

Para descargar la imagen de nuevo, al igual que para eliminarla existen 2 posibilidades, buscarla desde Docker Desktop y descargarla con el botón "`Pull`"o directamente con el comando:

```
docker pull fuejomusta/ejercicio3:v1
```

Y comprobamos de nuevo tenemos la imagen

 <img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417182343960.png" alt="image-20250417182343960" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417182402419.png" alt="image-20250417182402419" style="zoom:67%;" />

Una vez descargada, es necesario iniciarla, para ello usamos el comando:

```
docker run -d -p 8080:80 fuejomusta/ejercicio3:v1
```

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417183010253.png" alt="image-20250417183010253" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Tarea Docker\Imagenes\image-20250417182952574.png" alt="image-20250417182952574" style="zoom:67%;" />