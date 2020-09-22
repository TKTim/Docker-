# Docker 筆記

[一、VM2透過VM1連上網路](#VM2透過VM1連上網路)

[二、使用ADGUARD from Docker](#使用ADGUARD_from_Docker)

[三、使用ADGUARD from Docker](#第二節)

---


## VM2透過VM1連上網路

VM1網卡設置:

    NAT+Host+inernet內部網路


VM2網卡設置:

    inernet內部網路


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
    **開啟路由表

    [root@vm1 user]# iptables -A FORWARD -o enp0s3 -i enp0s9 -s 192.168.1.0/24 -m conntrack --ctstate NEW -j ACCEPT
    **
    FORWARD 是外來的連線，但最終目的地不是伺服器本身，只是轉送到其他機器。
    -A append、-o output、-i input、

    [root@vm1 user]# iptables -A FORWARD -o enp0s3 -i enp0s9 -s 192.168.1.0/24 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
    ** 
    接受任何關聯物件

    [root@vm1 user]# iptables -t nat -A POSTROUTING -o enp0s3 -s 192.168.1.0/24 -j MASQUERADE
    **
    網路轉換 -A 當封包準備發送時，若符合以下標準(Output==enp0s3 && Source==192.168.1.0/24) ，將根據情況動態修改封包的來源地址(MASQUERADE)。


VM2:

    ping 8.8.8.8
    **測試是否可連網
![im](https://github.com/TKTim/Docker-/blob/master/Picture/02.jpg)

---

## 使用ADGUARD_from_Docker

下載Docker

    $ sudo yum install -y yum-utils

    $ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

    $ sudo yum install docker-ce docker-ce-cli containerd.io

    $ sudo systemctl start docker



下載ADguard

登入[下載網站](https://hub.docker.com/r/adguard/adguardhome)，登入會員。

    $ sudo docker pull adguard/adguardhome

    $ sudo docker run --name adguardhome -v /my/own/workdir:/opt/adguardhome/work -v /my/own/confdir:/opt/adguardhome/conf -p 53:53/tcp -p 53:53/udp -p 67:67/udp -p 68:68/tcp -p 68:68/udp -p 80:80/tcp -p 443:443/tcp -p 853:853/tcp -p 3000:3000/tcp -d adguard/adguardhome

    $ sudo docker start adguardhome

如果埠號有占用記得使用

    netstat -tunlp | grep 埠號
    kill -9 行程

成功後，在瀏覽器輸入 VM1的enp0s8'sIP + ':' + '3000'

![im](https://github.com/TKTim/Docker-/blob/master/Picture/03.jpg)

依照以下網站設定ADguard
https://www.sakamoto.blog/synology-adguard-home-dns/

VM1:
   
    vim /etc/resolv.conf
    ** 使網路連線通過設置的inernet

![im](https://github.com/TKTim/Docker-/blob/master/Picture/04.jpg)

VM2:

    往後VM2若通過VM1聯網，youtube便不會有廣告。

---

## 第二節

### 前置動作

準備兩台虛擬機

   VM2網卡設置:

    NAT+Host


VM3網卡設置:

    NAT+Host

### 1. 確認是否已安裝Docker

    $ docker version 
    **測試是否安裝

### 2. 下載Docker compose

    $ sudo curl -L "https://github.com/docker/compose/releases/download/1.27.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

    $ sudo chmod +x /usr/local/bin/docker-compose

    $ docker-compose --version
    ** 測試是否安裝

### 3. 在Docker hub上申請帳號
[Docker_Hub](https://hub.docker.com/)

### 4. Docker講解
<div  align="center">    
 <img src="https://github.com/TKTim/Docker-/blob/master/Picture/05.png" width = "50%" height = "50%" alt="01" align=center />
</div>

>Docker 僅有最上層能夠更改，映像檔是唯讀的，所以我們每次要在映像檔上面再疊一層時，都需要先將他建立成容器實體（烤成蛋糕本人）。然後運用容器啟動時會在最上面建立可寫層的特性，我們就可以在容器裡在透過另外一個映像檔建立容器實體，最後把這個容器疊上容器的碗糕轉換成映像檔（Dockerize），我們就完成了映像檔堆疊。

### 5. Docker實作

    $ docker pull httpd
    **下載httpd from DockerHub

    $ docker images 
    **顯示存在的images

 <img src="https://github.com/TKTim/Docker-/blob/master/Picture/05.jpg" width = "50%" height = "50%" alt="01" align=center />
</div>

>以上塗紅處表示   [來源位置/帳號/檔名:版本]

    $ docker run -d --name myhttpd -p 8080:80 hdttpd
    ** -d  背景執行　--name 名稱　8080: 主機鋪號　80:Docker鋪號 
    ** 輸入後會出現 一大串識別碼
    EX.
         ed27d98eb361712c3e918a990f8c4261e917e89a3faf25d850dd7c0af38a0194
    
    $ 輸入http://192.168.56.102:8080/

 <img src="https://github.com/TKTim/Docker-/blob/master/Picture/06.jpg" width = "50%" height = "50%" alt="01" align=center />
</div>



### [參考資料]
https://medium.com/unorthodox-paranoid/docker-tutorial-101-c3808b899ac6

docker compose --- https://blog.techbridge.cc/2018/09/07/docker-compose-tutorial-intro/

---






