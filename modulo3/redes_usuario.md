# Redes definidas por el usuario

Tenemos que hacer una diferenciación entre dos tipos de redes **bridge**: 

* La red creada por defecto por docker para que funcionen todos los contenedores.
* Y las redes "bridge" definidas por el usuario.

Esta red "bridge" por defecto, que es la usada por defecto por los contenedores, se diferencia en varios aspectos de las redes "bridge" que creamos nosotros. Estos aspectos son los siguientes:

* Las redes que nosotros definamos proporcionan **resolución DNS** entre los contenedores.
* Puedo **conectar en caliente** a los contenedores redes "bridge" definidas por el usuario.
* Al usar redes definidas por el usuario obtengo más **aislamiento** y **control**, ya que los contenedores necesarios de una aplicación no comparten la red con otros contenedores.

En definitiva: **Es importante que nuestro contenedores en producción se estén ejecutando sobre una red definida por el usuario.**

Para gestionar las redes creadas por el usuario:

* **docker network ls**: Listado de las redes.
* **docker network create**: Creación de redes. Ejemplos:
    * `docker network create red1`
    * `docker network create -d bridge --subnet 172.24.0.0/16 --gateway 172.24.0.1 red2`
* **docker network rm/prune**: Borrar redes. Teniendo en cuenta que no puedo borrar una red que tenga contenedores que la estén usando. deberé primero borrar los contenedores o desconectar la red.
* **docker network inspect**: Nos da información de la red.

Nota: **Cada red docker que creo crea un puente de red específico para cada red que podemos ver con `ip a`**:

![docker](img/bridge2.png)

## Uso de las redes bridge definidas por el usuario

Vamos a crear una red tipo bridge definida por el usuario con la instrucción `docker network create`:

```bash
$ docker network create red1

```

Como no hemos indicado ninguna configuración en la red que hemos creado, docker asigna un direccionamiento a la red:

```bash
$ docker network inspect red1
[
    {
        "Name": "red1",
        ...
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        ...
]
```

Vamos a crear dos contenedores conectados a dicha red:

```bash
$ docker run -d --name my-apache-app --network red1 -p 8080:80 httpd:2.4
```
Lo primero que vamos a comprobar es la resolución DNS:

```bash
$ docker run -it --name contenedor1 --network red1 debian bash
root@98ab5a0c2f0c:/# apt update && apt install dnsutils -y
...
root@98ab5a0c2f0c:/# dig my-apache-app
...
;; ANSWER SECTION:
my-apache-app.		600	IN	A	172.18.0.2
...
;; SERVER: 127.0.0.11#53(127.0.0.11)
...
```

Podemos comprobar la configuración DNS del contenedor:

```bash
root@98ab5a0c2f0c:/# cat /etc/resolv.conf 
nameserver 127.0.0.11
...
```

Evidentemente desde los dos contenedores se pueden resolver los dos nombres:

```bash
root@98ab5a0c2f0c:/# dig contenedor1
...
;; ANSWER SECTION:
contenedor1.		600	IN	A	172.18.0.3
...
;; SERVER: 127.0.0.11#53(127.0.0.11)
...
```

## Más opciones al trabajar con redes en docker

Podemos conectar "en caliente" un contenedor a una nueva red con:

```
docker network connect <red> <contenedor>
```
Para desconectarla de una red podemos usar: `docker network disconnect`.

Tanto al crear un contenedor con el flag `--network`, como con la instrucción `docker network connect`, podemos usar algunos otros flags:

* `--dns`: para establecer unos servidores DNS predeterminados.
* `--ip`: Para establecer una ip fija en el contenedor.
* `--ip6`: para establecer la dirección de red ipv6
* `--hostname` o `-h`: para establecer el nombre de host del contenedor. Si no lo establezco será el ID del mismo.
* `--add-host`: añade entradas de nuevos hosts en el fichero `/etc/hosts`

Para más información sobre las redes: [Networking overview](https://docs.docker.com/network/).

---

* [Ejemplo 1: Despliegue de la aplicación Guestbook](guestbook.md)
