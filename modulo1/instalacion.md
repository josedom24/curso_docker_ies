# Instalación de docker

## Instalación de docker engine

**Docker Engine** está disponible en varias distribuciones de Linux, macOS y Windows 10 a través de Docker Desktop y como instalación binaria estática. 

En este apartado vamos a realizar la instalación binaria de Docker Engine sobre una distribución Linux. Puedes seguir la [documentación oficial](https://docs.docker.com/engine/install/) para aprender el método de instalación en otros sistemas.

Para el sistema operativo Debian puedes seguir las instrucciones y requerimientos en el siguiente [enlace](https://docs.docker.com/engine/install/debian/). Suponemos que vamos a realizar la instalación en un sistema operativo que no tiene instalado versiones antiguas de Docker.

Tenemos varios métodos de instalación disponible: instalando Docker Desktop, usando los repositorios oficiales de Docker, Descargando manualmente los ficheros `deb` o ejecutando un script de instalación.

En nuestro caso usaremos los repositorios apt de de Docker, para ello:

1. Actualizamos los repositorios e instalamos los paquetes necesarios para instalar desde repositorios HTTPS:

        sudo apt-get update
        sudo apt-get install ca-certificates curl gnupg

2. Añadir la clave GPG oficial de Docker:

        sudo install -m 0755 -d /etc/apt/keyrings
        curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
        sudo chmod a+r /etc/apt/keyrings/docker.gpg

3. Añadimos el repositorio oficial de Docker:

        echo \
        "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
        $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
        sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

4. Instalamos Docker Engine:

        sudo apt-get update
        sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

Para manejar Docker con un usuario sin privilegio, debemos añadir el usuario al grupo `docker`, para ello:

    sudo usermod -aG docker $USER

Debes volver a iniciar una terminal con el usuario, o ejecutar el siguiente comando:

    su - $USER

Y finalmente, comprobamos la versión con la que vamos a trabajar:

        docker --version
        Docker version 24.0.7, build afdd53b

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


---

* [El "Hola Mundo" de docker](holamundo.md)
