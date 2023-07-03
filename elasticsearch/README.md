# Elasticsearch

Elasticsearch 是一个分布式、RESTful 风格的搜索和数据分析引擎，能够解决不断涌现出的各种用例。

## 1. 单机部署

### 1.1 使用命令行方式部署

```bash
# 创建 Network: netes7
docker network create netes7

# elasticsearch
## 启动容器
docker run -d --name es79 --net netes7 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" --restart=always elasticsearch:7.9.3
## 进入容器
docker exec -it es79 bash

# kibana
## 启动容器
docker run -d --name kibana79 --net netes7 -p 5601:5601 --link es79 -e "ELASTICSEARCH_HOSTS=http://es79:9200" --restart=always kibana:7.9.3
## 进入容器
docker exec -it kibana79 bash
```

### 1.2 使用 compose 方式部署

```bash
# 部署命令
docker-compose -f es79-docker-compose.yaml up -d

# 查看启动日志
docker-compose -f es79-docker-compose.yaml logs -f
```
