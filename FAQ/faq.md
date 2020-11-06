
### ping 不通网络的情况

	1. 使用ipconfig /all观察本地网络设置是否正确；
	2. Ping 127.0.0.1，127.0.0.1回送地址Ping回送地址是为了检查本地的TCP/IP协议有没有设置好	3. Ping本机IP地址，这样是为了检查本机的IP地址是否设置有误；
	4.Ping本网网关或本网IP地址，这样的是为了检查硬件设备是否有问题，也可以检查本机与本地网络连接是否正常；（在非局域网中这一步骤可以忽略）
	
	针对Quectel模组，还要判断模组的问题。包括
	
	1.	AT+QPING 是否能ping通；
	2. AT+CGCONTRDP 看到模组的IP、NetMask、GateWay、DNS等信息，对非路由模式，这些信息和HostAP上的信息应该一致。注意是否出现HostAP上的IP被固定了。AT+CFUN切换几次，尝试模组换换IP看看；
	3. 检查路由和网关。

### 多个默认网关

linux主机在多个子网上，Linux 内核虽然支持多个默认网关，但它只会使用 metric 值最低的那个。

![default route](rc/route/默认路由.png)

metric 度量值两种方法
    度量值也就是优先级 如果有多个到相同的目的地址条目 那度量值越低越优先
    1. 如ROUTE DELELTE 0.0.0.0，把2个默认网关都删掉，再新增。新增时注意设置METRIC这个值不能一样。
    2. 用ROUTE CHANGE 来变更两个默认网关的的外网网关的METRIC值。。

改默认路由方法

    route change 默认不能修改自动生成的路由，要修改自动生成路由，就要先加一条路由, 覆盖默认路由，然后修改新增的路由即可

## 修改本地 DNS 缓存指向局域网里的IP 

/etc/hosts 指向一个局域网IP 如10.66.125.215

/etc/hosts add:

10.66.125.215 biao.com 

再另外一台电脑上，启动一个httpserver，python -m simpleHTTPServer 

修改host缓存的机器，可以直接通过浏览器打开biao.com 访问10.66.125.215

## 





:)
