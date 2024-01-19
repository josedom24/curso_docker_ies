# Ejemplo 1: Despliegue de la aplicación guestbook

En este ejemplo vamos a desplegar con Docker Compose la aplicación *guestbook*, que estudiamos en el módulo de redes: [Ejemplo 1: Despliegue de la aplicación Guestbook](../modulo3/guestbook.md).

Puedes encontrar el fichero `docker-compose.yaml` en en este [directorio](https://github.com/josedom24/curso_docker_ies/tree/main/ejemplos/modulo4/ejemplo1) del repositorio. 

En el fichero `docker-compose.yaml` vamos a definir el escenario. El comando `docker compose` se debe ejecutar en el directorio donde este ese fichero. 

```yaml
version: '3.1'
services:
  app:
    container_name: guestbook
    image: iesgn/guestbook
    restart: always
    environment:
      REDIS_SERVER: redis
    ports:
      - 8080:5000
  db:
    container_name: redis
    image: redis
    restart: always
    command: redis-server --appendonly yes
    volumes:
      - redis:/data
volumes:
  redis:
```

Veamos algunas observaciones:

* Aunque ya sabemos que la variable de entorno `REDIS_SERVER` tiene el valor `redis` por defecto, la hemos indicado indicando el nombre del contenedor redis.
* Podríamos haber usado también el nombre del servicio, es decir, `REDIS_SERVER: db`, ya que, como hemos comentado, la resolución se puede hacer usando el nombre del contenedor o el nombre del servicio.
* Como vimos en el ejemplo del módulo 3, al crear el contenedor tenemos que ejecutar el comando `redis-server --appendonly yes` para que redis guarde la información de la base de datos en el directorio `/datos`. Para indicar el comando que hay que ejecutar al crear el contenedor usamos el parámetro `command`.
* Por último indicar que hemos uso un volumen docker llamado `redis` para guardar la información de la base de datos (en el módulo3 usamos un bind mount).

Para crear el escenario:

```bash
$ docker compose up -d
[+] Running 4/4
 ✔ Network guestbook_default  Created                                                            0.3s 
 ✔ Volume "guestbook_redis"   Created                                                            0.0s 
 ✔ Container redis            Started                                                            0.5s 
 ✔ Container guestbook        Started                                                            0.5s
```

Para listar los contenedores:

```bash
$ docker compose ps
NAME        IMAGE             COMMAND                                                SERVICE   CREATED          STATUS          PORTS
guestbook   iesgn/guestbook   "python3 app.py"                                       app       18 seconds ago   Up 16 seconds   0.0.0.0:8080->5000/tcp, :::8080->5000/tcp
redis       redis             "docker-entrypoint.sh redis-server --appendonly yes"   db        18 seconds ago   Up 16 seconds   6379/tcp
```

Para parar los contenedores:

```bash
$ docker compose stop
[+] Stopping 2/2
 ✔ Container guestbook  Stopped                                                                  0.8s 
 ✔ Container redis      Stopped                                                                  0.8s 
```

Para eliminar el escenario:

```bash
$ docker compose down
[+] Running 3/3
 ✔ Container redis            Removed                                                            0.0s 
 ✔ Container guestbook        Removed                                                            0.0s 
 ✔ Network guestbook_default  Removed                                                            0.3s 
```

Recuerda que para eliminar también el volumen usaremos `docker compose down -v`.


---

* [Ejemplo 2: Despliegue de la aplicación Temperaturas](temperaturas.md)



