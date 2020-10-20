### 关于反向路由技术






有一台双网卡的机器，上面装有Fedora8，运行一个程序。该程序分别在两个网口上都接收多播数据，程序运行是正常的。但是，后来升级系统到Fedora13，发现就出问题了：在运行几秒钟后，第2个网口上就接收不到多播数据了。
   能不能收到多播，取决于交换机是不是往这个网口上转发多播数据。程序在起动的时候，会发一个IGMP的AddMembership的消息，交换机将把这个网口加入多播组。当在其他网口上收到该地址的多播包后，会转至这个网口。其后，为了确认该接收者一直在线，交换机会发送一个IGMPQuery消息，接收者反馈一个IGMP Report消息，以确认自己的存在。如果交换机没有收到IGMPReport，则认为该接收者已经断线，就不再往该网口上转发多播包了。

    用抓包工具定位了一下，发现程序在启动时确实发了AddMembership消息，这是正常的。在接收下来的5秒时间内，程序能够收到多播数据。接着，交换机发来了一条IGMPQuery，问题来了，这个Fedora13系统却没有反馈Report。这是很奇怪的。按理说，IGMP属于系统自动完成的协议，无需用户干预；那么按照预期，Linux会自动反馈IGMPReport的。事实上，Feodra8和WinXP系统就是这么做的，都很正常。为什么到了Fedora13反而不正常了呢？

    在调查为什么不反馈IGMPReport的事情上，花了一周时间都没有进展，后来发现其实不至Fedora13，其他的主流linux如Ubuntu10,SUSE14也存在同样的问题。

   查了众多论坛都没有一点提示信息。后来，终于在一个英文网站上扫到了一个信息:rp_filter。后来证明，这个关键词是解决问题的关键。reverse-pathfiltering，反向过滤技术，系统在接收到一个IP包后，检查该IP是不是合乎要求，不合要求的IP包会被系统丢弃。该技术就称为rpfilter。怎么样的包才算不合要求呢？例如，用户在A网口上收到一个IP包，检查其IP为B。然后考查：对于B这个IP，在发送时应该用哪个网口，如果在不应该接收到该包的网口上接收到该IP包，则认为该IP包是hacker行为。

例如：

A: 192.168.8.100

B: (IGMP Query) 10.0.0.1 来自路由器

查找路由表

网卡1为默认路由: 172.17.5.100  172.17.5.1

网卡2          192.168.8.100  192.168.8.1

系统根据路由表，认为10.0.0.1这个IP应该在第一个网卡172.17.5.100上收到，现实的情况是在第二张网卡192.168.8.100上收到了。认为这是不合理的，丢弃该包。致命的问题的，该包是来自路由器的IGMPQuery包。

The rp_filter can reject incoming packets if their sourceaddress doesnt match the network interface that theyre arrivingon, which helps to prevent IP spoofing. Turning this on, however,has its consequences: If your host has several IP addresses ondifferent interfaces, or if your single interface has multiple IPaddresses on it, youll find that your kernel may end up rejectingvalid traffic. Its also important to note that even if you do notenable the rp_filter, protection against broadcast spoofing isalways on. Also, the protection it provides is only against spoofedinternal addresses; external addresses can still be spoofed.. Bydefault, it is disabled.

 

解决方法：

系统配置文件
1. /etc/sysctl.conf
把 net.ipv4.conf.all.rp_filter和net.ipv4.conf.default.rp_filter设为0即可
net.ipv4.conf.default.rp_filter = 0
net.ipv4.conf.all.rp_filter = 0

net.ipv4.conf.eth0.rp_filter = 0
系统启动后，会自动加载这个配置文件，内核会使用这个变量

2. 命令行
显示一个内核变量 sysctl net.ipv4.conf.all.rp_filter
设置一个内核变量 sysctl -w net.ipv4.conf.all.rp_filter=0
设置完后，会更新内核（实时的内存）中的变量的值，但不会修改sysctl.conf的值

3. 使用/proc文件系统
查看 cat /proc/sys/net/ipv4/conf/all/rp_filter
设置 echo "0">/proc/sys/net/ipv4/conf/all/rp_filter
