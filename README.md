# 카카오페이증권 DevOps 과제
---

## 테스트용 Kubernetes 환경 구축

윈도우 개인 PC에서 테스트 진행을 위해 VMware 설치
* https://www.vmware.com/kr/products/workstation-player/workstation-player-evaluation.html 

테스트 환경 구축
* OS
    * Ubuntu 20.04

리소스 할당
* Control node (hostname=compute)
```
CPU : 2 core
Ram : 4GB
Disk : 30GB
Network
ens33 – NAT
ens37 – Internal Network
```

* Compute node(hostname=node1)
```
CPU : 2 core
Ram : 4GB
Disk : 30GB
Network
ens33 – NAT
ens37 – Internal Network
```


시스템 설정
* network ip 설정
```
Control
ens33 – 192.168.159.128(dhcp)
ens37 – 100.100.0.100

node1
ens33 – 192.168.159.129(dhcp)
ens37 – 100.100.0.101
```

/etc/hosts 수정
* Control 에서 진행
```
$ sudo vim /etc/hosts
>>> (파일 하단에 내용 추가)
100.100.0.100 master
100.100.0.101 node1
```

SSH keygen
* Control 에서 진행
```
$ ssh-keygen –t rsa
$ ssh-copy-id node1
```


Kubeadm, kubelet, kubectl 설치
* Control 에서 진행
```
$ sudo apt-get update
$ sudo apt-get install -y apt-transport-https ca-certificates curl
$ sudo curl -fsSLo /etc/apt/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
$ echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
$ sudo apt-get update
$ sudo apt-get install -y kubelet kubeadm kubectl
$ sudo apt-mark hold kubelet kubeadm kubectl
```