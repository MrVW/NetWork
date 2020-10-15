名称
systemd-networkd.service, systemd-networkd — 网络管理器

大纲
systemd-networkd.service

/usr/lib/systemd/systemd-networkd

描述
systemd-networkd 是用于管理网络的系统服务。 它能够检测并配置网络连接， 也能够创建虚拟网络设备。

若要配置独立于网络的低级别物理连接， 参见 systemd.link(5) 手册。

systemd-networkd 基于 systemd.netdev(5) 文件中的 [Match] 小节，创建虚拟网络设备。

systemd-networkd 基于 systemd.network(5) 文件中的 [Match] 小节， 配置所有匹配的网络连接的地址与路由。 在启动匹配的网络连接时，会首先清空该连接原有的地址与路由。 所有未被 .network 文件匹配到的网络连接都将被忽略(不对其做任何操作)。 同时，systemd-networkd 还会忽略那些在 systemd.network(5) 文件中明确设为 Unmanaged=yes 的网络连接。

当 systemd-networkd 服务退出时， 通常不做任何操作，以保持当时已经存在的网络设备与网络配置不变。 一方面，这意味着，从 initramfs 切换到实际根文件系统以及重启该网络服务都不会导致网络连接中断。 另一方面，这也意味着，更新网络配置文件并重启 systemd-networkd 服务之后， 那些在更新后的网络配置文件中已经被删除的虚拟网络设备(netdev)仍将存在于系统中， 有可能需要手动删除。

配置文件
该服务的配置文件 分别位于： 优先级最低的 /usr/lib/systemd/network 目录、 优先级居中的 /run/systemd/network 目录、 优先级最高的 /etc/systemd/network 目录。

网络使用 .network 文件进行配置(详见 systemd.network(5))， 虚拟网络设备使用 .netdev 文件进行配置(详见 systemd.netdev(5))。


