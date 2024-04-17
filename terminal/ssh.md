# SSH

![ssh__banner.png](images/ssh__banner.png)

## 什么是 SSH ？

安全外壳协议（Secure SHell Protocol，SSH）是一种加密的网络传输协议，可在不安全的网络中为网络服务提供安全的传输环境。

SSH 通过在网络中创建安全隧道来实现 SSH 客户端与服务器之间的连接。SSH 最常见的用途是远程登录系统，人们通常利用 SSH
来传输命令行界面和远程执行命令。

## SSH 版本历史

### 1.x 版本

1995年芬兰赫尔辛基理工大学的塔图·于勒宁编写了一套保护信息传输的程序，并称其为 “secure shell”，简称 SSH。设计目标是取代先前的
Telnet、FTP 和 rsh 等安全性不足的协议。

### 2.x 版本

2006年，SSH-2 协议成为了新的标准。

与 SSH-1 相比，SSH-2
进行了一系列功能改进并增强了安全性，例如基于[迪菲-赫尔曼密钥交换](../general_knowledge/cryptography/cryptography.md#迪菲-赫尔曼密钥交换diffiehellman-key-exchange)
的加密和基于[消息认证码](../general_knowledge/cryptography/cryptography.md#消息认证码)的完整性检查。SSH-2 还支持通过单个
SSH 连接任意数量的 shell 会话。

SSH-2 协议与 SSH-1 不兼容，由于更加流行，一些实现（例如 lsh 和 Dropbear）只支持SSH-2协议。

### 1.99 版

RFC 4253 规定兼容 SSH 2.0 及以前版本的 SSH 服务器应将其原始版本标为“1.99”。

“1.99”并不是实际的软件版本号，而是为了表示向下兼容。

## OpenSSH

OpenSSH（OpenBSD Secure Shell）是使用 SSH 透过计算机网络加密通信的实现。它是取代由 SSH Communications Security
所提供商用版本的开放源代码方案。

OpenSSH 是 SSH 协议最广泛的应用实现。由于客户端和服务器之间的连接相对简单，因此 OpenSSH 常使用密钥进行身份认证。

## SSH 远程登录指令

SSH 指令是 OpenSSH 套件的组成部分，是远程登录服务 SSH 的客户端程序，用于登录远程主机。

```shell
ssh [options] [-p port] [user@]hostname [command] 
```

`options` 选项说明：

```
-V：显示版本信息。

-l <login_name>：指定登录远程主机的用户。可以在配置文件中对每个主机单独设定这个参数。
-p <port>：指定远程主机的端口。可以在配置文件中对每个主机单独设定这个参数。
-i <identity_file>：手动指定一个认证所需的身份（私钥）文件。sshv1的默认文件是 ~/.ssh/identity，sshv2的默认文件是 ~/.ssh/id_rsa 和 ~/.ssh/id_dsa 文件。可以同时使用多个 -i 选项，也可以在配置文件中指定多个身份文件。
-F <config_file>：指定 ssh 指令的配置文件，将忽略系统级配置文件 /etc/ssh/ssh_config 和用户级配置文件 ~/.ssh/config。
-L <[bind_address:]port:host:hostport>：将本地主机的地址和端口接收到的数据通过安全通道转发给远程主机的地址和端口。
-R <[bind_address:]port:host:hostport>：将远程主机上的地址和端口接收的数据通过安全通道转发给本地主机的地址和端口。
-q：安静模式。消除大多数的警告和诊断信息。
-f：ssh 在执行命令前退至后台。
-o <option>：可以在这里给出某些选项，格式和配置文件中的格式一样。它用来设置那些没有单独的命令行标志的选项。

-1：强制只使用协议第一版。
-2：强制只使用协议第二版。
-4：强制只使用 IPv4 地址。
-6：强制只使用 IPv6 地址。

-A：允许转发认证代理的连接。可以在配置文件中对每个主机单独设定这个参数。
-a：禁止转发认证代理的连接。
-b <bind_address>：在拥有多个地址的本地机器上，指定连接的源地址。
-C：压缩所有数据。压缩算法与 gzip(1) 使用的相同
-c <cipher_spec>：指定一组用逗号隔开、按优先顺序排列的加密算法。
-D <[bind_address:]port>：指定一个本地主机动态的应用程序级的转发端口。工作原理是这样的，本地机器上分配了一个 socket 侦听 port 端口，一旦这个端口上有了连接，该连接就经过安全通道转发出去，根据应用程序的协议可以判断出远程主机将和哪里连接。目前支持 SOCKS4 和 SOCKS5 协议，而 ssh 将充当 SOCKS 服务器. 只有 root 才能转发特权端口。可以在配置文件中指定动态端口的转发。
-m <mac_spec>：对于sshv2，可以指定一组用逗号隔开，按优先顺序排列的 MAC (message authentication code) 算法。
-N：不执行远程命令。
-W <host:port>：将客户端上的标准输入和输出通过安全通道转发给指定主机的端口。
-w <local_tun[:remote_tun]>：指定客户端和服务器之间转发的隧道设备。
-X：允许 X11 转发，可以在配置文件中对每个主机单独设定这个参数
-x：禁止 X11 转发
-Y：启用受信任的 X11 转发。受信任的 X11 转发不受 X11 安全扩展控制的约束
```

`command` 选项说明：

该选项指定了登录到远程服务器主机系统后执行的一条命令。

### SSH 配置文件说明

通常，通过 SSH 连接到远程服务器时，需要指定远程用户名，主机名和端口等配置信息。

SSH 指令按照如下顺序来获取相关配置信息：

1. 命令行指定。
2. 用户配置文件 (`~/.ssh/config`)。
3. 系统级配置文件 (`/etc/ssh/ssh_config`)。

如果配置文件未指定，则端口默认为 `22`，用户名默认为当前用户。

## 首次连接

若客户端从未连接过目标主机，则目标主机的 SSH 密钥尚未受信，无法进行后续操作。

要解决这个问题，可以尝试以下方法：

### 通过手动验证

执行一次 SSH 连接到目标主机，并手动验证主机密钥。

第一次连接到目标主机时，SSH 会提示接受或拒绝主机密钥。可以通过确认主机密钥是否正确，来决定是否继续连接。

如果确认无误后继续连接，则目标主机的密钥将被添加到 `known_hosts` 文件中。

```
ssh user@xx.xx.xx.xx
The authenticity of host 'xx.xx.xx.xx(xx.xx.xx.xx)' can't be established.
ED25519 key fingerprint is XXXXXXXXXX.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'xx.xx.xx.xx' (ED25519) to the list of known hosts.
root@xx.xx.xx.xx's password: 
Linux VM-4-15-debian 5.10.0-19-amd64 #1 SMP Debian 5.10.149-2 (2022-10-21) x86_64
```

### 通过指令添加

使用 `ssh-keyscan` 命令扫描目标主机的 SSH 主机密钥，并将其添加到 `known_hosts` 文件中。

`ssh-keyscan` 是一个用于收集大量主机的 SSH 主机公钥的实用程序，用于帮助生成和验证 SSH `known_hosts` 文件。

`ssh-keyscan` 以并行方式联系尽可能多的主机，因此该实用程序非常高效。包括 1000 个主机的域中的密钥可以在几十秒内收集完毕，即使其中一些主机关闭或未运行
SSH。扫描时，不需要登录访问所扫描的计算机，扫描过程也不涉及加密。

```shell
ssh-keyscan [options] [hostname]
```

`options` 常用选项说明：

```
-H 对输出中的所有主机名和地址进行哈希处理。ssh 和 sshd 可以正常使用哈希后的名称。如果文件内容被泄露，不会泄露识别信息。
-p <port>：指定要连接的 SSH 端口，默认为 22。
-t <keytype>：指定要从被扫描的主机提取的密钥类型。type 的可能值包括用于协议版本 1 的 rsa1 和用于协议版本 2 的 rsa 或 dsa。可指定以逗号分隔的多个值。缺省值为 rsa。
-f <file>：从该文件中读取主机或地址列表名称列表对，每行读取一个。如果指定的不是文件名，ssh-keyscan 将从标准输入中读取主机或地址列表名称列表对。

-T <timeout> 设置连接尝试的超时时间。
-4 基于IPv4网络协议。
-6 基于IPv6网络协议。
-v 显示执行过程详细信息。
```

使用 `ssh-keyscan` 扫描目标主机并添加信任：

```shell
ssh-keyscan -H xx.xx.xx.xx >> ~/.ssh/known_hosts
```

```
# xx.xx.xx.xx:xx SSH-2.0-OpenSSH_9.2p1 Debian-2+deb12u2
# xx.xx.xx.xx:xx SSH-2.0-OpenSSH_9.2p1 Debian-2+deb12u2
# xx.xx.xx.xx:xx SSH-2.0-OpenSSH_9.2p1 Debian-2+deb12u2
# xx.xx.xx.xx:xx SSH-2.0-OpenSSH_9.2p1 Debian-2+deb12u2
# xx.xx.xx.xx:xx SSH-2.0-OpenSSH_9.2p1 Debian-2+deb12u2
```

## SSH 密钥登录

上文介绍的远程登录指令需要每次输入密码登录。鉴于每次输入密码并不方便，且密码的安全性直接关系到连接的安全，因此密钥登录是比密码登录更好的解决方案。

建立 SSH 密钥登录的原理是：

1. 客户端通过 `ssh-keygen` 生成自己的公私钥对。
2. 通过手动或通过 `ssh-copy-id` 命令自动将公钥追加到远程服务器 `authorized_key` 文件中。
3. 需要登录服务器时，客户端先向服务器发起 SSH 登录的请求。
4. 服务器检索 `authorized_key` 文件，确认该客户端的公钥是否存在。如果存在，则生成随机数 R，并用该公钥进行加密，并将密文传递给客户端。
5. 客户端使用私钥解密密文，得到原随机数 R。再使用私钥对随机数 R 等信息进行签名，并将签名发送给服务器。
6. 服务器使用该客户端的公钥解密，然后与原始数据进行对比，如果一致，则认证成功。此时不用输入密码，便建立了远程连接。

### 密钥对生成指令

```shell
ssh-keygen [options]
```

`options` 选项说明：

```
-t <dsa|ecdsa|ecdsa-sk|ed25519|ed25519-sk|rsa>：指定密钥的加密算法。
-C <comment>：添加注释，常用格式为 username@example.com。
-b <bits>：指定密钥长度。这个参数值越大，密钥就越不容易破解，但是加密解密的计算开销也会加大。一般来说，应该是1024，更安全一些可以设为2048或者更高。
-f <filename>：指定生成的密钥对的文件名。
-l：显示公钥文件的指纹数据。
-N <new_passphrase>：用于指定私钥的密码短语（passphrase）。
-p <passphrase>：用于重新指定私钥的密码短语（passphrase）。它与 -N 的不同之处在于，新密码不在命令中指定，而是执行后再输入。
-F <hostname|[hostname]:port>：检查某个主机名是否在 known_hosts 文件里面。
-R <hostname|[hostname]:port>：将指定的主机公钥移出 known_hosts 文件。
```

使用 `ssh-keygen` 命令，但又没有在命令中配置一些重要选项时，命令行会作一些简单的引导，来帮助用户对生成密钥的进行配置，如下：

```
Generating public/private rsa key pair.
Enter file in which to save the key (~/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in ~/.ssh/id_rsa
Your public key has been saved in ~/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:5VR8HyevbqJCxPDAtadvKPp/lp7r09iBb/G162cXKmo thelastcookies@icloud.com
The key's randomart image is:
+--[ED25519 256]--+
|     . ..  ..    |
|      +  . .. o..|
|       =. +  . +o|
|        +*      o|
|       .S ..   . |
|        .o. o ...|
|      ... o* =..o|
|     . ..E*o*.+.+|
+----[SHA256]-----+
```

上面示例中，出现的第一个问题，会询问密钥保存的文件名。

如果不作输入，则使用默认名称来命名公私钥文件。例如选择 `rsa` 算法，生成的密钥文件默认就会是 `id_rsa`（私钥）和 `id_rsa.pub`
（公钥），存放在 `~/.ssh/` 目录下。

接着，就会是第二个问题，询问是否要为私钥文件设定密钥密码（passphrase）。如果设定了密钥密码，即使私钥泄露，还可以提供一层保护。

如果为了方便，不想设定密钥密码，可以直接按回车键，密码就会为空。

> 注意，这里密钥密码的英文单词是 `passphrase`，这是为了避免与 Linux 账户的密码单词 `password` 混淆，表示这不是用户系统账户的密码。

最后，就会生成私钥和公钥，屏幕上还会给出公钥的指纹。

### 公钥上传

生成密钥以后，公钥必须上传到服务器，才能使用公钥登录。

OpenSSH 规定，在服务器的 `~/.ssh/authorized_keys` 文件中，保存每个客户端的公钥。
因此所谓公钥上传，就是将客户的公钥添加到 `authorized_keys` 文件中。

可以远程登录到服务器中，并把公钥追加到该文件末尾。如果文件不存在，则可以手动创建再追加公钥。即为**手动上传**。

> 注意，`authorized_keys` 文件要配置正确的权限，即文件所有者的读写权限。如果权限设置不对，SSH 服务器可能会无法读取该文件。
>
> `sudo chmod 600 ~/.ssh/authorized_keys`

同时，OpenSSH 自带一个 `ssh-copy-id` 命令，可以自动将公钥拷贝到远程服务器的 `~/.ssh/authorized_keys`
文件中。如果该文件不存在，`ssh-copy-id` 命令会自动创建。即为**自动上传**。

`ssh-copy-id` 命令也会给远程主机的用户主目录`~`、 `~/.ssh` 和 `~/.ssh/authorized_keys` 设置合适的权限。

```shell
ssh-copy-id -i [identity_file] [user@]host
```

上面命令中，`-i` 参数用来指定公钥文件，`user` 是登录到服务器账户名，`host` 是服务器地址。

如果省略用户名，默认为当前的本机用户名。执行完该命令，公钥就会拷贝到服务器。示例：

```shell
ssh-copy-id -i id_rsa user@host
```

上面命令中，公钥文件会自动匹配到 `~/.ssh/id_rsa.pub`。

> 注意，`ssh-copy-id` 会直接将公钥添加到 `authorized_keys` 文件的末尾。如果 `authorized_keys`
> 文件的末尾不是一个换行符，会导致新的公钥添加到前一个公钥的末尾，使得两个公钥都无法生效。所以，如果 `authorized_keys`
> 文件已经存在，使用 `ssh-copy-id` 命令之前，务必保证 `authorized_keys` 文件的末尾是换行符（假设该文件已经存在）。

### 设置 SSH 代理

SSH 代理是一个管理 SSH 私钥的程序，它可以安全地存储和管理 SSH 私钥，并在需要时提供给 SSH 客户端，提高 SSH 访问的便利性与安全性。

另外，SSH 代理也可以缓存密钥密码（passphrase），只需要在第一次使用 SSH 命令时输入密钥密码，后面都不需要再进行密钥密码验证。

#### agent 启动指令

```shell
ssh-agent [options]
```

`options` 选项说明：

```
-c：生成C-shell风格的命令输出。
-s：生成Bourne shell 风格的命令输出。
-d：调试模式。
-a <bind_address>：bind the agent to the UNIX-domain socket bind_address.
-t life：设置默认值添加到代理人的身份最大寿命。
-k：把ssh-agent进程杀掉。
```

#### 给 agent 添加私钥

```shell
ssh-add [options] [file ...]
```

`options` 选项说明：

```
-L：显示 ssh-agent 中缓存的公钥。
-l：显示 ssh-agent 中缓存的私钥。
-X：对 ssh-agent 进行解锁。
-x：对 ssh-agent 进行加锁。
-d <name_of_key_file>：从 ssh-agent 缓存中删除指定的私钥。
-D：从 ssh-agent 缓存中删除所有私钥。
-t <life>：对加载的密钥设置超时时间，超时 ssh-agent 将自动卸载密钥。
```

#### 设置代理的基本流程

首先启动代理服务，有下列几种方法：

- 使用 `ssh-agent`，新建一个命令行对话启动代理。
- 使用 `eval "$(ssh-agent -s)` 来在当前窗口的后台启动一个代理。
- *配置 [config](#config-文件) 文件。在配置了 Host 所使用的私钥文件路径后，使用 `ssh`
  指令连接远程主机时会静默地启动一个代理服务并读取私钥。这样也可以省去下面的步骤。

第二步，使用 `ssh-add` 命令添加私钥（比如`~/.ssh/id_rsa`，`~/.ssh/id_dsa`，`~/.ssh/id_ecdsa`，`~/.ssh/id_ed25519`
，或者其他具名指定密钥）。

添加私钥时，会要求输入密钥密码（passphrase）。此后，私钥已经被加载到内存，在当前对话里面再使用私钥，就不需要输入密钥密码了。

最后，如果要退出 `ssh-agent`，可以使用下面的命令。

```shell
ssh-agent -k
```

## `~/.ssh` 路径下各文件的说明

### `config` 文件

`config` 文件为 SSH 配置文件，采用以下结构：

```
Host hostname1
  SSH_OPTION value
  SSH_OPTION value

Host hostname2
  SSH_OPTION value

Host *
  SSH_OPTION value
```

配置项说明：

```
每项配置都是`参数名 参数值`或`参数值=参数名`两种形式，可以混用。

参数名不区分大小写，参数值区分大小写。

Host：定义主机别名，可以是主机名、IP 地址或模式匹配。
    可以使用通配符：`*` 代表0～n个非空白字符，`?` 代表一个非空白字符，`!` 表示例外通配。
HostName：指定远程主机的地址或主机名。
    可以直接使用数字 IP 地址。如果主机名中包含 ‘%h’ ，则实际使用时会被命令行中的主机名替换。
Port：指定远程主机端口号，默认为 22。
User：指定连接远程主机时使用的用户名。
IdentityFile：指定密钥认证使用的私钥文件路径。
    默认为 ~/.ssh/id_dsa, ~/.ssh/id_ecdsa, ~/.ssh/id_ed25519 或 ~/.ssh/id_rsa 中的一个。可以指定多个密钥文件，在连接的过程中会依次尝试这些密钥文件。
UserKnownHostsFile：指定一个或多个用户认证主机缓存文件，用来缓存通过认证的远程主机的密钥，多个文件用空格分隔。
    默认缓存文件为： ~/.ssh/known_hosts, ~/.ssh/known_hosts2。
GlobalKnownHostsFile：指定一个或多个全局认证主机缓存文件，用来缓存通过认证的远程主机的密钥，多个文件用空格分隔。
    默认缓存文件为：/etc/ssh/ssh_known_hosts, /etc/ssh/ssh_known_hosts2。
AddKeysToAgent：指定在使用 SSH 连接时，是否将使用的私钥添加到 SSH Agent 中。可以设置为 yes 或 no。
UseKeychain：*指定是否使用系统的密钥链来管理 SSH 私钥。
PubkeyAuthentication：指定是否启用公钥身份验证。可以设置为 yes 或 no。
PasswordAuthentication：指定是否启用密码身份验证。可以设置为 yes 或 no。
PreferredAuthentications：指定首选的身份验证方法。
```

> *在 macOS 系统上，当 UseKeychain 配置项设置为 yes 时，SSH 客户端会尝试将 SSH 私钥添加到系统的密钥链中，并从密钥链中获取
> SSH 私钥。这样可以实现在登录时自动解锁 SSH 私钥，而无需用户手动输入密码或密钥短语。

配置文件的简单示例：

```
# configuration 1
Host cluster
  HostName 192.168.11.11
  User tom
  IdentityFile ~/.ssh/id_rsa

# configuration 2
Host=aliyun
  Hostname=202.44.2.2
  User=tom
```

配置文件可以简化 SSH 命令，如上的配置文件只需执行指令 `ssh cluster` 就可以将配置项 `cluster`
中配置的地址，登录用户名和关联私钥应用到指令中，连接到服务器。

### `known_hosts` 文件

该文件保存了曾建立过 SSH 连接的服务器的信息，包括服务器的地址、公钥、加密方法。每连接一个新的服务器都会追加一条新的数据记录。

客户端第一次与服务器建立 SSH 连接的时候，会有一个提示：

```
The authenticity of host 'xxx' can't be established.
ED25519 key fingerprint is SHA256:rwoFnUCaDuUIr+v0mEE0xgsRpcYt7VZ4hHzVCSSA+mA.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])
```

表示当前服务器为未知服务器，并提供了该服务器的公钥指纹。当用户根据第三方途径确认公钥指纹吻合之后，可以选择继续连接。

同时，此服务器的信息就会被记录到本地的 `~/.ssh/known_hosts` 文件中（如果该文件不存在则会自动创建）。

### 成对出现的 `<example>` 与 `<example>.pub` 文件

`ssh-keygen` 生成的公私钥对，有 `.pub` 后缀的为公钥，没有后缀的为私钥。

`.pub` 文件结构：

```
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAIEAvpB4lUbAaEbh9u6HLig7amsfywD4fqSZq2ikACIUBn3GyRPfeF93l/weQh702ofXbDydZAKMcDvBJqRhUotQUwqV6HJxqoqPDlPGUUyo8RDIkLUIPRyqypZxmK9aCXokFiHoGCXfQ9imUP/w/jfqb9ByDtG97tUJF6nFMP5WzhM= username@example.com
```

上面的示例是一个 ras 类型的公钥，可以看到包含以下三个部分：

- 密钥对加密类型: 最常见的是 `ssh-rsa`，另外由于安全性目前更推荐使用 `ssh-ed25519`。
- 公钥的 Base64 字符串。
- 一个 Comment，通常包含这个 Key 的用途，或者 Key 所有者的邮箱地址。

### `authorized_keys` 文件

服务器端文件。保存客户端公钥，用于客户端的密钥登录验证。

详见：[公钥上传](#公钥上传)

## 示例一——从本地通过 SSH 密钥登录服务器

### `ssh-kengen` 生成密钥对

```shell
$ ssh-keygen -t ed25519 -f tlc_server  
Generating public/private ed25519 key pair.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in tlc_server
Your public key has been saved in tlc_server.pub
The key fingerprint is:
SHA256:gakuibK5b3kVaHppr1kRMPYLxiB69OqGv+B+xYHsdvg xinwentao@Xinwt-MBP
The key's randomart image is:
+--[ED25519 256]--+
|. o +            |
|.o = + o         |
|. o *.= .        |
| . =o+.o .       |
|  oo+.+.S        |
| +.*++..         |
|= *+=o.          |
|o*oo.E.          |
|=*=oo.           |
+----[SHA256]-----+
```

### 查看 `tlc_server.pub` 公钥

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICkcZ/JzICCRJGFztcfHQehcCOa2dQ722GTDYmKNmqDk xinwentao@Xinwt-MBP
```

### SSH 登录测试远程服务器连接

```shell
$ ssh root@124.222.xxx.xxx
The authenticity of host '124.222.xxx.xxx (124.222.xxx.xxx)' can't be established.
ED25519 key fingerprint is SHA256:rwoFnUCaDuUIr+v0mEE0xgsRpcYt7VZ4hHzVCSSA+mA.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '124.222.xxx.xxx' (ED25519) to the list of known hosts.
root@124.222.xxx.xxx's password: 
Linux VM-4-15-debian 5.10.0-19-amd64 #1 SMP Debian 5.10.149-2 (2022-10-21) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri May  5 21:33:06 2023 from 49.77.xxx.xxx
root@VM-4-15-debian:~# 
```

### 在本地进行公钥上传

```shell
$ ssh-copy-id -i tlc_server.pub root@124.222.xxx.xxx
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "tlc_server.pub"
sed: RE error: illegal byte sequence
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@124.222.xxx.xxx's password: 

Number of key(s) added:        1

Now try logging into the machine, with:   "ssh 'root@124.222.xxx.xxx'"
and check to make sure that only the key(s) you wanted were added.
```

### 在服务器查看上传结果

```shell
vim ~/.ssh/authorized_keys 
```

可以看到在 `authorized_keys` 文件中，已经保存了刚才在本地生成的公钥，上传成功。

### 设置本地 `config` 配置文件

```shell
vim ~/.ssh/config
```

在配置文件中配置：

```
Host tlc-server
  HostName 124.222.xxx.xxx
  User root
  IdentityFile ~/.ssh/tlc_server
```

设置了该服务器的别名，以及登录用户。使用别名可以更方便命令行登录。

保存后，在本地测试密钥登录：

```shell
$ ssh tlc-server
Linux VM-4-15-debian 5.10.0-19-amd64 #1 SMP Debian 5.10.149-2 (2022-10-21) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Thu Jun  1 15:12:29 2023 from 49.77.xxx.xxx
```

没有输入密码的步骤，直接登录了服务器，SSH 密钥登录成功。

## 示例二——使用 SSH 密钥建立与 github.com 的免密安全连接

### 生成新 SSH 密钥

按照 [Github 文档](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
的建议在终端输入：

```shell
$ ssh-keygen -t ed25519 -C "thelastcookies@icloud.com"
Generating public/private ed25519 key pair.
Enter file in which to save the key (~/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in ~/.ssh/id_ed25519
Your public key has been saved in ~/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:5VR8HyevbqJCxPDAtadvKPp/lp7r09iBb/G162cXKmo thelastcookies@icloud.com
The key's randomart image is:
+--[ED25519 256]--+
|     . ..  ..    |
|      +  . .. o..|
|       =. +  . +o|
|        +*      o|
|       .S ..   . |
|        .o. o ...|
|      ... o* =..o|
|     . ..E*o*.+.+|
+----[SHA256]-----+
```

### 将 SSH 密钥添加到 ssh-agent

在后台启动 ssh 代理。

```shell
eval "$(ssh-agent -s)"
```

```
Agent pid 84568
```

打开 `~/.ssh/config` 文件，添加 Github 配置：

```
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

将 SSH 私钥添加到 `ssh-agent` 并将密码存储在密钥链中。

```shell
$ ssh-add --apple-use-keychain ~/.ssh/id_ed25519
Identity added: ~/.ssh/id_ed25519 (thelastcookies@icloud.com)
```

### 向 Github 账户添加新的 SSH 公钥

- 将 `~/.ssh/id_ed25519.pub` 的公钥内容复制到剪切板。
- 在 github.com 中点击个人头像，进入 `Settings`。
- 在左侧边栏中单击 `SSH 和 GPG keys`。
- 点击 `New SSH Key` 按钮。
- 在 `Title` 字段中，为新密钥添加描述性标签。
- 在 `Key` 字段中，粘贴刚才复制的公钥。
- 单击 `Add SSH key` 按钮。

### 测试 SSH 连接

```shell
$ ssh -T git@github.com
The authenticity of host 'github.com (140.82.112.3)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
Hi thelastcookies! You've successfully authenticated, but GitHub does not provide shell access.
```

### 在命令行拉取代码

```shell
$ git pull
Updating 0d01e9e..43a9dc1
Fast-forward
 general_knowledge/xor.md                                  | 84 ++++++++++++++++++++++++
 .../stable_diffusion/01) sd_env.md                        |  0
 static/images/general_knowledge/xor__banner.svg           | 87 +++++++++++++++++++++++++
 3 files changed, 171 insertions(+)
 create mode 100644 general_knowledge/xor.md
 rename {machine_learning_&_ai => machine_learning&ai}/stable_diffusion/01) sd_env.md (100%)
 create mode 100644 static/images/general_knowledge/xor__banner.svg
```

参考资料：

> https://wangchujiang.com/linux-command/c/ssh.html
>
> https://wangdoc.com/ssh/key
