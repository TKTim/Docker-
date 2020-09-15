# Docker 筆記

[一、VM2透過VM1連上網路](#VM2透過VM1連上網路)
[二、]
---




## VM2透過VM1連上網路

VM1:

    NAT+Host+inernet


VM2:

    inernet


## [設定IP]

VM1:


    ip addr add 192.168.1.1/24 brd + dev enp0s9



VM2:
    
    ip addr add 192.168.1.2/24 brd + dev enp0s3
    **brd 廣播
    ip route add default via 192.168.1.1
    **透過IP上網

VM1試連:

    ping 192.168.1.2

![im](https://github.com/TKTim/Docker-/blob/master/Picture/01.jpg)


VM1:

    echo 1 > /proc/sys/net/ipv4/ip_forward
    [root@vm1 user]# iptables -A FORWARD -o enp0s3 -i enp0s9 -s 192.168.1.0/24 -m conntrack --ctstate NEW -j ACCEPT
    [root@vm1 user]# iptables -A FORWARD -o enp0s3 -i enp0s9 -s 192.168.1.0/24 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
    [root@vm1 user]# iptables -t nat -A POSTROUTING -o enp0s3 -s 192.168.1.0/24 -j MASQUERADE

VM2:

    ping 8.8.8.8
    **測試是否可連網
![im](https://github.com/TKTim/Docker-/blob/master/Picture/02.jpg)

---







