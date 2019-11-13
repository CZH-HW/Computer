
[TOC]

# Docker 基础笔记（从入门到实践）

##  1. Docker 介绍

Docker 是一个开源的应用**容器引擎**，基于 Go 语言并遵从 Apache2.0 协议开源。并基于 Linux 内核的 cgroup，namespace，以及 AUFS 类的 Union FS 等技术，**对进程进行封装隔离，属于操作系统层面的虚拟化技术**。由于隔离的进程独立于宿主和其它的隔离的进程，因此也称其为容器。最初实现是基于 LXC，从 0.7 版本以后开始去除 LXC，转而使用自行开发的 libcontainer，从 1.11 开始，则进一步演进为使用 **runC** 和 **containerd**。

>runc 是一个 Linux 命令行工具，用于根据 OCI 容器运行时规范创建和运行容器。
>
>containerd 是一个守护程序，它管理容器生命周期，提供了在一个节点上执行容器和管理镜像的最小功能集。

Docker 在容器的基础上，进行了进一步的封装，从文件系统、网络互联到进程隔离等等，极大的简化了容器的创建和维护。使得 Docker 技术比虚拟机技术更为轻便、快捷。

Docker 可以让开发者打包**他们的应用以及依赖包**到一个**轻量级、可移植的容器**中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。


###  1.1 容器和虚拟机的比较

- 传统虚拟机如 VMware，VisualBox 之类的需要模拟整台机器包括硬件，每台虚拟机都需要有自己的操作系统，虚拟机一旦被开启，预分配给它的资源将全部被占用。**每一台虚拟机包括应用，必要的二进制和库，以及一个完整的用户操作系统**。虚拟机是为操作系统设计的

- 而容器技术是和我们的宿主机**共享硬件资源及操作系统，可以实现资源的动态分配**。容器包含应用和其所有的依赖包，但是**与其他容器共享内核**。容器在宿主机操作系统中，在用户空间以分离的进程运行。容器本质上是单个应用程序

>容器技术是实现**操作系统虚拟化**的一种途径，可以让您在资源受到隔离的进程中运行应用程序及其依赖关系。
>通过使用容器，我们可以轻松打包应用程序的代码、配置和依赖关系，将其变成容易使用的构建块，从而实现环境一致性、运营效率、开发人员生产力和版本控制等诸多目标。
>容器可以帮助保证应用程序快速、可靠、一致地部署，其间不受部署环境的影响。容器还赋予我们对资源更多的精细化控制能力，让我们的基础设施效率更高。

Docker相比于传统虚拟化方式具有更多的优势：

- docker 启动快速属于秒级别。虚拟机通常需要几分钟去启动

- docker 需要的资源更少， docker 在操作系统级别进行虚拟化， docker 容器和内核交互，几乎没有性能损耗，性能优于通过 Hypervisor 层与内核层的虚拟化
  
- docker 更轻量， docker 的架构可以共用一个内核与共享应用程序库，所占内存极小。同样的硬件环境， Docker 运行的镜像数远多于虚拟机数量，对系统的利用率非常高
  
- 与虚拟机相比， docker 隔离性更弱， docker 属于进程之间的隔离，虚拟机可实现系统级别隔离
  
- 安全性： docker 的安全性也更弱。 Docker 的租户 root 和宿主机 root 等同，一旦容器内的用户从普通用户权限提升为root权限，它就直接具备了宿主机的root权限，进而可进行无限制的操作。虚拟机租户 root 权限和宿主机的 root 虚拟机权限是分离的，并且虚拟机利用如 Intel 的 VT-d 和 VT-x 的 ring-1 硬件隔离技术，这种隔离技术可以防止虚拟机突破和彼此交互，而容器至今还没有任何形式的硬件隔离，这使得容器容易受到攻击
  
- 可管理性： docker 的集中化管理工具还不算成熟。各种虚拟化技术都有成熟的管理工具，例如 VMware vCenter 提供完备的虚拟机管理能力
  
