# Mysql 部署

## 设置允许外部访问

### 登陆mysql数据库

```
mysql -u root -p
```

查看 `user` 表

```
mysql> use mysql;
Database changed
mysql> select host,user,password from user;
+————–+——+——————————————-+
| host         | user | password                                  |
+————–+——+——————————————-+
| localhost    | root | *A731AEBFB621E354CD41BAF207D884A609E81F5E |
| 192.168.1.1  | root | *A731AEBFB621E354CD41BAF207D884A609E81F5E |
+————–+——+——————————————-+
2 rows in set (0.00 sec)
```

可以看到在 `user` 表中已创建的 `root` 用户。

`host` 字段表示登录的主机，其值可以用 `IP`，也可用主机名，

### 实现远程连接（授权法）

将 `host` 字段的值改为 `%` 就表示在任何客户端机器上能以 `root` 用户登录到 `mysql` 服务器，建议在开发时设为 `%` 。

```shell
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY <PASSRORD> WITH GRANT OPTION;
```

其中 `*.*` 代表所有资源所有权限，`'root'@'%'` 中的 `root` 代表用户名，`%` 代表所有的访问地址。IDENTIFIED BY `<PASSRORD>`
，这里提供数据库用户的密码，`WITH GRANT OPTION` 表示允许级联授权。

将权限改为 `ALL PRIVILEGES`，这样机器就可以以用户名，密码的形式远程访问该机器上的 `MySql`。

因此，以上语句的含义表示为：

允许任何 ip 地址的客户端使用 `root` 用户和 `<PASSRORD>` 密码来访问这个 MySQL 服务。

刷新权限设置

```
flush privileges;
```

### 实现远程连接（改表法，不推荐）

```shell
use mysql;

update user set host = '%' where user = 'root';
```

这样在远端就可以通过 `root` 用户访问 Mysql.

### 配置 MySQL

`MySQL` 配置文件 `my.conf` 的位置因 Linux 发行版而异。

常见的位置有：

```
/etc/my.cnf
/etc/mysql/my.cnf
/usr/local/mysql/etc/my.cnf
/etc/mysql/mysql.conf.d/mysqld.cnf
```

常见的参数定义

```
[mysqld] 
# MySQL 服务器设置
port = 3306                     # MySQL 默认端口
bind-address = 0.0.0.0          # 绑定地址（本地或远程访问）
datadir = /var/lib/mysql        # 数据库文件存放路径
socket = /var/run/mysqld/mysqld.sock  # Socket 文件路径
log-error = /var/log/mysql/error.log  # 错误日志文件

# 性能优化
max_connections = 200           # 最大连接数
key_buffer_size = 16M           # 索引缓冲区大小
query_cache_size = 64M          # 查询缓存大小

# 字符集设置
character-set-server = utf8mb4  # 服务器字符集
collation-server = utf8mb4_general_ci  # 字符集排序规则

[client]
# 客户端设置
port = 3306
socket = /var/run/mysqld/mysqld.sock
default-character-set = utf8mb4

[mysqldump]
# 数据库备份工具设置
quick
max_allowed_packet = 16M
```
