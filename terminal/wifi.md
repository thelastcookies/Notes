# Wi-Fi

以下命令key在 Linux 设备下查看当前连接的 Wi-Fi 网络情况。

## `iwconfig` 命令

`iwconfig` 命令可以用来查看无线网络接口的信息，包括连接状态、SSID、信号强度等等。

```shell
iwconfig
```

## `nmcli` 命令

`nmcli` 是 NetworkManager 的命令行工具，可以用来管理网络连接。可以使用 `nmcli` 来查看当前的 Wi-Fi 连接情况。

```
nmcli device wifi list
```

这个命令会列出可用的 Wi-Fi 网络，包括它们的 SSID、信号强度、频道、加密方式等信息。

## `iwlist` 命令

iwlist 命令可以扫描无线网络并显示相关信息。在终端中运行以下命令：

```shell
sudo iwlist scanning
```

## 使用 iw 命令：

```shell
iw dev <interface> link
```

其中 `<interface>` 是网络接口名称，可以是 `wlan0`、`wlan1` 等。
指令会显示特定网络接口的连接信息，包括连接的 Wi-Fi 网络名称（SSID）、BSSID（路由器的 MAC 地址）、信号强度等。

要在 Linux 中连接或断开 Wi-Fi，可以使用以下命令：

连接到 Wi-Fi 网络：

```shell
nmcli device Wi-Fi connect <SSID> password <password>
```

其中 `<SSID>` 是要连接的 Wi-Fi 网络的名称，`<password>` 是连接所需的密码。

断开当前连接的 Wi-Fi：

```shell
nmcli device disconnect wlan0
```

其中 `wlan0` 是当前连接的 Wi-Fi 网络的接口名称。

注意：使用 `nmcli` 命令连接 Wi-Fi 时，需要 NetworkManager 服务处于运行状态。
