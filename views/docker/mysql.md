---
layout: views
title: MySql
parent: docker
nav_order: 1
permalink: /docker/mysql
---

# ![mysql icon](/assets/images/icon/mysql_original_logo_icon_146416.png){:class="icon-title"} MySql

* [Docker run 명령어 실행](#docker-run-명령어-실행)

Ubuntu 20.04 Docker로 적용
{:class="desc-text"}

# Docker run 명령어 실행

```bash
docker run -d -p 3306:3306 -i -t \
-e MYSQL_ROOT_PASSWORD=1q2w3e4r \
-v /home/makestar/docker/mount/mysql/var/lib/mysql:/var/lib/mysql \
-v /home/makestar/docker/mount/mysql/etc/mysql:/etc/mysql \
-v /home/makestar/docker/mount/mysql/var/lib/mysql-files:/var/lib/mysql-files/ \
--name mysql --restart always mysql:latest
```

db 및 유저 생성

```sql
create database test_db character set utf8mb4 collate utf8mb4_general_ci;

create user test_user@'%' identified by 'testPass!';

grant all privileges on test_db.* to test_user@'%';

flush privileges;
```

my.cnf 설정

```bash
sudo vi /home/makestar/docker/mount/mysql/etc/mysql/my.cnf
```

my.cnf 파일 내용 -  docker 재시작 해야 적용

```bash
[mysqld]
sort_buffer_size = 256K         # (인덱스를 사용할 수 없는) 정렬에 필요한 버퍼의 크기, ORDER BY 또는 GROUP BY 연산 속도와 관련
join_buffer_size = 256K         # 조인이 테이블을 풀스캔 하기 위해 사용하는 버퍼크기, 드리븐 테이블이 FULL SCAN할 때 사용됨
read_buffer_size = 256K         # 테이블 스캔에 필요한 버퍼크기
read_rnd_buffer_size = 256K     # 디스크 검색을 피하기위한 랜덤 읽기 버퍼크기, 정렬 대상이 커서 two pass 알고리즘을 쓸 때만 사용

long_query_time=1 # 쿼리 응답 시간이 1초 이상일 경우
slow_query_log = 1 # Slow Query Log 설정 ON
# slow_query_log_file = /var/log/mysql/mysql_slow.log # 로그 저장 경로  
# show variables like 'slow_query_%'; 로 default 로그 경로를 알 수 있다.
```