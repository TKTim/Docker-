## 將iris放到harbor並且部屬於k8s

<div  align="center">  
 <img src="https://raw.githubusercontent.com/TKTim/Docker-/master/Picture/50.jpg" width = "60%" height = "60%" alt="01" align=center />

 <div align="left">

 <div  align="center">  
 <img src="https://raw.githubusercontent.com/TKTim/Docker-/master/Picture/51.jpg" width = "60%" height = "60%" alt="01" align=center />

 <div align="left">

### 到vm2、vm3、vm4:

    連入harbor和增加daemon
    [root@vm4 harbor]# ssh vm2
    root@vm2's password:
    Last login: Tue Jan  5 01:03:00 2021 from vm4
    [root@vm2 ~]# cd /etc/docker
    [root@vm2 docker]# ls
    key.json
    [root@vm2 docker]# vim daemon.json
    [root@vm2 docker]# systemctl daemon-reload
    [root@vm2 docker]# systemctl restart docker.service
    [root@vm2 docker]# exit
    logout
    Connection to vm2 closed.
    [root@vm4 harbor]# ssh vm3
    root@vm3's password:
    Last login: Tue Jan  5 01:03:18 2021 from vm4
    [root@vm3 ~]# cd /etc/docker/
    [root@vm3 docker]# ls
    key.json
    [root@vm3 docker]# vim daemon.json
    [root@vm3 docker]# systemctl daemon-reload
    [root@vm3 docker]# systemctl restart docker.service
    [root@vm3 docker]# docker login 192.168.56.104
    Username: admin
    Password:
    WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
    Configure a credential helper to remove this warning. See
    https://docs.docker.com/engine/reference/commandline/login/#credentials-store

    Login Succeeded
    [root@vm3 docker]# exit
    logout
    Connection to vm3 closed.
    [root@vm4 harbor]# ssh vm2
    root@vm2's password:
    Last login: Tue Jan  5 03:25:23 2021 from vm4
    [root@vm2 ~]# docker login 192.168.56.104
    Username: admin
    Password:

    Login Succeeded
    [root@vm2 ~]# exit

### 建立k8s部屬yaml

    kubectl create deployment mydocker --image=192.168.56.104/library/iris:1.0.0 --dry-run -o yaml > iris-deployment.yaml

    [root@vm4 harbor]# kubectl apply -f iris-deployment.yaml
    deployment.apps/myiris created


    [root@vm4 harbor]# kubectl get pod
    NAME                      READY   STATUS              RESTARTS   AGE
    myiris-686bc64d95-dskrc   1/1     Running             0          2m48s

    [root@vm4 harbor]# kubectl expose deployment myiris --port=8888 --target-port=8888 --type=NodePort
    service/myiris exposed
    [root@vm4 harbor]# kubectl get svc
    NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
    kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP          35d
    myiris       NodePort    10.105.219.198   <none>        8888:31715/TCP   8s

    {這裡的31715要改到client.py裡}

    [root@vm3 Downloads]# python client.py
    setosa

    {這樣就代表成功了}




