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

