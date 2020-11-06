netstat

usage: netstat [-vWeenNcCF] [<Af>] -r         netstat {-V|--version|-h|--help}
       netstat [-vWnNcaeol] [<Socket> ...]
       netstat { [-vWeenNac] -i | [-cnNe] -M | -s [-6tuw] }

        -r, --route              显示路由表
        -i, --interfaces         display interface table
        -g, --groups             display multicast group memberships
        -s, --statistics         display networking statistics (like SNMP)
        -M, --masquerade         display masqueraded connections

        -v, --verbose            显示详细信息
        -W, --wide               don't truncate IP addresses
        -n, --numeric            不解析名称
        --numeric-hosts          不解析主机名
        --numeric-ports          忽略端口名称
        --numeric-users          忽略用户名
        -N, --symbolic           resolve hardware names
        -e, --extend             显示更多信息
        -p, --programs           display PID/Program name for sockets
        -o, --timers             display timers
        -c, --continuous         continuous listing

        -l, --listening          display listening server sockets
        -a, --all                display all sockets (default: connected)
        -F, --fib                display Forwarding Information Base (default)
        -C, --cache              display routing cache instead of FIB
        -Z, --context            display SELinux security context for sockets

  <Socket>={-t|--tcp} {-u|--udp} {-U|--udplite} {-S|--sctp} {-w|--raw}
           {-x|--unix} --ax25 --ipx --netrom
  <AF>=Use '-6|-4' or '-A <af>' or '--<af>'；默认： inet


## netstat -i

v@v:~$ netstat -i
Kernel Interface table
Iface   MTU Met   RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
eth0       1500 0      7870      0      0 0          3749      0      0      0 BMRU
lo        65536 0       443      0      0 0           443      0      0      0 LRU

TX-ERR 和 RX-ERR 数据大，表示网线有问题
TX-DROP 和 RX-DROP 过大，表示网络负载重，主机来不及处理
TX-OVR 和 RX-OVR 是由于速率不匹配



