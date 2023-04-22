# 几种国内访问 GitHub 的方法

## 配置 hosts

### hosts 订阅源
以下为两个 GitHub Hosts 定期更新地址。

```
https://raw.hellogithub.com/hosts

https://hosts.gitcdn.top/hosts.txt
```

### hosts 文件位置

Windows 下：

```
C:\Windows\System32\drivers\etc\hosts
```

***修改 hosts 文件可能只能解决访问的问题，并不能保证速度，所以以下提供几种可以加速访问的方法。***

## 使用公用加速源

**[ghproxy.com](https://ghproxy.com/)**

这是一个 GitHub 文件、Releases、archive、gist 、raw.githubusercontent.com 文件代理加速下载服务。

特点：使用简单，仅需将 `https://ghproxy.com` 加在需要 clone 的 GitHub 仓库链接前；不支持 SSH 方式。

## 使用镜像网站

```
hub.fastgit.org
github.com.cnpmjs.org
```

使用方法：使用时将 github 域名替换为对应的镜像网站域名即可。

特点：因为成本问题，一般速度很难保证。

## 使用代理应用

**[dev-sidecar](https://github.com/docmirror/dev-sidecar)**

这是一个为 GitHub 开发者边车，命名取自service-mesh的service-sidecar，意为为开发者打辅助的边车工具。

> 开发者提示：开着ds重启电脑会导致无法上网，你可以再次打开ds，然后右键小图标退出ds即可。

特点：全平台支持；可以加速 Stack Overflow 和 npm。

## 配置本地 `HTTP` 代理

需要科学上网方式，首先要查看上网工具的 `http` 或 `socks5` 代理端口（假如为 `7890` 或 `1080`）。

### 全局代理设置

```shell
# http
$ git config --global http.proxy http://127.0.0.1:7890
$ git config --global https.proxy https://127.0.0.1:7890

# socks5
$ git config --global http.proxy socks5://127.0.0.1:1080
$ git config --global https.proxy socks5://127.0.0.1:1080
```

### 只对 `github.com` 进行代理（推荐）

```shell
# http
$ git config --global http.https://github.com.proxy http://127.0.0.1:7890
$ git config --global https.https://github.com.proxy https://127.0.0.1:7890

# socks5
$ git config --global http.https://github.com.proxy socks5://127.0.0.1:1080
$ git config --global https.https://github.com.proxy socks5://127.0.0.1:1080
```

### 查看已有配置

```shell
$ git config --global -l
```

### 取消代理

```shell
# 全局
$ git config --global --unset http.proxy
$ git config --global --unset https.proxy

# 仅 github.com
$ git config --global --unset http.https://github.com.proxy
$ git config --global --unset https.https://github.com.proxy
```

## 本地 `SSH` 代理配置

*To be continued*