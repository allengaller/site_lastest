    ```
        containers:
        web:
         build: .
         command: python app.py
         ports:
         - "5000:5000"
         volumes:
         - .:/code
         links:
         - redis
         environment:
         - PYTHONUNBUFFERED=1
        redis:
         image: redis:latest
         command: redis-server --appendonly yes
     ```

     ```
        wiki2:
        image: 'nickstenning/mediawiki'
        ports:
            - "8880:80"
        links:
            - db:database
        volumes:
            - /data/wiki2:/data

        db:
        image: "mysql"
        expose:
            - "3306"
        environment:
            - MYSQL_ROOT_PASSWORD=defaultpass

    上面的YAML文件定义了两个容器应用，第一个容器运行Python应用，并通过当前目录的Dockerfile文件构建。
    第二个容器是从Docker Hub注册中心的Redis官方仓库中构建。links指令用来定义依赖，意思是Python应用依赖于Redis应用。
    定义完成后，通过下面的命令来启动应用：

    docker-compose up

    links指令关注的是Python和Redis容器之间的依赖关系，Redis容器是最先开始构建，紧随其后的是Python容器。

# Variable substitution
            
Both $VARIABLE and ${VARIABLE} syntax are supported. 
Extended shell-style features, such as ${VARIABLE-default} and ${VARIABLE/foo/bar}, are not supported.
            db:
  image: "postgres:${POSTGRES_VERSION}"
            web:
  build: .
  command: "$$VAR_NOT_INTERPOLATED_BY_COMPOSE"
        
# install

    curl -L https://raw.githubusercontent.com/docker/compose/$(docker-compose --version | awk 'NR==1{print $NF}')/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose
        doc
            docker-compose.yml reference
                https://docs.docker.com/compose/yml/
        keywords
            image
                指定为镜像名称或镜像 ID。如果镜像在本地不存在，Compose 将会尝试拉去这个镜像
                image: ubuntu
            build
                指定 Dockerfile 所在文件夹的路径
                Compose 将会利用它自动构建这个镜像，然后使用这个镜像
                build: /path/to/build/dir
            dockerfile
            command
                覆盖容器启动后默认执行的命令。
                command: bundle exec thin -p 3000
            links
                链接到其它服务中的容器
                使用服务名称（同时作为别名）或服务名称：服务别名 （SERVICE:ALIAS） 格式都可以
                    links:
 - db
 - db:database
 - redis
                使用的别名将会自动在服务容器中的 /etc/hosts 里创建,相应的环境变量也将被创建
                    172.17.2.186  db
172.17.2.186  database
172.17.2.187  redis
            external_links
                链接到 docker-compose.yml 外部的容器
                甚至 并非 Compose 管理的容器。参数格式跟 links 类似
                external_links:
 - redis_1
 - project_db_1:mysql
 - project_db_1:postgresql
            extra_hosts
            ports
                暴露端口信息
                使用宿主：容器 （HOST:CONTAINER）格式或者仅仅指定容器的端口（宿主将会随机选择端口）都可以
                ports:
 - "3000"
 - "8000:8000"
 - "49100:22"
 - "127.0.0.1:8001:8001"
            expose
                暴露端口，但不映射到宿主机，只被连接的服务访问
                仅可以指定内部端口为参数
                expose:
 - "3000"
 - "8000"
            volumes
                卷挂载路径设置
                可以设置宿主机路径 （HOST:CONTAINER） 或加上访问模式 （HOST:CONTAINER:ro）
                volumes:
 - /var/lib/mysql
 - cache/:/tmp/cache
 - ~/configs:/etc/configs/:ro
            volumes_from
                从另一个服务或容器挂载它的所有卷
                volumes_from:
 - service_name
 - container_name
            environment
                设置环境变量。你可以使用数组或字典两种格式。
                只给定名称的变量会自动获取它在 Compose 主机上的值，可以用来防止泄露不必要的数据
                environment:
  RACK_ENV: development
  SESSION_SECRET:

environment:
  - RACK_ENV=development
  - SESSION_SECRET
            env_file
                从文件中获取环境变量，可以为单独的文件路径或列表。
                如果通过 docker-compose -f FILE 指定了模板文件，则 env_file 中路径会基于模板文件路径。
                如果有变量名称与 environment 指令冲突，则以后者为准。
                env_file: .env

env_file:
  - ./common.env
  - ./apps/web.env
  - /opt/secrets.env
                环境变量文件中每一行必须符合格式，支持 # 开头的注释行
                # common.env: Set Rails/Rack environment
RACK_ENV=development
            extends
                基于已有的服务进行扩展
                例如我们已经有了一个 webapp 服务，模板文件为 common.yml
                # common.yml
webapp:
  build: ./webapp
  environment:
    - DEBUG=false
    - SEND_EMAILS=false
                编写一个新的 development.yml 文件，使用 common.yml 中的 webapp 服务进行扩展
                # development.yml
web:
  extends:
    file: common.yml
    service: webapp
  ports:
    - "8000:8000"
  links:
    - db
  environment:
    - DEBUG=true
db:
  image: postgres
                后者会自动继承 common.yml 中的 webapp 服务及相关环节变量
            labels
            container_name
            log driver
            net
                设置网络模式
                使用和 docker client 的 --net 参数一样的值
                net: "bridge"
net: "none"
net: "container:[name or id]"
net: "host"
            pid
                跟主机系统共享进程命名空间
                打开该选项的容器可以相互通过进程 ID 来访问和操作
                pid: "host"
            dns
                配置 DNS 服务器
                可以是一个值，也可以是一个列表
                dns: 8.8.8.8
- dns:

  - 8.8.8.8
  - 9.9.9.9
            cap_add, cap_drop
                添加或放弃容器的 Linux 能力（Capabiliity）
                cap_add:
  - ALL

- cap_drop:

  - NET_ADMIN
  - SYS_ADMIN
            dns_search
                配置 DNS 搜索域
                可以是一个值，也可以是一个列表
                dns_search: example.com
- dns_search:

  - domain1.example.com
  - domain2.example.com
            devices
            security_opt
            working_dir, entrypoint, user, hostname, domainname, 
mac_address, mem_limit, memswap_limit, privileged, 
restart, stdin_open, tty, cpu_shares, cpuset, 
read_only, volume_driver
                这些都是和 docker run 支持的选项类似
                cpu_shares: 73

working_dir: /code
entrypoint: /code/entrypoint.sh
user: postgresql

hostname: foo
domainname: foo.com

mem_limit: 1000000000
privileged: true

restart: always

stdin_open: true
tty: true
        keyword
            build
                Path to a directory containing a Dockerfile.
                build: /path/to/build/dir
            cap_add, cap_drop
                Add or drop container capabilities. See man 7 capabilities for a full list.
                cap_add:
  - ALL

cap_drop:
  - NET_ADMIN
  - SYS_ADMIN
            command
                Override the default command.
                command: bundle exec thin -p 3000
            cgroup_parent
                Specify an optional parent cgroup for the container.
                cgroup_parent: m-executor-abcd
            container_name
                Specify a custom container name, rather than a generated default name.
                container_name: my-web-container
            devices
                List of device mappings. Uses the same format as the --device docker client create option.
                devices:
  - "/dev/ttyUSB0:/dev/ttyUSB0"
            dns
                Custom DNS servers. Can be a single value or a list.
                dns: 8.8.8.8
dns:
  - 8.8.8.8
  - 9.9.9.9
            dns_search
                Custom DNS search domains. Can be a single value or a list.
                dns_search: example.com
dns_search:
  - dc1.example.com
  - dc2.example.com
            dockerfile
                Alternate Dockerfile.
Compose will use an alternate file to build with.
                dockerfile: Dockerfile-alternate
            env_file
                Add environment variables from a file. Can be a single value or a list.
If you have specified a Compose file with docker-compose -f FILE, 
paths in env_file are relative to the directory that file is in.
Environment variables specified in environment override these values.
                env_file: .env

env_file:
  - ./common.env
  - ./apps/web.env
  - /opt/secrets.env
                Compose expects each line in an env file to be in VAR=VAL format.
 Lines beginning with # (i.e. comments) are ignored, as are blank lines.

# Set Rails/Rack environment
RACK_ENV=development
            environment
                Add environment variables. You can use either an array or a dictionary.
                Any boolean values; true, false, yes no, need to be enclosed in quotes
 to ensure they are not converted to True or False by the YML parser.
                Environment variables with only a key are resolved to their values on the machine Compose is running on, 
which can be helpful for secret or host-specific values.
                environment:
  RACK_ENV: development
  SHOW: 'true'
  SESSION_SECRET:

environment:
  - RACK_ENV=development
  - SHOW=true
  - SESSION_SECRET
            expose
                Expose ports without publishing them to the host machine - 
they’ll only be accessible to linked services. Only the internal port can be specified.
                expose:
 - "3000"
 - "8000"
            extends
                Extend another service, in the current file or another, optionally overriding configuration.
You can use extends on any service together with other configuration keys.
 The extends value must be a dictionary defined with a required service and an optional file key.
                extends:
  file: common.yml
  service: webapp
            external_links
                Link to containers started outside this docker-compose.yml or even outside of Compose,
 especially for containers that provide shared or common services. 
external_links follow semantics similar to links when specifying both the container name and the link alias (CONTAINER:ALIAS).
                external_links:
 - redis_1
 - project_db_1:mysql
 - project_db_1:postgresql
            extra_hosts
                Add hostname mappings. Use the same values as the docker client --add-host parameter.
                extra_hosts:
 - "somehost:162.242.195.82"
 - "otherhost:50.31.209.229"
                An entry with the ip address and hostname will be created in /etc/hosts inside containers for this service, e.g:

162.242.195.82  somehost
50.31.209.229   otherhost
            image
                Tag or partial image ID. Can be local or remote - 
Compose will attempt to pull if it doesn’t exist locally.
                image: ubuntu
image: orchardup/postgresql
image: a4bc65fd
            labels
                Add metadata to containers using Docker labels. You can use either an array or a dictionary.
It’s recommended that you use reverse-DNS notation to prevent your labels from conflicting with those used by other software.
                labels:
  com.example.description: "Accounting webapp"
  com.example.department: "Finance"
  com.example.label-with-empty-value: ""

labels:
  - "com.example.description=Accounting webapp"
  - "com.example.department=Finance"
  - "com.example.label-with-empty-value"
            links
                Link to containers in another service. 
Either specify both the service name and the link alias (SERVICE:ALIAS), 
or just the service name (which will also be used for the alias).
                links:
 - db
 - db:database
 - redis
                An entry with the alias’ name will be created in /etc/hosts 
inside containers for this service, e.g:

172.17.2.186  db
172.17.2.186  database
172.17.2.187  redis
            log_driver
                Specify a logging driver for the service’s containers, as with the --log-driver option for docker run
                The default value is json-file.
Note: Only the json-file driver makes the logs available directly from docker-compose up and docker-compose logs. 
Using any other driver will not print any logs.
                log_driver: "json-file"
log_driver: "syslog"
log_driver: "none"
            log_opt
                Specify logging options with log_opt for the logging driver, as with the --log-opt option for docker run.
                log_driver: "syslog"
log_opt:
  syslog-address: "tcp://192.168.0.42:123"
            net
                Networking mode. Use the same values as the docker client --net parameter.
                net: "bridge"
net: "none"
net: "container:[name or id]"
net: "host"
            pid
                pid: "host"
                Sets the PID mode to the host PID mode. 
This turns on sharing between container and the host oprating system the PID address space.
 Containers launched with this flag will be able to access and manipulate other containers
 in the bare-metal machine’s namespace and vise-versa.
            ports
                Expose ports. Either specify both ports (HOST:CONTAINER), 
or just the container port (a random host port will be chosen).
                Note: When mapping ports in the HOST:CONTAINER format, 
you may experience erroneous results when using a container port lower than 60, 
because YAML will parse numbers in the format xx:yy as sexagesimal (base 60). 
For this reason, we recommend always explicitly specifying your port mappings as strings.
                ports:
 - "3000"
 - "3000-3005"
 - "8000:8000"
 - "9090-9091:8080-8081"
 - "49100:22"
 - "127.0.0.1:8001:8001"
 - "127.0.0.1:5000-5010:5000-5010"
            security_opt
                Override the default labeling scheme for each container.
                security_opt:
    - label:user:USER
    - label:role:ROLE
            ulimits
                Override the default ulimits for a container. 
You can either specify a single limit as an integer or soft/hard limits as a mapping.
                ulimits:
    nproc: 65535
    nofile:
      soft: 20000
      hard: 40000
            volumes, volume_driver
                Mount paths as volumes, optionally specifying a path on the host machine 
(HOST:CONTAINER), or an access mode (HOST:CONTAINER:ro).
                volumes:
 - /var/lib/mysql
 - ./cache:/tmp/cache
 - ~/configs:/etc/configs/:ro
                You can mount a relative path on the host, which will expand relative to the director
y of the Compose configuration file being used. Relative paths should always begin with . or ...
                If you use a volume name (instead of a volume path), you may also specify a volume_driver.
volume_driver: mydriver
Note: No path expansion will be done if you have also specified a volume_driver.
            volumes_from
                Mount all of the volumes from another service or container, 
optionally specifying read-only access(ro) or read-write(rw).
                volumes_from:
 - service_name
 - container_name
 - service_name:rw
            cpu_shares, cpuset, domainname, entrypoint, hostname, 
ipc, mac_address, mem_limit, memswap_limit, privileged, 
read_only, restart, stdin_open, tty, user, working_dir
                Each of these is a single value, analogous to its docker run counterpart.
                cpu_shares: 73
cpuset: 0,1

entrypoint: /code/entrypoint.sh
user: postgresql
working_dir: /code

domainname: foo.com
hostname: foo
ipc: host
mac_address: 02:42:ac:11:65:43

mem_limit: 1000000000
memswap_limit: 2000000000
privileged: true

restart: always

read_only: true
stdin_open: true
tty: true