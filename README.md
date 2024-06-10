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

+ moo

~~~dockerfile
FROM ubuntu:24.04
CMD ["apt", "moo"]
~~~

~~~bash
$ docker build -t moo . 
~~~

~~~bash
$ docker run --rm moo
~~~

+ neofetch

~~~dockerfile
FROM ubuntu:24.04
RUN apt update -y && apt install neofetch -y
CMD ["neofetch"]
~~~

~~~bash
$ docker build -t neofetch . 
~~~

~~~bash
$ docker run --rm  neofetch
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

### Usando .env

.env

~~~bash
#DATABASE
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=password
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