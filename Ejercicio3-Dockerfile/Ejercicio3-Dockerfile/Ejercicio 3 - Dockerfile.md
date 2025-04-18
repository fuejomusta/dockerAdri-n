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

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417163858278.png" alt="image-20250417163858278" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417143200203.png" alt="image-20250417143200203" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417143214528.png" alt="image-20250417143214528" style="zoom:67%;" />

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

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417143354493.png" alt="image-20250417143354493" style="zoom:67%;" />

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

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417165220794.png" alt="image-20250417165220794" style="zoom:67%;" />

Comprobamos que ha creado la imagen:

```
docker images
```

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417165912392.png" alt="image-20250417165912392" style="zoom:67%;" />

Ejecutamos el contenedor en modo desatendido (`-d`) en el puerto 8080:

```dockerfile
docker run -d -p 8080:80 adriangarciaejerciciodokerfile/ejercicio3:v1
```

Como podemos ver en Docker Desktop ya vemos la imagen creada:

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417170551229.png" alt="image-20250417170551229" style="zoom:67%;" />

Comprobamos si se accede a través del navegador:

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417171939437.png" alt="image-20250417171939437" style="zoom:67%;" />

### Comprobaciones 

#### Subida de imagenes

Para subir una imagen a Docker Hub lo primero es logarse con el comando `docker login`

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417172953028.png" alt="image-20250417172953028" style="zoom: 67%;" />

En este caso al intentar subir la imagen a Docker hub da error por que el nombre indicado en la imagen no coincide con el usuario de Docker hub, por lo que procedemos a modificar la etiqueta de la imagen de `adriangarciaejerciciodokerfile/ejercicio3:v1` a `fuejomusta/ejercicio3:v1`, creando una nueva etiqueta para esa imagen y permitiendo subirla:

```
docker tag adriangarciaejerciciodokerfile/ejercicio3:v1 fuejomusta/ejercicio3:v1
```

Una vez solucionado el problema de la etiqueta tenemos 2 opciones para subir la imagen a Docker Hub, a través de Docker Desktop, de manera muy sencilla, junto a la etiqueta aparecen 3 simbolos, un botón de play, un cubo de basura y 3 puntos verticales que al pulsar sobre ellos nos dejara indicar `Push to Docker Hub`, lo que iniciará la subida:

# <img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417174623200.png" alt="image-20250417174623200" style="zoom:67%;" />

Otra manera es a través de la terminal con el código:

```
docker push fuejomusta/ejercicio3:v1
```

Una vez confirmado que ha subido la imagen, accedemos a nuestro repositorio en la web de Docker Hub y confirmamos que está la imagen creada.

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417174826890.png" alt="image-20250417174826890" style="zoom:67%;" />

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

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417175945155.png" alt="image-20250417175945155" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417180006659.png" alt="image-20250417180006659" style="zoom:67%;" />

Para descargar la imagen de nuevo, al igual que para eliminarla existen 2 posibilidades, buscarla desde Docker Desktop y descargarla con el botón "`Pull`"o directamente con el comando:

```
docker pull fuejomusta/ejercicio3:v1
```

Y comprobamos de nuevo tenemos la imagen

 <img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417182343960.png" alt="image-20250417182343960" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417182402419.png" alt="image-20250417182402419" style="zoom:67%;" />

Una vez descargada, es necesario iniciarla, para ello usamos el comando:

```
docker run -d -p 8080:80 fuejomusta/ejercicio3:v1
```

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417183010253.png" alt="image-20250417183010253" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio3-Dockerfile\Imagenes\image-20250417182952574.png" alt="image-20250417182952574" style="zoom:67%;" />