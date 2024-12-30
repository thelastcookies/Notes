# WebDAV

## 准备工作

• 确保服务器已安装 Docker 和 Docker Compose（可选）。
• 准备一个目录用作 WebDAV 的文件存储目录，例如 /data/webdav。

## 拉取 WebDAV 镜像

推荐使用成熟的 WebDAV Docker 镜像，例如 bytemark/webdav。

运行以下命令拉取镜像：

```shell
docker pull ugeek/webdav:arm64
```

## 创建 WebDAV 容器

运行以下命令启动 WebDAV 服务：

```shell
docker run --name webdav \
  --restart=unless-stopped \
  -p 80:80 \
  -v $HOME/docker/webdav:/media \
  -e USERNAME=webdav \
  -e PASSWORD=webdav \
  -e TZ=Europe/Madrid  \
  -e UDI=1000 \
  -e GID=1000 \
  -d  ugeek/webdav:arm64
```

参数说明：
• -p 8080:80：将容器的 80 端口映射到主机的 8080 端口。
• -v /data/webdav:/var/lib/dav/data：将主机目录挂载到容器的 WebDAV 数据目录。
• -e AUTH_TYPE=Basic：设置为基本身份验证。
• -e USERNAME=yourusername 和 -e PASSWORD=yourpassword：设置 WebDAV 访问的用户名和密码。

## 使用 Docker Compose 部署（可选）

创建一个 docker-compose.yml 文件：

```dockerfile
version: "3.8"
services:
  webdav:
    image: bytemark/webdav
    container_name: webdav
    ports:
      - "8080:80"
    volumes:
      - /data/webdav:/var/lib/dav/data
    environment:
      - AUTH_TYPE=Basic
      - USERNAME=yourusername
      - PASSWORD=yourpassword
```

## 启动服务：

```shell
docker-compose up -d
```

## 访问 WebDAV

- 使用浏览器或 WebDAV 客户端（如 Windows 文件管理器、Cyberduck 等）访问 http://<your-server-ip>:8080。
- 输入设置的用户名和密码进行身份验证。

## 可选配置

- 启用 HTTPS：将 WebDAV 服务放置在 Nginx 反向代理后，使用 SSL 证书。
- 设置权限：调整挂载目录 /data/webdav 的读写权限，确保容器具有正确的访问权限。
