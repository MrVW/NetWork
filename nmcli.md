������
nmcli is a command-line tool for controlling NetworkManager and reporting network status. It can be utilized as a replacement for nm-applet or other graphical clients.
nmcli is used to create, display, edit, delete, activate, and deactivate network connections, as well as control and display network device status.
ʹ����������һ���������ӣ��������� = ���� + ����������Ϣ��
��1��connection����������
��2��device������

2��nmcli�����ѡ�������

�﷨��

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

3��nmcli����ʹ��ʾ��

�鿴����������Ϣ���������ӡ������豸
    # �鿴ip����ͬ��ifconfig��ip addr
    nmcli
    # �鿴connection�б�
    nmcli c show
    # �鿴connection��ϸ��Ϣ
    nmcli c show {������}
    # �鿴����ӿ��豸�б�
    nmcli d

���á�ֹͣ��ɾ����������
    # ����connection����ͬ��ifup
    nmcli c up {������}
    # ֹͣconnection����ͬ��ifdown
    nmcli c down
    # ɾ��connection����ͬ��ifdown��ɾ��ifcfg�����ļ�
    nmcli c delete {������}

������������
    # ����connection�����þ�̬ip��
    # ��ͬ���޸������ļ���BOOTPROTO=static��ipup�����ӿڡ�
    nmcli c add type ethernet con-name {�����豸��} ifname {������} ipv4.addr 172.16.123.201/24 ipv4.gateway 172.16.123.1 ipv4.method manual
    # ����connection�����ö�̬ip��
    # ��ͬ���޸������ļ���BOOTPROTO=dhcp��ipup�����ӿ�
    nmcli c add type ethernet con-name {�����豸��} ifname {������} ipv4.method auto

�޸�IP��ַ
    # �޸�ip���ǽ���ʽ��
    nmcli c modify {������} ipv4.addr '172.16.123.201/24'
    nmcli c up {������}
    # �޸�ip������ʽ��
    nmcli c edit {������}
    nmcli> goto ipv4.addresses
    nmcli ipv4.addresses> change
    Edit 'addresses' value: 172.16.123.201/24
    Do you also want to set 'ipv4.method' to 'manual'? [yes]: yes
    nmcli ipv4> save
    nmcli ipv4> activate
    nmcli ipv4> quit

����������Ϣ����Ч
    # �������������ļ���ifcfg��route����������Ч
    nmcli c reload
    # ����ָ��{������}�������ļ���ifcfg��route����������Ч
    nmcli c load /etc/sysconfig/network-scripts/ifcfg-{������}
    nmcli c load /etc/sysconfig/network-scripts/route-{������}
    # ��������ӿڣ�ʹ������Ч����ͬ��systemctl restart network
    nmcli c up {������}
    nmcli d reapply {������}
    nmcli d connect {������}
