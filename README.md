# docker-for-php-developers

## Basics

Para dar build(criar) na imagem em basics/docker-phpinfo e rodar o container dessa imagem

```sh
$ docker build -t phpinfo .
$ docker run -p 8080:80 -d --name=my-phpinfo phpinfo
```

Subindo containers em basics/docker-phpinfo dando build nas images com docker-compose

```sh
$ docker-compose up --build
# Or, if you want to run it in the background
$ docker-compose up -d --build
# Now list all the containers running
$ docker-compose ps
```