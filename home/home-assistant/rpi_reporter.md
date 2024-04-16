# RPi-Reporter-MQTT2HA-Daemon

## 介绍

[RPi-Reporter-MQTT2HA-Daemon](https://github.com/ironsheep/RPi-Reporter-MQTT2HA-Daemon) 是一个简单的 Linux python
脚本，用于查询运行它的 Raspberry Pi 的各种配置和状态值，然后通过 MQTT 将这些值报告给 Home Assistant，以便您可以通过 Home
Assistant 仪表板跟踪它们。

## 安装

本文档适用的硬件环境：
Raspberry Pi 5（Raspberry OS）

### 安装 Python 环境

```shell
sudo apt-get install git python3 python3-pip python3-tzlocal python3-sdnotify python3-colorama python3-unidecode
python3-apt python3-paho-mqtt python3-requests
```

### 拉取代码

```shell
sudo git clone https://github.com/ironsheep/RPi-Reporter-MQTT2HA-Daemon.git /opt/RPi-Reporter-MQTT2HA-Daemon
cd /opt/RPi-Reporter-MQTT2HA-Daemon
```

## 配置 Python venv

```shell
sudo python -m venv /opt/RPi-Reporter-MQTT2HA-Daemon
```

## 在 venv 中安装依赖

```shell
sudo /opt/RPi-Reporter-MQTT2HA-Daemon/bin/pip install -r /opt/RPi-Reporter-MQTT2HA-Daemon/requirements.txt
```

## MQTT Broker 环境

安装配置 MQTT Broker 可参考文档：[01_mosquitto.md](/hardware/mqtt/01_mosquitto.md)。

```shell
https://test.mosquitto.org:1883
https://test.mosquitto.org:8883
```

## 脚本配置文件

```shell
sudo cp /opt/RPi-Reporter-MQTT2HA-Daemon/config.{ini.dist,ini}
sudo vim /opt/RPi-Reporter-MQTT2HA-Daemon/config.ini
```

config.ini 的基本配置项：

```
# MQTT Broker 的 host 地址
hostname = {hostname}

# 默认情况下，Home Assistant 会监听 /homeassistant 前缀，因此使用默认的 `homeassistant` 即可
discovery_prefix = {homeassistant} 

# MQTT 主题，格式为 {base_topic/sersor_name}，使用默认的 `home/nodes` 即可
base_topic = {home/nodes}

# 如果 MQTT Broker 中配置了用户认证则需要配置用户名和密码
username = {username}
password = {password}
```

## 设置为系统服务

为了让 HA 收到 RPi 是否在线/离线以及获取上次报告的时间，必须将此脚本设置为作为系统服务运行，且必须在配置文件中启用守护程序模式。

首先，要向运行服务的用户帐户授予对硬件的访问权限。

### 设置守护程序帐户以允许访问温度值

默认情况下，此脚本作为 `user: group daemon: daemon` 运行。
由于此脚本需要访问 GPU，因此需要为守护程序用户添加对 GPU 的访问权限，如下：

```shell
➜ raspberrypi ~ > groups daemon
daemon : daemon
➜ raspberrypi ~ > sudo usermod daemon -a -G video
➜ raspberrypi ~ > groups daemon
daemon : daemon video
```

### 设置脚本以系统服务运行

```shell
# 设置
sudo ln -s /opt/RPi-Reporter-MQTT2HA-Daemon/isp-rpi-reporter.service /etc/systemd/system/isp-rpi-reporter.service

sudo systemctl daemon-reload

# 设置开机时运行
sudo systemctl enable isp-rpi-reporter.service

# 后台执行脚本
sudo systemctl start isp-rpi-reporter.service &

# 检查状态，确保是否启动成功
sudo systemctl status isp-rpi-reporter.service
```

## 在 Home Assistant 中配置 MQTT

点击[配置链接](https://my.home-assistant.io/redirect/config_flow_start/?domain=mqtt)快速转到配置界面。

完成配置表单后，即可将 MQTT 集成添加到 Home Assistant 中。

## 参考资料

https://github.com/ironsheep/RPi-Reporter-MQTT2HA-Daemon

https://www.mqtt.cn/mqtt-tutorial/home-assistant

https://www.mqtt.cn/1101.html

https://www.mqtt.cn/1004.html
