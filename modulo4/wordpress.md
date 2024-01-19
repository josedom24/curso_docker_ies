# Ejemplo 3: Despliegue de WordPress + Mariadb

En este ejemplo vamos a desplegar con Docker Compose la aplicación WordPress + MariaDB, que estudiamos en el módulo de redes: [Ejemplo 3: Despliegue de Wordpress + mariadb ](../modulo3/wordpress.md).

Puedes encontrar los ficheros `docker-compose.yaml` en este [directorio](https://github.com/josedom24/curso_docker_ies/tree/main/ejemplos/modulo4/ejemplo3) del repositorio. 


## Utilizando volúmenes docker

Por ejemplo para la ejecución de wordpress persistente con volúmenes docker podríamos tener un fichero `docker-compose.yaml` con el siguiente contenido:

```yaml
version: '3.1'
services:
  wordpress:
    container_name: servidor_wp
    image: wordpress
    restart: always
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: user_wp
      WORDPRESS_DB_PASSWORD: asdasd
      WORDPRESS_DB_NAME: bd_wp
    ports:
      - 80:80
    volumes:
      - wordpress_data:/var/www/html/wp-content
  db:
    container_name: servidor_mysql
    image: mariadb
    restart: always
    environment:
      MYSQL_DATABASE: bd_wp
      MYSQL_USER: user_wp
      MYSQL_PASSWORD: asdasd
      MYSQL_ROOT_PASSWORD: asdasd
    volumes:
      - mariadb_data:/var/lib/mysql
volumes:
    wordpress_data:
    mariadb_data:
```

Para crear el escenario:

```bash
$ docker compose up -d
[+] Running 5/5
 ✔ Network wordpress_default          Created                                                    0.2s 
 ✔ Volume "wordpress_wordpress_data"  Created                                                    0.0s 
 ✔ Volume "wordpress_mariadb_data"    Created                                                    0.0s 
 ✔ Container servidor_mysql           Started                                                    0.5s 
 ✔ Container servidor_wp              Started                                                    0.5s 
```

Para listar los contenedores:

```bash
$ docker compose ps
NAME             IMAGE       COMMAND                                     SERVICE     CREATED          STATUS          PORTS
servidor_mysql   mariadb     "docker-entrypoint.sh mariadbd"             db          21 seconds ago   Up 19 seconds   3306/tcp
servidor_wp      wordpress   "docker-entrypoint.sh apache2-foreground"   wordpress   21 seconds ago   Up 19 seconds   0.0.0.0:80->80/tcp, :::80->80/tcp
```

Para parar los contenedores:

```bash
$ docker compose stop
[+] Stopping 2/2
 ✔ Container servidor_mysql  Stopped                                                             0.9s 
 ✔ Container servidor_wp     Stopped                                                             1.8s 
```

Para borrar los contenedores:

```bash
$ docker compose rm
? Going to remove servidor_wp, servidor_mysql Yes
[+] Removing 2/0
 ✔ Container servidor_mysql  Removed                                                             0.0s 
 ✔ Container servidor_wp     Removed                                                             0.0s 
```

Para eliminar el escenario (contenedores, red y volúmenes):

```bash
$ docker compose down -v
...
```

## Utilizando bind-mount

Por ejemplo para la ejecución de wordpress persistente con bind mount podríamos tener un fichero `docker-compose.yaml` con el siguiente contenido:

```yaml
version: '3.1'
services:
  wordpress:
    container_name: servidor_wp
    image: wordpress
    restart: always
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: user_wp
      WORDPRESS_DB_PASSWORD: asdasd
      WORDPRESS_DB_NAME: bd_wp
    ports:
      - 80:80
    volumes:
      - ./wordpress:/var/www/html/wp-content
  db:
    container_name: servidor_mysql
    image: mariadb
    restart: always
    environment:
      MYSQL_DATABASE: bd_wp
      MYSQL_USER: user_wp
      MYSQL_PASSWORD: asdasd
      MYSQL_ROOT_PASSWORD: asdasd
    volumes:
      - ./mysql:/var/lib/mysql
```

---

* [Ejemplo 4: Despliegue de tomcat + nginx](tomcat.md)

