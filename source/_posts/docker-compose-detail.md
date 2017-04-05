---
title: docker compose
categories:
- docker
tags:
- detail
---

# about

code: https://github.com/docker/compose
doc: https://docs.docker.com/compose/
in production: https://docs.docker.com/compose/production/

orchestration 官方编排工具, 用于将一个多容器应用编排成一个单一应用
Fig工具的替代品: [fig](http://www.fig.sh/)可以快速搭建开发环境,通过YAML文件管理多个容器;
Fit cmd: add fig.yml; fig up

# install

[link](http://docs.docker.com/compose/install/)
            
        docker-compose --version

    64bits Linux or MacOS X:

            curl -L https://github.com/docker/compose/releases/download/1.1.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
            chmod +x /usr/local/bin/docker-compose
    
    win and other:

            sudo pip install -U docker-compose
# command

- docker-compose up -d

- docker exec
    
    docker exec -it example_web_1 bash

- docker-compose stop && docker-compose rm --force

- docker-compose build
    Build or rebuild services

- docker-compose help

- docker-compose kill
    Kill containers 通过发送 SIGKILL 信号来强制停止服务容器
    支持通过参数来指定发送的信号
        docker-compose kill -s SIGINT

- docker-compose logs
    View output from containers

- docker-compose port
    Print the public port for a port binding

- docker-compose ps
    List containers

- docker-compose pull
    Pulls service images

- docker-compose rm
    Remove stopped containers

- docker-compose run

    Run a one-off command 在一个服务上执行一个命令
    docker-compose run ubuntu ping docker.com
        将会启动一个 ubuntu 服务，执行 ping docker.com 命令
    默认情况下，所有关联的服务将会自动被启动，除非这些服务已经在运行中。
    该命令类似启动容器后运行指定的命令，相关卷、链接等等都将会按照期望创建。
    两个不同点：
        给定命令将会覆盖原有的自动运行命令；
        不会自动创建端口，以避免冲突。
    如果不希望自动启动关联的容器，可以使用 --no-deps 选项
        docker-compose run --no-deps web python manage.py shell
        将不会启动 web 容器所关联的其它容器

- docker-compose scale

    Set number of containers for a service 设置同一个服务运行的容器个数
    service=num
    docker-compose scale web=2 worker=3

- docker-compose start: Start services

- docker-compose stop: Stop services

- docker-compose restart

    Restart services
        env variable

            COMPOSE_PROJECT_NAME
                设置通过 Compose 启动的每一个容器前添加的项目名称，默认是当前工作目录的名字。
            COMPOSE_FILE
                设置要使用的 docker-compose.yml 的路径。默认路径是当前工作目录。
            DOCKER_HOST
                设置 Docker daemon 的地址。默认使用 unix:///var/run/docker.sock，与 Docker 客户端采用的默认值一致。
            DOCKER_TLS_VERIFY
                如果设置不为空，则与 Docker daemon 交互通过 TLS 进行。
            DOCKER_CERT_PATH
                配置 TLS 通信所需要的验证（ca.pem、cert.pem 和 key.pem）文件的路径，默认是 ~/.docker 。

- docker-compose up

    Create and start containers
        $ docker-compose up -d

- docker-compose logs

- docker-compose version

    Show the Docker-Compose version information

- docker-compose unpause

    Unpause services

- docker-compose migrate-to-labels

    Recreate containers to add labels
   