- 高可用和可恢复性： docker 对业务的高可用支持是通过快速重新部署实现的。虚拟化具备负载均衡，高可用，容错，迁移和数据保护等经过生产实践检验的成熟保障机制， VMware 可承诺虚拟机 99.999% 高可用，保证业务连续性
  
- 快速创建、删除：虚拟化创建是分钟级别的， Docker 容器创建是秒级别的， Docker 的快速迭代性，决定了无论是开发、测试、部署都可以节约大量时间
  
- 交付、部署：虚拟机可以通过镜像实现环境交付的一致性，但镜像分发无法体系化。 Docker 在 Dockerfile 中记录了容器构建过程，可在集群中实现快速分发和快速部署

---
##  2. Docker 安装

在 Linux 中安装 Docker，一般需要两步：

1. 设置稳定仓库：安装所需要的、依赖的软件包——>设置稳定的远程仓库（GPG 密钥）
   
2. 安装最新版本或指定版本的 Docker Engine-Community（docker-ce 社区版） 和 containerd

3. 启动 Docker 服务： `sudo service docker start`或 `sudo systemctl start docker`

---

##  3. Docker 架构

Docker 包括三个基本概念:

- 镜像（Image）：相当于是一个 root 文件系统。镜像（Image）就是一堆只读层（read-only layer）的统一视角

- 容器（Container）：镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。实际上，容器 = 镜像 + 读写层

- 仓库（Repository）：仓库可看着一个代码控制中心，用来保存镜像，仓库是存放镜像的场所。

> Docker 把应用程序及其依赖，打包在 image 文件里面。只有通过这个文件，才能生成 Docker 容器。image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。同一个 image 文件，可以生成多个同时运行的容器实例。
> 
> image 是二进制文件。实际开发中，一个 image 文件往往通过继承另一个 image 文件，加上一些个性化设置而生成。举例来说，你可以在 Ubuntu 的 image 基础上，往里面加入 Apache 服务器，形成你的 image。
> 
> image 文件生成的容器实例，本身也是一个文件，称为容器文件。也就是说，一旦容器生成，就会同时存在两个文件： image 文件和容器文件。而且关闭容器并不会删除容器文件，只是容器停止运行而已。
  
Docker 使用客户端-服务器 (C/S) 架构模式，使用远程 API 来管理和创建 Docker 容器。

Docker Registry(服务器) 公有仓库是开放给用户使用、允许用户管理镜像的 Registry 服务。一般这类公开服务允许用户免费上传、下载公开的镜像，并可能提供收费服务供用户管理私有镜像。

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/docker_1.png)

运行docker push、docker pull、docker search时，实际上是通过 docker daemon 与 docker registry 通信

除了使用公开服务外，**用户还可以在本地搭建私有 Docker Registry**。Docker 官方提供了 Docker Registry 镜像，可以直接使用做为私有 Registry 服务。当用户创建了自己的镜像之后就可以使用 push 命令将它上传到公有或者私有仓库，这样下次在另外一台机器上使用这个镜像时候，只需要从仓库上 pull 下来就可以了。

---



##  4. Docker 基本命令

