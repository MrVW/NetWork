命令简介
nmcli is a command-line tool for controlling NetworkManager and reporting network status. It can be utilized as a replacement for nm-applet or other graphical clients.
nmcli is used to create, display, edit, delete, activate, and deactivate network connections, as well as control and display network device status.
使用网卡创建一个网络连接，网络连接 = 网卡 + 网络配置信息。
（1）connection：网络连接
（2）device：网卡

2、nmcli命令的选项与参数

语法：

nmcli [OPTIONS] OBJECT { COMMAND | help }

OPTIONS
      -a, --ask                ask for missing parameters
      -c, --colors auto|yes|no            whether to use colors in output
      -e, --escape yes|no            escape columns separators in values
      -f, --fields <field,...>|all|common        specify fields to output
      -g, --get-values <field,...>|all|common    shortcut for -m tabular -t -f
      -h, --help                print this help
      -m, --mode tabular|multiline        output mode
      -o, --overview                overview mode
      -p, --pretty                pretty output
      -s, --show-secrets            allow displaying passwords
      -t, --terse                terse output
      -v, --version                show program version
      -w, --wait <seconds>            set timeout waiting for finishing operations

OBJECT
      g[eneral]    NetworkManager's general status and operations
      n[etworking]    overall networking control
      r[adio]        NetworkManager radio switches
      c[onnection]     NetworkManager's connections
      d[evice]    devices managed by NetworkManager
      a[gent]    NetworkManager secret agent or polkit agent
      m[onitor]    monitor NetworkManager changes

3、nmcli命令使用示例

查看网卡配置信息、网络连接、网卡设备
    # 查看ip，等同于ifconfig、ip addr
    nmcli
    # 查看connection列表
    nmcli c show
    # 查看connection详细信息
    nmcli c show {网卡名}
    # 查看网络接口设备列表
    nmcli d

启用、停止、删除网络连接
    # 启用connection，等同于ifup
    nmcli c up {网卡名}
    # 停止connection，等同于ifdown
    nmcli c down
    # 删除connection，等同于ifdown后删除ifcfg配置文件
    nmcli c delete {网卡名}

创建网络连接
    # 创建connection，配置静态ip。
    # 等同于修改配置文件，BOOTPROTO=static，ipup启动接口。
    nmcli c add type ethernet con-name {网络设备名} ifname {网卡名} ipv4.addr 172.16.123.201/24 ipv4.gateway 172.16.123.1 ipv4.method manual
    # 创建connection，配置动态ip。
    # 等同于修改配置文件，BOOTPROTO=dhcp，ipup启动接口
    nmcli c add type ethernet con-name {网络设备名} ifname {网卡名} ipv4.method auto

修改IP地址
    # 修改ip（非交互式）
    nmcli c modify {网卡名} ipv4.addr '172.16.123.201/24'
    nmcli c up {网卡名}
    # 修改ip（交互式）
    nmcli c edit {网卡名}
    nmcli> goto ipv4.addresses
    nmcli ipv4.addresses> change
    Edit 'addresses' value: 172.16.123.201/24
    Do you also want to set 'ipv4.method' to 'manual'? [yes]: yes
    nmcli ipv4> save
    nmcli ipv4> activate
    nmcli ipv4> quit

载入配置信息与生效
    # 重载网络配置文件（ifcfg、route），但不生效
    nmcli c reload
    # 重载指定{网卡名}的配置文件（ifcfg、route），但不生效
    nmcli c load /etc/sysconfig/network-scripts/ifcfg-{网卡名}
    nmcli c load /etc/sysconfig/network-scripts/route-{网卡名}
    # 重启网络接口，使配置生效，等同于systemctl restart network
    nmcli c up {网卡名}
    nmcli d reapply {网卡名}
    nmcli d connect {网卡名}
