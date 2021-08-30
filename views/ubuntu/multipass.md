---
layout: views
title: Multipass
parent: ubuntu
nav_order: 2
permalink: /ubuntu/multipass
---

# ![multipass icon](/assets/images/icon/a12a3159-Multipass-favicon_32px.png){:class="icon-title"}  Multipass on LXD

* 1\. [SSH profile 설정](#1-ssh-profile-설정)
* 2\. [ubuntu server 에 RDP 접속](#2-ubuntu-server-에-rdp-접속)
    * 2-1\. [Server](#2-1-server)
        * 2-1-1\. [xrdp 설치](#2-1-1-xrdp-설치)
    * 2-2\. [Client](#2-2-client)
        * 2-2-1\. [Remmina RDP 설정](#2-2-1-remmina-rdp-설정)

```
💡️ Canonical 에서 가상화 구축 tool
CPU 의 가상화 설정이 필요
window 의 경우 Hyper-V 혹은 VirtualBox를 이용
```

ubuntu 20.04 진행

# Multipass 설치

```bash
sudo snap install multipass
```

![/assets/images/views/ubuntu/multipass/Untitled.png](/assets/images/views/ubuntu/multipass/Untitled.png)

# lxd 설치 및 multipass launch

```bash
sudo snap install lxd
```

![/assets/images/views/ubuntu/multipass/Untitled1.png](/assets/images/views/ubuntu/multipass/Untitled1.png)

```bash
# lxd init
sudo lxd init --auto

# multipass lxd connect
sudo snap connect multipass:lxd lxd

# multipass local driver lxd 로 사용
sudo multipass set local.driver=lxd

# name node-test 의 instance 생성
sudo multipass launch -n **node-test**
```

# lxc 정보 확인

```bash
# lxc 로 생성된 instance 정보 확인
lxc info **node-test** --project **multipass**

```

![/assets/images/views/ubuntu/multipass/Untitled2.png](/assets/images/views/ubuntu/multipass/Untitled2.png)

```bash
# instance stop
lxc stop **node-test** --project **multipass**

```

--project multipass 는 lxc project list 로 확인 가능

multipass 가 local.driver lxd 설정후 multipass launch 실행하면 생성되고 multipass 로 생성된 instance 들은 --project multipass 에 묶인다

```bash
# lxc project 정보
lxc project list
lxc project show **multipass**

```

![/assets/images/views/ubuntu/multipass/Untitled3.png](/assets/images/views/ubuntu/multipass/Untitled3.png)

![/assets/images/views/ubuntu/multipass/Untitled4.png](/assets/images/views/ubuntu/multipass/Untitled4.png)

```bash
# lxc network 정보
lxc network list
lxc network show enp3s0
lxc network info enp3s0
```

![/assets/images/views/ubuntu/multipass/Untitled5.png](/assets/images/views/ubuntu/multipass/Untitled5.png)

# lxc 로 multipass 에 macvlan 설정의 device 추가

```bash
# lxc instance에 network device 추가
lxc config device add **node-test** **eth1** nic name=**eth1** nictype=macvlan parent=**enp3s0** --project **multipass**
```

![/assets/images/views/ubuntu/multipass/Untitled6.png](/assets/images/views/ubuntu/multipass/Untitled6.png)

multipass eth0 에 enp5s0 가 연결 되어 있는것으로 보인다.

# multipass instance 다시 실행

```bash
multipass start **node-test**
```

# multipass instance 의 netplan 에 device 설정

```bash
multipass start **node-test**

# net-tools 설치
****sudo apt install net-tools

# network device 확인
ifconfig
```

![/assets/images/views/ubuntu/multipass/Untitled7.png](/assets/images/views/ubuntu/multipass/Untitled7.png)

```bash
sudo vi /etc/netplan/50-cloud-init.yaml
```

![/assets/images/views/ubuntu/multipass/Untitled8.png](/assets/images/views/ubuntu/multipass/Untitled8.png)

enp6s0 설정 추가  y2y -2줄 복사 p - 붙여넣기

```bash
# netplan 설정 적용
sudo netplan apply

# 적용 확인
ifconfig
```

![/assets/images/views/ubuntu/multipass/Untitled9.png](/assets/images/views/ubuntu/multipass/Untitled9.png)

# ping 확인

![/assets/images/views/ubuntu/multipass/Untitled10.png](/assets/images/views/ubuntu/multipass/Untitled10.png)