```shell
# 列出本机所有的镜像
docker image ls
docker images

# 将 image 文件从仓库抓取到本地，默认保存到 /var/lib/docker 目录下
docker image pull library/[imageName]
# library/[imageName] 是 image 文件在仓库里面的位置，其中 library 是 image 文件所在的组
# 由于 Docker 官方提供的 image 文件，都放在 library 组里面，所以它的是默认组，可以省略
docker pull [OPTIONS] NAME[:TAG|@DIGEST]


# 运行 image 文件，此时相应生成一个容器
docker run [imageName]
# docker run命令具有自动抓取 image 文件的功能。如果发现本地没有指定的 image 文件，就会从仓库自动抓取

docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
# [OPTIONS]常用
# -d：后台运行
# -p：指定端口映射，格式为 主机端口:容器端口
# -t：为容器重新分配一个伪输入终端，通常与 -i 同时使用
# --name=" "：为容器指定一个名称
# -v：绑定一个卷

# 运行一个在后台执行的容器，同时还能用控制台管理
docker run -i -t -d ubuntu:latest
# 运行一个带命令在后台不断执行的容器，不直接展示容器内部信息
docker run -d ubuntu:latest ping www.docker.com
# 使用镜像 nginx:latest 以交互模式启动一个容器,在容器内执行 /bin/bash 命令。
docker run -it nginx:latest /bin/bash
# 运行一个在后台不断执行的容器，同时带有命令，程序被终止后还能重启继续跑，还能用控制台管理
docker run -d --restart=always ubuntu:latest ping www.docker.com
# 为容器指定一个名字
docker run -d --name=ubuntu_server ubuntu:latest
# 容器暴露80端口，并指定宿主机80端口与其通信
docker run -d --name=ubuntu_server -p 80:80 ubuntu:latest
# 指定容器内目录与宿主机目录共享(宿主机文件夹:容器需共享的文件夹)
docker run -d --name=ubuntu_server -v /etc/www:/var/www ubuntu:latest



# 列出本机正在运行的容器（两种方式都可）
docker container ls
docker ps

# 列出本机所有容器，包括终止运行的容器（两种方式都可）
docker ps -a
docker container ls --all

# 重启、停止一个容器
docker start [containerName]/[containerId]   # 重启处于终止状态的容器
docker restart [containerName]/[containerId]   # 将一个正在运行的容器终止，然后再重新启动它
docker stop [containerName]/[containerId]
docker kill [containerName]/[containerId]


# 终止运行的容器文件，依然会占据硬盘空间，可以使用 docker rm 命令删除。
docker rm [containerName]/[containerId]
# 此时才能删除相应的镜像文件
docker rmi [imageName]
docker image rm [imageName]/[imageID]
# 清理掉所有处于终止状态的容器
docker container prune


# docker logs 命令用来查看 docker 容器的输出，即容器里面 Shell 的标准输出。
# 如果 docker run 命令运行容器的时候，没有使用 -it 参数，就要用这个命令查看输出。
docker logs [containerID]


# docker exec 命令用于进入一个正在运行的 docker 容器。
# 如果 docker run 命令运行容器的时候，没有使用 -it 参数，就要用这个命令进入容器。一旦进入了容器，就可以在容器的 Shell 执行命令了。
docker exec -it [containerID] /bin/bash

# docker cp 命令用于从正在运行的 Docker 容器里面，将文件拷贝到本机。下面是拷贝到当前目录的写法。
docker cp [containerID]:[/path/to/file] .
```
---

## 5 使用镜像

### 5.1 获取镜像

从 Docker 镜像仓库获取镜像的命令是 docker pull。其命令格式为：

```shell
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
```

- Docker 镜像仓库地址：地址的格式一般是`<域名/IP>[:端口号]`。默认地址是 Docker Hub。
- 仓库名：如之前所说，这里的仓库名是两段式名称，即`<用户名>/<软件名>`。对于 Docker Hub，如果不给出用户名，则默认为 library，也就是官方镜像。


比如：
```shell
$ docker pull ubuntu:18.04
18.04: Pulling from library/ubuntu
bf5d46315322: Pull complete
9f13e0ac480c: Pull complete
e8988b5b3097: Pull complete
40af181810e7: Pull complete
e6f7c7e5c03e: Pull complete
Digest: sha256:147913621d9cdea08853f6ba9116c2e27a3ceffecf3b492983ae97c3d643fbbe
Status: Downloaded newer image for ubuntu:18.04
```

从下载过程中可以分层存储的概念，镜像是由多层存储所构成。下载也是一层层的去下载，并非单一文件。下载过程中给出了每一层的 ID 的前 12 位。并且下载结束后，给出该镜像完整的 sha256 的摘要，以确保下载一致性。

在使用上面命令的时候，你可能会发现，你所看到的层 ID 以及 sha256 的摘要和这里的不一样。这是因为官方镜像是一直在维护的，有任何新的 bug，或者版本更新，都会进行修复再以原来的标签发布，这样可以确保任何使用这个标签的用户可以获得更安全、更稳定的镜像。


### 5.2 镜像体积

