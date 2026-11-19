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
        # 请求的是已存在的目录且未带斜杠时，自动补斜杠（rewrite 默认保留参数）
        if (-d $request_filename) {
            rewrite ^(.*[^/])$ https://$host$1/ permanent;
        }
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

### Nginx ssl 配置示例：
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
        # 请求的是已存在的目录且未带斜杠时，自动补斜杠（rewrite 默认保留参数）
        if (-d $request_filename) {
            rewrite ^(.*[^/])$ https://$host$1/ permanent;
        }
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

### proxy_pass 的配置规则说明

`proxy_pass` 的语法是：

```text
proxy_pass http://主机[:端口][路径];
```

方括号里的“路径”为 URI，nginx 的判断逻辑为：

- **有路径**：用这个路径替换掉 `location` 匹配到的部分。
- **没路径**：不替换，直接用原始请求的 URI 进行路径拼接。

**`proxy_pass` 后面如果带了路径（哪怕只是一个 `/`），nginx 就会把 `location` 匹配到的那部分前缀替换掉；如果没带路径，就原样转发。**

#### 请求示例

假设客户端请求：

```text
http://backend-origin/api/foo?x=1
```

`location` 为：

```nginx
location /api/ {
    ...
}
```

匹配情况：

- `location` 匹配到的前缀为：`/api/`
- 剩余部分是：`foo?x=1`

#### 情况一：`proxy_pass http://backend;`（不带路径）

```nginx
location /api/ {
    proxy_pass http://backend;
}
```

`proxy_pass` 里只有主机 `backend`，**没有路径部分**。  
此时 nginx 的规则是：**不替换，直接把原始 URI 原封不动地转发。**

所以最终的结果为：

```text
/api/foo?x=1
```

也就是：

```text
http://backend-origin/api/foo?x=1 -> http://backend/api/foo?x=1
```

#### 情况二：`proxy_pass http://backend/;` （带路径 `/`）

```nginx
location /api/ {
    proxy_pass http://backend/;
}
```

注意此时 `backend` 后面有 `/`，这就是路径部分。  
此时 nginx 的规则是：**把 `location` 匹配到的前缀 `/api/` 替换成 `proxy_pass` 里的路径 `/`，然后把剩余部分拼上去。**

- 匹配前缀：`/api/` 替换成：`/`
- 剩余部分：`foo?x=1`
- 拼接结果：`/foo?x=1`

所以最终的结果为：

```text
/foo?x=1
```

也就是：

```text
http://backend-origin/api/foo -> http://backend/foo
```

#### 几种常见组合

| location | proxy_pass | 请求 `/api/foo` 转发到 |
|---|---|---|
| `/api/` | `http://backend` | `http://backend/api/foo` |
| `/api/` | `http://backend/` | `http://backend/foo` |
| `/api/` | `http://backend/v1/` | `http://backend/v1/foo` |
| `/api/` | `http://backend/v1` | `http://backend/v1foo` ⚠️ 不推荐 |

> 最稳妥的写法是 **location 和 proxy_pass 的尾斜杠保持一致**：

#### 正则 location 的特殊限制

如果 `location` 用了正则，比如：

```nginx
location ~ ^/api/ {
    proxy_pass http://backend/;   # ❌ 会报错
}
```

nginx 不允许在正则 location 中使用带路径的 `proxy_pass`。  
正则 location 里只能写：

```nginx
location ~ ^/api/ {
    proxy_pass http://backend;    # 只能不带路径
}
```

所以一般建议用前缀 location，更直观。

### snippets 的配置说明

`snippets` 用于把公共 proxy 配置写成一个文件，然后在需要的地方 `include`。

#### 建一个公共片段

`/etc/nginx/snippets/proxy.conf`：

