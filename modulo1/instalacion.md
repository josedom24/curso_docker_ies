# Instalación de docker

## Instalación de la versión de la comunidad de docker: moby

Si usamos debian o ubuntu podemos realizar la instalación de la versión de la comunidad:

```bash
apt install docker.io
```

Si queremos usar el cliente de docker con un usuario sin privilegios:

```bash
usermod -aG docker usuario
```

El caso de Debian 12 la versión de la comunidad es la siguiente:

```bash
$ docker --version
Docker version 20.10.24+dfsg1, build 297e128
```

En es caso de Ubuntu 20.04, la versión será:

```bash
$ docker --version
Docker version 20.10.7, build 20.10.7-0ubuntu5~20.04.2
```

Si queréis instalar la versión de Docker ofrecida por la empresa: *docker-ce*, podéis encontrar los distintos métodos y las distintas plataformas en la  la siguiente [página](https://docs.docker.com/get-docker/).

---

* [El "Hola Mundo" de docker](holamundo.md)
