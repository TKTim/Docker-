# Docker 筆記

[一、VM2透過VM1連上網路，使用ADGUARD from Docker](https://github.com/TKTim/Docker-/blob/master/files/20200915.md)

[二、Docker實作篇:compose、基本操作](https://github.com/TKTim/Docker-/blob/master/files/20200922.md)

[三、Docker實作篇: 資料和運行環境分離](https://github.com/TKTim/Docker-/blob/master/files/20200929.md)

[四、Docker實作篇: 備份tar、建立私有倉儲goharbor、快速刪除img&con](https://github.com/TKTim/Docker-/blob/master/files/20201006.md)

[五、Docker實作篇: Network、Volume](https://github.com/TKTim/Docker-/blob/master/files/20201013.md)

[例外: VM4 設定 sshd 與連線](https://github.com/TKTim/Docker-/blob/master/files/setVm4.md)

[六、Docker實作篇: MySQL搭配php、ngrok 使外網連上Docker、機器學習](https://github.com/TKTim/Docker-/blob/master/files/20201020.md)

[七、透過Gitlab完成自動副署、.gitlab-ci.yml Part1](https://github.com/TKTim/Docker-/blob/master/files/20201027.md)

[八、透過Gitlab完成自動副署、.gitlab-ci.yml Part2](https://github.com/TKTim/Docker-/blob/master/files/20201103.md)

[九、Docker swarm](https://github.com/TKTim/Docker-/blob/master/files/20201117.md)

[十、Docker Replicas、更換版本、constraint](https://github.com/TKTim/Docker-/blob/master/files/20201124.md)

[十一、K8s Part 1:安裝、](https://github.com/TKTim/Docker-/blob/master/files/20201201.md)

[十二、實作滾動升級](https://github.com/TKTim/Docker-/blob/master/files/20201208.md)

[十三、附載均衡、修正無法透過其他虛擬機IP連線問題](https://github.com/TKTim/Docker-/blob/master/files/20201215.md)

[十四、Dashboard、Helm](https://github.com/TKTim/Docker-/blob/master/files/20201229.md)

[十五、完整架設Flask、k8s](https://github.com/TKTim/Docker-/blob/master/files/20210105.md)


---

## docker swarm 實用語法

    **container刪除

    [root@localhost ~]# docker rm -f `docker ps -a | awk '{print $1}'`
        * 可再搭配grep
        * 例如
            docker rm -f `docker ps -a | grep goharbor | awk '{print $1}'`

  
    **images刪除

    [root@localhost ~]# docker rmi `docker images | awk '{print $3}'`

    **docker service刪除

    [root@vm4 ~]# docker service rm `docker service ls | awk '{print $1}'`