```nginx
# --- 关键请求头配置 ---
# 设置真实域名，防止后端有根据 Host 进行的逻辑校验
proxy_set_header Host $host;
# 将客户端真实 IP 传给后端
proxy_set_header X-Real-IP $remote_addr;
# 经过代理的多级 IP 链路
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
# 传递请求协议（http 或 https）
proxy_set_header X-Forwarded-Proto $scheme;

# --- 超时时间配置 (可选) ---
proxy_connect_timeout 60s;
proxy_send_timeout 60s;
proxy_read_timeout 60s;
```

#### 2. 三种放置层级

`proxy_*` 指令在 `http`、`server`、`location` 三个上下文都能用，所以有三种放法。

##### 放法 A：放在 `server` 块里（最省事）

```nginx
server {
    listen 443 ssl;
    server_name your_domain.com;

    include /etc/nginx/snippets/proxy.conf;   # 整个 server 生效

    ...
}
```

优点：一个 server 里所有 `location` 都会自动继承，不用重复引入。  
缺点：静态 `location` 也会“继承”这些指令。

##### 放法 B：放在 `nginx.conf` 的 `http` 块里（全局）

```nginx
http {
    include /etc/nginx/snippets/proxy.conf;   # 所有 server 生效
    include /etc/nginx/conf.d/*.conf;
}
```

优点：所有 server 通用，全局引入。  
缺点：如果某个 server 想用不同配置，会比较麻烦，且缺少灵活性。

##### 放法 C：每个 `location` 单独 include

```nginx
location /some/api/ {
    proxy_pass http://ip:port1/api/;
    include /etc/nginx/snippets/proxy.conf;
}

location /other/api/ {
    proxy_pass http://ip:port2/api/;
    include /etc/nginx/snippets/proxy.conf;
}
```

优点：最清晰，每个 location 显式声明。  
缺点：接口多的时候会重复很多次。

#### 注意：继承是“替换”，不是“合并”

> 如果当前层级写了**任意一个** `proxy_set_header`，上层所有 `proxy_set_header` **全部失效**，不是只覆盖同名的那一个。

举例：

```nginx
server {
    include /etc/nginx/snippets/proxy.conf;   # 里面定义了 Host、X-Real-IP...

    location /some/api/ {
    proxy_pass http://ip:port/api/;
        proxy_set_header Host $proxy_host;    # ⚠️ 这一行会让 server 级所有 proxy_set_header 失效
    }
}
```

location 里出现了 `proxy_set_header`，server 级的整套就不再继承。

所以如果使用了server 级 include，**location 里就不能再写任何 `proxy_set_header`**。  
如果某个接口确实要特殊 header，就在那个 location 里把 snippet 再次 include，然后再加特殊 header：

```nginx
location /special/api/ {
    proxy_pass http://ip:port/api/;
    include /etc/nginx/snippets/proxy.conf;
    proxy_set_header Host $proxy_host;   # 只加这一条，snippet 里的其他条也在
}
```

特别指出：`proxy_http_version`、`proxy_connect_timeout` 这些不受此规则影响，它们是各自独立继承的。

#### 最佳实践

服务多、且都在同一台 nginx 上：

- 公共头放 `/etc/nginx/snippets/proxy.conf`；
- 在 `http` 或 `server` 层级 include 一次；
- 所有 API `location` 只写 `proxy_pass`，不写 `proxy_set_header`；
- 个别特殊的接口，在那个 location 里重新 include snippet 再加特殊 header。

#### 进阶：不同服务不同 header

如果不同服务需要不同 `Host` 或不同超时时间，可以把 snippet 拆分：

```text
snippets/
├── proxy-base.conf      # http_version、X-Real-IP、X-Forwarded-*
├── proxy-host-origin.conf
└── proxy-host-backend.conf
```

按需组合：

```nginx
location /some/api/ {
    proxy_pass http://ip:port/api/;
    include /etc/nginx/snippets/proxy-base.conf;
    include /etc/nginx/snippets/proxy-host-backend.conf;
}
```
