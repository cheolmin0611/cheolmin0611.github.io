---
layout: views
title: docker
nav_order: 2
has_children: true
permalink: /docker
---

# ![docker icon](/assets/images/icon/docker.svg){:class="icon-title"} Docker 설치

* [필수 패키지 설치](#필수-패키지-설치)
* [GPG Key 인증](#gpg-key-인증)
* [docker repository 등록](#docker-repository-등록)
* [apt docker 설치](#apt-docker-설치)
* [권한 설정](#권한-설정)
* [docker private registry 설정](#docker-private-registry-설정)
    * [docker private registry 설치](#docker-private-registry-설치)
    * [docker private registry 연결 설정](#docker-private-registry-연결-설정)
    * [docker private registry repository 제거](#docker-private-registry-repository-제거)
* [Docker Images](#docker-images)
    * [MySql](#docker-images)
    * [GitLab](#docker-images)
    * [Jenkins](#docker-images)
    * [Redis](#docker-images)
    * [RabbitMQ](#docker-images)
    * [Elasticsearch](#docker-images)


Ubuntu 20.04 Docker 적용
{:class="desc-text"}

# 필수 패키지 설치

```bash
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```

![docker package install result](/assets/images/views/docker/Untitled.png)

# GPG Key 인증

Docker의 GPG Key 인증
{:class="desc-text"}

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

![docker GPG key 인증](/assets/images/views/docker/Untitled1.png)

# docker repository 등록

아키텍쳐에 맞춰서 Docker repository를 등록
{:class="desc-text"}

아키텍쳐 확인을 원하시면 **arch** 명령어
{:class="desc-text"}

![cpu arch check](/assets/images/views/docker/Untitled2.png)

```bash
sudo add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) \
stable"
```

![docker repository regist](/assets/images/views/docker/Untitled3.png)



# apt docker 설치

```bash
sudo apt-get update && sudo apt-get install -y docker-ce docker-ce-cli containerd.io
```

![docker apt install](/assets/images/views/docker/Untitled4.png)



# 권한 설정

현재 사용자에게 Docker에 대한 권한을 부여
{:class="desc-text"}

```bash
sudo usermod -aG docker $USER
```

$USER는 현재 사용자를 나타내는 환경 변수

login 된 계정 exit 후 다시 접속 시 권한부여 완료

# docker private registry 설정

docker의 private repository를 설정
{:class="desc-text"}

## docker private registry 설치

```bash
docker run -d -i -t -p 5000:5000 \
-v ~/docker/mount/registry/var/lib/registry/docker/registry:/var/lib/registry/docker/registry \
--name registry \
--restart=always registry
```

## docker private registry 연결 설정

```bash
sudo vi /etc/docker/daemon.json

{
    "insecure-registries": ["10.10.0.141:5000"]
}
```

설정 적용을 위한 docker restart

```bash
sudo service docker restart
```

## docker private registry repository 제거

```bash
curl -X GET http://10.10.0.141:5000/v2/_catalog 

docker exec -it registry sh      => registry container에 shell로 접속

cd /var/lib/registry/docker/registry/v2
rm -rf ./repositories/**ubuntu**/   => 레파지토리 삭제
exit

docker exec -it registry  bin/registry garbage-collect  /etc/docker/registry/config.yml

docker stop registry
docker start registry
```



# Docker Images

<details>
<summary>docker image list</summary>
<div markdown="1">

***
> #### ![mysql icon](/assets/images/icon/mysql_original_logo_icon_146416.png){:class="icon-title"} [MySql](/docker/mysql)
> #### ![gitlab icon](/assets/images/icon/gitlab-icon-rgb.png){:class="icon-title"} [GitLab](/docker/gitlab)
> #### ![jenkins icon](/assets/images/icon/jenkins.png){:class="icon-title"} [Jenkins](/docker/jenkins)
> #### ![Redis icon](/assets/images/icon/redis.png){:class="icon-title"} [Redis](/docker/redis)
> #### ![rabbitMQ icon](/assets/images/icon/rabbitmq.png){:class="icon-title"} [RabbitMQ](/docker/rabbitmq)
> #### ![elasticsearch icon](/assets/images/icon/elastic-elasticsearch.png){:class="icon-title"} [Elasticsearch](/docker/elasticsearch)

</div>
</details>
---
