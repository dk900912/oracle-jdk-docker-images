本仓库主要分享当前主流的 Oracle JDK LTS 版本的 Docker 镜像，即 JDK 8、JDK 11 和 JDK 17。主要参考了 [oracle/docker-images](https://github.com/oracle/docker-images) 官方镜像仓库，唯一改变是在原生`Dockerfile`基础上追加了以下指令：
```shell
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone
```
### 1 镜像构建环境
- 腾讯云轻量应用服务器 CPU: 2核 内存: 4GB
- CentOS Linux release 8.2.2004 (Core)
- Docker version 20.10.5, build 55c4c88
### 2 镜像构建过程
##### JDK 8
下载`server-jre-8uXX-linux-x64.tar.gz`，[传送门](https://www.oracle.com/java/technologies/downloads/#java8)
```shell
docker build -t oracle/serverjre:8 .
```
##### JDK 11
下载`jdk-11.XX_linux-x64_bin.tar.gz`，[传送门](https://www.oracle.com/java/technologies/downloads/#java11)
```shell
docker build -t oracle/jdk:11 .
```
##### JDK 17
下载`jdk-17_linux-x64_bin.tar.gz`，[传送门](https://www.oracle.com/java/technologies/downloads/#java17)
```shell
docker build -t oracle/jdk:17 .
```

> 镜像信息一览
> ```shell
> [root@VM-16-10-centos ~]# docker images -a|grep oracle
> oracle/jdk         17        6fb4d610fab0   23 hours ago     564MB
> oracle/jdk         11        a2c8098a73ca   23 hours ago     447MB
> oracle/serverjre   8         289e30b87cf2   45 hours ago     316MB
> oraclelinux        8         5308c29a8a1f   7 days ago       229MB
> oraclelinux        7-slim    6a34bf539669   4 weeks ago      133MB
> ```

> 容器信息一览
> ```shell
> [root@VM-16-10-centos ~]# docker run -d --name oracle-server-jre oracle/serverjre:8
> [root@VM-16-10-centos ~]# docker run -d --name oracle/jdk oracle/jdk:11
> [root@VM-16-10-centos ~]# docker run -d --name oracle/jdk oracle/jdk:17
> 
> 
> [root@VM-16-10-centos ~]# docker ps -a
> CONTAINER ID   IMAGE                COMMAND                CREATED             STATUS             PORTS                    NAMES
> 42b5f6a68dfd   oracle/serverjre:8   "/bin/bash"            23 hours ago        Up 23 hours                                 oracle-jre-8
> 16b3563ae504   oracle/jdk:11        "jshell"               23 hours ago        Up 23 hours                                 oracle-jdk-11
> 12d2db0d7e29   oracle/jdk:17        "jshell"               23 hours ago        Up 23 hours                                 oracle-jdk-17
> 
> 
> [root@VM-16-10-centos ~]# docker exec -it oracle-jre-8 /bin/bash
> bash-4.2# java -version
> java version "1.8.0_341"
> Java(TM) SE Runtime Environment (build 1.8.0_341-b10)
> Java HotSpot(TM) 64-Bit Server VM (build 25.341-b10, mixed mode)
> 
>
> [root@VM-16-10-centos ~]# docker exec -it oracle-jdk-11 /bin/bash
> bash-4.2# java --version
> java 11.0.16.1 2022-08-18 LTS
> Java(TM) SE Runtime Environment 18.9 (build 11.0.16.1+1-LTS-1)
> Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.16.1+1-LTS-1, mixed mode, sharing)
> 
> 
> [root@VM-16-10-centos ~]# docker exec -it oracle-jdk-17 /bin/bash
> [root@12d2db0d7e29 /]# java --version
> java 17.0.4.1 2022-08-18 LTS
> Java(TM) SE Runtime Environment (build 17.0.4.1+1-LTS-2)
> Java HotSpot(TM) 64-Bit Server VM (build 17.0.4.1+1-LTS-2, mixed mode, sharing)
> ```
### 3 如何使用
进入[下载页面](https://github.com/dk900912/oracle-jdk-docker-images/releases) 选择某版本 JDK 镜像 tar 包，然后使用`docker load -i xxoo.tar`载入镜像即可。