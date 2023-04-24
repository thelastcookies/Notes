# 🐿 Go

![go--logo.png](../static/images/go/go__logo.png)

# Why Go? 

## 云 & 网络服务

![go--cloud-white.svg](../static/images/go/go__cloud_white.svg)

拥有强大的工具和API生态系统，使用Go可以在云服提供商上非常容易地构建服务。

## 命令行接口（CLI）

![go--clis-white.svg](../static/images/go/go__clis_white.svg)

具有流行的开源包和强大的标准库，使用Go可以创建快速和优雅的CLI。

## Web开发

![go--webdev-white.svg](../static/images/go/go__webdev_white.svg)

具有增强的内存性能，支持多种IDE，使用Go可以支持快速和可扩展的Web应用程序。

## 开发运维 & 网站可靠性工程

![go--devops-white.svg](../static/images/go/go__devops_white.svg)

具有快速的构建时间、精简的语法、自动格式化程序和文档生成器。Go原生支持DevOps和SRE。

## 语言特性
### 优势
- 极其简单的部署方式，不依赖其他库，可以编译为机器码
- 静态类型语言
- 对并发的天生支持，对多核的充分利用
- 强大的标准库支持
  - runtime系统调度机制
  - 高效的GC垃圾回收
  - 丰富的标准库
- 简单易学
  - 仅25个关键字
  - 语法简洁，C语言支持
  - 跨平台

### 劣势
- 集中于 Github 的包管理。
- 所有异常都用 Error 来处理，比较有争议。
- 并非无缝兼容 C预约。

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

## 常用的命令行指令
### 代码运行
```shell
$ go run [go_file.go]
```
`run` 表示编译并执行代码。
### 编译
```shell
$ go build [go_file.go]
```
`build` 表示编译代码为可执行文件。

## 基本概念
### 包
Go 语言是使用包来组织源代码的，包（package）是多个 Go 源码的集合，
是一种高级的代码复用方案。Go语言中为我们提供了很多内置包，如 `fmt`、`os`、`io` 等。

任何源代码文件必须属于某个包，同时源码文件的第一行有效代码必须是package packageName 语句，通过该语句声明自己所在的包。
## 基本语法
### 数据类型

Go语言将数据类型分为四类：
- Basic Types（基础类型）
- Aggregate Types (复合类型）
- Reference Types (引用类型）
- Interface Types (接口类型）

#### 基础类型：
数字、字符串和布尔型。布尔型的值只可以是常量 true 或者 false。数字类型支持整型和浮点型，并且支持复数，其中位的运算采用补码。

#### 复合数据类型：
数组、结构体是通过组合简单类型，来表达更加复杂的数据结构。

#### 引用类型：
指针、slice、map、 channel、接口和函数类型。当声明引用类型的变量时，创建的变量被称作标头（header）值。
每个引用类型创建的标头值是包含一个指向底层数据结构的指针。

### 变量声明与初始化

使用 `var` 关键字声明变量。

#### 标准声明 & 初始化格式：
```go
var foo [type_foo] = bar
```
#### 仅声明：
```go
var foo [type_foo]
```
仅声明时，声明的变量拥有默认值。其中整型和浮点型变量的默认值为`0`，字符串变量的默认值为`''`，布尔型变量默认为 `false`，切片、函数、指针变量的默认为 `nil`。

#### 类型推导：
```go
var foo = bar
```
不显式指定变量类型时，变量类型会根据等号右边的值来推导。

#### 短变量声明
```go
foo := bar
```
`:=` 声明仅可用于函数体内部。

#### 批量声明
```go
// 一般用于声明全局变量
var (
    foo int
    bar string
    ...
)
// 为多个变量指定相同类型
var foo, bar, baz int = 1, 2, 3
// 有初始值的批量声明
var foo, bar = 100, "str"

// 函数体内
baz, qux := true, 5
```

#### 匿名变量
使用多重赋值时，如果想要忽略某个值，可以使用匿名变量（anonymous variable）。

匿名变量用一个下划线_表示
```go
// foo() 的第二个返回值会被忽略
x, _ := foo()
```
匿名变量不占用命名空间，不会分配内存，所以匿名变量之间不存在重复声明。

### 常量
使用 `const` 关键字声明变量。

常量只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型。

```go
var foo [type_foo] = bar
```
多种声明方式：
```go
const str string = "abc"
const pi = 3.1415

// 同时声明多个常量
const (
    pi = 3.1415
    e = 2.7182
)
// 声明多个相同值常量
const (
    n1 = 100
    n2
    n3
)
```

### iota
`iota`是 go 语言的常量计数器，自增长，只能在常量的表达式中使用。

`iota` 在 `const` 关键字出现时将被重置为`0`。`const` 中每新增一行常量声明将使 `iota` 计数加一(`iota`可理解为 `const` 语句块中的行索引)。 

使用`iota`能简化定义，在定义枚举时很有用。
 

```go
const (
	n1 = iota // iota == 0
	n2        // iota == 1
	n3        // iota == 2
	n4        // iota == 3
)

// 使用 _ 跳过某些值
const (
	n1 = iota // iota == 0 
	n2        // iota == 1 
	_ 
	n4        // iota == 3
)

// 声明中插值
const (
    n1 = iota // iota == 0
    n2 = 100  // iota == 1
    n3 = iota // iota == 2
    n4        // iota == 3
)
const n5 = iota // iota == 0
```

## 高阶

## 模块管理





