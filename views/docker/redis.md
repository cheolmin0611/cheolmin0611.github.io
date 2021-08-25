---
layout: views
title: Redis
parent: docker
nav_order: 4
permalink: /docker/redis
---

# ![redis icon](/assets/images/icon/redis.png){:class="icon-title"} Redis

* [redis docker 실행](#redis-docker-실행)
* [redis cli 실행](#redis-cli-실행)

Ubuntu 20.04 Docker로 적용
{:class="desc-text"}

# redis docker 실행

```bash
docker run -d -i -t \
-p 6379:6379 \
-v ~/docker/mount/redis/data:/data \
--name redis --restart always \
redis redis-server --appendonly yes
```

# redis cli 실행

```bash
docker run -it --rm \
--net host \
redis redis-cli -p 6379
```