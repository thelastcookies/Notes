# Docker 安装配置
针对阿里云的 Debian 系统，安装步骤与标准的 Ubuntu 略有不同，主要是源的配置。阿里云的 Debian 镜像通常已经预装了阿里云的内网软件源，安装速度会非常快。

以下是针对 Debian 11 / 12 的标准安装流程：

## 1. 更新环境并安装依赖
   首先，确保系统的软件包索引是最新的，并安装必要的支持工具。

```shell
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg lsb-release
```

## 2. 添加 Docker 官方 GPG 密钥
   密钥用于验证下载的软件包是否安全。
```shell

sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

## 3. 写入软件源配置
   这里直接使用阿里云提供的 Docker 镜像源，这样在安装和更新时无需走国际带宽。

```shell

echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://mirrors.aliyun.com/docker-ce/linux/debian \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## 4. 安装 Docker 引擎
   执行安装命令：

```shell
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

## 5. 管理 Docker 服务
   安装完成后，启动并检查状态：

### 启动并设置自启
```shell
sudo systemctl enable docker --now
```

### 验证运行状态

```shell
sudo systemctl status docker
```

## 6. 进阶：配置阿里云专属镜像加速器
1. 在阿里云上使用 Docker，最重要的一步是配置容器镜像服务 (ACR) 提供的加速地址。

2. 登录 阿里云容器镜像服务控制台。

3. 在左侧菜单栏选择 镜像工具 -> 镜像加速器。

4. 你会看到一个专属地址，https://cr.console.aliyun.com/cn-shanghai/instances/mirrors

5. 将该地址填入下方的配置文件中：

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF' 
{ "registry-mirrors": ["https://1n2gjsld.mirror.aliyuncs.com"] }
EOF
```

6.  重启 Docker
sudo systemctl daemon-reload
sudo systemctl restart docker
💡 阿里云 Debian 环境特别说明：
防火墙： 阿里云 Debian 默认可能没有启用 ufw，但云控制台的安全组是生效的。如果你的容器应用（如 Web 服务）无法通过公网访问，请检查阿里云后台安全组是否放行了对应端口。

内核版本： Debian 11/12 的内核版本较新，完美支持 Docker 的各种特性（如 Overlay2 存储驱动），通常不需要额外升级内核。

# Nginx 安装配置

使用 apt 安装 Nginx

## 1. 更新软件包索引
```shell
sudo apt update
```

## 2. 安装 Nginx

直接使用 apt 安装 Nginx 包：

```shell
sudo apt install nginx -y
```
-y 会自动确认安装，若想手动确认可去掉该参数。

## 3. 启动 Nginx 服务

使用 systemctl 启动服务：

```shell
sudo systemctl start nginx
```
检查运行状态，确保没有错误：

```shell
sudo systemctl status nginx
```

如果看到 active (running) 字样，说明启动成功。

## 4. 设置开机自启（可选但推荐）

让 Nginx 在系统重启后自动运行：

```shell
sudo systemctl enable nginx
```

## 5. 配置虚拟主机（站点配置）
不要直接修改 nginx.conf，推荐为每个域名创建一个独立的配置文件。

创建配置文件：

```shell
sudo nano /etc/nginx/sites-available/my_site.conf
```
输入基础配置模板：
```
server {
listen 80;
server_name your_domain.com; # 替换成你的域名或公网IP

    root /var/www/html; # 你的网页文件存放目录
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

激活配置并检查：

```shell
# 建立软链接激活配置
sudo ln -s /etc/nginx/sites-available/my_site.conf /etc/nginx/sites-enabled/
# 检查语法是否有误
sudo nginx -t
# 重启生效
sudo systemctl reload nginx
```

## 6. 关键步骤：开放端口
阿里云安全组开放端口无论用哪种方式，必须在阿里云控制台开放端口，否则外部无法访问：
  1. 登录 阿里云 ECS 控制台。
  2. 点击左侧 安全组 -> 点击当前实例使用的安全组。
  3. 在 入方向 点击 快速添加。
  4. 勾选 HTTP (80) 和 HTTPS (443)。
  5. 点击确定。

## 7. 进阶：HTTPS
直接通过 IP 地址实现 HTTPS，自签名证书（Self-Signed Certificate） 是最直接的方案。
它的本质是：你自己既是“申请人”，也是“发证机关”。虽然浏览器不认识你（会报警告），但它依然能提供加密传输功能。
1. 生成自签名证书在 Debian 终端执行以下命令。我们会使用 openssl 工具生成一个有效期为 10 年（3650天）的证书。

```shell
sudo openssl req -x509 -nodes -days 3650 -newkey rsa:2048 \
-keyout /etc/ssl/private/nginx-selfsigned.key \
-out /etc/ssl/certs/nginx-selfsigned.crt
```

执行后会有交互提示，关键点如下：
- Common Name (e.g. server FQDN or YOUR name): 这一项最重要，填入你的公网 IP 地址。
- 其他项（Country, State, Locality等）可以直接按回车跳过，或者随便填（例如：CN, Shanghai, MyProject）。

2. 配置 Nginx 使用该证书

你需要修改你的 Nginx 站点配置文件（通常是 `/etc/nginx/sites-available/default`）。
```
server {
listen 443 ssl;
listen [::]:443 ssl;
server_name 你的公网IP; # 这里填入你的阿里云 IP

    # 指向生成的证书和私钥
    ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
    ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;

    # 推荐的 SSL 优化配置
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

# (可选) 将 80 端口的流量重定向到 443
server {
  listen 80;
  server_name 你的公网IP;
  return 301 https://$host$request_uri;
}
```

3. 生效并验证
 - 测试配置： sudo nginx -t
 - 重载服务： sudo systemctl reload nginx
 - 安全组检查： 确保阿里云控制台入方向放行了 443 端口。

4. 访问时的预期表现
当你通过浏览器访问 https://你的IP 时，会发生以下情况：
- 红色警告： 浏览器会提示“您的连接不是私密连接”或“潜在的安全风险”。这是因为你的证书不是由受信任的机构（如 DigiCert 或 Let's Encrypt）签发的。
- 如何进入： 点击页面上的 “高级” (Advanced) -> “继续前往...” (Proceed to...)。
- 加密状态： 进入后，地址栏的锁可能会带个红叉，但此时你和服务器之间的数据传输已经是 TLS 加密 的了。
