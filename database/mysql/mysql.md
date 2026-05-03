# Mysql 部署

## 使用 Docker 部署

### 拉取官方镜像

拉取最新版 MySQL 镜像：

```shell
docker pull mysql 
```

或拉取指定版本：

```shell
docker pull mysql:5.7
```

### 检查是否拉取成功

```
sudo docker images
```

### 建立容器

```shell
sudo docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql
```

- –name：容器名，此处命名为 `mysql`
- -p：端口映射，映射主机`3306`端口到容器的`3306`端口
- -e：配置信息，配置 `mysql` 的 `root` 用户的登陆密码
- -d：后台运行容器，保证在退出终端后容器继续运行

如果要在建立容器时建立目录映射，则

```shell
sudo docker run --name mysql \
--restart=unless-stopped \
-p 3306:3306 \
-v /etc/docker/mysql/conf:/etc/mysql \
-v /etc/docker/mysql/logs:/var/log/mysql \
-v /etc/docker/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=your_password \
-d mysql \
--secure-file-priv=""
```

- -v：主机和容器的目录映射关系
- ":"：前为主机目录，之后为容器目录

### 检查容器是否正确运行

```shell
sudo docker ps
```

可以看到容器ID，容器的源镜像，启动命令，创建时间，状态，端口映射信息，容器名字。

## 设置允许外部访问

### 连接 MySQL

```shell
sudo docker exec -it mysql bash
mysql -u root -p
```

查看 `user` 表

```
mysql> SELECT user, host FROM mysql.user;
+------------------+-----------+
| user             | host      |
+------------------+-----------+
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| root             | localhost |
+------------------+-----------+
4 rows in set (0.00 sec)
```

可以看到在 `user` 表中已创建的 `root` 用户。

`host` 字段表示登录的主机，其值可以用 `IP`，也可用主机名，

### 实现远程连接（授权法）

创建一个可以供任何 IP 地址访问的用户。

```shell
# 创建用户
CREATE USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'your_password';
# 设置权限
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
# 刷新权限设置
FLUSH PRIVILEGES;
```

其中 `*.*` 代表所有资源所有权限，`'root'@'%'` 中的 `root` 代表用户名，`%` 代表所有的访问地址。IDENTIFIED BY `your_password`
，这里提供数据库用户的密码，`WITH GRANT OPTION` 表示允许级联授权。

将权限改为 `ALL PRIVILEGES`，这样机器就可以以用户名，密码的形式远程访问该机器上的 `MySql`。

因此，以上语句的含义表示为：

允许任何 ip 地址的客户端使用 `root` 用户和 `your_password` 密码来访问这个 MySQL 服务。
