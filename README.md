# workshop-docker

![](https://c4.wallpaperflare.com/wallpaper/414/284/24/docker-containers-brand-wallpaper-preview.jpg)

## O que é o Docker?

O Docker é uma plataforma open source que facilita a criação e administração de ambientes isolados (containers)

## Diferença entre VM e Container

![](https://4linux.com.br/wp-content/uploads/2021/08/imagem-1024x594.png)

## Instalação do Docker

+ Linux:

~~~bash
$ curl -fsSL https://get.docker.com | sudo bash
~~~

+ Outros Sistemas.

    [DOCUMENTAÇÃO](https://docs.docker.com/get-docker/)

## Comandos

+ Baixar imagem

~~~bash
$ docker pull ubuntu
~~~

+ Listar images baixadas

~~~bash
$ docker images # docker image ls
~~~

+ Rodandar um container
	flags: -it, -d e muitas outras

~~~bash
$ docker run -it ubuntu
~~~

~~~bash
$ docker run -d --name nginx -p 8081:80 nginx
~~~

+ Listar containers
+ flags: -a

~~~bash
$ docker ps # docker container ls
~~~

+ Para container

~~~bash
$ docker stop nginx # docker container stop nginx
~~~

+ Remover containers

~~~bash
$ docker rm nginx # docker container rm ngnix
~~~

+ Remover containers e imagens

~~~bash
$ docker rmi nginx # docker image rm nginx
~~~

## Rodando Postgres com Docker

+ Postgres

~~~bash
$ docker run -d --rm --name postgres -p 5432:5432 -e POSTGRES_USER=postgres -e POSTGRES_DB=workshop -e POSTGRES_PASSWORD=password postgres:16.3
~~~

## Dockerfile

database.sql

~~~sql
CREATE TABLE users(
	id SERIAL PRIMARY KEY,
	name VARCHAR(60) NOT NULL,
	email VARCHAR(60) UNIQUE NOT NULL,
	password VARCHAR(60) NOT NULL
);
~~~

Dockerfile

~~~dockerfile
FROM postgres:16.3

COPY ./database.sql /docker-entrypoint-initdb.d/
~~~

~~~bash
$ docker build -t workshopdb:1.0 . 
~~~

~~~bash
$ docker run -d --rm --name postgres -p 5432:5432 -e POSTGRES_USER=postgres -e POSTGRES_DB=workshop -e POSTGRES_PASSWORD=password workshop:1.0
~~~

## Compose

docker-compose.yml

~~~yml
name: workshop-docker
services:
  postgres:
    image: postgres:16.3
    container_name: workshop
    restart: always
    ports:
      - 5432:5431
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: workshop
    volumes:
      - postgres:/var/lib/postgresql/data  

volumes:
  postgres:
~~~

~~~bash
$ docker compose up
~~~

~~~bash
$ docker compose down
~~~

### Usando .env

.env

~~~bash
#DATABASE
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=1234
DB_NAME=workshop
~~~

docker-compose.yml

~~~yml
name: workshop-docker
services:
  postgres:
    image: postgres:16.3
    container_name: workshop
    //restart: always
    ports:
      - 5432:${DB_PORT}
    environment:
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
      POSTGRES_DB: ${DB_NAME}
        volumes:
      - postgres:/var/lib/postgresql/data  

volumes:
  postgres:    
~~~

~~~bash
$ docker compose up
~~~