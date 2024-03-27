# 🐳 Docker

![docker-logo.jpg](images/docker__banner.png)

## 什么是 Docker？

Docker 是一种用于开发（build）、运行（run）和交付（deliver）应用程序的开源平台，它通过容器化技术来实现轻量级的应用程序部署。

## Why Docker？

### 环境配置的难题

软件开发会遇到最大的麻烦之一，就是环境配置。
用户计算机的环境各异，对于操作系统的设置，各种库和组件的安装和版本情况也不相同，开发者常常会遇到"它在我的机器可以运行"（It
works
on my machine）的情况。

### 虚拟机

虚拟机（Virtual Machine）就是软件连同环境安装的一种解决方案。
它可以在一种操作系统里面运行另一种操作系统，比如在 Windows 系统里面运行 Linux 系统，应用程序对此毫无感知。
虚拟机和宿主机互不干涉互不影响。

但是，这个方案有几个缺点：

- 资源占用多。虚拟机会独占一部分内存和硬盘空间。在它运行的时候，其他程序就无法使用这些资源。哪怕虚拟机里面的应用程序，真正使用的内存只有
  1MB，虚拟机依然需要几百 MB 的内存才能运行。
- 冗余步骤多。虚拟机是完整的操作系统，一些系统级别的操作步骤，往往无法跳过，比如用户登录。
- 启动慢。启动操作系统需要多久，启动虚拟机就需要多久。可能要等几分钟，应用程序才能真正运行。

### Linux 容器

由于虚拟机存在这些缺点，Linux 发展出了另一种虚拟化技术：Linux 容器（Linux Containers，缩写为 LXC）。

Linux 容器不是模拟一个完整的操作系统，而是对进程进行隔离，对于容器里面的进程来说，其接触到都是虚拟资源，从而了来实现与宿主机的隔离。

由于容器是进程级别的，相比虚拟机有很多优势。

- 启动快。容器里面的应用，只是作为宿主机的一个进程。所以，启动容器相当于启动本机的一个进程，启动速度有很大提升。
- 资源占用少。容器只占用需要的资源，多个容器可以共享资源。
- 体积小。容器只要包含用到的组件即可，所以相较于虚拟机文件要小很多。

总之，容器类似轻量级的虚拟机，能够提供虚拟化的环境，但是成本开销大大降低。

### Docker

Docker 是目前最流行的 Linux 容器解决方案，属于 Linux 容器的一种封装，提供简单易用的容器使用接口。

总体来说，Docker 有以下特点：

- **轻量级和快速启动**：Docker 容器具有 Linux 容器的优点，相比虚拟机更加轻量级，容器启动和停止的速度非常快。这使得开发人员可以快速迭代和部署应用程序，提高了开发和部署的效率。

- **环境一致性**：Docker 可以将应用程序和其依赖的运行时环境打包成一个独立的容器，确保在不同的环境中具有相同的运行结果，解决了“它在我的机器上可以运行”的问题。

- **隔离性和安全性**：Docker 使用 Linux 内核的命名空间和控制组技术来实现容器之间的隔离，每个容器都拥有自己的文件系统、网络和进程空间，保证了容器之间的互相隔离，提高了安全性。

- **可移植性**：Docker 容器可以在任何支持 Docker 的平台上运行，包括开发人员的笔记本电脑、测试环境、生产环境等，提供了很高的可移植性。

- **资源利用率高**：Docker 容器共享操作系统的内核，减少了资源的浪费，提高了资源的利用率。多个容器可以运行在同一台物理机上，硬件资源的搞笑利用。

- **便于扩展和自动化**：Docker 提供了一系列工具和 API，可以轻松地进行扩展和自动化，比如 Docker Compose、Docker
  Swarm、Kubernetes 等，帮助开发团队构建和管理大规模的容器化应用程序。

## 基本概念

### 镜像（Image）

Docker Image 是 Docker 中的一个关键概念，用于打包应用程序、运行时环境和其他依赖项的文件系统。

Image 是容器（Container）的基础，包含了用于创建容器的所有文件和配置信息，可以被视为容器的模板或蓝图。同一个 Image
文件，可以生成多个同时运行的容器实例。

实际开发中，一个 Image 文件往往通过继承另一个 Image 文件，加上一些定制化设置而生成。

### 容器

Docker Container 是 Docker 应用程序的运行实例，由 Docker Image 生成。

容器化技术通过将应用程序及其所有依赖项打包成一个标准化的容器，从而实现了软件在不同环境中的快速部署、可移植性和隔离性。

每个容器都有自己的文件系统、进程空间、网络空间和用户空间，使得容器之间相互独立，互不干扰。

### 数据卷

Docker Volumes 是用于在 Docker 容器和宿主机之间共享数据的一种机制。

由于容器创建后产生的数据是易失性的，在容器停止后会全部丢失，因此引入 Volumes 来将容器的数据持久化。Volumes
允许将宿主机上的目录或文件挂载到容器中，使得容器中的数据能够持久化。

