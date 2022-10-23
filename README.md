# Curso Docker

Curso sobre contenedores Docker.

## 1. Introducción a los contenedores Docker
* Introducción a Docker
* [Instalación de docker](modulo1/instalacion.md)
* [El "Hola Mundo" de docker](modulo1/holamundo.md)
* [Ejecución simple de contenedores](modulo1/contenedor.md)
* [Ejecutando un contenedor interactivo](modulo1/interactivo.md)
* [Creando un contenedor demonio](modulo1/demonio.md)
* [Creando un contenedor con un servidor web](modulo1/web.md)
* [Configuración de contenedores con variables de entorno](modulo1/configuracion.md)

## 2. Imágenes Docker 
* Registros de imágenes: Docker Hub
* Gestión de imágenes
* ¿Cómo se organizan las imágenes?
* Creación de contenedores desde imágenes
* Ejemplo: Desplegando la aplicación mediawiki

## 3. Almacenamiento y redes en Docker 
* Volúmenes docker y bind mount
* Asociando almacenamiento a los contenedores: volúmenes Docker
* Asociando almacenamiento a los contenedores: bind mount
* Redes en Docker
* Redes definidas por el usuario
* Ejemplo 1: Despliegue de la aplicación Guestbook
* Ejemplo 2: Despliegue de la aplicación Temperaturas
* Ejemplo 3: Despliegue de Wordpress + mariadb
* Ejemplo 4: Despliegue de tomcat + nginx

## 4. Creando escenarios multicontenedor con docker-compose 
* Instalación de docker-compose
* El fichero docker-compose.yml
* El comando docker-compose
* Almacenamiento con docker-compose
* Ejemplo 1: Despliegue de la aplicación guestbook
* Ejemplo 2: Despliegue de la aplicación Temperaturas
* Ejemplo 3: Despliegue de WordPress + Mariadb
* Ejemplo 4: Despliegue de tomcat + nginx

## 5. Creación de imágenes en docker 
* Creación de imágenes a partir de un contenedor
* Creación de imágenes a partir de un Dockerfile
* Distribución de imágenes
* Ejemplo 1: Construcción de imágenes con una página estática
* Ejemplo 2: Construcción de imágenes con una una aplicación PHP
* Ejemplo 3: Construcción de imágenes con una una aplicación Python
* Ejemplo 4: Construcción de imágenes configurables con variables de entorno
* Ciclo de vida de las aplicaciones
