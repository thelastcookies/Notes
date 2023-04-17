# 🥦 curl - Command Line Tool and Library for Transferring Data with URLs 

![curl-logo.svg](../static/images/curl--logo.svg)
##  什么是 curl ？
`curl`是开源项目**cURL**的产品之一。
旨在基于网络协议，通过命令行的简单指令，使用指定的协议，对指定URL进行数据传输。


##  curl支持的协议
`curl` 支持的通信协议主要有 `HTTP(S)`, `FTP(S)`, `SFTP` , `IMAP(S)`, `SMTP(S)`,
`LDAP(S)`, `POP3(S)`, `RTSP`, `TELNET`等。

##  通用指令格式：
```
$ curl [options / URLs]
```
## 实现对 RESTful API 的请求
- #### `-X, --request <method>`  指定METHOD。
  指定与 HTTP 服务器通信时要使用的自定义请求方法。
  ```
  $ curl -X POST https://example.com
  $ curl -X PUT https://example.com
  $ curl -X DELETE https://example.com
  ```
- #### `-d, --data <data>` 数据选项。
  将指定数据发送到 HTTP 服务器。默认格式为`application/x-www-form-urlencoded`。
  ```
  $ curl -d "id=foo&name=bar" https://example.com
  ```
- #### `-G, --get` 发送GET请求。
  使用此选项时使用 `-d`、`--data`、`--data-binary` 或 `--data-urlencode` 
  指定的数据用于 HTTP GET 请求，而不是 POST 请求。数据将以`?`分隔后的形式追加到URL之后。
  ```
  $ curl -G https://example.com
  $ curl -G -d "tool=curl" -d "age=old" https://example.com
  $ curl -G -I -d "tool=curl" https://example.com
  ```
- #### `-H, --header <header/@file>` 定义发送信息中的额外Header。
  当在 HTTP 请求中使用时，它被添加到常规请求标头中。
  ```
  $ curl -H "User-Agent: yes-please/2000" https://example.com
  $ curl -H "Host:" https://example.com
  $ curl -X POST -H "application/json" -d '{"id": "foo", "name": "bar"}' https://example.com
  ```
- #### `-I, --head` 发送HEAD请求。
  ```
  $ curl -I https://example.com
  ```
- #### `-F, --form <name=content>`模拟表单提交。
  请求头为`Content-Type: multipart/form-data`。
  可以通过此指令进行文件上传。
  ```
  $ curl -F name=John -F shoesize=11 https://example.com/
  $ curl -F profile=@portrait.jpg https://example.com/upload.cgi
  $ curl -F "story=<hugefile.txt" https://example.com/
  ```

## 重定向
`-L, --location` 如果服务器报告请求的页面已移动到不同的位置（即返回状态码 3XX），
此选项将使 curl 在新位置重新请求。
```
$ curl -L https://example.com
```

## 文件传输
- #### `-O, --remote-name`/`-o, --output <file>`下载。
  ```
  $ curl -O https://example.com/file
  $ curl -o filename https://example.com/file
  $ curl -o file https://example.com -o file2 https://example.net
  $ curl example.com example.net -o file -o file2
  ```
- #### `--output-dir <dir>`/`--create-dirs` 修改/新建应存储文件的目录。
  ```
  $ curl --output-dir "tmp" -O https://example.com
  $ curl --create-dirs --output local/dir/file https://example.com
  ```
- #### `-T, --upload-file <file>` 上传。
  该指令会将指定的本地文件传输到远程 URL。

  **FTP 文件上传：** 
  如果上传的 URL 以 `/` 结尾，curl会认为该URL中没有指定文件名，将提交本地文件名作为远程文件名。 
  如果上传的 URL 以其他名称结尾，curl 会将最后一个目录名作为要使用的远程文件名。
  ```
  $ curl -T file https://example.com
  $ curl -T "img[1-1000].png" ftp://ftp.example.com/
  ```
  **HTTP 文件上传：**
  服务器需要接受 PUT 请求，然后成功上传。
  ```
  $ curl -T - https://www.upload.com/myfile
  ```
- #### `--limit-rate <speed>` 限速。
  用于指定下载和上传的最大传输速率。单位为B，可附加k、m、g、t、p等后缀。
  ```
  $ curl --limit-rate 100K https://example.com
  $ curl --limit-rate 1000 https://example.com
  $ curl --limit-rate 10M https://example.com
  ```
- #### `-Y, --speed-limit <speed>`, `-y, --speed-time <seconds>`  用于指定最小传输速率与超时。
- 如果在 `speed-time` 内传输速度低于 `speed-limit`，则传输将中止。
  ```
  $ curl -Y 300 -y 10 https://example.com
  ```
- #### `-C, --continue-at <offset>` 断点续传
  在给定偏移处继续/恢复先前的文件传输。
  ```
  $ curl -C - https://example.com
  $ curl -C 400 https://example.com
  ``` 
- #### 进度条说明：
  ![](../static/images/curl--file-transfer.png)

  ```
  从左到右的标注依次：
  % - 整个传输完成的百分比
  Total - 整个预期传输的总大小
  % - 下载完成的百分比
  Received - 当前下载的字节数
  % - 上传完成的百分比
  Xferd - 当前上传的字节数
  Average Speed Dload - 下载的平均传输速度
  Average Speed Upload - 上传的平均传输速度
  Time Total - 完成操作的预期时间
  Time Current - 自调用以来经过的时间
  Time Left - 预计完成时间
  Curr.Speed - 最后 5 秒的平均传输速度（传输的前 5 秒当然是基于较短的时间。）
  ```

## 使用密码
`-u, --user <user:password>`
可以在`HTTP`、`FTP`等协议里使用下述两种方式来进行用户验证。

```
$ curl ftp://name:passwd@machine.domain:port/full/path/to/file
$ curl -u name:passwd ftp://machine.domain:port/full/path/to/file
$ curl http://name:passwd@machine.domain/full/path/to/file
$ curl -u name:passwd http://machine.domain/full/path/to/file
```

## 详细信息：
`-v, --verbose` 用于查看和调试连接情况。

```
$ curl -v https://example.com
```
![curl--verbose.png](../static/images/curl--verbose.png)
其中，以`>`开头的行表示`Request Headers`，以`<`开头的行表示`Response Headers`，以`*`开头的行表示`curl`提供的附加信息。

另外，`-i, --include` 用于只查看 `Response Headers`。`-D, --dump-header <filename>` 用于将`Response Headers`写入指定文件进行存档。

```
$ curl -i https://example.com
$ curl --D store.txt https://example.com
```

## 代理
`-x, --proxy [protocol://]host[:port]` 使用指定的代理。

可以使用`protocol://`前缀指定代理字符串，默认为 HTTP 代理。

```
$ curl --proxy http://proxy.example https://example.com
$ curl -x my-proxy:888 ftp://ftp.leachsite.com/README
$ curl -u user:passwd -x my-proxy:888 http://www.get.this/
```
