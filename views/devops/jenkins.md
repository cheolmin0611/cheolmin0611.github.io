---
layout: views
title: Jenkins
parent: devops
nav_order: 1
permalink: /devops/jenkins
---

# ![jenkins icon](/assets/images/icon/jenkins.png){:class="icon-title"} Jenkins

* [Jenkins plugin list](#jenkins-plugin-list)
* [Global Tool Configuration](#global-tool-configuration)
    * [JDK-15](#jdk-15)
    * [Gradle](#gradle)
    * [NodeJS](#nodejs)
* [Java Server](#java-server)
    * [General](#general)
    * [소스 코드 관리](#소스-코드-관리)
    * [빌드 유발](#빌드-유발)
    * [빌드 환경](#빌드-환경)
    * [Build](#build)
        * [Gradle](#gradle-1)
        * [Excute shell](#excute-shell)
        * [SSH shell command](#ssh-shell-command)
* [NodeJS](#nodejs-1)
    * [General](#general-1)
    * [소스 코드 관리](#소스-코드-관리-1)
    * [빌드 유발](#빌드-유발-1)
    * [빌드 환경](#빌드-환경-1)
    * [Build](#build-1)
        * [NodeJS](#nodejs-2)
        * [Excute shell](#excute-shell-1)
        * [SSH shell command](#ssh-shell-command-1)

jenkins 

# Jenkins plugin list
jenkins로 gradle build와 docker image 생성을 위해 설치한 plugin 목록

![jenkins plugin list](/assets/images/views/devops/jenkins/Untitled.png)

- AnsiColor - console log 결과에 색상을 표시하기 위해
- Gradle Plugin -  project의 gradle bulid
- SSH Plugin - ssh excute shell 을 위한 plugin
- NodeJS Plugin - node project build 및 배포

# Global Tool Configuration

> ### **JDK-15**
> linux binary 파일을 jenkins docker가 접근가능 한 경로로 copy

> ![/assets/images/views/devops/jenkins/Untitled1.png](/assets/images/views/devops/jenkins/Untitled1.png)

> ### **Gradle**
> 현재 build 되고 있는 gradle 프로젝트의 version 설정

> ![/assets/images/views/devops/jenkins/Untitled2.png](/assets/images/views/devops/jenkins/Untitled2.png)

> ### **NodeJS**
> 현재 build 되고 있는 node 프로젝트의 version 설정

> ![/assets/images/views/devops/jenkins/Untitled3.png](/assets/images/views/devops/jenkins/Untitled3.png)

# Java Server
Freestyle project 생성

Git source pull → gralde build → docker build → docker image push

> ### **General**

> ![/assets/images/views/devops/jenkins/Untitled4.png](/assets/images/views/devops/jenkins/Untitled4.png)

> build script 에서 사용되는 parameter

> ### **소스 코드 관리**

> ![/assets/images/views/devops/jenkins/Untitled5.png](/assets/images/views/devops/jenkins/Untitled5.png)

> git source pull

> ### **빌드 유발**

> skip

> ### **빌드 환경**

> ![/assets/images/views/devops/jenkins/Untitled6.png](/assets/images/views/devops/jenkins/Untitled6.png)

> consol log color 를 위한 ansi color 체크

> ### **Build**

> ![/assets/images/views/devops/jenkins/Untitled7.png](/assets/images/views/devops/jenkins/Untitled7.png)

>   > ### **Gradle**

>   > ![/assets/images/views/devops/jenkins/Untitled8.png](/assets/images/views/devops/jenkins/Untitled8.png)

>   > gradle build - multi module project 이기 때문에 개별적으로 build 할 project 명 멍시

>   > ex) clean build -b build.gradle -p **makestar-user**

>   > ### **Excute shell**

>   > ![/assets/images/views/devops/jenkins/Untitled9.png](/assets/images/views/devops/jenkins/Untitled9.png)

>   > Dockerfile 생성, CURRENT_COMMIT_VER 의 이름으로 docker image tag 명 지정

>   > ### **SSH shell command**

>   > ![/assets/images/views/devops/jenkins/Untitled10.png](/assets/images/views/devops/jenkins/Untitled10.png)

>   > docker 로 운용되는 jenkins의 host 서버에서 docker image build, push

# NodeJS
Nuxt  - Freestyle project 생성

Git source pull → node version setting → npm build & docker file → docker image push

> ### General

> ![/assets/images/views/devops/jenkins/Untitled11.png](/assets/images/views/devops/jenkins/Untitled11.png)

> ![/assets/images/views/devops/jenkins/Untitled12.png](/assets/images/views/devops/jenkins/Untitled12.png)

> build script 에서 사용되는 parameter

> ### 소스 코드 관리

> ![/assets/images/views/devops/jenkins/Untitled13.png](/assets/images/views/devops/jenkins/Untitled13.png)

> git source pull

> ### 빌드 유발

> skip

> ### 빌드 환경

> ![/assets/images/views/devops/jenkins/Untitled14.png](/assets/images/views/devops/jenkins/Untitled14.png)

> consol log color 를 위한 ansi color 체크

> ### Build

> ![/assets/images/views/devops/jenkins/Untitled7.png](/assets/images/views/devops/jenkins/Untitled7.png)

>   > ### **NodeJS**

>   > ![/assets/images/views/devops/jenkins/Untitled15.png](/assets/images/views/devops/jenkins/Untitled15.png)

>   > build 시 사용할 node 버전 명시

>   > ### **Excute shell**

>   > ![/assets/images/views/devops/jenkins/Untitled16.png](/assets/images/views/devops/jenkins/Untitled16.png)

>   > Dockerfile 생성

>   > echo "FROM node:14.16.1" > Dockerfile 

>   > echo "EXPOSE 9529" >> Dockerfile

>   > ### **SSH shell command**

>   > ![/assets/images/views/devops/jenkins/Untitled17.png](/assets/images/views/devops/jenkins/Untitled17.png)



### AWS 설치 참고

```bash
aws s3 bucket 이전 build 버전 파일 삭제

aws s3 rm s3://beatle-test/test/ --recursive --exclude "$(cat previous.ver)/*"

# docker 로 운용되는 jenkins의 host 서버에서 docker image build, push
```