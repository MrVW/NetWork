[WikiArchLinux](https://wiki.archlinux.org/index.php/Systemd-networkd_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)

systemd-networkd 是一个管理网络配置的系统守护进程。它会在网络设备出现时检测和配置；它还可以创建虚拟网络设备。这个服务对被 systemd-nspawn 管理的容器或者虚拟机的复杂网络配置尤其有用，同样也适用于简单的网络配置。


ystemd/udev 会自动为所有本地以太网，WLAN 和 WWAN 接口分配可预测，稳定的网络接口名。使用 networkctl list 以列出系统上所有设备。


DHCP动态分配网络适配器

	/etc/systemd/network/20-wired.network
	[Match]
	Name=enp1s0

	[Network]
	DHCP=ipv4

静态 IP 的有线适配器

	/etc/systemd/network/20-wired.network
	[Match]
	Name=enp1s0

	[Network]
	Address=10.1.10.9/24
	Gateway=10.1.10.1
	DNS=10.1.10.1
	#DNS=8.8.8.8

为了能够使用 systemd-networkd 连接一个无线网络，需要一个被其他应用，比如 WPA supplicant 或 Iwd，配置好的无线适配器。

	/etc/systemd/network/25-wireless.network
	[Match]
	Name=wlp2s0

	[Network]
	DHCP=ipv4
	
	
<table class="wikitable">
<tbody><tr>
<th>参数</th>
<th>说明</th>
<th>值类型</th>
<th>默认值
</th></tr>
<tr>
<td><code>DHCP=</code></td>
<td>Controls DHCPv4 and/or DHCPv6 client support.</td>
<td>boolean, <code>ipv4</code>, <code>ipv6</code></td>
<td><code>false</code>
</td></tr>
<tr>
<td><code>DHCPServer=</code></td>
<td>If enabled, a DHCPv4 server will be started.</td>
<td>boolean</td>
<td><code>false</code>
</td></tr>
<tr>
<td><code>MulticastDNS=</code></td>
<td>Enables <a rel="nofollow" class="external text" href="https://tools.ietf.org/html/rfc6762">multicast DNS</a> support. When set to <code>resolve</code>, only resolution is enabled, but not host or service registration and announcement.</td>
<td>boolean, <code>resolve</code></td>
<td><code>false</code>
</td></tr>
<tr>
<td><code>DNSSEC=</code></td>
<td>Controls DNSSEC DNS validation support on the link. When set to <code>allow-downgrade</code>, compatibility with non-DNSSEC capable networks is increased, by automatically turning off DNSSEC in this case.</td>
<td>boolean, <code>allow-downgrade</code></td>
<td><code>false</code>
</td></tr>
<tr>
<td><code>DNS=</code></td>
<td>Configure static <a href="/index.php/DNS" class="mw-redirect" title="DNS">DNS</a> addresses. May be specified more than once.</td>
<td><a rel="nofollow" class="external text" href="http://man7.org/linux/man-pages/man3/inet_pton.3.html"><code>inet_pton</code></a></td>
<td>
</td></tr>
<tr>
<td><code>Domains=</code></td>
<td>A list of domains which should be resolved using the DNS servers on this link. <a rel="nofollow" class="external text" href="https://www.freedesktop.org/software/systemd/man/systemd.network.html#Domains=">more information</a></td>
<td>domain name, optionally prefixed with a tilde (<code>~</code>)</td>
<td>
</td></tr>
<tr>
<td><code>IPForward=</code></td>
<td>If enabled, incoming packets on any network interface will be forwarded to any other interfaces according to the routing table.</td>
<td>boolean, <code>ipv4</code>, <code>ipv6</code></td>
<td><code>false</code>
</td></tr>
<tr>
<td><code>IPv6PrivacyExtensions=</code></td>
<td>Configures use of stateless temporary addresses that change over time (see <a rel="nofollow" class="external text" href="https://tools.ietf.org/html/rfc4941">RFC 4941</a>). When <code>prefer-public</code>, enables the privacy extensions, but prefers public addresses over temporary addresses. When <code>kernel</code>, the kernel's default setting will be left in place.</td>
<td>boolean, <code>prefer-public</code>, <code>kernel</code></td>
<td><code>false</code>
</td></tr></tbody></table>


