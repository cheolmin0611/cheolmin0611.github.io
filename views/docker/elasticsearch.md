---
layout: views
title: Elasticsearch
parent: docker
nav_order: 6
permalink: /docker/elasticsearch
---

# ![elasticsearch icon](/assets/images/icon/elastic-elasticsearch.png){:class="icon-title"} Elasticsearch

* [docker elasticsearch 설치](#docker-elasticsearch-설치)
* [docker kibana 설치](#docker-kibana-설치)

# docker elasticsearch 설치

```bash
docker run -d -i -t \
-e node.name=kifarunix-demo-es \
-e cluster.name=es-docker-cluster \
-e discovery.type=single-node \
-e "ES_JAVA_OPTS=-Xms512m -Xmx512m" \
-v ~/docker/mount/elasticsearch:/usr/share/elasticsearch/data \
-p 9200:9200 \
--name elasticsearch  --restart always elasticsearch:7.11.2
```

# docker kibana 설치

```bash
docker run -d -i -t \
-e "ELASTICSEARCH_URL=http://10.10.0.102:9200" \
-e "ELASTICSEARCH_HOSTS=http://10.10.0.102:9200" \
-p 5601:5601 \
--name kibana --restart always kibana:7.11.2
```