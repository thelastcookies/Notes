# 🐿 Go

![go--logo.png](../static/images/go--logo.png)

# Why Go? 

## 云 & 网络服务

![go--cloud-white.svg](../static/images/go--cloud-white.svg)

拥有强大的工具和API生态系统，使用Go可以在云服提供商上非常容易地构建服务。

## 命令行接口（CLI）

![go--clis-white.svg](../static/images/go--clis-white.svg)

具有流行的开源包和强大的标准库，使用Go可以创建快速和优雅的CLI。

## Web开发

![go--webdev-white.svg](../static/images/go--webdev-white.svg)

具有增强的内存性能，支持多种IDE，使用Go可以支持快速和可扩展的Web应用程序。

## 开发运维 & 网站可靠性工程

![go--devops-white.svg](../static/images/go--devops-white.svg)

具有快速的构建时间、精简的语法、自动格式化程序和文档生成器。Go原生支持DevOps和SRE。

## Golang 生态

### Web 框架

- [Gin](https://github.com/gin-gonic/gin) 是一个用 Go 编写的 Web 框架。 由于 httprouter，性能最高可提高 40 倍。
- [Beego](https://github.com/beego/beego) 用于在 Go 中快速开发企业应用程序，包括 RESTful API、Web 应用程序和后端服务。
- [echo](https://github.com/labstack/echo)
- [Iris](https://github.com/kataras/iris) 是一个快速、简单但功能齐全且非常高效的 Go 网络框架。

### 微服务框架

- [Go kit](http://gokit.io) 是一个微服务工具包。
- [Istio](https://github.com/istio/istio) 是一个开源服务网格，其透明地分层到现有的分布式应用程序上，提供了一种统一且更有效的方式来保护、连接和监控服务。Ist是实现负载均衡、服务到服务身份验证、监控的捷径——而几乎不需要更改服务代码。

### 容器编排

- [Kubernetes](https://github.com/kubernetes/kubernetes)，也称为 K8s，是一个开源系统，用于管理跨多个主机的容器化应用程序。
- [Swarm](https://github.com/docker-archive/classicswarm) 是一个为Docker设计的本地集群系统。它通过使用API代理系统，将一组Docker主机转换为单个虚拟主机。

### 服务发现

- [Consul](https://github.com/hashicorp/consul) 是一种分布式、高可用性和数据中心感知解决方案，用于跨动态、分布式基础设施连接和配置应用程序。
- [etcd](https://github.com/etcd-io/etcd) 是分布式系统最关键数据的分布式可靠键值存储。

### 存储殷勤

- [TiDB](https://github.com/pingcap/tidb) (/’taɪdiːbi:/, “Ti”代表 Titanium）是一个支持混合事务和分析处理 (HTAP) 工作负载的开源分布式 SQL 数据库。 它兼容MySQL，具有水平扩展、强一致性和高可用性的特点。

### 静态建站

- [Hugo](https://github.com/gohugoio/hugo) 是一个用 Go 编写的静态 HTML 和 CSS 网站生成器。

### 中间件

- [NSQ](https://github.com/nsqio/nsq) 是一个实时分布式消息传递平台，旨在大规模运行，可以每天处理数十亿条消息。
- [Zinx](https://github.com/aceld/zinx) 是一个基于 Golang 的轻量级并发服务器框架。
- [Leaf](https://github.com/name5566/leaf) 是 Go (golang) 中的实用游戏服务器框架。
- [gRPC-Go](https://github.com/grpc/grpc-go) 是一个高性能、开源、通用的 RPC 框架。
- [Codis](https://github.com/CodisLabs/codis) 是一个用 Go 编写的基于代理的高性能 Redis 集群解决方案。

### 爬虫框架

- [Colly](link) 是一个为 Gophers 准备的闪电般快速且优雅的爬虫框架。


## 环境配置 (Debian环境)
### 安装
```shell
$ apt install golang
 ```
验证安装结果
```shell
$ go version
```

### 配置环境变量

Debian 下 `GOPATH` 的默认值为 `/root/go`。

```shell
# 创建 go workspace
$ cd $HOME
$ mkdir go
# 创建 bin/src/pkg 文件
$ cd $HOME/go
$ mkdir bin
$ mkdir src
$ mkdir pkg
```

配置全局环境变量

```shell
$ export PATH=$PATH:$HOME/go/bin
```
配置一些常用的环境变量
```shell
$ go env | grep GOROOT
GOROOT="/usr/lib/go-1.15"
$ export GOPATH=$HOME/go
$ export GOROOT=/usr/lib/go-1.15
$ export GOBIN=$GOROOT/bin
```

> ### `GO env` 部分属性说明 
> 
> `GOROOT` Golang 的安装路径
> 
> `GOPATH` 存放sdk以外的第三方类库，`GOPATH` 下默认约定有三个文件夹。
> 
> - src 存放源代码(比如：.go .c .h .s 等)。默认约定，`go run`，`go install`等命令的工作路径。Go 1.14版本之后，都推荐使用`go mod`模式来管理依赖了，也不再强制把源代码必须写在src目录下。
> - pkg 包含编译后的包代码，即 .a 文件。
> - bin 编译后生成的可执行文件。

### 配置代理
默认`GOPROXY`配置是：

```shell
$ go env | grep GOPROXY
GOPROXY="https://proxy.golang.org,direct"
```

可以修改为：https://goproxy.io 或 https://goproxy.cn

```shell
$ go env -w GOPROXY=https://goproxy.io,direct
```

## 语言特性

## 基本语法

## 高阶

## 模块管理





