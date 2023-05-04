# 📦 apt - Advanced Package Manager in Debian

## 什么是 apt ？

`apt`是一个命令行实用程序，用于在 Ubuntu、Debian 和相关 Linux 发行版上安装、更新、删除和管理`deb`软件包及依赖。

## 通用指令格式：
```
$ apt [options] [command] [package ...]
```

- options：可选，选项包括 -h（帮助），-y（当安装过程提示选择全部为"yes"），-q（不显示安装的过程）等等。
- command：要进行的操作。
- package：安装的包名。

## 指令：
#### `$ apt install <package_name>` 安装指定包。
- `--no-upgrade` 安装一个包，但如果包已经存在，则不要升级它。
- `<package_name>=<version_number>` 安装指定版本的包。

#### `$ apt remove <package_name>` 移除指定包。
#### `$ apt purge <package_name>` 移除指定包及配置文件。
#### `$ apt update` 更新 Linux 系统的包索引或包列表，它不会升级任何软件包。
  > `/etc/apt/sources.list`包索引文件是一个文件或数据库，
  > 其中包含在位于该文件的存储库中定义的软件包列表。
  > 其他软件包列表位于`/etc/apt/sources.list.d`目录中。
  
#### `$ apt update <package_name>` 更新指定包的索引。

#### `$ apt upgrade` 升级所有包，如果出现依赖性问题，则不会升级。
#### `$ apt full-upgrade` 破坏性地升级所有包，会删除旧包。使用此命令时要格外小心。
#### `$ apt upgrade <package_name>` 升级指定包。

#### `$ apt autoremove` 移除不再被任何包所依赖的包。
#### `$ apt show <package_name>`显示包具体信息，例如：版本号，安装大小，依赖关系，软件包源等等。
#### `$ apt list` 列出所有可用包。
- `--installed` 列出已安装的包。
- `--upgradeable` 列出可升级的包。
- `--all-versions` 列出包的版本信息。

#### `$ apt search <package_name>` 搜索指定包。


## dpkg、apt-get 与 apt

- dpkg: 用来安装.deb文件时，不会解决模块的依赖关系，且不会关心ubuntu的软件仓库内的软件，可以用于安装本地的deb文件。

- apt-get: 会解决和安装模块的依赖问题，并会咨询软件仓库，但不会安装本地的deb文件，apt-get是建立在dpkg之上的软件管理工具。

- apt：是对之前的 apt-get apt-cache 等指令的封装，提供更加统一，更加适合终端用户使用的接口。

- dkpg（底层工具）-> apt-get（上层工具）-> apt（apt-get的再封装）


## 参考资料：
> https://www.runoob.com/linux/linux-comm-apt.html
> 
> https://zhuanlan.zhihu.com/p/528353275
> 
> https://www.yisu.com/zixun/677410.html
