---
title: dockerhub
categories:
- docker
tags:
- detail
---

# dockerhub

## about dockerhub [link](https://hub.docker.com)

- types:

        official image
        user image

## command

- docker login

        comfig: cat ~/.dockercfg

- build: Automated Build/Trusted Build
    
- registry

        docker pull registry
        docker run -p 5000:5000 -d -i -t registry
        docker commit 3ie9djk 127.0.0.1:5000/my_image:v1
            [registry_host: registry_port\image_name:image_tag]
        docker push 127.0.0.1:5000/my_image:v1