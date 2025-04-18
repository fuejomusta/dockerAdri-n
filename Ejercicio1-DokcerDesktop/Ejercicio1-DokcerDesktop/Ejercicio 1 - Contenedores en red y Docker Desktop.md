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

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417184709141.png" alt="image-20250417184709141" style="zoom:67%;" />

Una vez instalada, nos aparecerá en la parte izquierda, en la sección de extensiones, pulsamos sobre ella y en el botón central `"Add Network"`

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417184930518.png" alt="image-20250417184930518" style="zoom:67%;" />

La creamos con nombre `redej1` como solicita el enunciado:

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417185040821.png" alt="image-20250417185040821" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417185159770.png" alt="image-20250417185159770" style="zoom:67%;" />

## 2. Crea un contenedor con una imagen de `mariaDB`

Para crear el contenedor, el primer paso es usar el buscador de Docker Desktop para buscar la imagen oficial de `mariadb` (Descargaremos la imagen oficial, que se india con un símbolo de una medalla junto al nombre), la descargamos con el botón `Pull` y creamos el contenedor con el botón `Run`.

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417185900459.png" alt="image-20250417185900459" style="zoom:67%;" />

Para crear el contenedor con los datos requeridos en el enunciado, accedemos a la documentación de la imagen la cual podemos revisar o desde la web de Docker Hub o debajo de los propios botones de `Pull` y `Run`

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417191228887.png" alt="image-20250417191228887" style="zoom:67%;" />

### Definir credenciales 

Al pulsar el botón `Run` desplegamos `Optional settings` y procedemos a definir las opciones:

- Definir la contraseña para el usuario root: Revisando la documentación, observamos que la variable se llama `MARIADB_ROOT_PASSWORD`, le estableceremos de contraseña `root`.

- Definir usuario con mi nombre y contraseña: En la documentación encontramos un enlace ([MariaDB's Knowledge Base : MariaDB Server Docker Official Image Environment Variables ](https://mariadb.com/kb/en/mariadb-server-docker-official-image-environment-variables/)) a la web oficial de `mariadb` en esta observamos el nombre de las variables para asignar valores que deseamos.

  - Nombre de usuario: La variable de entorno según documentación es `MARIADB_USER`, a la cual le daremos como valor mi nombre y apellido, `AdrianGarcia`.
  - Contraseña: La variable de entorno se llama `MARIADB_PASSWORD`, a la que le daremos como valor `laboral`.

- Nombre de la base de datos `DAW`, para ello le asignaremos a la variable de entorno `MARIADB_DATABASE` el valor `DAW`.

  <img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417192839781.png" alt="image-20250417192839781" style="zoom:67%;" />

  

Una vez finalizada la configuración, pulsamos sobre `Run` y comienza a descargarse y configurarse.

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417193104887.png" alt="image-20250417193104887" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417193215356.png" alt="image-20250417193215356" style="zoom:67%;" />

## 3. Crear un contenedor con `Adminer` 

Al igual que se hizo anteriormente con mariadb en el buscador buscamos `Adminer`, lo descargamos con el botón `Pull` y creamos el contenedor con `Run`.

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417194721730.png" alt="image-20250417194721730" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417194803521.png" alt="image-20250417194803521" style="zoom:67%;" />

## 4. Conectar contenedores a redej1

### Configurar redes

Como podemos ver en la siguiente imagen, actualmente tenemos ambos contenedores conectados a la red `bridge` por defecto, debemos desconectarlos y conectarlos a la red creada `redej1`

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417202445909.png" alt="image-20250417202445909" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417203054122.png" alt="image-20250417203054122" style="zoom:67%;" />

Una vez conectados comprobamos su ip con `ifconfig` y realizamos un ping para comprobar la conexión:

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417203344637.png" alt="image-20250417203344637" style="zoom:67%;" />

### Conexión a interfaz grafica

Para conectarnos a través del navegador indicamos:

```
http://localhost:8081/
```

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417204448529.png" alt="image-20250417204448529" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417204516168.png" alt="image-20250417204516168" style="zoom:67%;" />

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

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417205042713.png" alt="image-20250417205042713" style="zoom:67%;" />

Con el script ya ejecutado, podemos comprobar en registros que realmente se han añadido:

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417205213578.png" alt="image-20250417205213578" style="zoom:67%;" />

## 5.Instalación y comprobaciones con `Disk Usage` 

Lo primero que debemos hacer al igual que con `mariadb` y `adminer` es buscarlo en el buscador, nos vamos a la pestaña `Extensions` y pulsamos `Install` 

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417205659282.png" alt="image-20250417205659282" style="zoom:67%;" />

Una vez instalado, en el menú de la izquierda nos vamos a `Extensions` y seleccionamos `Disk usage`, nos mostrará un grafico con el espacio utilizado por las imágenes y los volúmenes y un listado de los mismos:

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417205927928.png" alt="image-20250417205927928" style="zoom:67%;" />

Borramos algún contenedor y alguna imagen

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417210117482.png" alt="image-20250417210117482" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417210217587.png" alt="image-20250417210217587" style="zoom: 67%;" />

Vemos que tenemos mucho almacenamiento usado en cache, pero usando la herramienta `Reclaim space` (botón azul a la derecha de la imagén) la cual nos indica que tenemos 1.03GB para liberar,  lo liberamos:

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417210514101.png" alt="image-20250417210514101" style="zoom:67%;" />

## Borrado de volúmenes, contenedores y la red

Para eliminar los contenedores, en el menú contenedores usamos el botón `delete`  con forma de cubo de basura:

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417210915015.png" alt="image-20250417210915015" style="zoom:67%;" />

<img src="C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417210954310.png" alt="image-20250417210954310" style="zoom:67%;" />

Para borrar la red, nos dirigimos a `Extensions`, y en `PortNavigator` nos mostrará todas las redes, y en la red `redej1` pulsamos en la `X` de arriba a la derecha para borrarla.



![image-20250417211152584](C:\Users\fuejo\OneDrive\Desktop\Ejercicio1-DokcerDesktop\Imagenes\image-20250417211152584.png)