docker image ls 列表中的镜像体积总和并非是所有镜像实际硬盘消耗。由于 Docker 镜像是多层存储结构，并且可以继承、复用，因此不同镜像可能会因为使用相同的基础镜像，从而拥有共同的层。由于 Docker 使用 Union FS，相同的层只需要保存一份即可，因此实际镜像硬盘占用空间很可能要比这个列表镜像大小的总和要小的多。

你可以通过以下命令来便捷的查看镜像、容器、数据卷所占用的空间。

```shell
$ docker system df

TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
Images              24                  0                   1.992GB             1.992GB (100%)
Containers          1                   0                   62.82MB             62.82MB (100%)
Local Volumes       9                   0                   652.2MB             652.2MB (100%)
Build Cache        
```

### 5.3 列出部分镜像
不加任何参数的情况下，`docker image ls`会列出所有顶层镜像，但是有时候我们只希望列出部分镜像。`docker image ls`有好几个参数可以帮助做到这个事情。

根据仓库名列出镜像
```shell
$ docker image ls ubuntu
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              18.04               f753707788c5        4 weeks ago         127 MB
ubuntu              latest              f753707788c5        4 weeks ago         127 MB
```

列出特定的某个镜像，也就是说指定仓库名和标签
```shell
$ docker image ls ubuntu:18.04
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              18.04               f753707788c5        4 weeks ago         127 MB
```

除此以外，`docker image ls`还支持强大的过滤器参数`--filter`，或者简写`-f`。比如，我们希望看到在 mongo:3.2 之后建立的镜像，可以用下面的命令：
```shell
$ docker image ls -f since=mongo:3.2
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
redis               latest              5f515359c7f8        5 days ago          183 MB
nginx               latest              05a60462f8ba        5 days ago          181 MB
```
想查看某个位置之前的镜像也可以，只需要把 since 换成 before 即可。

此外，如果镜像构建时，定义了 LABEL，还可以通过 LABEL 来过滤。
```
$ docker image ls -f label=com.example.version=0.1
...
```


### 5.4 删除本地镜像

用`docker image ls`命令来配合
像其它可以承接多个实体的命令一样，可以使用`docker image ls -q`来配合使用`docker image rm`，这样可以成批的删除希望删除的镜像。很多过滤镜像列表的方式都可以拿过来使用。

比如，我们需要删除所有仓库名为 redis 的镜像：
```
$ docker image rm $(docker image ls -q redis)
```

或者删除所有在 mongo:3.2 之前的镜像：
```
$ docker image rm $(docker image ls -q -f before=mongo:3.2)
```
充分利用你的想象力和 Linux 命令行的强大，你可以完成很多非常赞的功能。

---

## 6 操作 Docker 容器

### 6.1 启动容器

启动容器有两种方式，一种是基于镜像新建一个容器并启动，另外一个是将在终止状态（stopped）的容器重新启动。

因为 Docker 的容器实在太轻量级了，很多时候用户都是随时删除和新创建容器。


1.新建并启动
所需要的命令主要为`docker run`

例如，下面的命令输出一个 “Hello World”，之后终止容器。
```shell
$ docker run ubuntu:18.04 /bin/echo 'Hello world'
Hello world
```
这跟在本地直接执行 /bin/echo 'hello world' 几乎感觉不出任何区别。

下面的命令则启动一个 bash 终端，允许用户进行交互。
```
$ docker run -t -i ubuntu:18.04 /bin/bash
root@af8bae53bdd3:/#
```
其中，`-t`选项让 Docker 分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上，`-i`则让容器的标准输入保持打开。

在交互模式下，用户可以通过所创建的终端来输入命令，例如
```
root@af8bae53bdd3:/# pwd
/
root@af8bae53bdd3:/# ls
bin boot dev etc home lib lib64 media mnt opt proc root run sbin srv sys tmp usr var
```

当利用 docker run 来创建容器时，Docker 在后台运行的标准操作包括：

