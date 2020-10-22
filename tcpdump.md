tcpdump 用来监听网络接口所有经过的数据包。

Usage: tcpdump [-aAbdDefhHIJKlLnNOpqStuUvxX#] [ -B size ] [ -c count ]
                [ -C file_size ] [ -E algo:secret ] [ -F file ] [ -G seconds ]
                [ -i interface ] [ -j tstamptype ] [ -M secret ] [ --number ]
                [ -Q in|out|inout ]
                [ -r file ] [ -s snaplen ] [ --time-stamp-precision precision ]
                [ --immediate-mode ] [ -T type ] [ --version ] [ -V file ]
                [ -w file ] [ -W filecount ] [ -y datalinktype ] [ -z postrotate-command ]
                [ -Z user ] [ expression ]

Print a description of the contents of packets on a network interface that match the boolean expression; the description is preceded by a time stamp, printed, by default, as hours, minutes, seconds, and fractions of a second since midnight.  It can also  be  run with  the  -w flag, which causes it to save the packet data to a file for later analysis, and/or with the -r flag, which causes it to read from a saved packet file rather than to read packets from a network interface.  It can also be run with the -V flag, which  causes  it  to read a list of saved packet files. In all cases, only packets that match expression will be processed by tcpdump.


tcpdump 命令的详细介绍可以man  tcpdump查询。实际上这个命令拿来抓网络包（wireshark可以打开）和usb log。
tcpdump 抓的数据包可以是二进制/十六进制字符串形式的（方便直接查看），也可以是二进制的（用wireshark、sniffer打开）。

---

### 示例1、从特定接口捕获数据包

当我们在没用任何选项的情况下运行 tcpdump 命令时，它将捕获所有接口上的数据包，因此，要从特定接口捕获数据包，请使用选项 -i，后跟接口名称。

语法：

	tcpdump -i {接口名}

假设我想从接口 enp0s3 捕获数据包。

输出将如下所示：

```

tcpdump: verbose output suppressed, use -v or -vv for full protocol decode

listening on enp0s3, link-type EN10MB (Ethernet), capture size 262144 bytes

06:43:22.905890 IP compute-0-1.example.com.ssh > 169.144.0.1.39374: Flags [P.], seq 21952160:21952540, ack 13537, win 291, options [nop,nop,TS val 26164373 ecr 6580205], length 380

06:43:22.906045 IP compute-0-1.example.com.ssh > 169.144.0.1.39374: Flags [P.], seq 21952540:21952760, ack 13537, win 291, options [nop,nop,TS val 26164373 ecr 6580205], length 220

06:43:22.906150 IP compute-0-1.example.com.ssh > 169.144.0.1.39374: Flags [P.], seq 21952760:21952980, ack 13537, win 291, options [nop,nop,TS val 26164373 ecr 6580205], length 220

06:43:22.906291 IP 169.144.0.1.39374 > compute-0-1.example.com.ssh: Flags [.], ack 21952980, win 13094, options [nop,nop,TS val 6580205 ecr 26164373], length 0

06:43:22.906303 IP 169.144.0.1.39374 > compute-0-1.example.com.ssh: Flags [P.], seq 13537:13609, ack 21952980, win 13094, options [nop,nop,TS val 6580205 ecr 26164373], length 72

06:43:22.906322 IP compute-0-1.example.com.ssh > 169.144.0.1.39374: Flags [P.], seq 21952980:21953200, ack 13537, win 291, options [nop,nop,TS val 26164373 ecr 6580205], length 220

^C

109930 packets captured

110065 packets received by filter

133 packets dropped by kernel

[[email protected] ~]#

```

### 示例2、从特定接口捕获特定数量数据包

假设我们想从特定接口（如 enp0s3）捕获 12 个数据包，这可以使用选项 -c {数量} -I {接口名称} 轻松实现。

root@compute-0-1 ~]# tcpdump -c 12 -i enp0s3

### 示例3、显示 tcpdump 的所有可用接口

使用 -D 选项显示 tcpdump 命令的所有可用接口：


```

[root@compute-0-1 ~]# tcpdump -D

1.enp0s3

2.enp0s8

3.ovs-system

4.br-int

5.br-tun

6.nflog (Linux netfilter log (NFLOG) interface)

7.nfqueue (Linux netfilter queue (NFQUEUE) interface)

8.usbmon1 (USB bus number 1)

9.usbmon2 (USB bus number 2)

10.qbra692e993-28

11.qvoa692e993-28

12.qvba692e993-28

13.tapa692e993-28

14.vxlan_sys_4789

15.any (Pseudo-device that captures on all interfaces)

16.lo [Loopback]

[[email protected] ~]#

```


我正在我的一个 openstack 计算节点上运行 tcpdump 命令，这就是为什么在输出中你会看到数字接口、标签接口、网桥和 vxlan 接口。



### 示例4、捕获带有可读时间戳的数据包（-tttt 选项）

 默认情况下，在 tcpdump 命令输出中，不显示可读性好的时间戳，如果您想将可读性好的时间戳与每个捕获的数据包相关联，那么使用 -tttt 选项，示例如下所示：

```

[[email protected] ~]# tcpdump -c 8 -tttt -i enp0s3

tcpdump: verbose output suppressed, use -v or -vv for full protocol decode

listening on enp0s3, link-type EN10MB (Ethernet), capture size 262144 bytes

2018-09-26 10:23:36.954883 IP compute-0-1.example.com.ssh > 169.144.0.1.39406: Flags [P.], seq 1449206247:1449206435, ack 3062020950, win 291, options [nop,nop,TS val 86178422 ecr 21583714], length 188

2018-09-26 10:23:36.955046 IP 169.144.0.1.39406 > compute-0-1.example.com.ssh: Flags [.], ack 188, win 13585, options [nop,nop,TS val 21583717 ecr 86178422], length 0

2018-09-26 10:23:37.140097 IP controller0.example.com.amqp > compute-0-1.example.com.57818: Flags [P.], seq 814607956:814607964, ack 2387094506, win 252, options [nop,nop,TS val 86172228 ecr 86176695], length 8

2018-09-26 10:23:37.140175 IP compute-0-1.example.com.57818 > controller0.example.com.amqp: Flags [.], ack 8, win 237, options [nop,nop,TS val 86178607 ecr 86172228], length 0

2018-09-26 10:23:37.355238 IP compute-0-1.example.com.57836 > controller0.example.com.amqp: Flags [P.], seq 1080415080:1080417400, ack 1690909362, win 237, options [nop,nop,TS val 86178822 ecr 86163054], length 2320

2018-09-26 10:23:37.357119 IP controller0.example.com.amqp > compute-0-1.example.com.57836: Flags [.], ack 2320, win 1432, options [nop,nop,TS val 86172448 ecr 86178822], length 0

2018-09-26 10:23:37.357545 IP controller0.example.com.amqp > compute-0-1.example.com.57836: Flags [P.], seq 1:22, ack 2320, win 1432, options [nop,nop,TS val 86172449 ecr 86178822], length 21

2018-09-26 10:23:37.357572 IP compute-0-1.example.com.57836 > controller0.example.com.amqp: Flags [.], ack 22, win 237, options [nop,nop,TS val 86178825 ecr 86172449], length 0

8 packets captured

134 packets received by filter

69 packets dropped by kernel

[[email protected] ~]#

```

### 示例5、捕获数据包并将其保存到文件（-w 选项）

使用 tcpdump 命令中的 -w 选项将捕获的 TCP/IP 数据包保存到一个文件中，以便我们可以在将来分析这些数据包以供进一步分析。
