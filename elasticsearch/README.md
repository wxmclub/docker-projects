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

## 2. 扩展

### 2.1 中文 ik 分词

```bash
# 进入容器
docker exec -it es79 /bin/bash

# 切换路径到plugins下
cd plugins
# 创建 ik 目录
mkdir ik
cd ik
# 下载文件，版本要与es一致
curl -O https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.9.3/elasticsearch-analysis-ik-7.9.3.zip
# docker cp elasticsearch-analysis-ik-7.9.3.zip es79:/usr/share/elasticsearch/plugins/ik
# 等待下载完成后 解压
unzip elasticsearch-analysis-ik-7.9.3.zip
## 把解压文件elasticsearch下的全部内容移动到当前目录
#mv elasticsearch/* .
# 这时候就可以重启容器了 先退出
exit

# 重启容器
docker restart es79
```

- 测试结果

```
GET _analyze
{
  "analyzer": "ik_smart",
  "text": ["中文分词语"]
}
```

- 结果

```json
{
  "tokens": [
    {
      "token": "中文",
      "start_offset": 0,
      "end_offset": 2,
      "type": "CN_WORD",
      "position": 0
    },
    {
      "token": "分",
      "start_offset": 2,
      "end_offset": 3,
      "type": "CN_CHAR",
      "position": 1
    },
    {
      "token": "词语",
      "start_offset": 3,
      "end_offset": 5,
      "type": "CN_WORD",
      "position": 2
    }
  ]
}
```
