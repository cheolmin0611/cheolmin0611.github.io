---
layout: views
title: Jenkins
parent: docker
nav_order: 3
permalink: /docker/jenkins
---

# ![jenkins icon](/assets/images/icon/jenkins.png){:class="icon-title"} Jenkins

* [Docker run 명령어 실행](#docker-run-명령어-실행)

jenkins

# Docker run 명령어 실행

```bash
docker run -d -i -t \
-v ~/docker/mount/jenkins/var/jenkins_home:/var/jenkins_home \
--env JENKINS_OPTS='--httpPort=50002' \
--env TZ=Asia/Seoul \
--network host \
--name jenkins \
--restart always \
-u root \
jenkins/jenkins:lts
```
