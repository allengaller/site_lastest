---
title: dockerfile
categories:
- docker
tags:
- detail
---

# about dockerfile


# dockerfile command

- point to a Dockerfile anywhere in your file system
    
        docker build -f /path/to/a/Dockerfile .

- specify a repository and tag at which to save the new image if the build succeeds
    
        docker build -t shykes/myapp .

# dockerfile keyword

- FROM: base image

- MAINTAINER

        MAINTAINER ag "allengaller@gmail.com"

- USER: set user

        USER root

- RUN: run system cmd

        RUN apt-get update
        RUN ["apt-get", "update"]
        RUN apt-get install -y nginx
        RUN touch test.txt && echo "abc" >> abc.txt

- EXPOSE: expose port

- ADD

    The ADD instruction copies new files, directories or remote file URLs from <src> and adds them to the filesystem of the container at the path <dest>.
    
    pattern: ADD <src>... <dest>; ADD ["<src>",... "<dest>"]
    add folder: ADD /webapp /opt/webapp
    add file: ADD abc.txt /opt/
    add network file: ADD https://www.baidu.com/img/bd_logo1.png /opt/

- ENV: set env variable

        ENV WEBAPP_PORT = 9090

- WORKDIR: set working directory
    
    The WORKDIR instruction sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile.The WORKDIR instruction can resolve environment variables previously set using ENV. You can only use environment variables explicitly set in the Dockerfile. For example:

        ENV DIRPATH /path
        WORKDIR $DIRPATH/$DIRNAME
        RUN pwd

    The output of the final pwd command in this Dockerfile would be /path/$DIRNAME.It can be used multiple times in the one Dockerfile. If a relative path is provided, it will be relative to the path of the previous WORKDIR instruction. For example:

        WORKDIR /a
        WORKDIR b
        WORKDIR c
        RUN pwd

    The output of the final pwd command in this Dockerfile would be /a/b/c.
    
        WORKDIR /opt/

- ENTRYPOINT: set boot command, append parameter to boot cmd
    
    An ENTRYPOINT allows you to configure a container that will run as an executable.For example, the following will start nginx with its default content, listening on port 80:
            
        docker run -i -t --rm -p 80:80 nginx
    
        ENTRYPOINT ["ls"]
        ENTRYPOINT ["ls"]
        CMD ["-l", "-a"]

- CMD: set boot parameter
    
        CMD ["ls", "-a", "-l"]
        CMD ls -l -a

- VOLUME: set volume
    
        VOLUME ["/data", "/var/www"]

- ONBUILD: trigger for child image
    
        ONBUILD ADD . /app/src
        ONBUILD RUN echo "on build excuted" >> onbuild.txt

- ARG

- STOPSIGNAL

# best practice [link](https://docs.docker.com/engine/articles/dockerfile_best-practices/)

- Containers should be ephemeral

    The container produced by the image your Dockerfile defines should be as ephemeral as possible.By “ephemeral,” we mean that it can be stopped and destroyed and a new one built and put in place with an absolute minimum of set-up and configuration.

- Use a .dockerignore file

    In most cases, it’s best to put each Dockerfile in an empty directory. Then, add to that directory only the files needed for building the Dockerfile. To increase the build’s performance, you can exclude files and directories by adding a .dockerignore file to that directory as well. This file supports exclusion patterns similar to .gitignore files. For information on creating one, see the .dockerignore file.
    
- Avoid installing unnecessary packages

    In order to reduce complexity, dependencies, file sizes, and build times, you should avoid installing extra or unnecessary packages just because they might be “nice to have.” For example, you don’t need to include a text editor in a database image.

- Run only one process per container

    In almost all cases, you should only run a single process in a single container. Decoupling applications into multiple containers makes it much easier to scale horizontally and reuse containers. If that service depends on another service, make use of container linking.

- Minimize the number of layers

    You need to find the balance between readability (and thus long-term maintainability) of the Dockerfile and minimizing the number of layers it uses. Be strategic and cautious about the number of layers you use.

- Sort multi-line arguments

    Here’s an example from the buildpack-deps image:

    RUN apt-get update && apt-get install -y \
      bzr \
      cvs \
      git \
      mercurial \
      subversion

- Build cache

# .dockerignore

    */temp*
    */*/temp*
    temp?
