---
layout: views
title: Jmeter
parent: devops
nav_order: 2
permalink: /devops/jmeter
---

# ![jmeter icon](/assets/images/icon/jmeter_400x400.jpg){:class="icon-title"} Jmeter

* [설치](#설치)
* [Jmeter Remote 설정](#jmeter-remote-설정)
    * [Client(Master)](#clientmaster)
        * [1) remote host 설정 - ip 만 적을 경우 default port 인 1099 로 설정](#1-remote-host-설정---ip-만-적을-경우-default-port-인-1099-로-설정)
        * [2) client rmi local port 설정](#2-client-rmi-local-port-설정)
    * [Server(Slave)](#serverslave)
        * [1) server port 설정](#1-server-port-설정)
        * [2) server rmi localport 설정](#2-server-rmi-localport-설정)
* [실행](#실행)
    * [Client(Master)](#clientmaster-1)
    * [Server(Slave)](#serverslave-1)
* [Plugin](#plugin)

jmeter

# 설치

공식 페이지 에서 다운로드

[https://jmeter.apache.org/download_jmeter.cgi](https://jmeter.apache.org/download_jmeter.cgi){:target="_blank"}

# Jmeter Remote 설정

jmeter 는  Master, Slave 구조로 remote 로 원격 설정이 가능

대부분의 설정은 **jmeter.properties** 에서 설정

> ## Client(Master)

> ### 1) remote host 설정 - ip 만 적을 경우 default port 인 1099 로 설정

> ```bash
#Remote Hosts - comma delimited
remote_hosts={Slave1 IP}:{Slave1 Port},{Slave2 IP}:{Slave2 Port}
#remote_hosts=localhost:1099,localhost:2010
> ```

> ### 2) client rmi local port 설정

> **jmeter.sh** 동작하는 client의 port 는 remote 를 동작시키면 open 됨 

> 1099로 설정시 1100 1101 로 열리는 것이 확인 그중 1101 이 server 의 결과를 받는 listen port로 확인

> ```bash
# Parameter that controls the base for RMI ports used by RemoteSampleListenerImpl and RemoteThreadsListenerImpl (The Controller)
# Default value is 0 which means ports are randomly assigned
# If you specify a base port, JMeter will (at the moment) use the ports that start one after the given base.
# You may need to open Firewall port on the Controller machine
client.rmi.localport=1099
> ```

> ![/assets/images/views/devops/jmeter/Untitled.png](/assets/images/views/devops/jmeter/Untitled.png)

> ## Server(Slave)

> ### 1) server port 설정

> ```bash
# RMI port to be used by the server (must start rmiregistry with same port)
server_port=1099
> ```

> ### 2) server rmi localport 설정

> ```bash
# To use a specific port for the JMeter server engine, define
# the following property before starting the server:
server.rmi.localport=1099
> ```

> ![/assets/images/views/devops/jmeter/Untitled1.png](/assets/images/views/devops/jmeter/Untitled1.png)

# 실행

public 으로 연결시 pubilc ip

> ## Client(Master)

> ```bash
sh jmeter -Djava.rmi.server.hostname={Master IP}
> ```

> sh jmeter -Djava.rmi.server.hostname={master public ip}

> ## Server(Slave)

> ```bash
./jmeter-server -Djava.rmi.server.hostname={Slave IP}
> ```

> ./jmeter-server -Djava.rmi.server.hostname={client public ip}

# Plugin

> ![/assets/images/views/devops/jmeter/Untitled2.png](/assets/images/views/devops/jmeter/Untitled2.png)

<!-- 
# Sample file

# [HTTP Request.jmx](/assets/file/devops/jmeter/HTTP_Request.jmx)
-->