- 检查本地是否存在指定的镜像，不存在就从公有仓库下载
- 利用镜像创建并启动一个容器
- 分配一个文件系统，并在只读的镜像层外面挂载一层可读写层
- 从宿主主机配置的网桥接口中桥接一个虚拟接口到容器中去
- 从地址池配置一个 ip 地址给容器
- 执行用户指定的应用程序
- 执行完毕后容器被终止


2.启动已终止容器
可以利用 docker container start 命令，直接将一个已经终止的容器启动运行。

容器的核心为所执行的应用程序，所需要的资源都是应用程序运行所必需的。除此之外，并没有其它的资源。可以在伪终端中利用 ps 或 top 来查看进程信息。
```
root@ba267838cc1b:/# ps
  PID TTY          TIME CMD
    1 ?        00:00:00 bash
   11 ?        00:00:00 ps
```
可见，容器中仅运行了指定的 bash 应用。这种特点使得 Docker 对资源的利用率极高，是货真价实的轻量级虚拟化。


### 6.2 进入容器

在使用`-d`参数时，容器启动后会进入后台。

某些时候需要进入容器进行操作，包括使用`docker attach`命令或`docker exec`命令，推荐大家使用`docker exec`命令，原因会在下面说明。

1.`docker attach`命令。

```shell
$ docker run -dit ubuntu
243c32535da7d142fb0e6df616a3c3ada0b8ab417937c853a9e1c251f499f550

$ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
243c32535da7        ubuntu:latest       "/bin/bash"         18 seconds ago      Up 17 seconds                           nostalgic_hypatia

$ docker attach 243c
root@243c32535da7:/#
```

2.`docker exec`命令

docker exec 后边可以跟多个参数，这里主要说明`-i`，`-t`参数。

只用`-i`参数时，由于没有分配伪终端，界面没有我们熟悉的 Linux 命令提示符，但命令执行结果仍然可以返回。

当`-i -t`参数一起使用时，则可以看到我们熟悉的 Linux 命令提示符。

```shell
$ docker run -dit ubuntu
69d137adef7a8a689cbcb059e94da5489d3cddd240ff675c640c8d96e84fe1f6

$ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
69d137adef7a        ubuntu:latest       "/bin/bash"         18 seconds ago      Up 17 seconds                           zealous_swirles

$ docker exec -i 69d1 bash
ls
bin
boot
dev
...

$ docker exec -it 69d1 bash
root@69d137adef7a:/#
```
如果从这个 stdin 中 exit，不会导致容器的停止。这就是为什么推荐大家使用 docker exec 的原因。


### 6.3 导入和导出容器

#### 6.3.1 导出容器

如果要导出本地某个容器，可以使用`docker export`命令。

```shell
$ docker container ls -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                    PORTS               NAMES
7691a814370e        ubuntu:18.04        "/bin/bash"         36 hours ago        Exited (0) 21 hours ago                       test
$ docker export 7691a814370e > ubuntu.tar
```

这样将导出容器快照到本地文件。

#### 6.3.2 导入容器快照
可以使用`docker import`从容器快照文件中再导入为镜像，例如

```shell
$ cat ubuntu.tar | docker import - test/ubuntu:v1.0
# 或
$ docker import ./ubuntu.tar test/ubuntu:v1.0
$ docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED              VIRTUAL SIZE
test/ubuntu         v1.0                9d37a6082e97        About a minute ago   171.3 MB
```

此外，也可以通过指定 URL 或者某个目录来导入，例如

```shell
$ docker import http://example.com/exampleimage.tgz example/imagerepo
```

注：用户既可以使用`docker load`导入镜像存储文件到本地镜像库，也可以使用`docker import`来导入一个容器快照到本地镜像库。这两者的区别在于**容器快照文件将丢弃所有的历史记录和元数据信息（即仅保存容器当时的快照状态），而镜像存储文件将保存完整记录，体积也要大。此外，从容器快照文件导入时可以重新指定标签等元数据信息**。

---

## 7 访问仓库

### 7.1 推送镜像

用户也可以在登录后通过`docker push`命令来将自己的镜像推送到 Docker Hub。

以下命令中的`username`请替换为你的 Docker 账号用户名。

