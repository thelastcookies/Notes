# Home Assistant

## 安装

使用 docker 拉取 home-assistant 镜像，进行基础设置，并运行。

```shell
sudo docker run -d \
  --name home \
  --privileged \
  --restart=unless-stopped \
  -e TZ=Asia/Shanghai \
  -v /etc/docker/homeassistant:/config \
  -v /run/dbus:/run/dbus:ro \
  --network=host \
  homeassistant/home-assistant
```

https://github.com/hacs/integration

## HACS

sudo mkdir /etc/docker/homeassistant/custom_components

sudo mv ./hacs /etc/docker/homeassistant/custom_components
