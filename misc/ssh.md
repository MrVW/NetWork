SSH
=======

大家都会用SSH远程连远程机器。都知道SSH默认端口22.

比较少的人用过

SSH 远程连另外一台Windows电脑的里的Vitrualbox/WMware虚拟机里的Ubuntu系统，或者是WSL，或者是连另外一台电脑的运行的Docker容器里的Ubuntu系统。

这其中的原理操作，两种
	
	1.修改SSH默认的端口，SSH不再使用默认端口22

	2.端口转发。

前者需要修改系统里ssh的默认配置文件ssh_config，后者需要开启端口映射功能，看情况在Windows或者Ubuntu系统里配置。远程连接一台Window机器，还需要在Windows系统里配置入口和出可规则。



虚拟机里的网卡，如果是NAT，可以用端口映射访问，否则远程连过去，目标Ip就是Host的IP， 如果采用桥接网卡，目标IP是桥接网卡的IP.



实验楼提供了一种SSH远程直连，会员才能享受。
