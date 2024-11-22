# Linux 下的 Tomcat 环境搭建（以 Debian 为例）

## 安装包获取

使用以下命令在线获取

```shell
wget https://downloads.apache.org/tomcat/tomcat-9/v9.x.x/bin/apache-tomcat-9.x.x.tar.gz
```

或者到 Tomcat 官方下载合适版本的 Tomcat 包。

## 解压并移动到合适目录

```shell
tar -zxvf apache-tomcat-9.x.x.tar.gz

sudo mv apache-tomcat-9.x.x [Tomcat 安装目录]
```

## 配置 Tomcat 环境变量（可选）

编辑配置文件

```shell
sudo vim /etc/profile
```

添加环境变量

```shell
export CATALINA_HOME=[Tomcat 安装目录]
export PATH=$CATALINA_HOME/bin:$PATH
```

保存后，执行以下命令使其生效：

```shell
source /etc/profile
```

## 启停 Tomcat

```shell
# 启动命令
$CATALINA_HOME/bin/startup.sh
# 或
startup.sh

# 停止命令
$CATALINA_HOME/bin/shutdown.sh
# 或
shutdown.sh
```

## 验证安装

打开浏览器访问 http://<服务器IP>:8080，如果看到 Tomcat 欢迎页面，说明配置成功。

## 修改文件夹权限（可选）：

如果在启动 Tomcat 时提示权限问题，则可能是系统无权限执行相关文件。

修改相关文件权限

```shell
sudo chmod -R 755 /opt/apache-tomcat-9.x.x
```

## 设置 Tomcat 为服务（可选）

为了开机字段启动，可以将 Tomcat 配置为系统服务。

创建服务文件：
```shell
sudo nano /etc/systemd/system/tomcat.service
```

添加以下内容：
```
[Unit]
Description=Apache Tomcat Web Server
After=network.target

[Service]
Type=forking

Environment=JAVA_HOME=[Java 安装目录]
Environment=CATALINA_PID=[Tomcat 安装目录]/temp/tomcat.pid
Environment=CATALINA_HOME=[Tomcat 安装目录]
Environment=CATALINA_BASE=[Tomcat 安装目录]
ExecStart=[Tomcat 安装目录]/bin/startup.sh
ExecStop=[Tomcat 安装目录]/bin/shutdown.sh

User=tomcat
Group=tomcat
Restart=always

[Install]
WantedBy=multi-user.target
```

重新加载服务并启用：
```shell
sudo systemctl daemon-reload
sudo systemctl enable tomcat
sudo systemctl start tomcat
sudo systemctl status tomcat
```