```shell
$ docker tag ubuntu:18.04 username/ubuntu:18.04
$ docker image ls

REPOSITORY                                               TAG                    IMAGE ID            CREATED             SIZE
ubuntu                                                   18.04                  275d79972a86        6 days ago          94.6MB
username/ubuntu                                          18.04                  275d79972a86        6 days ago          94.6MB


$ docker push username/ubuntu:18.04
$ docker search username

NAME                      DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
username/ubuntu
























---

##  10. 实例：制作自己的 Docker 容器
制作自己的 image 文件，需要用到 Dockerfile 文件，Dockerfile 文件是一个文本配置文件，用来配置 image，Docker 根据该文件生成二进制的 image 文件。

以 kao-demos 开源项目为例，下载源码
```shell
git clone https://github.com/ruanyf/koa-demos.git
cd koa-demos     # 进入项目根目录
```

###  10.1 编写 Dockerfile 文件

首先，在项目的根目录下，新建一个文本文件.dockerignore，写入下面的内容。

```shell
node_modules
npm-debug.log
```

上面代码表示，这三个路径要排除，不要打包进入 image 文件。如果你没有路径要排除，这个文件可以不新建。

然后，在项目的根目录下，新建一个文本文件 Dockerfile，写入下面的内容。
Dockerfile 是由一行行命令语句组成，并且支持已 # 开头的注释行。

```shell
FROM node:8.4
COPY . /app
WORKDIR /app
RUN npm install --registry=https://registry.npm.taobao.org
EXPOSE 3000
CMD node demos/01.js
```

一般来说，我们可以将 Dockerfile 分为四个部分：

1. 基础镜像(父镜像)信息指令 FROM
2. 维护者信息指令 MAINTAINER
3. 镜像操作指令 RUN 、 EVN 、 ADD 和 WORKDIR 等
4. 容器启动指令 CMD 、 ENTRYPOINT 和 USER 等

- `FROM node:8.4`：该 image 文件继承官方的 node image，冒号表示标签，这里标签是8.4，即8.4版本的 node。
- `COPY . /app`：将当前目录下的所有文件（除了.dockerignore排除的路径），都拷贝进入 image 文件的/app目录。
- `WORKDIR /app`：指定接下来的工作路径为/app。
- `RUN npm install`：在/app目录下，运行npm install命令安装依赖。注意，安装后所有的依赖，都将打包进入 image 文件。
- `EXPOSE 3000`：将容器 3000 端口暴露出来，允许外部连接这个端口。
- `CMD node demos/01.js`：容器启动后自动执行node demos/01.js

RUN 命令与 CMD 命令的区别在哪里？

简单说，RUN 命令在 image 文件的构建阶段执行，执行结果都会打包进入 image 文件；CMD 命令则是在容器启动后执行。
另外，一个 Dockerfile 可以包含多个RUN命令，但是只能有一个CMD命令。

注意，指定了 CMD 命令以后，docker run 命令就不能附加命令了（比如前面的/bin/bash），否则它会覆盖 CMD 命令。


###  10.2 创建 image 文件

有了 Dockerfile 文件以后，就可以使用`docker image build`命令创建 image 文件了。

```shell
docker build -t koa-demo .
# 或者
docker build -t koa-demo:0.0.1 .
```

上面代码中，`-t`参数用来指定 image 文件的名字，后面还可以用冒号指定标签。如果不指定，默认的标签就是 latest。
最后的那个点表示 Dockerfile 文件所在的路径，上例是当前路径，所以是一个点。
如果运行成功，就可以看到新生成的 image 文件 koa-demo 了。

###  10.3 生成容器

```
docker run -p 8000:3000 -it koa-demo /bin/bash
# 或者
docker run -p 8000:3000 -it koa-demo:0.0.1 /bin/bash
```

参数含义：
+ -it：容器的 Shell 映射到当前的 Shell，然后你在本机窗口输入的命令，就会传入容器。
  
+ /bin/bash：容器启动以后，内部第一个执行的命令。这里是启动 Bash，保证用户可以使用 Shell


--- 












