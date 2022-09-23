本仓库主要分享当前主流的 Oracle JDK LTS 版本的 Docker 镜像，即 JDK 8、JDK 11 和 JDK 17。主要参考了 [oracle/docker-images](https://github.com/oracle/docker-images) 官方镜像仓库，唯一改变是在原生`Dockerfile`基础上追加了以下指令：
```shell
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone
```
### 1 镜像构建环境
- 腾讯云轻量应用服务器 CPU: 2核 内存: 4GB
- CentOS Linux release 8.2.2004 (Core)
- Docker version 20.10.5, build 55c4c88
### 2 镜像构建过程
- JRE 8

下载`server-jre-8u341-linux-x64.tar.gz`，[传送门](https://www.oracle.com/java/technologies/downloads/#java8)
```shell
docker build -t oracle/serverjre:8 .
```
- JDK 8

下载`jdk-8u341-linux-x64.tar.gz`，[传送门](https://www.oracle.com/java/technologies/downloads/#java8)
```shell
docker build -t oracle/jdk:8 .
```
- JDK 11

下载`jdk-11.XX_linux-x64_bin.tar.gz`，[传送门](https://www.oracle.com/java/technologies/downloads/#java11)
```shell
docker build -t oracle/jdk:11 .
```
- JDK 17

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
> [root@VM-16-10-centos ~]# docker run -it --name oracle-server-jre -d oracle/serverjre:8
> [root@VM-16-10-centos ~]# docker run -it --name oracle-jdk-8 -d oracle/jdk:8
> [root@VM-16-10-centos ~]# docker run -it --name oracle-jdk-11 -d oracle/jdk:11
> [root@VM-16-10-centos ~]# docker run -it --name oracle-jdk-17 -d oracle/jdk:17
> 
> 
> [root@VM-16-10-centos ~]# docker ps
> CONTAINER ID   IMAGE                COMMAND                CREATED         STATUS         PORTS                    NAMES
> 644ed20474a6   oracle/jdk:8         "/bin/bash"            7 minutes ago   Up 7 minutes                            oracle-jdk-8
> 42b5f6a68dfd   oracle/serverjre:8   "/bin/bash"            47 hours ago    Up 47 hours                             oracle-jre-8
> 16b3563ae504   oracle/jdk:11        "jshell"               47 hours ago    Up 47 hours                             oracle-jdk-11
> 12d2db0d7e29   oracle/jdk:17        "jshell"               47 hours ago    Up 47 hours                             oracle-jdk-17
> 
> 
> [root@VM-16-10-centos ~]# docker exec -it oracle-jre-8 /bin/bash
> bash-4.2# java -version
> java version "1.8.0_341"
> Java(TM) SE Runtime Environment (build 1.8.0_341-b10)
> Java HotSpot(TM) 64-Bit Server VM (build 25.341-b10, mixed mode)
> 
> 
> [root@VM-16-10-centos ~]# docker exec -it oracle-jdk-8 /bin/bash
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
- JRE 8

下载`oracle-jre-8.tar`，[传送门](https://pan.baidu.com/s/19IPPYUbHpawceFUTa2uXbw?pwd=r174)
```shell
docker load -i oracle-jre-8.tar
```
- JDK 8

下载`oracle-jdk-8.tar`，[传送门](https://pan.baidu.com/s/1gV8dm6QNNmV1LhGSDhI2Hw?pwd=98f0)
```shell
docker load -i oracle-jdk-8.tar
```
- JDK 11

下载`oracle-jdk-11.tar`，[传送门](https://pan.baidu.com/s/1NFuNnxWqJDRPNuvOzyFBiA?pwd=sy9k)
```shell
docker load -i oracle-jdk-11.tar
```
- JDK 17

下载`oracle-jdk-17.tar`，[传送门](https://pan.baidu.com/s/1VV_dV5_pcQ8BJ4ycd8iGlQ?pwd=idl1)
```shell
docker load -i oracle-jdk-17.tar
```

> 上述这些`tar`包是通过`docker save -o oracle-jdk-17.tar oracle/jdk:17`导出的！