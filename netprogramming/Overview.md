## 什么是TCP/IP

通常说的TCP/IP协议一般包括两组协议。
提供可靠数据传输的是传输控制协议TCP,提供无连接数据包服务的协议是网际协议IP.
实际上我们说的TCP/IP协议是很多协议的组合。

相比于专家提出的OSI七层模型，TCP/IP协议一般归纳为四层模型。


- 应用层

相当于OSI中的应用层、表示层和会话层。包括网络进程和应用层序，如远程登录、文件传输和电子邮件等。

- 传输层

相当于OSI的传输层，为应用程序提供端对端的通信服务。TCP/UDP/ICMP都属于。

- 网络层

OSI的网络层。处理机器之间的通信，接受传输层请求，传输某个目的地址信息的分组，把分组封装到IP数据包中，填入数据包的头部，使用路由算法来选择是直接把数据包发给目标地址还是发给路由器，再将数据包交给网络接口层的对应网络接口模块。

- 网络接口层

OSI中的数据链路层和物理层，接收IP数据包、把数据包通过选定的网络发送出去。

		应用层Application
			  |			<--- 报文和数据流
		传输层Transport
			  |			<--- 传输协议包
		网络层Internet
			  |			<--- IP数据包
		网络接口层Netware Interface	  
			  |			<--- 网络帧
		硬件层Hardware
		
## 什么是Socket

网络进程ID。
使用端口号和网络地址就能唯一确定整个网络中的一个网络进程。
把端口号和网络地址放在一个结构体里，这个套接字结构体以sockaddr_开头。

[struct sockaddr ](https://elixir.bootlin.com/linux/v4.14.181/source/include/linux/socket.h#L30)

	struct sockaddr {
		sa_family_t	sa_family;	/* address family, AF_xxx	*/
		char		sa_data[14];	/* 14 bytes of protocol address	*/
	};

[sockaddr_in](https://elixir.bootlin.com/linux/v4.14.181/source/include/uapi/linux/in.h#L232)

	struct sockaddr_in {
	  __kernel_sa_family_t		sin_family;	/* Address family		*/
	  __be16					sin_port;	/* Port number	16位端口号*/
	  struct in_addr			sin_addr;	/* Internet address		*/

	  /* Pad to size of `struct sockaddr'. 备用，为了和struct sockaddr保持相同的字节数*/
	  unsigned char		__pad[__SOCK_SIZE__ - sizeof(short int) -
				sizeof(unsigned short int) - sizeof(struct in_addr)];
	};
	
	所谓的网络地址，其实就是一个32位的数。
	struct in_addr {
		__be32	s_addr;
	};
	
## 常用的转换接口

#### 字节序

网络字节序使用的是大端字节序，而主机字节序可能是小端字节序。网络协议中处理的都是网络字节序。

``` 字节序
#include <netinet/in.h>
//返回网络字节序
uint16_t htons(uint16_t hostvalue);
uint32_t htonl(uint32_t hostvalue);
//返回主机字节序
uint16_t ntohs(uint16_t netvalue);
uint32_t ntohl(uint16_t netvlaue);
```

#### 自己内存操作

```
bzero
bcopy
memset
memcopy
memcpy

```

#### 地址转换

``` 
in_addr_t inet_addr(const char*straddr);
返回32位二进制网络字节序地址

```

```


```

