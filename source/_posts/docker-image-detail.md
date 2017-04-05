---
title: docker image
categories:
- docker
tags:
- detail
---

# about docker image

- standard: [Docker Image Specification](https://github.com/docker/docker/blob/master/image/spec/v1.md)

- layer

        r & w layer-container
        add nginx-image2
        add nginx-image1
        ubuntu-base image
        kernel-bootfs

- duplication while writing 写时复制机制

# docker image command
        
- docker pull

- docker run

- docker images: check out
    
        docker images ububtu

- docker inspect

        docker inspect ubuntu

- docker search: AUTOMATED-automatic build

- docker rmi: delete image

        docker rmi c03k349dfjn2
    
    -f if some container depends on this image:

        docker rmi -f ubuntu
    
    delete all:
        
        docker rm $(docker ps -a -q)

- docker commit: one way to create local image, the other way is dockerfile
    commit changes to user image
    
        sudo docker run -t -i ubuntu
        apt-get install sqlite3
        echo 'test docker commit' >> hellodocker
        exit
        sudo docker commit -m="message" --author="ag" CONTAINERID ag/sqlite3:v1
        sudo docker run -t -i ag/sqlite3:v1
        cat hellodocker
        sqlite3 -version

- docker build: build image with dockerfile
    
    -rm=false: do not delete the tmp image while building

    -t: set namespace, repo name, tag

        sudo docker build -t ag/test:v1

- docker tag

        sudo docker tag ag/test:v1 ag/test:v2
        (v1 and v2 will have the same image id)
    
    build with github:
    
        sudo docker build -t ag/test:v1 git://github.com/ag/dockerfile.git

- docker save

- docker load

- docker diff
    
        docker diff container