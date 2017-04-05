---
title: docker storage
categories:
- docker
tags:
- core
- storage
---

# about

- data volumes 数据卷
   
    - create
        
        using dockerfile:
            
                VOLUME /var/lib/postgresql
        
        docker run -v:

                docker run -d -P -v /webapp training/webapp python app.py
                docker inspect my_data
                docker inspect --format {{.Volums}} my_data
    
    - mount file
        
            $ sudo docker run --rm -it -v ~/.bash_history:/.bash_history ubuntu /bin/bash
    
    - mount folder
        
            $ sudo docker run -d -P --name web -v /src/webapp:/opt/webapp training/webapp python app.py
    
    - mount local directory
        
            sudo docker run -d -P --name webapp -v `pwd`:/webapp:ro training/webapp python app.py

- data volume containers 数据卷容器
    
    - tips:

            $ sudo docker run -it -v /dbdata --name dbdata training/postgres
            sudo docker run -d --volumes-from=dbdata --name db1 training/postgres
            sudo docker run -d --name db2 --volumes-from=dbdata training/postgres
            sudo docker run -d --name db2 --volumes-from=db1 training/postgres
            docker rm -v db3

    - migration
        
        backup:

                $ sudo docker run --volumes-from dbdata -v $(pwd):/backup --name worker ubuntu tar cvf /backup/backup.tar /dbdata
                should use sudo 
            
        restore:
        
                $ sudo docker run -v /dbdata --name dbdata2 ubuntu /bin/bash

# link
[Manage data in containers](https://docs.docker.com/engine/userguide/containers/dockervolumes/)

# docker storage command

- docker volume create

- docker volume inspect

- docker volume ls

- docker volume rm