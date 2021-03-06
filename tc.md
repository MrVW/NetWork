How to Use the Linux Traffic Control
==========

from

https://netbeez.net/blog/category/network-monitoring/distributed-network-monitoring/

Linux Traffic Control
------

Traffic control (tc) is a very useful Linux utility that gives you the ability to configure the kernel packet scheduler. If you are looking for reasons to mess with the kernel scheduler, here are a few: Firstly, its fun to play with the different options and become familiar of all of Linuxs features. In addition, you can utilize Linuxs helpful tools to simulate packet delay and loss for UDP or TCP applications, or limit the bandwidth usage of a particular service to simulate Internet connections (DSL, Cable, T1, etc).

On Debian Linux, tc comes bundled with iproute, so in order to install it you have to run:

apt-get install iproute

Network Delay
------

The first example is how to add constant delay to an interface. The syntax is as follows (run this as root):

	tc qdisc add dev eth0 root netem delay 200ms

Here is what each option means:

	qdisc: modify the scheduler (aka queuing discipline)
	add: add a new rule
	dev eth0: rules will be applied on device eth0
	root: modify the outbound traffic scheduler (aka known as the egress qdisc)
	netem: use the network emulator to emulate a WAN property
	delay: the network property that is modified
	200ms: introduce delay of 200 ms


Note: this adds a delay of 200 ms to the egress scheduler, exclusively. If it were to add the delay to both the ingress and egress schedulers, the total delay would have totaled 400 ms. In general, all of these traffic control rules are applied to the egress scheduler only.


Here is how ping looks like before:


	netbeez.net$ ping google.com
	PING google.com (172.217.6.78) 56(84) bytes of data.
	64 bytes from sfo07s17-in-f78.1e100.net (172.217.6.78): 
	icmp_seq=1 ttl=53 time=11.9 ms
	64 bytes from sfo07s17-in-f78.1e100.net (172.217.6.78): 
	icmp_seq=2 ttl=53 time=12.0 ms

Here is what ping looks like after applying this rule:

	netbeez.net$ ping google.com
	PING google.com (172.217.5.110) 56(84) bytes of data.
	64 bytes from sfo03s07-in-f14.1e100.net (172.217.5.110): 
	icmp_seq=1 ttl=53 time=213 ms
	64 bytes from sfo03s07-in-f14.1e100.net (172.217.5.110): 
	icmp_seq=2 ttl=53 time=210 ms


In order to display the active rules use:

	netbeez.net$ tc qdisc show  dev eth0
	qdisc netem 8003: root refcnt 2 limit 1000 delay 200.0ms

You can see that details of the existing rules that adds 200.0 ms of latency.

To delete all rules use the following command:

	netbeez.net$ tc qdisc del dev eth0 root

And now we can see what are the default rules of the linux scheduler:


	netbeez.net$ tc qdisc show  dev eth0
	qdisc pfifo_fast 0: root refcnt 2 bands 3 priomap  1 2 2 2 1 2 0 0 1 1 1 1 1 1 1 1


Without going into too much detail, we see that the scheduler works under First In First Out (FIFO) rules which is the most basic and fair rule if you dont want to set any priorities on specific packets. You can think about it like the line at the bank: customers are being taken care off in the order they arrive.

Note that if you have an existing rule you can change it by using  "tc qdisc change ..." and if you dont have any rules you add rules with " tc qdisc add... "

Here are some other examples:



-Delay of 100ms and random +-10ms uniform distribution:

	tc qdisc change dev eth0 root netem delay 100ms 10ms


-Delay of 100ms and random 10ms uniform variation with correlation value 25% (since network delays are not completely random):

	tc qdisc change dev eth0 root netem delay 100ms 10ms 25%


-Delay of 100ms and random +-10ms normal distribution (other distribution options are pareto, and paretonormal):

	add dev eth0 root netem delay 100ms 20ms distribution normal

Packet Loss and Packet Corruption
---------


Without explaining the syntax in detail here is now to introduce a packet loss of 10%:

	tc qdisc add dev eth0 root netem loss 10%

We can test this by running a ping test with 100 ICMP packets. Here is what the aggregate statistics look like:

	netbeez.net$ ping google.com -c 100
	PING google.com (216.58.194.174) 56(84) bytes of data.
	64 bytes from sfo07s13-in-f174.1e100.net (216.58.194.174): icmp_seq=1 ttl=53 time=10.8 ms
	.....
	64 bytes from sfo07s13-in-f174.1e100.net (216.58.194.174): icmp_seq=99 ttl=53 time=11.8 ms
	64 bytes from sfo07s13-in-f174.1e100.net (216.58.194.174): icmp_seq=100 ttl=53 time=10.5 ms
	 
	--- google.com ping statistics ---
	100 packets transmitted, 89 received, 11% packet loss, time 99189ms
	rtt min/avg/max/mdev = 10.346/12.632/21.102/2.640 ms


As you can see, there was 11% packet loss which is very close to the value was set. Note that if you are sshed to the Linux box that you are running these commands on, your connection might be lost due to having set the packet loss too high.

The following rule corrupts 5% of the packets by introducing single bit error at a random offset in the packet:

	tc qdisc change dev eth0 root netem corrupt 5%

This one duplicates 1% of the sent packets:

	tc qdisc change dev eth0 root netem duplicate 1%

Bandwidth limit
------

In order to limit the egress bandwidth we can use the following command:

	tc qdisc add dev eth0 root tbf rate 1mbit burst 32kbit latency 400ms

- tbf: use the token buffer filter to manipulate traffic rates
- rate: sustained maximum rate
- burst: maximum allowed burst
- latency: packets with higher latency get dropped


The best way to demonstrate this is with an iPerf test. In my lab I get 95 Mbps of performance before applying any bandwidth rules:



	netbeez.net$ iperf -c 172.31.0.142
	------------------------------------------------------------
	Client connecting to 172.31.0.142, TCP port 5001
	TCP window size: 85.3 KByte (default)
	------------------------------------------------------------
	[  3] local 172.31.0.25 port 40233 connected with 172.31.0.142 port 5001
	[ ID] Interval       Transfer     Bandwidth
	[  3]  0.0-10.0 sec   113 MBytes  95.0 Mbits/sec
	

And here is the performance after applying the 1 Mbps limit:

	netbeez.net$ iperf -c 172.31.0.142
	------------------------------------------------------------
	Client connecting to 172.31.0.142, TCP port 5001
	TCP window size: 85.3 KByte (default)
	------------------------------------------------------------
	[  3] local 172.31.0.25 port 40232 connected with 172.31.0.142 port 5001
	[ ID] Interval       Transfer     Bandwidth
	[  3]  0.0-11.0 sec  1.50 MBytes  1.14 Mbits/sec


As you can see the measured bandwidth of 1.14 Mbps is very close to the limit that was configured.

Traffic control (tc), in combination with network emulator (netem), and token bucket filters (tbf), can perform much more advanced configurations than just tc alone.  A few examples of advanced configurations are maximizing TCP throughput on an asymmetric link, prioritizing latency sensitive traffic, or managing oversubscribed bandwidth. Some of these tasks can be performed effectively with other tools or services, but tc is a great utility to have in your arsenal when the need arises.
