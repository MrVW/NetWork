����
systemd-networkd.service, systemd-networkd �� ���������

���
systemd-networkd.service

/usr/lib/systemd/systemd-networkd

����
systemd-networkd �����ڹ��������ϵͳ���� ���ܹ���Ⲣ�����������ӣ� Ҳ�ܹ��������������豸��

��Ҫ���ö���������ĵͼ����������ӣ� �μ� systemd.link(5) �ֲᡣ

systemd-networkd ���� systemd.netdev(5) �ļ��е� [Match] С�ڣ��������������豸��

systemd-networkd ���� systemd.network(5) �ļ��е� [Match] С�ڣ� ��������ƥ����������ӵĵ�ַ��·�ɡ� ������ƥ�����������ʱ����������ո�����ԭ�еĵ�ַ��·�ɡ� ����δ�� .network �ļ�ƥ�䵽���������Ӷ���������(���������κβ���)�� ͬʱ��systemd-networkd ���������Щ�� systemd.network(5) �ļ�����ȷ��Ϊ Unmanaged=yes ���������ӡ�

�� systemd-networkd �����˳�ʱ�� ͨ�������κβ������Ա��ֵ�ʱ�Ѿ����ڵ������豸���������ò��䡣 һ���棬����ζ�ţ��� initramfs �л���ʵ�ʸ��ļ�ϵͳ�Լ�������������񶼲��ᵼ�����������жϡ� ��һ���棬��Ҳ��ζ�ţ��������������ļ������� systemd-networkd ����֮�� ��Щ�ڸ��º�����������ļ����Ѿ���ɾ�������������豸(netdev)�Խ�������ϵͳ�У� �п�����Ҫ�ֶ�ɾ����

�����ļ�
�÷���������ļ� �ֱ�λ�ڣ� ���ȼ���͵� /usr/lib/systemd/network Ŀ¼�� ���ȼ����е� /run/systemd/network Ŀ¼�� ���ȼ���ߵ� /etc/systemd/network Ŀ¼��

����ʹ�� .network �ļ���������(��� systemd.network(5))�� ���������豸ʹ�� .netdev �ļ���������(��� systemd.netdev(5))��


