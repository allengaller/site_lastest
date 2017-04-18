---
title: docker network core
categories:
- docker
tags:
- core
- network
---

# about

- docker daemon ini process(docker -d)

    [/var/lib/docker|116d5cd4] +job init_networkdriver()
    [/var/lib/docker|116d5cd4.init_networkdriver()] creating new bridge for docker0
    [/var/lib/docker|116d5cd4.init_networkdriver()] getting iface addr
    [/var/lib/docker|116d5cd4] -job init_networkdriver() = OK (0)

- default mode
    bridge
        docker0

- expose port

    -P: randomly expose a port between 49000-49900
            sudo docker run -d -P traning/webapp python app.py
    -p:
            ip:hostPort:containerPost | ip::containerPort | hostPort:containerPort

- check out network setting

        sudo docker inspect --format '{{.NetworkSettings}}' CID

- container link

    about: docker0 bridge; iptables

    --link name:alias

            sudo docker -d --name dbdata training/postgres
            sudo docker run -d -P --name web --link dbdata:db training/webapp python app.py
            sudo docker inspect web

        Links: /dbdata:/web/db

        how web container use dbdata:

            - env variable
                sudo docker run --rm --name web2 --link dbdata:webdb training/webapp env
                <name>_PORT_<port>_<protocol>_ADDR/PORT/PROTO

            - /etc/hosts

    - ambassador
        about: 代理连接
        connect redis client and server via 2 ambassador
                sudo docker run -d --name redis ag/redis
                sudo docker run -d --name redis ag/redis

# docker network design

https://blog.docker.com/2016/03/docker-networking-design-philosophy/

# cnm design

https://github.com/docker/libnetwork/blob/master/docs/design.md

# docker network command

- docker network ls

- docker network create

- docker network connect

- docker network disconnect

- docker network inspect

- docker network rm

# docker network api

- network driver api

- IPAM api
