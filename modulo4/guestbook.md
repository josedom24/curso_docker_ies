# Ejemplo 1: Despliegue de la aplicación guestbook

En este ejemplo vamos a desplegar con docker-compose la aplicación *guestbook*, que estudiamos en el módulo de redes: [Ejemplo 1: Despliegue de la aplicación Guestbook](../modulo3/guestbook.md).

Puedes encontrar el fichero `docker-compose.yml` en en este [directorio](https://github.com/josedom24/curso_docker_ies/tree/main/ejemplos/modulo4/ejemplo1) del repositorio. 

En el fichero `docker-compose.yml` vamos a definir el escenario. El programa `docker-compose` se debe ejecutar en el directorio donde este ese fichero. 

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
$ docker-compose up -d
Creating network "guestbook_default" with the default driver
Creating guestbook ... done
Creating redis     ... done
```

Para listar los contenedores:

```bash
$ docker-compose ps
  Name                 Command               State          Ports        
-------------------------------------------------------------------------
guestbook   python3 app.py                   Up      0.0.0.0:80->5000/tcp
redis       docker-entrypoint.sh redis ...   Up      6379/tcp            
```

Para parar los contenedores:

```bash
$ docker-compose stop 
Stopping guestbook    ... done
Stopping redis ... done
```

Para eliminar el escenario:

```bash
docker-compose down
Stopping guestbook ... done
Stopping redis     ... done
Removing guestbook ... done
Removing redis     ... done
Removing network guestbook_default
```



---

* [Ejemplo 2: Despliegue de la aplicación Temperaturas](temperaturas.md)



