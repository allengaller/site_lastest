---
title: docker swarm detail
categories:
- docker
tags:
- detail
---

# about



# docker swarm command

- docker-machine ls

- docker-machine create -d virtualbox local

- $(docker-machine env local)

- docker run swarm create

        $  docker-machine create -d virtualbox --swarm --swarm-master --swarm-discovery token://63e7a1adb607ce4db056a29b1f5d30cf swarm-master

        $(docker-machine env --swarm swarm-master)

- docker-machine ls
