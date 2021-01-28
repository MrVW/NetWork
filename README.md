NetWork Topics
======

人生，少一点抱怨，多一点努力和坚持！
<br>
与其抱怨，不如去选择！
<br>

[https://mrvw.github.io/NetWork/#/](https://mrvw.github.io/NetWork/#/)

随着移动蜂窝网络的发展，我们对数据连接的需求也在增加。电话、短信(CS业务)等任务已经不那么重要，客户更关心的，是网络数据连接需求。Quectel模组，从2G到5G，上下行速率也随着核心网的升级不断发展。其中使用Quectel模组，尤其是标准模组，是希望将Quectel模组作为一个网络接入方式，在客户的AP上，Quectel模组承担的是移动网络接入的功能。客户AP的外部网络连接走的就是Quectel模组。客户的AP上也有各种网络接口设备，如以太网卡、WIFI、虚拟网卡、网桥等。Quectel模组生成的网卡不是唯一网卡节点,因此为了满足客户产品应用的需求，就需要配置网络。



### 客户的网络需求

客户的系统分为两大类

- 桌面OS或者服务器OS

- 嵌入式系统

其中，桌面/服务器系统主要是Ubuntu/Debian/Centos这些及其衍生，这类的特点是系统Framework里有一套网络管理机制.

譬如 ![nmcli1](rc/devices1.png)

其中usb0 是一个移远模组GobiNet生成的网卡。可见，usb0 被加进NetWorkManager里了。因此usb0 也会被NetWorkManager 统一管理。

这类系统包括 树莓派自带的操作系统，它也是基于Debian的。

嵌入式系统，主要指的是客户自行编译的，bootloader、kernel、rootfs都有源码，尤其是指rootfs是用Buildroot编译得到的，基于emdebian编译得到的rootfs更接近桌面/服务器OS。嵌入式系统也可以根据rootfs划分成上层有明确网络管理的和没有确定的网络管理的网络管理的。

有明确网络管理的，指客户移植了一些网络管理应用，譬如客户移植了modemmanager、libqmi等，也包括OpenWrt系统。OpenWrt的网络配置在互联网搜索引擎上能找到很多资料，也有强大的社区服务支持。

嵌入式系统的特点是，经过裁剪后的系统，客户可能连内核配置了哪些内核模块、网络协议栈都不清楚，许多网络相关的工具都没有，譬如没有dhcp client工具，没有tcpdump，vim不可用，dos2unix也没有，系统里的可用空间也也很小，需要挂载nfs.

以下主要针对以上两类系统进行学习。

## 基础命令

----

### 路由

[gateway](gateway)

[route](route)


### 其他

[ss](ss)


## NetWorkManager

### nmcli

[nmcli](nmcli)

### nmtui

[nmtui](nmtui)

### systemd-network

[systemd-network](systemd-network)

[systemd-network](systemd-network2)


