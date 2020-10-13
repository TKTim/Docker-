VM4 設定 sshd 與連線
   
    # vim /etc/sysconfig/netowrk-scripts/ifcfg-enp0s8
    DEVICE=enp0s8
    BOOTPROTO=none
    BROADCAST=192.168.56.255
    IPADDR=192.168.56.104 
    NETMASK=255.255.255.0
    GATEWAY=192.168.56.254

    # vim /etc/resolv.conf 
    nameserver 8.8.8.8

    # /etc/init.d/network restart

    # reboot
