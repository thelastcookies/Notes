# Linux 下的 Java 环境搭建（以 Debian 为例）

## Java 安装包获取

### 在线安装

使用包管理器下载安装包

```shell
sudo apt update
sudo apt install default-jdk
```

### 离线安装：

在没有网络的设备需要完成以下步骤：

1. 在联网环境中到 Oracle 官方下载页面 或 OpenJDK 官网下载适合系统的 JDK tar.gz 安装包（适用于所有 Linux 发行版），如：
   `jdk-x_linux-x64_bin.tar.gz`

2. 拷贝到目标服务器，使用 U 盘或通过内网将下载的文件拷贝到无法联网的目标服务器。

3. 解压文件：

  ```shell
  sudo tar -zxvf jdk-x_linux-x64_bin.tar.gz -C [Java安装目录]
  ```

## 配置环境变量

检查 Java 环境变量：

```shell
echo $JAVA_HOME
```

如果没有设置，则需要将环境变量手动添加到配置文件，如 `/etc/profile` 中：

```
export JAVA_HOME=[Java安装目录]
export PATH=$JAVA_HOME/bin:$PATH
```

保存后，执行以下命令使其生效：

```shell
source /etc/profile
```

## 验证

```shell
java -version
```

如果有版本信息输出，则部署成功。

## 修改文件夹权限（可选）：

如果在执行 Java 命令时提示权限问题，则可能是系统无权限执行相关文件。

修改相关文件权限

```shell
sudo chmod -R 755 [Java安装目录]
```

