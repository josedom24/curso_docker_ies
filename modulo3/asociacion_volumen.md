# Asociando almacenamiento a los contenedores: volúmenes Docker

Veamos como puedo usar los volúmenes y los bind mounts en los contenedores. Aunque dos formas de asociar el almacenamiento al contenedor nosotros vamos a usar el flag `--volume` o `-v`.

Si usamos imágenes de DockerHub, debemos leer la información que cada imagen nos proporciona en su página ya que esa información suele indicar cómo persistir los datos de esa imagen, ya sea con volúmenes o bind mounts, y cuáles son las carpetas importantes en caso de ser imágenes que contengan ciertos servicios (web, base de datos etc...)

## Ejemplo usando volúmenes docker

Lo primero que vamos a hacer es crear un volumen docker:

```bash
$ docker volume create miweb
miweb
```

A continuación creamos un contenedor con el volumen asociado, usando `-v`, y creamos un fichero `index.html`:

```bash
$ docker run -d --name my-apache-app -v miweb:/usr/local/apache2/htdocs -p 8080:80 httpd:2.4
b51f89eb21701362279489c5b52a06b1a44c10194c00291de895b404ab347b80

$ docker exec my-apache-app bash -c 'echo "<h1>Hola</h1>" > /usr/local/apache2/htdocs/index.html'

$ curl http://localhost:8080
<h1>Hola</h1>

$ docker rm -f my-apache-app 
my-apache-app
```

Después de borrar el contenedor, volvemos a crear otro contenedor con el mismo volumen asociado:

```bash
$ docker run -d --name my-apache-app -v miweb:/usr/local/apache2/htdocs -p 8080:80 httpd:2.4
baa3511ca2227e30d90fa2b4b225e209889be4badff583ce58ac1feaa73d5d77
```

Y podemos comprobar que no no se ha perdido la información (el fichero `index.html`):

```bash
$ curl http://localhost:8080
<h1>Hola</h1>
```

Algunas aclaraciones:

* Al no indicar el volumen, se creará un nuevo volumen.
* Si usamos el flag `-v` e indicamos un nombre, se creará un volumen docker nuevo.
* Al usar tanto volúmenes como bind mount, el contenido de lo que tenemos sobreescribirá la carpeta destino en el sistema de ficheros del contenedor en caso de que exista.

---

* [Asociando almacenamiento a los contenedores: bind mount](asociacion_bind_mount.md)
