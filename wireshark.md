Wireshark 简介

Wireshark 是自由开源的、跨平台的基于 GUI 的网络数据包分析器，可用于 Linux、Windows、MacOS、Solaris 等。它可以实时捕获网络数据包，并以人性化的格式呈现。Wireshark 允许我们监控网络数据包直到其微观层面。Wireshark 还有一个名为 tshark 的命令行实用程序，它与 Wireshark 执行相同的功能，但它是通过终端而不是 GUI。

Wireshark 可用于网络故障排除、分析、软件和通信协议开发以及用于教育目的。Wireshark 使用 pcap 库来捕获网络数据包。

Wireshark 具有许多功能：

支持数百项协议检查

能够实时捕获数据包并保存，以便以后进行离线分析

许多用于分析数据的过滤器

捕获的数据可以即时压缩和解压缩

支持各种文件格式的数据分析，输出也可以保存为 XML、CSV 和纯文本格式

数据可以从以太网、wifi、蓝牙、USB、帧中继、令牌环等多个接口中捕获

 

在 Ubuntu 16.04/17.10 上安装 Wireshark 的方法

Wireshark 在 Ubuntu 默认仓库中可用，只需使用以下命令即可安装。但有可能得不到最新版本的 wireshark：

linuxyw@nixworld:~$ sudo apt-get update

linuxyw@nixworld:~$ sudo apt-get install wireshark -y

因此，要安装最新版本的 wireshark，我们必须启用或配置官方 wireshark 仓库。

使用下面的命令来配置仓库并安装最新版本的 wireshark 实用程序：

linuxyw@nixworld:~$ sudo add-apt-repository ppa:wireshark-dev/stable

linuxyw@nixworld:~$ sudo apt-get update

linuxyw@nixworld:~$ sudo apt-get install wireshark -y

一旦安装了 wireshark，执行以下命令，以便非 root 用户也可以捕获接口的实时数据包。

linuxyw@nixworld:~$ sudo setcap 'CAP_NET_RAW+eip CAP_NET_ADMIN+eip' /usr/bin/dumpcap


---

关于Wireshark的使用

略

to be done

总之，这是一个可以用GUI操作的，可以根据 IP 地址、端口号，也可以使用来源和目标过滤器、数据包大小等对数据进行排序和过滤，也可以将两个或多个过滤器组合在一起以创建更全面的搜索。


操作和Windows上Wireshark相同。

----




