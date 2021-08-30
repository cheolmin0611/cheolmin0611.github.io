---
layout: views
title: Remmina
parent: ubuntu
nav_order: 1
permalink: /ubuntu/remmina
---

# ![remmina icon](/assets/images/icon/Org.remmina.Remmina.png){:class="icon-title"}  Remmina

* 1\. [SSH profile 설정](#1-ssh-profile-설정)
* 2\. [ubuntu server 에 RDP 접속](#2-ubuntu-server-에-rdp-접속)
    * 2-1\. [Server](#2-1-server)
        * 2-1-1\. [xrdp 설치](#2-1-1-xrdp-설치)
    * 2-2\. [Client](#2-2-client)
        * 2-2-1\. [Remmina RDP 설정](#2-2-1-remmina-rdp-설정)

```
💡️ POSIX 기반 컴퓨터 운영 체제 용 원격 데스크톱 클라이언트. RDP, VNC, NX, XDMCP, SPICE 및 SSH 프로토콜을 지원
```

# 1. SSH profile 설정

key file 인증 방식일 경우 인증방식의 필드에 SSH ID 파일을 선택 후 확인 파일 선택
{:class="desc-text"}

![remmina ssh profile setting](/assets/images/views/ubuntu/remmina/Untitled.png)

# 2. ubuntu server 에 RDP 접속

Remmina 를 통해 원격 연결 사용 설정
{:class="desc-text"}

## 2-1. Server

### 2-1-1. xrdp 설치

Microsoft Windows 이외의 운영 체제가 완전한 기능의 RDP 호환 원격 데스크톱 환경을 제공 할 수 있도록하는 Microsoft RDP 서버의 무료 오픈 소스
{:class="desc-text"}

```bash
sudo apt install xrdp

# Xrdp가 설치되면 SSL 인증서 키 (ssl-cert-snakeoil.key)가/etc/ssl/private/폴더에 저장됩니다. "사용자가 파일을 읽을 수 있도록 xrdp 사용자를 ssl-cert 그룹에 추가해야합니다.
sudo adduser xrdp ssl-cert

# xrdp 실행
service xrdp start

# xrpd 설정 - ex) port 3389
sudo vi /etc/xrdp/xrdp.ini
```

참고 - [https://ko.linux-console.net/?p=393](https://ko.linux-console.net/?p=393){:target="_blank"}

## 2-2. Client

### 2-2-1. Remmina RDP 설정

기본설정

![remmina rdp setting](/assets/images/views/ubuntu/remmina/Untitled1.png)

고급설정 - 품질- 낮음, 보안 - TLS, 게이트웨이 전송 타입 - auto

![remmina rdp setting](/assets/images/views/ubuntu/remmina/Untitled2.png)