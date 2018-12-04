# docker-for-php-developers

## Files and videos
[Link](https://gumroad.com/d/60d1328259cbf9b23dcc371f0e8772fa)


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

## LAMP

Na imagem php do docker ja vem com um script para instalar extensoes php: docker-php-ext-install

WORDIR no dockerfile muda o diretorio que o container inicial


## Laravel

Build de uma imagem de um Dockerfile, em que o arquivo está em uma pasta direfente, considerando o contexto (pasta onde será considerada a raiz do copy) a pasta atual

```sh
docker build -t docker-laravel -f docker\Dockerfile
```

Executando o container criado acima:

```sh
docker run -d -p 8080:80 docker-laravel
```