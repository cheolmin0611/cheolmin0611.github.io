---
layout: views
title: GitLab
parent: docker
nav_order: 2
permalink: /docker/gitlab
---

# ![gitlab icon](/assets/images/icon/gitlab-icon-rgb.png){:class="icon-title"} GitLab

* [Docker run 명령어 실행](#docker-run-명령어-실행)
* [Gitlab runner 설치](#gitlab-runner-설치)

Ubuntu 20.04 Docker로 적용
{:class="desc-text"}

# Docker run 명령어 실행

```bash
docker run -d -p **50001**:**50001** \
--name gitlab \
--restart always \
-v ~/docker/mount/gitlab/etc/gitlab:/etc/gitlab \
-v ~/docker/mount/gitlab/var/log/gitlab:/var/log/gitlab \
-v ~/docker/mount/gitlab/var/opt/gitlab:/var/opt/gitlab \
gitlab/gitlab-ce

```

/etc/gitlab/gitlab.rb  파일의 포트를 80 에서 위에 기입한 50001 로 변경 

external_url '[**http://10.10.0.141:50001**](http://10.10.0.141:50001/)'

컨테이너 안에서 실행

```bash
sudo gitlab-ctl reconfigure
sudo gitlab-ctl restart
```

registry_external_url '[**http://10.10.0.141:5000**](http://10.10.0.141:50001/)' — 이건 안해도 됨

컨테이너 안에서 실행

```bash
sudo gitlab-ctl reconfigure
sudo gitlab-ctl restart
```

root user의 비밀번호와 id를 찾고 싶은 경우

```bash
# container 안에서...
gitlab-rails console -e production
```

```bash
user = User.where(id:1).first
user.password='1q2w3e4r'
user.password_confirmation='1q2w3e4r'
```

![gitlab root user password find](/assets/images/views/docker/gitlab/Untitled.png)

# Gitlab runner 설치

pipeline을 사용하기 위해 gitlab runner 를 docker 로 설치 한다
{:class="desc-text"}

```bash
docker run -d --name gitlab-runner --restart always \
  --net=host \
  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  gitlab/gitlab-runner:latest
```

[http://[git_external_url]/admin/runners](http://10.10.0.141:50001/admin/runners) {:target="_blank"}

gitlab-ce 주소로 이동

docker container 에서 gitab-runner 등록

```bash
docker exec -ti gitlab-runner bash
```

```
root@48aea5eded7e:/# gitlab-runner register

Runtime platform                                    arch=amd64 os=linux pid=249 revision=a987417a version=12.2.0
Running in system-mode.

Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
https://mydomain.com

Please enter the gitlab-ci token for this runner:
<<your gitlab ci token here>>

Please enter the gitlab-ci description for this runner:
[48aea5eded7e]: sample-docker-runner

Please enter the gitlab-ci tags for this runner (comma separated):
docker (whatever you need)
Registering runner... succeeded                     runner=4usxjjv2

Please enter the executor: custom, docker-ssh, parallels, shell, docker+machine, docker, ssh, virtualbox, docker-ssh+machine, kubernetes:
docker

Please enter the default Docker image (e.g. ruby:2.6):
alpine:latest
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!

root@48aea5eded7e:/#
```

[https://bravenamme.github.io/2020/11/09/gitlab-runner/](https://bravenamme.github.io/2020/11/09/gitlab-runner/){:target="_blank"}