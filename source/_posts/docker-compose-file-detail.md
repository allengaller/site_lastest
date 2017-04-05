---
title: docker compose
categories:
- docker
tags:
- detail
---

# about

- official: https://docs.docker.com/compose/compose-file/
- django example: https://docs.docker.com/compose/django/
- rails example: https://docs.docker.com/compose/rails/
- machine, swarm, compose: https://blog.docker.com/2015/02/orchestrating-docker-with-machine-swarm-and-compose/

The Compose file is a YAML file defining services, networks and volumes. The default path for a Compose file is ./docker-compose.yml.

A service definition contains configuration which will be applied to each container started for that service, much like passing command-line parameters to docker run. Likewise, network and volume definitions are analogous to docker network create and docker volume create.

As with docker run, options specified in the Dockerfile (e.g., CMD, EXPOSE, VOLUME, ENV) are respected by default - you don’t need to specify them again in docker-compose.yml.

# service configuration reference

- build

# volume configuration reference

# network configuration reference

# versioning

# variable substitution