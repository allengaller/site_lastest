---
title: docker engine
categories:
- docker
tags:
- detail
---

## about
source code: https://github.com/docker/docker
installation: https://www.docker.com/products/overview

## docker engine command

- tips

    Delete all containers:
        
        docker rm $(docker ps -a -q)

    Delete all images:
        
        docker rmi $(docker images -q)

- env

    - info
    
    - version

- life-cycle

    - create:  (ini: stop)

            --restart: check for exit code then restart container; always or on-failure; on-failure:5 restart 5 times max

            sudo docker run --restart=always --name docker_restart -d ubuntu /bin/sh -c "while true;do echo hello world;sleep 1;done"

    - exec: exec cmd insid container

            sudo docker exec -d daemon_dave touch /etc/new_config_file
            sudo docker exec -t -i daemon_dave /bin/bash

    - kill: send SIGKILL signal to container process
    
    - pause

    - restart

    - rm: cannot remove a running container; docker stop or kill first or docker rm -f bad_ubuntu
        
        -q: list only container ids;
        delete all container at once:
            
            docker rm `docker ps -a -q`

    - run: [reference](https://docs.docker.com/engine/reference/run/);(ini: run); 

        equals: docker create & docker start

        2 types of container
        - interactive
            -i: STDIN
            -t: open terminal
            exit?: docker stop or kill;exit

                sudo docker run -i -t --name=inspect_shell ubuntu /bin/bash
                inspect_shell: container name
                base image: ubuntu
                command: /bin/bash
                file system: image+writable layer
                network: virtual network interface bridge to host & set a IP
        
        - daemon: -d
            exit?: docker stop or kill
            
                sudo docker run --name daemon_while -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
            
                return token
            
                docker ps

    - start: start existing container
            
            sudo docker start inspect_shell or cid

    - stop: works for both interactive and daemon container;send SIGTERM signal to container process

            sudo docker stop daemon_while
            sudo docker stop s39c938dj34489d
        
    - unpause

- registry

    - login
    
    - logout
    
    - pull
    
    - push
    
    - search

- image

    - build
    
    - images
    
    - import:             

            cat my_container.rar | sudo docker import - imported:container
            repository: imported, tag: container
            docker import url res:tag

    - load
    
    - rmi
    
    - save
    
    - tag
    
    - commit

- container

    - attach: attach terminal to interactive container
    
    - export

            sudo docker run -i -t --nam=inspect_import ubuntu /bin/bash
            #... do something
            sudo docker export inspect_import > my_container.tar

    - inspect: check out the configuration

            sudo docker inspect daemon_dave

        -f or --format:

            sudo docker inspect --format='{{ .State.Running }}' daemon_dave
    
    - port
    
    - ps: checkout existing container

        -a: all
            Exited(0): exit
        -l: latest container
        -n=x: latest x container

    - rename
    
    - stats
    
    - top: check out UID PID PPID...
        
            sudo docker run -d --name="daemon_top" ubuntu /bin/bash -c 'while true;do sleep 1;done'
        
        2 process:
        
            sudo docker top daemon_top

    - wait
    
    - cp
    
    - diff

- sys log

    - events
    
    - history
    
    - logs

        -f: realtime
        --tail=x: last x line
                
            sudo docker logs -f --tail=5 -t daemon_logs

- other

    - docker daemon: [link](https://docs.docker.com/engine/reference/commandline/daemon/), A self-sufficient runtime for linux containers.
            
        The Docker daemon can listen for Docker Remote API requests via three different types of Socket: unix, tcp, and fd.
        By default, a unix domain socket (or IPC socket) is created at /var/run/docker.sock, requiring either root permission, or docker group membership.