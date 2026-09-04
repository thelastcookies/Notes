# Nginx

## 使用 Docker 配置 Nginx

使用 Docker 安装 Nginx 更干净、更方便迁移。

## 创建映射目录（用于存放配置文件和网页）
```shell
mkdir -p ~/nginx/html ~/nginx/conf.d
```

## 启动容器
```shell
docker run -d \
  --name nginx \
  --restart=unless-stopped \
  -p 80:80 \
  -v ~/nginx/html:/usr/share/nginx/html:ro \
  -v ~/nginx/conf.d:/etc/nginx/conf.d:ro \
  nginx:latest
```

## 配置文件

Nginx 配置文件主要有以下几种：

```
nginx.conf	主配置文件。唯一被 Nginx 进程直接读取的入口文件。
conf.d/	通用配置目录，通常存放站点或功能配置（*.conf）。
sites-available/	所有站点的“配置仓库”，存放具体的server配置（默认不生效）。
sites-enabled/	存放指向sites-available的软链接，只有被链接进来的配置才会被加载。
mime.types	MIME类型映射文件（定义文件后缀与Content-Type的关系）。
proxy_params	代理相关通用参数（被include引用）。
fastcgi_params	FastCGI相关通用参数（如PHP处理）。
```

创建配置文件：

```shell
sudo nano ~/nginx/conf.d/default.conf
```

自动映射配置示例：
```
server {
    listen 80;
    server_name your_domain.com; # 替换成你的域名或公网IP
    
    root /usr/share/nginx/html;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    error_page 404 /404.html;
    location = /404.html {
        internal;
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        internal;
    }
}
```

## 重启
```shell
sudo docker restart nginx
```

## 其他

Nginx ssl 配置示例：
```
server {
listen 443 ssl;
listen [::]:443 ssl;
server_name your_domain.com; # 替换成你的域名或公网IP

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
