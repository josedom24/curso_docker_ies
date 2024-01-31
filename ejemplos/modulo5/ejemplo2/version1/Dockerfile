# syntax=docker/dockerfile:1
FROM debian:stable-slim
RUN apt-get update && apt-get install -y apache2 libapache2-mod-php7.4 php7.4 && apt-get clean && rm -rf /var/lib/apt/lists/*
COPY app /var/www/html/
RUN rm /var/www/html/index.html
EXPOSE 80
CMD apache2ctl -D FOREGROUND
