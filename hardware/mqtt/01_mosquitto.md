# Mosquitto

## 简介

Mosquitto 是一个开源的 MQTT 代理服务器，实现了 MQTT 协议的版本 3.1 和 3.1.1。它是 Eclipse Foundation
的一个项目，旨在提供一个轻量级、开放源代码的 MQTT 代理实现，以支持物联网（IoT）和实时通信应用的开发。

其特点和功能有：

1. **轻量级和高性能：** Mosquitto 的设计注重轻量级和高效性能，适合在资源受限的设备和网络环境中运行。它的内存和处理器占用都相对较低，可以在嵌入式系统、单片机等设备上运行。
2. **支持多种平台：** Mosquitto 可以在多种操作系统上运行，包括 Linux、Windows、macOS 等，同时也支持在各种硬件平台上部署，从树莓派到企业级服务器都可以使用。
3. **安全性：** Mosquitto 支持基于 TLS/SSL 的加密通信功能，可以确保消息在传输过程中的机密性和完整性。此外，它还支持基于用户名和密码的身份验证，以及基于
   ACL（Access Control List）的访问控制。
4. **灵活的配置选项：** Mosquitto 提供了丰富的配置选项，可以根据实际需求进行定制和调整。管理员可以配置连接限制、消息队列大小、日志级别等参数，以满足特定应用的需求。
5. **QoS 支持：** Mosquitto 支持 MQTT 的三种服务质量（QoS）级别，包括至多一次（QoS 0）、至少一次（QoS 1）和只有一次（QoS
   2），可以根据应用需求选择适当的级别以确保消息的可靠传递。
6. **集群和高可用性：** Mosquitto 支持构建 MQTT 代理的集群和高可用性架构，可以通过搭建多个 Mosquitto
   服务器来实现负载均衡和故障恢复，以确保系统的稳定性和可靠性。

## 使用 Docker 安装

### 建立本地目录

为了 `mosquitto` 数据的持久化存储，先要在本地建立目录，然后在创建容器时使用 `-v` 选项将这些目录挂载到容器内部。

```shell
sudo mkdir -p /etc/docker/mosquitto/config
sudo mkdir -p /etc/docker/mosquitto/data
sudo mkdir -p /etc/docker/mosquitto/log
sudo touch /etc/docker/mosquitto/config/passwd_file
sudo vim /etc/docker/mosquitto/config/mosquitto.conf
```

以上命令建立了 `config`、`data`、`log` 三个目录，新建了 `passwd_file`，编辑并新建 `mosquitto.conf`。

在这里，遵循 docker 的惯例，将相关文件新建在 `/etc/docker/` 下，当然也可以新建在其他地方。

> **请注意**，`passwd_file` 是为了之后建立用户认证而提前准备的。如果要禁用用户认证，则可以忽略这个文件。

### 为 `mosquitto.conf` 填充配置

```
# 启用数据持久化，配置相关目录
persistence true
persistence_file mosquitto.db
persistence_location /mosquitto/data/
log_dest file /mosquitto/log/mosquitto.log

# 匿名登录设置
allow_anonymous false
password_file /mosquitto/config/passwd_file

# 证书验证设置
# require_certificate false

# 启用 TCP 模式
listener 1883
protocol mqtt

# 启用 WebSocket 模式
# listener 9001
# protocol websockets

# 启用 TCP 模式 (SSL)
# listener 8883
# protocol mqtt
# keyfile /mosquitto/config/ssl/server.key
# certfile /mosquitto/config/ssl/server.crt
# cafile /mosquitto/config/ssl/ca.crt

# 启用 WebSocket 模式 (SSL)
# listener 8884
# protocol websockets
# keyfile /mosquitto/config/ssl/server.key
# certfile /mosquitto/config/ssl/server.pem
# cafile /mosquitto/config/ssl/root.cer
```

以上配置禁用了未认证的匿名访问，启用了 `TCP` 模式，监听 `1883` 接口。可以根据实际的使用场景来择取配置项。

如果要禁用用户认证，开启匿名登录，则：

```
# 匿名登录设置
allow_anonymous true
# password_file /mosquitto/config/passwd_file
```

> **请注意**，配置项中的路径是容器中的路径而不是主机路径。

### 建立 Docker 容器

```shell
sudo docker run -d \
  --name mqtt \
  --restart=unless-stopped \
  -p 1883:1883 \
  -v /etc/docker/mosquitto/config:/mosquitto/config \
  -v /etc/docker/mosquitto/data:/mosquitto/data \
  -v /etc/docker/mosquitto/log:/mosquitto/log \
  eclipse-mosquitto
```

### 配置用户与密码

如果要配置用户认证，则需要为 `mosquitto` 新建用户。

#### 进入容器内部

```shell
sudo docker exec -it mqtt sh
```

#### 设置用户名和密码

```shell
mosquitto_passwd -c /mosquitto/config/passwd_file mqtt
Password: 
Reenter password: 
```

#### 重新启动容器

```shell
sudo docker restart mqtt
```

### 测试

#### 在主机安装客户端

```shell
sudo apt-get install mosquitto-clients
```

测试需要用到 `mosquitto_sub` 与 `mosquitto_pub`
，分别充当订阅者和发布者的角色，订阅者和发布者的通信测试需要在不同的 `Terminal` 内进行。

### 有认证的通信测试

订阅者 Terminal：

```shell
mosquitto_sub -v -t 'hello/topic' -u mqtt -P <password>
```

发布者 Terminal：

```shell
mosquitto_pub -t 'hello/topic' -m 'hello MQTT' -u mqtt -P <password>
```

如果在订阅者的 Terminal 中可以看到 `hello/topic hello MQTT` 的消息，则说明通信正常。

#### 无认证的通信测试

订阅者 Terminal：

```shell
mosquitto_sub -v -t 'hello/topic'
```

发布者 Terminal：

```shell
mosquitto_pub -t 'hello/topic' -m 'hello MQTT'
```

### 使用 url 测试

mqtt 服务的 url 格式：`mqtt(s)://[username[:password]@]host[:port]/topic`

订阅者 Terminal：

```shell
mosquitto_sub -v -L mqtt://mqtt:<password>@localhost/hello/topic
```

发布者 Terminal：

```shell
mosquitto_pub -L mqtt://mqtt:<password>@localhost/hello/topic -m 'hello MQTT'
```
