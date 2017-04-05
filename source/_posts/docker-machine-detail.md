---
title: docker machine
categories:
- docker
tags:
- detail
---

# about
code: https://github.com/docker/machine
doc: https://docs.docker.com/machine/overview/

# faq

- What’s the difference between Docker Engine and Docker Machine?

When people say “Docker” they typically mean Docker Engine, the client-server application made up of the Docker daemon, a REST API that specifies interfaces for interacting with the daemon, and a command line interface (CLI) client that talks to the daemon (through the REST API wrapper). Docker Engine accepts docker commands from the CLI, such as docker run <image>, docker ps to list running containers, docker images to list images, and so on.