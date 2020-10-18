traceroute 和 traceroute6、tracerpath 命令，跟踪

A tool to display the path of packets across an IP network.
 

### tracepath

```
	m@m:~$ tracepath

	Usage
	  tracepath [options] <destination>

	Options:
	  -4             use IPv4
	  -6             use IPv6
	  -b             print both name and ip
	  -l <length>    use packet <length>
	  -m <hops>      use maximum <hops>
	  -n             no dns name resolution
	  -p <port>      use destination <port>
	  -V             print version and exit
	  <destination>  dns name or ip address

	For more details see tracepath(8).
```

	m@m:~$ tracepath 8.8.8.8
	 1?: [LOCALHOST]                      pmtu 1500
	 1:  _gateway                                              6.941ms
	 1:  _gateway                                              4.586ms
	 2:  192.168.24.1                                          2.618ms
	 3:  no reply
	 4:  no reply
	 5:  no reply
	 6:  no reply
	     Too many hops: pmtu 1500
	     Resume: pmtu 1500


### traceroute



