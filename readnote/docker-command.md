

temo



# docker 离线安装 Image

在有网的电脑下载离线 Image，在无网电脑中加载 Image

```
sudo docker pull ubuntu

sudo docker save -o ubuntu_image.docker ubuntu

sudo docker load ubuntu_image.docker
```



## COPY

- COPY <源路径>... <目标路径>
- COPY ["<源路径1>",... "<目标路径>"]

和 RUN 指令一样，也有两种格式，一种类似于命令行，一种类似于函数调用。

`<源路径>`: 可以使用 *通配符*

```
COPY package.json /usr/src/app/

COPY hom* /mydir/
COPY hom?.txt /mydir/
```

`<目标路径>`: 可以是容器内的绝对路径，也可以是相对于工作目录的相对路径（工作目录可以用 WORKDIR 指令来指定）。目标路径不需要事先创建，如果目录不存在会在复制文件前先行创建缺失目录。


## ADD

>不建议使用

ADD 是 COPY 的扩展。

## CMD

- shell 格式：CMD <命令>
- exec 格式：CMD ["可执行文件", "参数1", "参数2"...]
- 参数列表格式：CMD ["参数1", "参数2"...]。在指定了 ENTRYPOINT 指令后，用 CMD 指定具体的参数。


推荐使用 `exec` 格式, 会被解析成 json 一定要使用使用 `"` 双眼号。

```
# shell
CMD echo $HOME

# exec
CMD [ "sh", "-c", "echo $HOME" ]

```


## ENTRYPOINT

## ENV 设置环境变量

## ARG 构建参数

## VOLUME 定义匿名卷

- VOLUME ["<路径1>", "<路径2>"...]
- VOLUME <路径>


容器运行时应该尽量保持容器存储层不发生写操作，对于数据库类需要保存动态数据的应用，其数据库文件应该保存于卷(volume)中

这里的 /data 目录就会在运行时自动挂载为匿名卷，任何向 /data 中写入的信息都不会记录进容器存储层，从而保证了容器存储层的无状态化。

## EXPOSE 声明端口

```
-p <宿主端口>:<容器端口>
```

## WORKDIR 指定工作目录

- WORKDIR <工作目录路径>

如该目录不存在，WORKDIR 会帮你建立目录

## HEALTHCHECK 健康检查

- HEALTHCHECK [选项] CMD <命令>：设置检查容器健康状况的命令
- HEALTHCHECK NONE：如果基础镜像有健康检查指令，使用这行可以屏蔽掉其健康检查指令


在没有 HEALTHCHECK 指令前，Docker 引擎只可以通过容器内主进程是否退出来判断容器是否状态异常。但是如果程序出现死锁状态就无法发现。



# Contiainer 相关

## 启动

```
# 从 Image 启动
docker run ubuntu:14.04 /bin/echo 'Hello world'

# 启动一个 stopped Container
docker constainer start
```

## 后台运行

`-d`


```
docker run -d ubuntu:17.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"

docker container ls

# 查看 Container 的输出内容
docker container logs [container ID or NAMES]
```

## 终止容器

```
docker container stop
```

## 重启容器

```
docker container restart
```

## 进入容器

### attach 不推荐使用

```
docker attach 243c
```

>注意： 如果从这个 stdin 中 exit，会导致容器的停止。

### exec 推荐使用

```
docker run -dit ubuntu

docker exec -it 69d1 bash
```

>从这个 stdin 中 exit，不会导致容器的停止

### 删除容器

```
docker container rm  trusting_newton
```

`-f`: 删除正在运行中的容器

### 清理所有处于终止状态的容器

```
docker container prune
```


# 仓库相关操作


## 拉取镜像

```
docker pull centos
```

## 推送镜像

```
docker tag ubuntu:17.10 username/ubuntu:17.10

docker push username/ubuntu:17.10
```

## 自动创建

自动创建（Automated Builds）功能对于需要经常升级镜像内程序来说，十分方便。


# 端口映射

```
-p 5000:5000
```

## 查看端口映射

```
docker port [Image Name | Image ID]
```