允许多个容器共享同一个数据卷，并且数据卷中的数据能够在重新创建或者删除容器后仍保持不变。
数据卷可以在容器创建时动态创建和挂载，也可以事先创建并在容器运行时挂载到容器中。

Docker 支持不同的数据卷驱动程序，用于将数据卷连接到不同的存储后端，如本地文件系统、网络存储、云存储等。

### Dockerfile

Dockerfile 是用于定义 Docker Image 构建过程的文本文件。

可以通过 Dockerfile，指定应用程序的环境、依赖关系和操作步骤，从而创建定制化的 Docker 镜像。

Dockerfile 的主要内容有：

#### 基础镜像（Base Image）

Dockerfile 通常以指定基础镜像开始构建过程。基础镜像是构建过程的起点，它包含了操作系统和一些基本的软件包。您可以选择从现有的公开镜像开始构建，或者创建自己的基础镜像。

#### 指令（Instructions）

Dockerfile
包含一系列指令，每个指令都会执行一个特定的操作。常见的指令包括FROM（指定基础镜像）、RUN（在镜像中执行命令）、COPY（复制文件到镜像中）、ADD（复制并解压文件到镜像中）、CMD（设置容器启动时要执行的命令）等。

#### 工作目录（Working Directory）

使用WORKDIR指令可以设置容器内的工作目录。在工作目录中执行的后续指令将在该目录下执行。

#### 复制文件（Copying Files）

使用COPY和ADD指令可以将本地文件复制到镜像中。这对于将应用程序代码、配置文件等添加到镜像中非常有用。

#### 构建环境（Build Environment）

通过ARG指令可以定义构建时的环境变量。这些变量可以在构建过程中传递给RUN指令，并在构建时灵活地进行配置。

#### 暴露端口（Expose Ports）

使用EXPOSE指令可以指定容器运行时要暴露的端口。这样可以使得容器中运行的应用程序能够接收外部的网络请求。

#### 入口点（Entry Point）

使用ENTRYPOINT指令可以设置容器启动时要执行的默认命令。与CMD指令不同，ENTRYPOINT指令设置的命令不会被docker
run命令中传递的参数覆盖。

通过编写一个Dockerfile，您可以将应用程序的构建过程描述为一系列清晰的步骤，从而实现应用程序在不同环境中的一致部署和运行。完成Dockerfile后，可以使用docker
build命令将其构建为Docker镜像，然后通过docker run命令启动容器并运行应用程序。

### 仓库

## 安装配置

下面提供一些官方的安装文档，并详细说明在 `Linux（Debian）` 环境下的安装过程。

- [Install On Mac](https://docs.docker.com/desktop/install/mac-install/)
- [Install On Linux](https://docs.docker.com/engine/install/)
- [Install On Windows](https://docs.docker.com/desktop/install/windows-install/)

### Linux（Debian）下的安装

#### 操作系统要求

安装 Docker 引擎，需要 64 位的 Debian 环境：

- Debian Bookworm 12（stable）
- Debian Bullseye 11（old-stable）

#### 卸载旧版本

在安装 Docker 引擎之前，需要卸载任何冲突的软件包。

需要卸载的非官方软件包包括：

- docker.io
- docker-compose
- docker-doc
- podman-docker

运行以下命令以卸载所有冲突的软件包：

```shell
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

#### 安装方法

根据需求，可以以不同的方式安装 Docker 引擎：

- 安装 Docker Desktop for Linux；
- 使用 apt 库进行安装；
- 手动安装并手动管理升级；
- 使用便捷脚本，仅建议用于测试和开发环境。

#### 使用 apt 进行安装

在新主机上首次安装 Docker 引擎之前，需要设置 Docker 的 apt 存储库。之后就可以使用 apt 安装和更新 Docker。

##### 1. 设置 Docker 的 apt 库

```shell
# 添加 Docker 官方 GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# 为 apt 添加仓库
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

##### 2. 安装 Docker

```shell
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

##### 3. 用 `hello-world` 镜像来验证安装结果

```shell
$ sudo docker run hello-world
```

这条指令会下载一个测试镜像并在容器中运行它。当容器运行时，它会打印一个确认消息然后退出。

更多帮助请参考 [Docker](https://www.docker.com) 官方文档。

## 常用命令

## 构建镜像

## 运行容器

## Docker Compose

## Kubernetes

https://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html

## 国内镜像源（DaoCloud）

可以将原仓库的 url 替换为 DaoCloud 的 url。

```text
docker.io => docker.m.daocloud.io
gcr.io => gcr.m.daocloud.io
ghcr.io => ghcr.m.daocloud.io
k8s.gcr.io => k8s-gcr.m.daocloud.io
registry.k8s.io => k8s.m.daocloud.io
quay.io => quay.m.daocloud.io
```

