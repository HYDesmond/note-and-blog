#ssh连接虚拟机
1. 设置虚拟机的网络为桥接模式
2. 设置虚拟机ｌｉｕｎｘ为固定ｉｐ
    ＋　设置IP和掩码`ifconfig eth0 192.168.1.250 netmask 255.255.255.0`
    ＋　设置网关`route add default gw 192.168.1.1`
2. 安装ssh`sudo apt-get install openssh-server`
2. 关闭防火墙`sudo ufw disable`
最后在本机ｐｉｎｇ一下可以ｐｉｎｇ通就可以连接上了