```
# 查看 某个容器 的端口映射状态
docker port nostalgic_morse

# 查看 5000 端口映射到哪里了，5000 是宿主主机的端口
docker port nostalgic_morse 5000

# 查看所有 容器 的端口映射状态
docker container ls -l

```


# docker-compose 相关命令

## up

启动 docker-compose.

它将尝试自动完成包括构建镜像，（重新）创建服务，启动服务，并关联服务相关容器的一系列操作。

```
docker-compose up
```

将会在后台启动并运行所有的容器。一般推荐生产环境下使用该选项。**通常在生产环境下使用**

```
docker-compose up -d
```

## build

```
docker-compose build [options] [SERVICE...]
```

## config

验证 Compose 文件格式是否正确，若正确则显示配置，若格式错误显示错误原因。

## down

停止所有通过 `up` 指令启动的 docker-compose

```
docker-compose down
```

## images

列出 docker-compose 包含的镜像

```
docker-compose images
```

## pause

暂停 docker-compose

```
docker-compose pause [SERVICE...]
```


# Compose 模板文件 格式

例子：

```
version: "3"

services:
  webapp:
    image: examples/web
    ports:
      - "80:80"
    volumes:
      - "/data"
```


## build

```
version: '3'
services:

  webapp:
    build: ./dir
```

指定 Dockerfile 所在文件夹的路径(可以是绝对路径，或者相对 docker-compose.yml 文件的路径）

Compose 将会利用它自动构建这个镜像，然后使用这个镜像。

## context

指定 Dockerfile 所在文件夹的路径

## dockerfile 

指定 Dockerfile 文件名

```
version: '3'
services:

  webapp:
    build:
      context: ./dir
      dockerfile: Dockerfile-alternate
      args:
        buildno: 1
```

## command

覆盖容器启动后默认执行的命令。

```
command: echo "hello world"
```

## container_name

指定容器名称。默认将会使用 项目名称_服务名称_序号 这样的格式。

```
container_name: docker-web-container
```

## depends_on

解决容器的依赖、启动先后的问题。

例子中会先启动 redis db 再启动 web

```
version: '3'

services:
  web:
    build: .
    depends_on:
      - db
      - redis

  redis:
    image: redis

  db:
    image: postgres
```

## dns

自定义 DNS 服务器

```
dns: 8.8.8.8

dns:
  - 8.8.8.8
  - 114.114.114.114
```

## healthcheck

通过命令检查容器是否健康运行。

```
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost"]
  interval: 1m30s
  timeout: 10s
  retries: 3
```

## image

指定为镜像名称或镜像 ID。

## labels

为容器添加 Docker 元数据（metadata）信息。例如可以为容器添加辅助说明信息。

```
labels:
  com.startupteam.description: "webapp for a startup team"
  com.startupteam.department: "devops department"
  com.startupteam.release: "rc3 for v1.0"
```

## logging

配置日志选项

```
logging:
  driver: syslog
  options:
    syslog-address: "tcp://192.168.0.42:123"  
```

支持三种日志驱动类型:

```
driver: "json-file"
driver: "syslog"
driver: "none"
```    

options 配置日志驱动的相关参数:

```
options:
  max-size: "200k"
  max-file: "10"
```

## ports

暴露端口信息, (HOST:CONTAINER)

```
ports:
 - "3000"
 - "8000:8000"
 - "49100:22"
 - "127.0.0.1:8001:8001"
```

## secrets

存储敏感数据，例如 mysql 服务密码。

```
version: "3"
services:

mysql:
  image: mysql
  environment:
    MYSQL_ROOT_PASSWORD_FILE: /run/secrets/db_root_password
  secrets:
    - db_root_password
    - my_other_secret

secrets:
  my_secret:
    file: ./my_secret.txt
  my_other_secret:
    external: true
```

## volumes

数据卷所挂载路径设置。

可以设置宿主机路径 （HOST:CONTAINER） 或加上访问模式 （HOST:CONTAINER:ro）

```
volumes:
 - /var/lib/mysql
 - cache/:/tmp/cache
 - ~/configs:/etc/configs/:ro
```










