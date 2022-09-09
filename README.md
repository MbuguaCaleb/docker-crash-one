## demo app - developing with Docker

## Create docker netowork


```
docker network create mong--network

```

## Start Mongo

```

Mongo Docker
docker run -p 27017:27017 -d -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo

```
```

Mongo Express

docker run -d -p 8081:8081 \
> -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
> -e ME_CONFIG_MONGODB_ADMINPASSWORD=password\              
> --net mongo-network \
> --name mongo-express \
> -e ME_CONFIG_MONGODB_SERVER=mongodb \
> mongo-express

```

**Docker Compose**

```
It makes running multiple containers with many configurations easier that when using the docker run command.

The docker_compose.yaml is a very important file where i run all the commands for running my multiple
containers...

Docker compose is a structured way to contain very normal docker commands.

N/B

Docker compose takes care of creating a common network for my Containers.

All containers in docker-compose.yaml are in the same network meaning 
they will communicate to each other via the name


Identation in the docker_compose.yaml must be correct

Docker compose is where i put my container start scripts,


```


**Docker Compose**

```
Starting all containers inside my mongo yaml via docker-compose


docker-compose -f mongo.yaml up 


Docker compose Up-detached Mode

docker-compose -f mongo.yaml up -d


Stop container(s) with docker compose

docker-compose -f mongo.yaml down


Stopping the containers with docker compose down also removes the network.


(Docker compose Yaml is where i write the start up scripts for the IMAGES

(Docker Hub Images my application depends upon)))

```


**Creating an Image of Our Own Application**

**Docker File**

```
How do i build a docker image for our Application.

To build a docker image we have to copy the contents of our application(artifact)
into the docker file-[jar,war,bundle.js]

A Docker File is a blue print for building images.


Docker file cheatsheet,

The purpose of the docker file is to copy the contents of my application and build an image.


(a)
FROM node ---i will have node js in my imahe(i can choose to have linux as my base image bit is a lot of work to then install node)

(b)

ENV MONGO_DB_USERNAME = admin
    MONGO_DB_PASSWORD =password

I can have my enviromnent variables as well in my docker file.


(c)
RUN {linux command}
RUN mkdir -p /home/app

With run i can be able to execute any linux command from my container.

(N/B)RUN command executes in my Container.

(d)

COPY {files in source}/{files in destination}
COPY . /home/app
COPY ./app /home/app


It brings the executable part of my application that i would love to run in my container.

(It can be a Jar file)
(It can be all my application files/JS Bundle)

(e)

CMD ["node","server.js"]

CMD is the entry point command of my application.

(It is the first command and the Only command that will get executed in my container)


By this i build an image and i can have the container of my application running.


N/B

I can have may run commands

But i can only have one entry point CMD Command.


```


**Building Docker Image FROM Docker File**

```

We use docker-build which takes in two arguments,


docker build -t my-app:1.0 .


(1) -t (tag) The name of the Image


(2) path to my Docker file (If i am executing the docker command in same folder i just say .)



N/B
Whenever i adjust a docker file, i must rebuild my image

Before deleting an image i must delete a container first,

There is no point of a container remaining if i have no image.

delete a container
docker ps -a |grep my-app
docker rm 56850d82eaf


delete an image
docker rmi 125da3336301


Getting into the container commandline,

docker exec -it f52bb6c5f229 /bin/sh 
docker exec -it f52bb6c5f229 /bin/bash

env
i see the environment variables


```

**Jenkins File**

```
Jenkins is a continuous integration tool.

It does two things:

1. It builds my application and genrates a docker image from it.

2. It then pushes the image of my application into a docker repository.


(Jenkins builds a docker image from the docker file)

(just like docker build does)

(It is automatic and does this once we push a commit)

For the purpose of the image being shared to other people thats where the
docker registry comes in Play.


```

**N/B**

```
When i restart a container,all the information gets Lost.


Docker volumes is what helps me have persistence between container restarts.



```