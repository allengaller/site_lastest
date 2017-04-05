---
title: k8s core
categories:
- docker
tags:
- core
---

#  Alibaba Cloud

guide: https://yq.aliyun.com/articles/68921?spm=5176.100240.searchblog.20.kVq4L7

## prepare

1. create vpc
vpc, vswitch,

2. create 2 ecs
CentOS 7.2-x64, Ubuntu 16.04 x64
create security group;

3. connect ecs 1 as master
$ssh root@112.74.175.225
$hostname # ecs instance name
example: 
    $curl -L 'http://aliacs-k8s.oss-cn-hangzhou.aliyuncs.com/installer/kubemgr.sh' | bash -s nice --node-type master --key-id $ACCESS_KEY_ID --key-secret $ACCESS_KEY_SECRET --region $REGION --discovery token://

install:
    $curl -L 'http://aliacs-k8s.oss-cn-hangzhou.aliyuncs.com/installer/kubemgr.sh' | bash -s nice --node-type master --key-id pFR3aVQnXvjZzwKn --key-secret Xil10SqrVumfSfd7ESNjSIcNlsrm1V --region cn-shenzhen --discovery token://
uninstall:
    $curl -L 'http://aliacs-k8s.oss-cn-hangzhou.aliyuncs.com/installer/kubemgr.sh' | bash -s nice --node-type down
log：
    $ journalctl -u kubelet -f

kubeadm join --discovery token://3f3793:697efc725dcda9e8@192.168.1.237:9898

4. connect ecs 2 3 as node
install: 
    $ curl -L 'http://aliacs-k8s.oss-cn-hangzhou.aliyuncs.com/installer/kubemgr.sh' | bash -s nice --node-type node --key-id pFR3aVQnXvjZzwKn --key-secret Xil10SqrVumfSfd7ESNjSIcNlsrm1V --region cn-shenzhen --discovery token://3f3793:697efc725dcda9e8@192.168.1.237:9898

# kubectl get nodes
NAME                      STATUS         AGE       VERSION
izwz92didsxigwodbj5mztz   Ready          27m       v1.6.0-alpha.0.2229+88fbc68ad99479-dirty
izwz99k8xfd8rnweyyp1y2z   Ready          25m       v1.6.0-alpha.0.2229+88fbc68ad99479-dirty
izwz99o2kucvflwkkpf9d2z   Ready,master   40m       v1.6.0-alpha.0.2229+88fbc68ad99479-dirty

5. network setting
goto master:
# curl -sSL http://aliacs-k8s.oss-cn-hangzhou.aliyuncs.com/conf/flannel-vpc.yml -o flannel-vpc.yml
# vi flannel-vpc.yml 
# kubectl apply -f flannel-vpc.yml
configmap "kube-flannel-cfg" created
daemonset "kube-flannel-ds" created

# kubectl --namespace=kube-system get ds
NAME              DESIRED   CURRENT   READY     NODE-SELECTOR                   AGE
kube-flannel-ds   3         3         3         beta.kubernetes.io/arch=amd64   1m
kube-proxy        3         3         3         <none>                          43m

6. create application
goto master:
# kubectl run nginx --image=registry.cn-hangzhou.aliyuncs.com/spacexnice/nginx:latest --replicas=2 --labels run=nginx
deployment "nginx" created

# kubectl get po
NAME                     READY     STATUS    RESTARTS   AGE
nginx-3579028506-rglm7   1/1       Running   0          38s
nginx-3579028506-rtgsw   1/1       Running   0          38s

# kubectl expose deployment nginx --port=80 --target-port=80 --type=LoadBalancer
service "nginx" exposed

# kubectl get svc
NAME         CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE
kubernetes   172.19.0.1     <none>          443/TCP        46m
nginx        172.19.3.137   120.77.128.24   80:32392/TCP   17s

NOTICE：
1. kubectl 命令的配置目前只放在了master上，但这并不意味着创建的应用都在Master上， 这是集群范围的
如果想其他地方执行Kubectl命令的话，把/etc/kubernetes/admin.conf copy过去就可以了

2. dockerfile
From ubuntu:16.04
RUN  apt update && apt install nginx
ENTRYPOINT  nginx
CMD ["-g","daemon off;"]

