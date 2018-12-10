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

### Debug

No Dockerfile teve que ser adicionado a extensão do xdebug, para que ao criar a imagem ela fosse instalada e habilidate

```Dokerfile

FROM php:7.2-apache-stretch

RUN  docker-php-ext-install pdo_mysql \
    # && pecl install xdebug \
    # $$ docker-php-ext-enable xdebug \
    # $$ service apache2 restart \
    && a2enmod rewrite

RUN curl -fsSL 'https://xdebug.org/files/xdebug-2.6.1.tgz' -o xdebug.tar.gz \
    && mkdir -p xdebug \
    && tar -xf xdebug.tar.gz -C xdebug --strip-components=1 \
    && rm xdebug.tar.gz \
    && ( \
    cd xdebug \
    && phpize \
    && ./configure --enable-xdebug \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r xdebug \
    && docker-php-ext-enable xdebug   

COPY . /var/www/html
COPY docker/vhost.conf  /etc/apache2/sites-available/000-default.conf
COPY docker/config/php/conf.d/*.ini /usr/local/etc/php/conf.d

RUN chown -R www-data:www-data /var/www/html
```

Um aquivo xdebug.ini foi criado e copiado para a imagem com uma configuração padrao para o xdebug

```ini
[xdebug]
xdebug.default_enable = 1
xdebug.remote_autostart = 1
xdebug.remote_connect_back = 0
xdebug.remote_host = 10.0.75.1
xdebug.remote_port = 9001
xdebug.remote_enable = 1
xdebug.idekey = DOCKER_XDEBUG
xdebug.profiler_enable = 1
xdebug.profiler_output_dir = /var/www/html/storage/logs
```
Em que o valor de xdebug.remote_host, deve ser o ip local separado para o docker.

Para o vscode, teve que ser instalado a extensão PHP Debug.
Ao iniciar a debug no VScode um aquivo 'launch.json' teve que ser configurado:

```json
{
    "version": "0.2.0",
    "configurations": [
        
        {
            "name": "Listen for XDebug",
            "type": "php",
            "request": "launch",
            "port": 9001,
            // Path to your source in container
            "pathMappings": {
                "/var/www/html": "${workspaceFolder}",
                "/app": "${workspaceRoot}/app"
            }
        },
        {
            "name": "Launch currently open script",
            "type": "php",
            "request": "launch",
            "program": "${file}",
            "cwd": "${fileDirname}",
            "port": 9001
        }
    ]
}
```
