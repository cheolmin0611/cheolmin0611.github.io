---
layout: views
title: RabbitMQ
parent: docker
nav_order: 5
permalink: /docker/rabbitmq
---

# ![rabbitmq icon](/assets/images/icon/rabbitmq.png){:class="icon-title"} RabbitMQ

* [rabbitmq docker 실행](#rabbitmq-docker-실행)
* [rabbitmq management web 접속](#rabbitmq-management-web-접속)
    * [User 생성](#user-생성)
    * [Virtual host 생성](#virtual-host-생성)
    * [Virtual Host Permission 부여](#virtual-host-permission-부여)

email 발송을 위한 RabbitMQ with docker guide

# rabbitmq docker 실행

최신버전 부터 환경설정으로의 RABBITMQ_DEFAULT_USER, RABBITMQ_DEFAULT_PASS deprecate 되고 config 파일로 관리됨으로 안정화 후 변경 필요

```bash
docker run -d \
--name rabbit-mq \
--restart=unless-stopped \
-p 15672:15672 \
-p 5672:5672 \
-v /home/rabbitmq/data:/var/lib/rabbitmq/ \
-v /home/rabbitmq/logs:/var/log/rabbitmq/ \
-e RABBITMQ_DEFAULT_USER=userName \
-e RABBITMQ_DEFAULT_PASS=password \
rabbitmq:3.8.17-management
```

# rabbitmq management web 접속

RabbitMQ web client 설정
{:class="desc-text"}

```text
http://{host}:15672/
```

> ### User 생성

> ![rabbitmq user 생성](/assets/images/views/docker/rabbitmq/Untitled.png)

> ### Virtual Hosts 생성

> ![Virtual host 생성](/assets/images/views/docker/rabbitmq/Untitled1.png)

> ### Virtual Host Permission 부여

> ![Virtual Host Permission 부여](/assets/images/views/docker/rabbitmq/Untitled2.png)